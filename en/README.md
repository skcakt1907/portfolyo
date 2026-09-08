*[Türkçe →](../README.md)*

# Aykut Yar — Portfolio

I build client web applications with Laravel at a digital agency: corporate
sites, e-commerce, multilingual catalogue sites and a CRM/invoicing system.
Everything below is **live and in daily use by real customers.**

This repository contains no source code and names no clients — the code and the
brands belong to them. What you'll find here is what I built, which problems I
solved and how, plus a few technical write-ups.

References and live URLs available on request.

---

## Live

| Project | What it is | Stack |
|---|---|---|
| [CRM &amp; invoicing](projects/crm-invoicing.md) | Customers, invoices, expenses, dealers, tasks · 200+ tables | Laravel 12 · MariaDB |
| [Curtain retailer](projects/curtain-retailer.md) | Multilingual catalogue site for the Dutch market | Laravel 13 |
| [Tours &amp; transfers](projects/tour-booking.md) | Booking flow, price tiers, multilingual | Laravel 12 |
| [Gourmet e-commerce](projects/gourmet-shop.md) | Catalogue, cart, payments, two languages | Laravel 13 |
| [Corporate site](projects/corporate-multilingual.md) | 4 languages with RTL layout for Arabic and Farsi | PHP |
| [Health tourism](projects/health-tourism.md) | Brochure site and enquiry form | PHP |
| [Roadside assistance](projects/roadside-assistance.md) | 24/7 towing company, mobile-first | PHP |
| [Flooring services](projects/flooring.md) | Service pages and quote form | PHP |

## In progress

| Project | What it is | Stack |
|---|---|---|
| [Boat tour booking](projects/boat-booking.md) | Multi-vendor booking, WhatsApp integration | Laravel 13 · **Filament** |
| [Car dealership &amp; parts](projects/car-dealership.md) | Showroom, parts shop and rentals in one panel | Laravel 13 |
| [Optical e-commerce](projects/optical-shop.md) | Cart, payments, accounts, admin panel | Laravel 13 |

## Product work

| Project | What it is | Stack |
|---|---|---|
| [Industry theme series](projects/theme-series.md) | 52 sellable themes across 24 industries, generated from one base | PHP |

## My own project

Not client work — built for my own needs.

| Project | What it is | Status |
|---|---|---|
| [SiteWatch](projects/sitewatch.md) | Uptime and SSL monitoring panel | works, not yet deployed |

---

## Technical write-ups

Patterns taken from real projects and made self-contained:

- [Preventing duplicate notifications](patterns/duplicate-notifications.md) — the race condition behind an e-mail sent twice
- [Resolving URLs per language](patterns/multilingual-slugs.md) — multilingual without a locale prefix
- [Rescuing URLs of a removed language](patterns/legacy-redirects.md) — redirecting 96 URLs without writing a list
- [Living with legacy data](patterns/varchar-dates.md) — querying dates stored as text without breaking them

---

## How I work

**I measure before I claim.** I don't say I've found the cause of a bug until
I've reproduced it locally. There's a real difference between "it's probably
this" and "I proved it".

**A backup and a rollback plan before touching production.** Every delivery
ships as separate steps: measure, apply, verify, roll back.

**I write down the why, next to the code.** If someone hit a trap, I record why,
so the next person doesn't hit it too — the code already says what it does.
