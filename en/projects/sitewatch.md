*[Türkçe →](../../projeler/sitewatch.md)*

# SiteWatch

**Laravel 11 · Livewire · Tailwind**

> An internal tool I wrote myself — not client work. It runs and has been tested
> against real sites, but is **not yet deployed to a server.**

Monitors whether the client sites I maintain are up, when their SSL certificates
expire and when their domains run out.

## Why I built it

Two things happened close together: a client site's detail pages were broken for
weeks and nobody noticed, and on another site an SSL certificate expired and the
client told us. Both were things we should have seen first.

## What it does

- Checks sites on a schedule, recording status code and response time
- Reads SSL certificate expiry and issuer
- E-mails both the administrator and the customer when a site goes down
- Panel filters: down, expiring SSL, expired SSL

The alert only fires **after two consecutive failed checks** — so a single blip
doesn't wake anyone at 3am.

## A limitation I know about

The panel checks only the homepage of each site. It **cannot catch** the failure
described above, where detail pages were broken while the homepage was fine.
Per-site additional check URLs are the next piece of work.

Knowing what a tool can't do matters as much as knowing what it can.
