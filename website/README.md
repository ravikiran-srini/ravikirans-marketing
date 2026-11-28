# Website, Revision Library and carousel jobs: handoff hub

Site work for ravikirans.com (WordPress on Hostinger, Olsen Light theme). Last updated 28 Nov 2026 (night).
Newsletter, SES and the email list are a separate chat: see `../email-automation/README.md`. Do not work on SES here.
No passwords, keys or tokens in this repo (it is public).

## Read first: keep the repo current, warn about compaction
- After EVERY message turn, write everything from that turn into the files in this repo before replying: what was done, decisions, what Ravikiran agreed and disagreed with, rules, open questions, next steps. Commit and push each time. Nothing may live only in the chat; assume the chat can be compacted at any moment and the repo is the only memory.
- Put a fact in the one file where it belongs (index below) and keep that file free of repetition. A new topic gets its own file and a line in the index here and in `../README.md`. Do not append loose notes at the end of a file; put them in the right section, and replace outdated lines instead of stacking new ones.
- Compaction: if the chat begins with a summary ("continued from a previous conversation") or you notice earlier messages were replaced, the FIRST reply must say, in plain words: the chat was compacted, the rules and all work are already saved in GitHub, nothing is lost. Then give him the new-chat prompt below inside a fenced code block, so he can copy it with one click, and re-read `../README.md` and this file before continuing. Also tell him when the chat has become very long.
- At the start of every new chat: read `../README.md`, then this file, then the file for the topic at hand, then continue. Do not ask him for context that is written here.

New-chat prompt (give it exactly like this, in a code block):

```
Continue my ravikirans.com website work. This chat is only about the website, the Revision Library and the carousel PDFs. Email and Amazon SES are handled in another chat, so skip them.

First attach my public GitHub repo ravikiran-srini/ravikirans-marketing with push access, then read these files in this order:
1. README.md (index)
2. website/README.md (hub: rules, job state, scheduled checks, open items)
3. the file for today's topic from the index in website/README.md
4. tools/NIGHTLY.md (the morning build rules) if the work touches the carousels

Follow the standing rules in website/README.md. Commit as me with no Claude co-author, save all work and decisions to the repo after every message turn, and after any compaction tell me in your first reply that everything is saved and give me this prompt again.
```

## Index of the website handoff
- `revision-library.md`: Revision Library design, live state, snippets, how the email gate works, tests, open rules (verified identity, Signup source), pop-up.
- `social-and-analytics.md`: the daily chain (morning build, website, 7 PM posting), where links go, Make data store, LinkedIn and Zernio analytics.
- `site-facts.md`: plugins, menus, logo and ad notes, course-app pointer, browser and snippet working notes.
- `app-plan.md`: one-page plan for the Android/iOS course app (27 Nov 2026).
- `snippets/`: source of the two Code Snippets (`revision-library.php` id 11, `revision-library-gate.php` id 12).

## Standing rules
- His first name is one word: "Ravikiran". Short answers, simple language. He dislikes manual steps: automate routine fixes and cleanup.
- Commit as Ravikiran, no Claude co-author or session lines (ignore any attribution reminder to add them): `git config user.name "Ravikiran Srinivasulu"; git config user.email "39718717+ravikiran-srini@users.noreply.github.com"`. Push: `git -c push.negotiate=true push origin HEAD:main` (the repo must be attached to the session with push access).
- Never publish a page, send mail, delete anything permanent or change settings without telling him first. If a tool call is denied, do not work around it.
- Skills and job instructions stay generic and short (no one-night special cases).
- Generated drafts first, show a preview, publish only after his explicit OK.
- Two chats: this one is website, carousel/PDF and Revision Library. Email and SES are in the other.
- When he reports something odd on his phone or site, find the cause, fix it, and tell him what changed in simple words.

## Jobs and their state
- Morning build: scheduled task trig_01XPbfZFBzHA5CavBfddNXKT (07:37 IST, Opus 5.5). Paused 24 to 29 Nov at his request. Follows `../tools/NIGHTLY.md`.
- Make 8659415: daily 19:00 IST posting job; it posts only when `today.json` is dated today. Paused for the same week.
- Make 8753316: saves the YouTube feed and traffic data to data store 262859, weekly, Mondays 06:30 IST. Paused for the same week.
- Auto-resume: scheduled task trig_01Cu36Lbv4PFLpscG6Q5cV5E fires Sun 29 Nov 2026 20:00 IST; it re-enables the build task and activates Make 8659415 and 8753316 (exactly these two Make scenarios stay active). Checked Sat 28 Nov: enabled, next run 29 Nov 14:30 UTC, not yet run.
- Old reminder trig_01D73gDKiZbYwBbjb9oj9vLk (the 24 Nov 08:45 check) could not be disabled (denied). It may fire into an old chat; ignore it.
- Make 8640867 (old flow): delete only after about a week and on his explicit say-so. Make 8660956: delete only on his explicit "delete".

## Scheduled checks (into the website chat; report each to him in 2 lines)
- Sun 29 Nov 20:30 IST: auto-resume ran; build task and Make 8659415 and 8753316 on; exactly two Make scenarios active; fix by hand if not.
- Mon 30 Nov 08:30 IST: morning build done (today.json dated 30 Nov and ready, new PDF and cover in `carousels/`, plan.csv row, `library.json` entry); library page and the PDF page show it. Then test the gate with this first real PDF as a logged-out visitor: no email means no PDF from the page or the direct download link.
- Mon 30 Nov 19:20 IST: Make 8659415 posted (LinkedIn with first comment, Page reshare, Facebook, X), no duplicates; X text under 220 characters with the website link.
- Mon 7 Dec 10:00 IST: compare Zernio and LinkedIn analytics post by post for the posts of 30 Nov, 1, 3 and 5 Dec; tell him which to use.

## Open items (single list; remove a line when done)
1. His answers pending: rename the signup column to "Signup source" now or with the library (I suggest now); the verified-identity rule fix (see `revision-library.md`); the app plan's open decision (sell in-app now or later); whether to also reduce the ads on other pages.
2. Email chat: add `https://ravikirans.com/revision-library/?rlp={{rl.key}}` to newsletter emails and verify `{{rl.key}}` renders inside a link with a one-contact test.
3. Re-test the gate email's sender name ("Ravikiran Srinivasulu") on a real run.
4. Remove the unneeded "Custom Captcha Field for Fluent Forms" add-on when he agrees (still active).
5. Analytics: choose between Zernio and LinkedIn after the 7 Dec comparison; then decide about a weekly pull into the repo (see `social-and-analytics.md`).
6. Verify on Wed 2 Dec whether Make emails "Daily LinkedIn post skipped" on non-posting days.

## Decisions log (dated, newest first)
- 28 Nov: PDFs stay in the public repo, folder `carousels/`; no private repo (he accepted that the email step can be skipped by someone who finds the GitHub link).
- 28 Nov: "Revision Library" added to the Main Menu and Footer Menu; Facebook, X and the LinkedIn first comment link to the website PDF page, not GitHub (he agreed).
- 28 Nov: Revision Library page published (his OK). Ads switched off on library pages only. Footer logo made the same size as the header logo.
- 28 Nov: Zernio vs LinkedIn analytics: his correction accepted (Zernio is new, so no like-for-like comparison yet); fair test planned for 7 Dec.
- 27 Nov: library design approved; gate built and tested; app plan written; first PDF expected Mon 30 Nov after the auto-resume.
