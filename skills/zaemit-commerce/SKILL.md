---
name: zaemit-commerce
description: Run the shop on a Zaemit site. Use when the user wants to add or change products, categories, options or stock, show products on a page, answer reviews, or process orders (deposit, tracking number, shipping status).
---

# Shop and orders

The shop is a plugin (`plugin_install`). Products appear on the site only after `product_embed` places them on a page.

## Workflows

### Sell the first product
1. `product_listCategories`; create the category with `product_upsertCategory` when it is missing.
2. `product_upsertProduct` with name, price and category; make it visible and purchasable when the user wants to sell now.
3. `product_embed` on the page the user named.
4. Report the page address, the product and its price. Report only what the responses show; do not raise worries about screens you cannot see.

### Ship one order
1. `order_getOrder`.
2. A bank transfer still in the ordered state: `confirm_deposit` after the user confirms the payment arrived.
3. `shipping_listCarriers` for the carrier code, then `set_tracking` with the tracking number.
4. Tell the user the customer may have been notified.

## Products
1. Check first: `product_listProducts` (up to 100 rows) or `product_getProduct` by id or exact name. Id 1 is usually the installed sample product.
2. Categories: `product_listCategories`; a new one with `product_upsertCategory`. A category slug that is not registered is rejected.
3. `product_upsertProduct`: without `id` it creates (name and price required); with `id` it updates.
4. `options` sets up to 3 option groups, for example `[{"name": "Size", "values": ["S", "M", "L"]}]` (no `=` or `/` in names and values). A product with no variants then gets one variant per combination; an empty array removes the groups.
5. `variants` is the full variant list (price, stock, display per combination). Variants left out are deleted, so read the product, change one variant and send the whole list. With `options`, only variants within the option combinations are accepted.
6. `display` (visible) and `sale` (purchasable) are separate. Hiding with `display=false` keeps order history; `product_setDisplay` changes these flags for up to 100 products.
7. Show products on a page: `product_embed` (list, showcase, rail, detail, wish). Card design comes from the shop; hand-written card markup is overwritten.
8. Reviews: `product_listReviews`; `product_replyReview` replaces the existing reply.

## Orders
- `order_status` for counts, `order_listOrders` for rows (without `date_from` every period is included), `order_getOrder` for one order.
- `order_updateOrderStatus` runs one step: `confirm_deposit` (bank transfer still in ordered state), `set_tracking` (carrier code from `shipping_listCarriers` and a tracking number of digits, letters and hyphens; a paid or preparing order moves to shipping), `preparing`, `shipping`, `delivered`, `confirmed`.
- Deposit confirmation and the move to shipping can notify the customer. Tell the user before running them.
- Cancellations and refunds are done in the Zaemit admin screen.
