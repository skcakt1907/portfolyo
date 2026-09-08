*[Türkçe →](../../projeler/tekne-rezervasyon.md)*

# Boat tour booking platform

**Laravel 13 · Filament · in progress**

A platform where boat owners list their vessels and visitors pick dates and send
enquiries. Admin panel built with Filament.

## Scope

- Boat records: photos, features, capacity, extras
- Seasonal price tiers
- Availability calendar and blocked periods
- Enquiries with status history and an audit log
- Per-vendor commission settings
- Consent records for data-protection compliance
- Pages and FAQs managed from the panel

## Technical decisions

**No payments — enquiry based.** The platform takes no money; the visitor sends
an enquiry and the deal is closed off-platform. This was deliberate: taking
payments means liability, reconciliation and refunds. It wasn't what the client
needed.

**WhatsApp integration with signature verification.** Enquiries also arrive over
WhatsApp. I added **signature verification** to the incoming webhooks — without
it the endpoint is open to anyone and forged messages can be injected. It's the
first thing I ask at any endpoint that accepts outside data: is this really from
the party that should be sending it?

**Availability conflicts.** Two enquiries can target the same dates; blocked
periods and existing bookings are checked separately.

## Working with changing scope

The project started as private boat charter and became **per-person group
tours.** That meant rebuilding pricing and capacity from scratch — price per
boat and price per person are two different models.

At one point a switch to a single-company model was tried and reverted the same
day. We could revert because the change moved in small commits.

When a client changes their mind, keeping the cost of reversal low works better
than arguing.
