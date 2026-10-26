# Zaemit

The `zaemit` MCP server manages one Zaemit website (Zaemit is 재밋 in Korean), the site chosen when the user signed in.

- Read the site first: `site_map`, `siteinfo_get`, `domain_list`. Use ids from list tools; do not guess them.
- Boards, inquiry forms, a shop and reservations are plugins: check `plugin_list`, install with `plugin_install`, then place them on a page with the feature's `*_embed` tool.
- A write can come back "not executed" with a confirmation request. Deleting and reverting need the user's yes to the exact target first.
- Names, phone numbers, emails and addresses come back masked unless raw access was allowed. Never send masked values back in a write.
- After a change, give the user the page address from the response. Report only what tool responses show.
- Reply in the user's language.

The skills in this extension (`zaemit-getting-started`, `zaemit-build-site`, `zaemit-site-pages`, `zaemit-boards-inquiries`, `zaemit-commerce`, `zaemit-reservations`) give the step-by-step workflows.
