*[Türkçe →](../../ornekler/varchar-tarih.md)*

# Living with legacy data — dates stored as text

## The situation

In a system I inherited, 56 date columns are `varchar`. Three different formats
sit side by side inside them:

```
2026-07-03              date
2026-07-03 14:22:10     datetime
1751500800              Unix timestamp
```

The consequence: comparisons like `whereBetween` do **string** comparison and
silently lose rows. Monthly reports were returning zero.

In one table 647 of 682 members (95%) were stored as Unix timestamps — the year
2022 showed "0 members" in reports when it actually had 313.

## Why I didn't just change the column type

The tempting move is `ALTER TABLE ... MODIFY date`. But:

- Values that can't be parsed become `0000-00-00` silently
- Dozens of places write to these columns and would all have to change at once
- It's hard to undo on a live accounting system

Fixing the **reads** first, so the system produces correct results, and cleaning
the data as a separate job, is safer.

## What I did

I moved the comparison into the database — `DATE()` interprets all three formats
correctly:

```php
// before (string comparison, losing rows)
->whereBetween('invoices.paid_at', [$from, $to])

// after
->whereRaw('DATE(invoices.paid_at) BETWEEN ? AND ?', [$from, $to])
```

There were 40 call sites. I turned the repeated shape into a query macro so new
code doesn't hit the same trap:

```php
Builder::macro('whereDateBetween', function (string $column, $from, $to) {
    $d = fn ($v) => $v instanceof DateTimeInterface
        ? $v->format('Y-m-d')
        : Carbon::parse($v)->toDateString();

    return $this->whereRaw("DATE($column) BETWEEN ? AND ?", [$d($from), $d($to)]);
});
```

## The cost

`DATE(column)` makes the column's index unusable. Row counts here are low enough
that it's an acceptable trade — but it's borrowed time, not a fix.

## The lesson

In legacy systems "the right thing" and "the thing you can do now" are different.
Stop the wrong answers first, clean the data as its own job. Trying to do both at
once is the fastest way to finish neither.
