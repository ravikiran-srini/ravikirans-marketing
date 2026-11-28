# ravikirans-marketing
Social posts, carousels, brand assets, website and newsletter work for ravikirans.com (public repo: no passwords, keys or tokens).
The daily job follows `tools/NIGHTLY.md`; Make publishes `today.json` at 7 PM IST.

## Start here in a new chat
1. Find your chat's folder in the ownership map below and read its hub file (`website/README.md` or `email-automation/README.md`).
2. Read `CROSS-CHAT.md` (items that need both chats).
3. Then the topic file you need. Do not ask Ravikiran for context that is written in these files.

## Ownership map (two chats, no overlap)
| Chat | Owns (writes only here) | Hub file |
|---|---|---|
| Website chat | `website/`, `tools/`, `carousels/`, `library.json`, `plan.csv`, `today.json`, `items.csv`, `categories.csv`, `brand/`, `design/` | `website/README.md` |
| Email chat | `email-automation/` | `email-automation/README.md` |
| Both (own rows/lines only) | this `README.md` (index lines for your own files), `CROSS-CHAT.md` (your own rows) | |

## Repo rules for every chat (decided 28 Nov 2026)
1. Write only inside your own folder. Never edit the other chat's folder. If something there must change, add a row to `CROSS-CHAT.md` owned by the other chat, and tell Ravikiran so he can pass it on.
2. In the two shared files (this index and `CROSS-CHAT.md`) touch only your own lines and rows.
3. Refer to the other chat's files by file name only, never by section name or line (sections get reorganised).
4. Before every save: `git pull --rebase origin main`. If a push is rejected, pull --rebase again and push again. Never force-push.
5. Commit as Ravikiran with no Claude co-author or session lines: `git config user.name "Ravikiran Srinivasulu"; git config user.email "39718717+ravikiran-srini@users.noreply.github.com"`. Push: `git -c push.negotiate=true push origin HEAD:main`.
6. After EVERY message turn save all work, decisions and rules to your own folder (commit and push) before replying. Nothing may live only in the chat.
7. After a compaction (the chat starts with a summary), the first reply says so, says everything is saved in GitHub, nothing is lost, and gives Ravikiran the new-chat prompt from your hub file in a fenced code block.
8. Put each fact in one file only; replace outdated lines instead of stacking notes at the end of a file.

## Index
- `CROSS-CHAT.md`: shared systems (who owns which snippet, plugin or data) and open items needing both chats.
- `website/README.md`: HUB of the website chat: rules, new-chat prompt, job state, scheduled checks, open items, decisions log. Its index points to:
  - `website/revision-library.md`: Revision Library design, snippets, email gate, tests, open rules.
  - `website/social-and-analytics.md`: daily chain (morning build, website, 7 PM posting), links, data store, analytics.
  - `website/site-facts.md`: plugins, menus, logo and ad notes, working notes.
  - `website/app-plan.md`: one-page plan for the Android/iOS course app.
  - `website/snippets/`: source of the two Code Snippets.
- `email-automation/README.md`: HUB of the email chat: newsletter, FluentCRM, Amazon SES.
- `library.json`: the Revision Library data the site reads; the morning job appends to it.
- `tools/NIGHTLY.md`, `tools/render.py`, `tools/style.css`, `tools/brand.py`: the morning carousel job and its renderer.
- `categories.csv`, `items.csv`, `plan.csv`, `today.json`, `carousels/`: job inputs and outputs (PDF and cover PNG per day in `carousels/`).
- `brand/`, `design/`: logos and design images.
