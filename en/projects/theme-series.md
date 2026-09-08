*[Türkçe →](../../projeler/tema-serisi.md)*

# Industry theme series

**52 themes · 24 industries · PHP · sellable product**

A theme series for small businesses that need a corporate site quickly, where
content and visual language change per industry.

## Industries covered

Corporate (14 variants), restaurant, hotel, construction, legal, architecture,
hairdressing, municipality, tours, gym, spa, optician, school, logistics,
accounting, medical, nursery, excavation, photography, associations, beach club,
serviced apartments and others.

## How they're produced

Instead of writing each by hand I built a **base theme** and a derivation
pipeline:

- Shared structure, components and admin panel live in the base theme
- Per-industry: block order, colour palette, typography and demo content
- Adding an industry means writing a definition, not a new theme

A fix in the base theme propagates to all of them; there aren't 52 codebases to
maintain.

## Why plain PHP

These get installed on shared hosting, often by someone with no technical
background. A setup requiring Composer, migrations and a terminal is a barrier
there. Upload the files, enter the database details, done.

Choosing the right tool isn't always choosing the most powerful one.

## What I took from it

In series production the hard part isn't code — it's **deciding where to stop
sharing.** Share too much and every industry looks the same, which removes the
reason to buy a specific theme; share too little and you're maintaining 52
projects. Structure became shared; appearance and content stayed separate.
