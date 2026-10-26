# Choosing a page tool

| The user wants to | Tool | Know this |
|---|---|---|
| Change wording only | `page_editText` | Markup, design and images stay. `find` is a substring; `all=true` replaces every match, otherwise the first one. When `find` is not found, the section's texts come back to choose from. |
| Change part of a section's HTML | `page_patchSection` | `find` matches the raw HTML from `page_getSection`, not the text preview. Each `find` is unique unless `all=true`; nothing saves when one is missing. |
| Replace a whole section's HTML | `page_updateSection` | Parts left out, including design, are lost. A section of 800 characters or more cannot be replaced with less than a fifth of its length. Long sections are safer with `page_patchSection`. |
| Give a section a new design | `page_replaceSectionWithBlock` | Pass the current wording again in `texts`; empty slots show the block's sample text. Photos may differ on each call. |
| Change a section's background | `page_setSectionStyle` | A dark background turns text white and inverts buttons. `background=reset` restores the original band. |
| Add a section | `page_insertSection` | HTML not taken from a block needs `basis`. It goes at the end by default, or at the start, or before or after a given section. |
| Move a section | `page_moveSection` | `to` is a 1-based position among body sections. Read `page_sections` again afterwards. |
| Delete a section | `page_deleteSection` | Confirmation tool. The last section cannot be deleted. Numbers shift afterwards. |
| Rename a page | `page_setTitle` | Menu items still using the old name follow; no revision is created, so a rollback does not undo it. Headings inside the page are wording (`page_editText`). |
| Rebuild a page from blocks | `page_compose` with `page_seq` | The whole body is replaced. |
| Replace a hand-written page | `page_update` | Block-built pages refuse css or js alone. Large pages save more reliably in two calls. |
| Delete a page | `page_delete` | Confirmation tool. The home page cannot be deleted. |
