*[Türkçe →](../../projeler/optik-eticaret.md)*

# Optical e-commerce

**Laravel 13 · Bootstrap 5 · payments · in progress**

End-to-end shop for an optician selling prescription glasses, sunglasses and
contact lenses: catalogue, cart, accounts, orders and an admin panel.

## What I paid attention to

**Stock locking.** If two people buy the same item at once, stock must not go
negative — the order path takes a row lock. I built this into the checkout flow
rather than patching it on later.

**Mass-assignment protection.** Models use an explicit fillable list; fields
like `role`, `total`, `status` and `order_no` cannot be set from a request and
are written server-side. An order total arriving from the request is one of the
classic e-commerce holes.

**Upload hardening.** The file extension is derived on the server, not taken
from the client; there's a mime whitelist (no SVG) and script execution is
disabled in the upload directory.

**Filtering.** Brand and gender filters are derived from product attributes
stored as JSON, and the brand list narrows to the selected category.
