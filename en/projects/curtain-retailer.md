*[Türkçe →](../../projeler/perde-firmasi.md)*

# Curtain retailer

**Laravel 13 · live**

Catalogue site for a Dutch curtain and sun-protection company: products,
project gallery, services, guides and contact. Originally four languages.

## A design decision worth explaining

There is no locale code in the URL — **the path itself carries the language:**

```
    /producten/jaloezieen      Dutch
    /produkte/jalousien        German
    /products/blinds           English
```

This reads better than `/nl/producten` and each language gets indexed under its
own keywords. The price is that path segments must be unique across all
languages; I enforce that with a dictionary class and a test.

## Problems I solved

**Three quarters of the site was down and nobody had noticed.** Every product,
category, gallery and service detail page returned an error — 144 of the 193
URLs in the sitemap. Listing pages worked, so from outside the site looked
healthy; a visitor clicking a product hit an error page.

The cause: detail pages resolve records through a per-language slug column, and
those columns had never been created in production. I diagnosed it by
reproducing rather than guessing — deleting the columns locally produced exactly
the same signature (listings fine, details broken) and restoring them fixed it.

**Removing a language took the whole site down.** The client wanted German and
Turkish dropped. Afterwards every page failed: the configured primary language
was still German, and route names are assigned based on the primary language, so
no page got its canonical name. I fixed the setting and added a permanent guard —
if the configured language is disabled, the system falls back to the first active
one, so this can't take the site down again.

**96 URLs from the removed languages.** Google had indexed them; leaving them
all as 404 would have cost the site its search position. Instead of a hand-written
list I built resolution from the records themselves, so it updates when products
change and disables itself if the languages ever come back.

Result: all 193 URLs either work or redirect correctly.

## What I took from it

A site being "up" doesn't mean its inside works. A monitor that only checks the
homepage would have missed this failure for weeks — and did.
