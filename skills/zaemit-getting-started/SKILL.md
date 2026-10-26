---
name: zaemit-getting-started
description: Orient in a Zaemit website connection. Use when the user has just connected Zaemit, asks what Zaemit can do or which site is connected, wants to turn on a feature such as a board, inquiry form, shop or reservations, or when a Zaemit call came back unconfirmed, masked or timed out.
---

# Working with a Zaemit site

Zaemit (재밋 in Korean) is a website builder. This connection works on one Zaemit site through the `zaemit` tools.

## Workflows

Each workflow runs in this order: check the current state, ask only for what is essential and missing, do the steps, confirm the result with tools, then report.

### First look at a connected site
1. Read `site_map`, `siteinfo_get` and `domain_list`.
2. Tell the user which site is connected and its public address, then offer a few concrete next steps.

### Turn on a feature (board, inquiry form, shop, reservations; contests and events can be installed and placed, and are run in the Zaemit admin screen)
1. `plugin_list`: install with `plugin_install` when the state is `none`. `plugin_get` shows settings it still needs.
2. Create the first item with the feature's own tools (see the feature's skill).
3. Place it on the page the user named with the feature's `*_embed` tool. If the user did not say where, ask once.
4. Confirm with the embed response (`inserted`, and for reservations `published` and `publish_missing`).
5. Report the page address, what is ready, and anything that must be finished in the Zaemit admin screen.

### Reporting
- Report only what tool responses show. You cannot see the rendered page: give the address and name what the user should look at, instead of guessing how it looks.
- When something was not done, say so first and give the reason from the response.

## The connection
- One connection is tied to one site. `site_list` shows which site it is; another site needs a new connection to that site.
- A connection made with the "new site" option on the consent screen starts with an empty site whose main page already exists (`site_map` → `main_page_seq`).
- Sites are public as soon as they are created. After a change, give the user the page address: `public_url` in the write response, or the default address from `domain_list`.
- Reply in the user's language. Call the service "Zaemit" (재밋 in Korean text).

## Features are plugins
- Boards, inquiry forms, a shop, reservations, contests and events are plugins. `plugin_list` shows each one's state (`none`, `inactive`, `active`); `plugin_get` shows settings it still needs, such as a map key; `plugin_install` installs it.
- Installing shows nothing on the site. Each feature's `*_embed` tool places it on a page.

## Confirmation
- A write can come back "not executed" with a confirmation request. Tell the user what will change, and call again with `confirmed=true` when it matches what they asked for.
- Deleting and reverting (`board_deletePost`, `board_deleteComment`, `page_delete`, `page_deleteSection`, `page_rollback`, `change_revert`) need the user's explicit agreement to the exact target. First call these tools without `confirmed`, show the user the target named in the response (post title, page, section) and wait for their yes. Ask every time, even when earlier changes in the same conversation were already confirmed. After the yes, `confirmed=true` returns a `confirm_token`, and a call with that token runs it.
- If the user says no or changes their mind, do not call again.

## Personal data
- Unless the connection was approved with raw personal data access, names, phone numbers, emails and addresses come back masked (for example 홍*동, 010-****-1234).
- Masked values are not data. Do not send them back in a write; send only the fields that change. When the user needs the original values, they can reconnect and allow raw access on the consent screen.
- The asterisks in masked values are Markdown emphasis marks. When you show masked values, put each one in inline code (for example `010-****-1234`) so the asterisks stay visible and the value does not look like real data.

## Ids and numbers
- Use ids returned by list tools; do not guess them. Id 1 is often a sample record.
- Section numbers shift after a section is moved or deleted; read `page_sections` again before the next section call.
- The "post 1" a user mentions is the number visitors see (`no`), not `post_seq`; look it up with `board_listPosts`.

## Slow saves
- Building a page with many photos can take longer than the client's time limit. When a save times out, read the page with `page_sections` before calling again; the save has often finished on the server.

## Not available in this connection
Managing contests and events (entries, judging, participants, registrations, sessions), coupons, visitor statistics, payments and refunds, bulk deletion, bulk messages, member withdrawal, publishing contest results, the sender mail password and changing the site's super administrator are done in the Zaemit admin screen. Say so plainly instead of offering a similar-sounding tool.
