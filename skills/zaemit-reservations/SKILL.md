---
name: zaemit-reservations
description: Take reservations on a Zaemit site. Use when the user wants to sell classes, rooms, tables, consultations or other time slots, set opening days and hours, put a booking screen on a page, or confirm and complete bookings.
---

# Reservations

Reservations are a plugin (`plugin_install`). Bookings are accepted only after three steps: a product, a schedule, and the booking screen published on a page.

## Workflows

### Start taking reservations
1. Check `plugin_list` and `reservation_listProducts`.
2. Ask only what is essential and missing: what is booked, which days and hours, how many people per slot.
3. `reservation_upsertProduct`, then `reservation_setSchedule` (`kind=weekly`), then `reservation_embed` on the page the user named. Do all three; a product without a booking screen takes no bookings.
4. Read `published` and `publish_missing` in the embed response.
5. Report the page address and what is open for booking. For each item in `publish_missing` that this connection cannot do, say it is done in the Zaemit admin screen. A test booking cannot be made in this connection: when it is the item left, tell the user to add one in the Zaemit admin screen under [예약자 관리 → 수동 예약 등록]. After it is added, call `reservation_embed` again: it publishes reservations once every condition is met. Do not offer to make the test booking.

### Handle new bookings
1. `reservation_listBookings` for requested bookings.
2. `reservation_confirm`, then `reservation_complete` after the visit.
3. Cancellations and refunds are done in the Zaemit admin screen.

## Set up
1. Check first: `reservation_listProducts`.
2. `reservation_upsertProduct`: a new product needs `name` and `booking_mode`. Without `id`, a product with exactly the same name is updated instead of duplicated.
3. `reservation_setSchedule`: `kind=weekly` for weekdays and hours, `kind=special` for one date (closed or changed hours). Without `id` a new schedule row is added; read `reservation_listSchedules` to change an existing one.
4. `reservation_embed(page_seq)` places the booking screen (`booking`) or the visitor's own bookings (`mine`). The response says whether reservations were published (`published`) and lists what is still missing (`publish_missing`). Tell the user what is missing.
5. `is_display=1` on the product alone does not show it to visitors.

## Bookings
- `reservation_listBookings` (requested, confirmed, completed, canceled, noshow), `reservation_getBooking`.
- `reservation_confirm` takes a slot; capacity-based products are re-checked against confirmed bookings. `reservation_complete` needs a confirmed booking.
- Customer details are masked unless raw access was allowed.
- Cancellations and refunds are done in the Zaemit admin screen.
