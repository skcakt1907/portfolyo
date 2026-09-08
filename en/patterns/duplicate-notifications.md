*[Türkçe →](../../ornekler/mukerrer-bildirim.md)*

# Preventing duplicate notifications — claim before you send

## The problem

A customer reported "the same reminder e-mail arrives twice". The logs held no
duplicate rows, so at first glance the complaint couldn't be confirmed.

In production data **every** notification record had been written twice, the
pairs 0–2 seconds apart:

```
362 | 1day | 2026-08-14 06:30:24
363 | 1day | 2026-08-14 06:30:25   ← one second later
```

## The cause

The code did this:

```php
if ($alreadySent($invoice)) {
    return;                       // 1. check
}

Mail::to($to)->send(...);         // 2. send
$log->record($invoice);           // 3. record
```

The job could be triggered from two different places. When both ran at once,
both saw "not sent" at step 1 and both sent.

A sibling table had a unique constraint — but that only blocked the second
**record**; the mail had already gone out. That is why the logs looked clean:
the constraint was erasing the evidence, not the problem.

## The fix

Reverse the order. The log row is written **before** sending, and that write is
used as an atomic claim:

```php
// Unique constraint on (invoice_id, type, date).
// insertOrIgnore returns 0 on the second run -> we skip WITHOUT sending.
$claimed = DB::table('notification_log')->insertOrIgnore([
    'invoice_id' => $invoice->id,
    'type'       => $type,
    'date'       => today(),
    'status'     => 'sending',
]);

if (! $claimed) {
    return;                        // another run already took it
}

try {
    $ok = Mail::to($to)->send(...);
    $log->update(['status' => $ok ? 'ok' : 'fail']);
} catch (\Throwable $e) {
    $log->update(['status' => 'fail', 'error' => $e->getMessage()]);
}
```

The key idea: **let the database close the gap between checking and acting.**
Anywhere you write "check, then do" in application code, that gap exists.

## Verification

Running the command twice in a row:

```
run 1 : mail sent,  log 1 row
run 2 : "already sent today, skipped",  log 1 row
```

## Note

This only works if the unique constraint actually exists. One sibling table was
missing it; adding it required cleaning up the existing duplicate rows first,
otherwise `ALTER TABLE` refuses.
