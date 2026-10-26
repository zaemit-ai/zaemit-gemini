---
name: zaemit-site-pages
description: Change an existing Zaemit page or site setting. Use when the user wants to edit wording, add, move, restyle, replace or delete sections, rename a page, change menus, header or footer, search settings, popups or site contacts, or undo a recent change.
---

# Editing a Zaemit site

## Workflows

### Several edits on one page
1. Read `page_sections` once and plan every change.
2. Do text and style changes first, then moves, then deletions. Section numbers shift after a move or deletion, so read `page_sections` again before the next numbered call.
3. Use the `page_seq` returned by each save for the next call.
4. Finish with `page_sections` and report the page address and each change.

### Tidy the menu
1. `menu_list`, then `menu_reorder`, `menu_update` or `menu_setDisplay`.
2. `component_get` on the header; if its links still show the old menu, update them with `component_update`.
3. Report the new menu and whether the header was changed too.

### Change contact details everywhere
1. `siteinfo_set` with the new contacts.
2. Find the old values written as text: `page_sections` with `include_content=true` on each page, and `component_get` on the header and footer.
3. Replace them with `page_editText` or `component_update`.
4. Report each place that was changed.

## Find the target first
1. `site_map` gives menus and pages; `page_list` gives every page with its address.
2. `page_sections(page_seq)` lists the body sections with index, block key and a text preview. Without `page_seq` it reads the home page. `include_content=true` adds each section's texts, links and images.
3. `page_getSection` returns the raw HTML of one section, for patches.

Header and footer are not page sections; they are site components (`component_list`, `component_get`, `component_update`).

## Pick the tool
See `references/page-tool-choice.md`. In short: wording → `page_editText`; part of a section's HTML → `page_patchSection`; a new design for a section → `page_replaceSectionWithBlock`; background color → `page_setSectionStyle`; order → `page_moveSection`; page name → `page_setTitle`.

## After each change
- Each page save becomes a new revision and returns a new `page_seq`; use the returned value for the next call on that page.
- Tell the user the page address from the response.

## Undo
- The last change on a page: `page_rollback` without `revision`. To go further back, read `page_revisions` and pass that row's revision. Revision 1 is usually the empty template from before the page was filled.
- Everything one earlier AI request changed (several pages, menus, components): `change_history` shows recent requests, `change_revert` restores one of them.
- Both are confirmation tools: show the user what will be restored and wait for their yes (see zaemit-getting-started).

## Menus
- `menu_list` gives `seq` and `id`, which are different numbers; `parent_id` refers to `id`.
- Building or publishing a page already adds its menu item. A page link stays hidden until the page passes the public quality check.
- Order: `menu_reorder`. Name or link of one item: `menu_update`. Show or hide: `menu_setDisplay`.
- Some headers have their menu links written in the header HTML, and menu changes do not reach them. After changing menu order, names or visibility, read the header with `component_get`; if its links still show the old menu, update them with `component_update` and tell the user the header was changed too.

## Header and footer
- They are shared by every page. Change them with `component_update`, not by inserting sections on a page.
- On a multilingual site each language has its own HTML; a structural change reaches one language, and `other_langs` in the response lists the others.

## Site information and search
- Name, contacts, address, keywords: `siteinfo_set`. Contacts already written as text inside pages or the footer do not change with it; update that text too (`page_editText`, `component_update`) and tell the user what was changed where.
- Search title, description, robots and verification codes: `seo_get` / `seo_set`. A temporary domain or a private site stays out of search even with search allowed.

## Popups
- `popup_upsert` without `id` creates a popup; with `id` it updates. On update, send title, size and position again: omitted ones reset to defaults. Omitted `is_use` and `is_window` keep their current values.
- Visitors see a popup only between `str_date` and `end_date` (YYYY-MM-DD). Turning it on without both dates is rejected; ask the user for the period when they did not give one.
- Buttons or links that do nothing when clicked are rejected.

## Languages
- `language_list`, `language_setDefault`. Adding a language is done in the Zaemit admin (it depends on the site plan); header and footer copies for an added language are edited with `component_update`.
