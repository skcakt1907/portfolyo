*[Türkçe →](../../ornekler/eski-adres-yonlendirme.md)*

# Rescuing the URLs of a removed language

## The problem

The client wanted two languages removed. **96 URLs** in those languages were
indexed by Google; leaving them all as 404 would have cost the site its search
position.

## Why not a static list

The easy route was a 96-line redirect table. But:

- A new product wouldn't be in the list
- A changed slug would make the list stale
- If the languages ever come back, the list has to be cleaned up by hand

## The approach

I derived the mapping from the records. The removed languages' slug columns are
**still in the database** — they were only taken out of the active locale list.
The record is found through them, then permanently redirected (301) to the
primary language's URL:

```php
/** Languages defined in the path dictionary but no longer active */
public static function removedLocales(): array
{
    return array_values(array_diff(
        array_keys(Paths::PAGES['catalog']),
        Locales::codes()
    ));
}
```

Routes are generated from that list. **If the languages come back the list is
empty and no routes are registered** — nothing else to undo.

## Two traps

**1. The redirect pointed at itself.** `/produkte` → `/produkte`.

The locale-detection middleware read the language from the path segment, then
the URL generator translated my Dutch target back into German. The fix was to
pin the locale to the primary language before building the target.

**2. Parameters were swapped.** Detail pages lost their path prefix.

Laravel passes untyped controller parameters **positionally, not by name.** The
route parameters arrived as `(slug, page)` while the method expected
`($page, $slug)`. I solved it by calling the controller explicitly:

```php
Route::get($p($page) . '/{slug}',
    fn (string $slug) => $controller()->detail($page, $slug));
```

Either of these reaching production would have produced "redirects exist but go
to the wrong place" — a hard failure to spot.

## Removed content

The discontinued service's pages weren't deleted, just deactivated. The redirect
found the record but the target page still failed. For inactive records I send
visitors to the relevant listing instead — and used **302, not 301**, because
the content can be switched back on from the admin panel. A 301 would have told
search engines to drop the URL permanently.

## Result

All 193 URLs either work or redirect correctly.
