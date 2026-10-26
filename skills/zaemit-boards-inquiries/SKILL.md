---
name: zaemit-boards-inquiries
description: Run bulletin boards and inquiry forms on a Zaemit site. Use when the user wants to create a board or notice page, write, edit or delete posts and comments, make or extend an inquiry or contact form, or read and answer submissions.
---

# Boards and inquiry forms

Both are plugins (`plugin_install`). Installing shows nothing; place them with `board_embed` or `inquiry_embed`.

## Workflows

### Open a notice board on the site
1. `board_list`: reuse a board when one with the same purpose exists; otherwise `board_create`.
2. `board_embed` on the page the user named. On the home page, `detail_page` makes it a recent-posts strip whose posts open on the board's own page.
3. `site_map` / `menu_list`: when the board's page has no menu item and the user wants it in the navigation, add one with `menu_create`, then check the header links (zaemit-site-pages, "Tidy the menu").
4. Write the first posts when the user gave them.
5. Report the page address and where the board appears.

### Rename a board
1. `board_update` with the new name.
2. The old name may also be written in page text or a menu item. Check with `page_sections` and `menu_list`, and change it with `page_editText` or `menu_update`.

### Add an inquiry form to a page
1. `page_sections` on that page: if it already has a form, use that form instead of creating another.
2. `inquiry_create` with `seed_basic_fields=true`, then `inquiry_addFields` for extra questions.
3. `inquiry_embed(form_id, page_seq)`.
4. Report the page address and the form's fields.

### Handle new inquiries
1. `inquiry_listSubmissions`, then `inquiry_getSubmission` for the one to answer.
2. Reply once with `inquiry_replySubmission` (it can notify the customer).
3. To move it to a stage, read `inquiry_listStages`. When the form has no stages, stage tracking is off: turning it on with `inquiry_updateForm` adds four default stages. Tell the user before turning it on; renaming stages is done in the Zaemit admin screen.

## Boards
1. `board_list` gives `board_seq` and `board_id`. A new board: `board_create` (types list, card, gallery, document, faq, qna, blog).
2. Put the board on a page with `board_embed(board_id, page_seq)`. An invented `board_id` is rejected and the rejection lists the real boards.
3. Posts: `board_addPost` with `fields` as one `{column: value}` object, for example `{"title": "Notice", "content": "..."}`. Edit with `board_updatePost`, sending only the columns that change.
4. The "post 3" a user mentions is the visible number (`no`). Find its `post_seq` with `board_listPosts` before editing or deleting. After a deletion the visible numbers are reassigned; read the list again before the next one.
5. Deleting a post or comment is a confirmation tool: show the user the post title and wait for their yes.
6. Posts cannot be moved between boards; adding a copy elsewhere leaves the original.

## Inquiry forms
1. `inquiry_list` shows forms. A new form: `inquiry_create` with `seed_basic_fields=true` (name, phone, email, message).
2. A page that already has a form block is connected on save; creating another form for it makes two forms. Check the page with `page_sections` first.
3. Place it with `inquiry_embed(form_id, page_seq)`; buttons can link to `#form-{form_id}`. A page being built takes the form through `page_compose` instead.
4. Extra fields: `inquiry_addFields`. A field added in the same call cannot be another field's display condition.

## Submissions
- `inquiry_listSubmissions` lists them, `inquiry_getSubmission` shows one. Personal data is masked unless raw access was allowed; say so when you show it.
- `inquiry_replySubmission` adds a reply each time it is called and can notify the customer. A sent notice cannot be recalled, so do not send the same reply twice. `is_secret=true` saves an internal memo the customer does not see.
- Stages: `inquiry_listStages` gives the stage codes the form accepts; `inquiry_setSubmissionStage` moves a submission.
