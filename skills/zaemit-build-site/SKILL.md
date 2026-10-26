---
name: zaemit-build-site
description: Build a Zaemit website or a new page from pre-designed blocks. Use when the user asks to make a website, a landing page or a new subpage (about, menu, contact, location), or to redesign a whole page.
---

# Building a Zaemit site

Pages are assembled from pre-designed blocks. The server adds styling, header, footer, menu links and photos: Zaemit library photos first, then stock photos from Unsplash and Lummi, and photos of people may be AI-generated (up to 6 per build, each charged in Zaemit credits). Photos from outside Zaemit are copied into the site storage on save.

## Workflows

### A new site from scratch
1. Follow "New site" below for the home page.
2. Subpages the user asked for: build each with "Subpages" below, or reserve them and ask whether to continue, following the connection's instructions.
3. Terms and privacy pages, when the footer needs them: `systempage_ensure`.
4. Run the finish check (next workflow), then report every page address.

### Finish check after building
1. `site_map`: every page the user asked for exists and has a menu item.
2. `component_get` on the header: its links match the menu (some headers have links written in their HTML; fix them with `component_update`).
3. `page_sections` on the home page: the sections are in the intended order, and the inquiry form appears once when one was asked for.
4. Report page addresses, what was built, and what the user still needs to give (hours, prices, real photos) where you used general wording they should replace.

## New site
1. Read the site: `site_map` (menus, pages, `main_page_seq`), `siteinfo_get` (name, industry text), `language_list` (default language).
2. Pages are written in the site's default language. If the user writes in another language, set it first with `language_setDefault`.
3. Set the site name, description and keywords with `siteinfo_set` when they are missing.
4. Pick blocks: `design_blocks(page="main")` for the home page. Use 10 to 15 body blocks in order (header and footer not counted): an opening block (TOP), content blocks (MID), and a closing or call-to-action block (BOTTOM). Subpages use 4 to 6. A page built from one block has a single section.
5. Read the slots of the chosen blocks in one call: `design_block(keys=[...])`.
6. Build the home page with `page_compose` and `page_seq` = `main_page_seq`. Building a new page instead gives it a `/page/{slug}` address and leaves the site root empty.
7. Give the user `public_url`.

## Subpages
- `design_blocks(page="sub")` → `design_block` → `page_compose` without `page_seq`; this creates the page and its menu item.
- Do not mix main and sub blocks on one page; the styles differ.
- Login, sign-up, my info, terms and privacy pages come from `systempage_ensure`, not from blocks.

## Filling blocks
- `texts` fill every slot in document order. An empty slot keeps the block's sample text and the save is rejected.
- Write the site's own content: its name, place, products, hours. Use the user's details; ask for them when they are essential and missing.
- Photo slots: `image_query` with an English stock-photo phrase, or one phrase per photo slot for galleries and cards.
- `concept` (minimal, rich, color, dark) is chosen once on the first page; later pages inherit it. Use minimal when unsure; dark makes every section dark.
- Buttons: `links` in the block's `link_slots` order (`/page/{slug}`, `#anchor` or an external URL). `anchor` names a section so other buttons can point to it.

## Header, footer and menus
- When the site has no header and footer, the first page save creates default ones. Change them with `component_update`.
- A header that lists subpages can be registered with `component_create(role="header")`.
- Pages built with `page_compose` add their own menu item. A menu item that points to a section anchor can be turned into a page link with `nav_convertToPage`.

## Forms and features on a page
- Inquiry form: give `page_compose` an `inquiry` value, or include a form block — not both, which makes two forms.
- Boards, products, reservations, contests and events are not blocks. Install the plugin, then place it with `board_embed`, `product_embed`, `reservation_embed`, `contest_embed` or `event_embed`.

## Hand-written HTML
- Use `page_publish` only for screens blocks cannot express or for moving existing content. It needs `basis` as "kind: reason" with kind `custom`, `migrate` or `data`.
- JavaScript is allowed, external CDN libraries included. Scripts that read login cookies, call admin pages, run eval or send passwords out, or hide the page body, are not saved; keep key wording in the HTML for search engines. Read `design_guide` for the brand rules that hand-written HTML follows.

## Finishing
- Saves return quality notes. Fix only the element a note names; do not rebuild the page.
- When a save times out, check with `page_sections` before calling again (see zaemit-getting-started).
