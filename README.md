# Zaemit for Gemini CLI and Antigravity

Zaemit is an AI web production service that takes your website from building to data management and day-to-day operations, all in one flow. This extension connects Zaemit to Gemini CLI and Antigravity so you can do it all by chatting.

## What it does

Building your site starts with one sentence: "Make a homepage for my pottery studio in Seongsu. Warm and calm, with class info, directions and a contact form." Zaemit designs the pages, writes the copy, fills in the photos and sets up the header, footer and menu. Your site goes live at its own address right away.

Then keep talking to run it:

- Page editing: headlines, sections, backgrounds, page names, menus, popups and search settings, with every change reversible.
- Inquiry forms: add forms and fields, check incoming inquiries, reply and track their status.
- Boards: open a notice board, write, edit or delete posts, and place the board on any page.
- Reservations: bookable services and schedules, see new bookings and confirm them.
- Shop: products, categories and options, prices and display, order status and review replies.

Contests, events, coupons and visitor statistics are run in the Zaemit admin screen.

## What's in it

- **The Zaemit MCP server** (`https://zaemit.ai/mcp/gemini`), declared in `gemini-extension.json` (Gemini CLI) and `mcp_config.json` (Antigravity).
- **`GEMINI.md`**: short context Gemini CLI loads with the extension.
- **Skills** (`skills/`): step-by-step workflows for getting started, building a site, editing pages, boards and inquiry forms, the shop, and reservations.

## Install

You need a Zaemit account (sign up at https://zaemit.ai). You don't need a site beforehand.

**Gemini CLI**

```
gemini extensions install https://github.com/zaemit-ai/zaemit-gemini
```

Then run `/mcp auth zaemit` and sign in to Zaemit in the browser.

**Antigravity CLI**

```
agy plugin install https://github.com/zaemit-ai/zaemit-gemini
```

**Antigravity app**

Download this repository and place the folder in `~/.gemini/config/plugins/` (all workspaces) or in `.agents/plugins/` at the root of a workspace (that workspace only), then restart Antigravity.

On the Zaemit consent screen, create a new site or pick an existing one. Each connection works on one site.

## Data

- The extension sends your requests to the Zaemit server at `zaemit.ai` through its MCP server entry, on behalf of the signed-in Zaemit account, for the connected site only.
- Responses contain the site's content and settings. Names, phone numbers, emails and addresses of members and customers are masked unless you allowed raw access when connecting.
- When Zaemit fills in photos, it uses the Zaemit photo library and stock photos from Unsplash and Lummi, and copies them into your site's storage. Photos of people may be AI-generated and use Zaemit credits.
- The extension itself stores nothing and contains no secrets. Gemini CLI and Antigravity keep the sign-in token on your machine. Zaemit handles the data it receives under its [privacy policy](https://zaemit.ai/privacy-policy).

## Support

Zaemit is operated by Weven.

- Customer center: https://zaemit.ai/customer-center (phone +82-2-6951-1136, weekdays 09:00-18:00, closed on weekends and public holidays)
- Privacy policy: https://zaemit.ai/privacy-policy
- Terms of service: https://zaemit.ai/terms-of-service

The customer center, privacy policy and terms are in Korean.

## License

MIT. See [LICENSE](LICENSE).
