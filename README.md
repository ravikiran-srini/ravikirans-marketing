# ravikirans-marketing
Social posts, carousels, brand assets and website design for ravikirans.com.
The daily job follows `tools/NIGHTLY.md`; Make publishes `today.json` at 7 PM IST.

## Index (start here in a new chat)
Rules for every chat: save all work, decisions and rules to these files after every message turn (commit and push), and after any compaction say so in the first reply, say everything is saved, and give Ravikiran the new-chat prompt in a code block. Full wording and the website prompt are in `website/README.md` (hub); the email chat has its own in `email-automation/README.md`.
- `website/README.md`: HUB for website, Revision Library and carousel/posting work: rules, new-chat prompt, job state, scheduled checks, open items, decisions log. Its index points to:
  - `website/revision-library.md`: Revision Library design, snippets, email gate, tests, open rules.
  - `website/social-and-analytics.md`: daily chain (morning build, website, 7 PM posting), links, data store, analytics.
  - `website/site-facts.md`: plugins, menus, logo and ad notes, working notes.
  - `website/app-plan.md`: one-page plan for the Android/iOS course app.
  - `website/snippets/`: source of the two Code Snippets.
- `email-automation/README.md`: newsletter, FluentCRM, Amazon SES (separate chat).
- `library.json`: the Revision Library data the site reads; the morning job appends to it.
- `tools/NIGHTLY.md`, `tools/render.py`, `tools/style.css`, `tools/brand.py`: the morning carousel job and its renderer.
- `categories.csv`, `items.csv`, `plan.csv`, `today.json`, `carousels/`: job inputs and outputs (PDF and cover PNG per day in `carousels/`).
- `brand/`, `design/`: logos and design images.
