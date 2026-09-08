*[Türkçe →](../../projeler/oto-galeri.md)*

# Car dealership &amp; parts

**Laravel 13 · in progress**

Three lines of one business in a single panel: a used-car showroom, a spare
parts shop and vehicle rentals.

## Modules

**Showroom** — vehicle records, multiple photos, a sold archive and a
comparison screen.

**Parts** — categorised catalogue, orders and order lines.

**Rentals** — rental records.

**Sell your car** — visitors request a valuation for their own vehicle.

## Why one system

They could have been three separate builds, but the vehicle record is common to
all three: for sale in the showroom, available in rentals, the subject of an
incoming offer in "sell your car". Keeping the same data in three places means
it breaks in three places.

I shared what is common and separated what is specific to each module.

## Note

Three features seen on a competitor's site were requested and added: a sold-cars
archive, vehicle comparison and "sell your car". With requests like these I try
to understand **why the feature works** rather than copying it — comparison works
because a used-car buyer is undecided and keeps flipping between two listings.
