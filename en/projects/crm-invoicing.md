*[Türkçe →](../../projeler/crm-faturalama.md)*

# CRM &amp; invoicing system

**Laravel 12 · MariaDB · live**

Customer tracking, quotes, invoicing, expense management, tasks and support,
a dealer network and automated reminder e-mails. 200+ tables, in daily use.

I inherited the system and now develop it: new modules, diagnosis, production
data fixes.

## Problems I solved

**Reports returned zero.** Date columns were stored as `varchar` holding three
different formats — plain dates, datetimes, and Unix timestamps. Queries doing
string comparison silently lost rows. I wrapped 40 call sites in `DATE()` and
added a query macro so the same trap isn't repeated in new code.

**The same reminder e-mail was sent twice.** Customers complained; the logs
showed nothing. In production data every notification had been written twice,
the pairs 0–2 seconds apart. The cause: the mail was sent first and the record
written after, so two triggers firing together both saw "not sent yet". I
reversed the order — claim the log row first, send only if the claim succeeds.

**Transactions were doing nothing.** The balance-payment flow was wrapped in
`DB::beginTransaction()` — the right instinct — but the tables were MyISAM,
which ignores transactions. I measured it:

```
    before transaction : 50
    inside transaction : 999999
    after rollback     : 999999      ← not rolled back
```

The practical risk: a balance deducted, then a failure, leaves the customer
paid but the invoice unpaid. I moved the eight money tables to InnoDB — proven
first on a separate copy, verified for row counts, indexes and collation, then
applied on production as measure → back up → convert → verify. 2,546 rows,
zero loss.

**A payment date that was never written.** The code wrote to a column that
did not exist. The guard failed every time, nothing was written, and **no error
was raised.** Of 1,688 paid invoices, 34 had no collection date and appeared in
no daily report. The same bug was repeated in three separate files.

**Multiple assignees on tasks.** I extended single-assignee tasks to many while
keeping a "primary assignee" concept, so existing reports that read only that
column kept working. 201 records migrated without loss.

## What I took from it

Silent failure is the expensive kind. Almost every serious bug I found here
raised no error and wrote no log — it just produced the wrong answer. I no
longer trust "it looks like it works"; I measure.
