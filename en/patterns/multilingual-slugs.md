*[Türkçe →](../../ornekler/coklu-dil-slug.md)*

# Resolving URLs per language

## Context

On a multilingual site we didn't want a locale code in the URL. The path itself
carries the language:

```
/producten/jaloezieen      Dutch
/produkte/jalousien        German
/products/blinds           English
```

This reads better than `/nl/producten` and each language indexes under its own
keywords. In exchange, two things are required: path segments must be unique
across every language, and content slugs must vary per language.

## Slug resolution

The primary language's slug lives in the base `slug` column, the others in
`slug_<code>` columns. The incoming URL's language is tried first, but every
language's column is checked — so a visitor holding an old URL after a language
change still lands on the right record:

```php
public function resolveRouteBinding($value, $field = null): ?Model
{
    $columns = [$this->slugColumn(app()->getLocale())];

    foreach (Locales::codes() as $code) {
        $column = $this->slugColumn($code);
        if (! in_array($column, $columns, true)) {
            $columns[] = $column;
        }
    }

    return $this->where(function ($q) use ($columns, $value) {
        foreach ($columns as $column) {
            $q->orWhere($column, $value);
        }
    })->first();
}

private function slugColumn(string $locale): string
{
    return $locale === Locales::primary() ? 'slug' : 'slug_' . $locale;
}
```

## Where this bites

The design has a hidden dependency: **the column must exist for every language.**
If it doesn't, the query fails with `Unknown column` and **every detail page**
breaks. Listing pages don't resolve slugs, so they keep working — meaning the
site looks healthy from outside.

That is exactly what happened in production: a migration had never been applied,
and three quarters of the site failed for weeks without being noticed.

The second bite: the column list is derived from the **primary language.** If the
primary language is German you need `slug_nl`; if it's Dutch you need `slug_de`.
Copying the local environment's list to production isn't enough — you have to
read production's primary language. I skipped that on the first attempt and added
the wrong columns.

## The lesson

When code depends on schema, don't assume the schema is there. Guarding with a
column check, or putting it on the deployment checklist, beats a silent failure
that lasts for weeks.
