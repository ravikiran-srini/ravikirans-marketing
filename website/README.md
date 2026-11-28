# Website, Revision Library and carousel jobs: handoff

Site work for ravikirans.com (WordPress on Hostinger, Olsen Light theme). Last updated 27 Nov 2026.
Newsletter, SES and the email list are a separate chat: see `../email-automation/README.md`. Do not work on SES here.
No passwords, keys or tokens in this repo (it is public).

## Read first: keep the repo current, warn about compaction
- After EVERY message turn, write everything from that turn into the files in this repo before replying: what was done, decisions, what he agreed and disagreed with, rules, open questions, next steps. Commit and push each time. Nothing may live only in the chat; assume the chat can be compacted at any moment and the repo is the only memory.
- Keep this file and the index in `../README.md` accurate and free of repetition. Put new topics in their own file and add them to the index.
- Compaction reminder: the moment you notice the chat has been compacted (a summary replaces earlier messages), tell Ravikiran in your next reply, in one line. Say that everything is saved in GitHub, then re-read `README.md` and this file before continuing. Also tell him when the chat has become very long, so he can start a new one with the prompt in his handoff.
- At the start of every new chat: read `README.md`, then this file, then continue. Do not ask him for context that is written here.

## Standing rules
- His first name is one word: "Ravikiran". Short answers, simple language. He dislikes manual steps: automate routine fixes and cleanup.
- Commit as Ravikiran, no Claude co-author or session lines: `git config user.name "Ravikiran Srinivasulu"; git config user.email "39718717+ravikiran-srini@users.noreply.github.com"`. Push: `git -c push.negotiate=true push origin HEAD:main` (the repo must be attached to the session with push access).
- Never publish a page, send mail, delete anything permanent or change settings without telling him first. If a tool call is denied, do not work around it.
- Skills and job instructions stay generic and short (no one-night special cases).
- Generated drafts first, show a preview, publish only after his explicit OK.
- Two chats: this one is website, carousel/PDF and Revision Library. Email and SES are in the other.

## Jobs and their state (27 Nov 2026)
- Morning build: scheduled task trig_01XPbfZFBzHA5CavBfddNXKT (07:37 IST, Opus 5.5). DISABLED for the week of 23 to 29 Nov at his request. Follows `tools/NIGHTLY.md`; writes `today.json`, `plan.csv`, `carousels/*`, and now also appends to `library.json`.
- Make 8659415: daily 19:00 IST post (Mon, Tue, Thu, Sat) from `today.json`. Paused for the week.
- Make 8753316: saves the YouTube feed and traffic data to data store 262859. Now WEEKLY, Mondays 06:30 IST. Paused for the week.
- Auto-resume: scheduled task trig_01Cu36Lbv4PFLpscG6Q5cV5E fires Sun 29 Nov 2026 20:00 IST; it re-enables the build task and activates Make 8659415 and 8753316, confirms all three, and tells him in one line. Checked Fri 27 Nov: enabled, next run 29 Nov 14:30 UTC, not yet run. After it fires, check it ran; if not, resume them by hand.
- Old reminder trig_01D73gDKiZbYwBbjb9oj9vLk (the 24 Nov 08:45 check) could not be disabled (denied). It may fire into an old chat; ignore it.
- Make 8640867: delete only after about a week and on his explicit say-so. Make 8660956: delete only on his explicit "delete".

## Revision Library (built 26 to 27 Nov 2026)
Design he approved:
1. A page grouped by category, newest first; each entry has cover, title, date, Download button. Filled automatically by the morning job from `library.json` (he edits nothing).
2. A "Quick Revision" box at the end of every study guide linking to that category on the library page. Applies to existing and new guides by itself.
3. A small public page per PDF (summary, cover, download button, related guide, his Udemy course).
4. Email gate: the download link is EMAILED (no attachment, no instant download). Clicking it confirms and subscribes the person and sets a cookie so the gate is skipped afterwards. Existing subscribers are recognised through personal links in newsletter emails (no re-entering email). A rare "Already subscribed? Enter your email" fallback is fine. No gate beyond email. Free on LinkedIn too; that is fine.
5. Later: a one-page plan for the Android/iOS course app (see below).

Done:
- Code Snippets snippet id 11 "Revision Library (page + Quick Revision box)", active, run everywhere. Source copy: `website/snippets/revision-library.php` (paste there if the snippet is ever lost). It adds the shortcode `[revision_library]` and a `the_content` filter for the box. It reads `library.json` from raw.githubusercontent.com (cached 1 hour in the transient `rl_library_items`).
- The box shows only when the guide's category has at least one PDF in `library.json`, so there are no dead links while the library is empty.
- WordPress page "Revision Library", id 50437, slug `revision-library`, status PUBLISHED (28 Nov 2026, on his OK), contains the shortcode. Empty state reads "Quick revision PDFs are on the way". Preview: https://ravikirans.com/?page_id=50437&preview=true (signed in).
- `library.json` created empty: `{"items": []}`. `tools/NIGHTLY.md` step 6 now appends an item for each `post` and `category` PDF.
- Tested with stubs (grouping, sort, box); not yet tested live with a real entry.
- 27 Nov: per-PDF page and email gate written as `website/snippets/revision-library-gate.php` (second snippet, needs snippet 11 for `rl_items`; snippet 11 now lets admins bypass the 1-hour cache). Helper tests pass with stubs. Install state: see "To do".
- How the gate works:
  - `/revision-library/<slug>/` is served through the Revision Library page itself (rewrite to `pagename=revision-library`), so it is public only while that page is published; admins see it in draft. No separate switch.
  - Form (email) -> signed link emailed (7 days, one PDF, nothing created yet) -> click creates or confirms the FluentCRM contact (list Newsletter, tag "Downloaded: <category>"; new contacts also tag "Website signup" and the signup field gets the category), sets cookie `rl_ok` (1 year), then the PDF is streamed through the site (the GitHub address is never shown). An unsubscribed contact is not resubscribed (FluentCRM guard) but still gets the PDF.
  - Raw GitHub PDF links stay public (accepted: the same PDFs are free on LinkedIn).
  - Old subscribers pass through personal links: the snippet registers the FluentCRM smartcode `{{rl.key}}`; newsletter links use `https://ravikirans.com/revision-library/?rlp={{rl.key}}` (the email chat adds them). FluentCRM's own cookie `fc_hash_secure` also counts. Fallback: same form, same email.
  - Spam control: hidden honeypot, 3 emails per address and 10 per IP per hour.
- Tested live 27 Nov with a sample entry and a test contact (form, emailed link, confirm, subscribe, cookie, PDF streamed at original size, visitor 404 while the page is a draft, bad link, unknown slug). Test contact, its tag and the sample entry were removed afterwards.

`library.json` item format: `{"slug":"<date>-<slug>","title","category","category_slug","date","cover","pdf","summary","guide_url"}` (cover and pdf are raw GitHub URLs; category_slug comes from `categories.csv` and must equal the WordPress category slug).

To do, in order:
1. Done 27 Nov: gate installed (Code Snippets id 12 "Revision Library PDF pages + email gate", active; id 11 updated) and tested. Admin preview: add `?rl_visitor=1` to a PDF page to see the visitor view. Not tested end to end: the personal `?rlp=` link (key format only); verify it with the email chat's one-contact test.
2. DONE 28 Nov: Revision Library page published on his OK (public check: 200, empty message; unknown slug 404). The first real PDF fills the page and the boxes.
- 28 Nov: on his phone, Google ads (side "Discover more" box + video banner, from the sidebar AdSense widget) overlaid the library page. Fixed in the gate snippet: the sidebar ad widget is skipped on /revision-library/ and its PDF pages (filter widget_display_callback). Live in snippet 12 (commit 0bff2f8); checked at phone width: no ad code. The same ads still show on other pages; his call whether to change them.
3. Email chat: add the personal link above to newsletter emails (verify `{{rl.key}}` renders inside a link with a one-contact test).
4. "Signup source" (see below).
5. App plan: done, see `app-plan.md`.

## Signup source (undecided: now, or with the library)
Popup form is Fluent Forms form 3 with hidden field `signup_page` = `{embed_post.permalink}` mapped to the FluentCRM custom field "Signup page". He wants ONE column called "Signup source": the page URL for ordinary popup signups, and the vendor or certification category for signups triggered by a PDF download. My recommendation: first source only, plus a tag "Downloaded: <vendor>" per later download. He has not chosen the timing.

## Popup and confirmation email
- Popup = Code Snippets snippet id 10, form 3. On 26 Nov 2026 the FluentCRM feed on form 3 got double opt-in turned on, the form message was changed to "Almost done! Check your inbox...", and the double opt-in email and thank-you text were reworded (FluentCRM > Settings > Email Settings > Double Opt-in Settings). The math captcha plugin is no longer needed; he asked about removing it (add-on "Custom Captcha Field for Fluent Forms"); checked 27 Nov: the add-on is still active (not removed).
- Delivery of confirmation emails depends on SES, which is handled in the email chat.

## Site facts
- Plugins: FluentCRM 3.3.5 free, Fluent Forms 6.2.17 free, FluentSMTP 7.1.5, Code Snippets, Yoast SEO, Pretty Links, Wordfence, Akismet, Ninja Forms, Reading Progress Bar, WPFront Scroll Top, Advanced Editor Tools, Fixed Widget. WordPress and several plugins have updates pending.
- Study guides are normal posts (Gutenberg, `.entry-content`, category plus tags). Example guide id 50370. Structure: entry-meta-top, title, entry-meta-bottom, featured image, entry-content, tags, utils, related, comments.
- Logo change done on the site. The old logo in the browser tab is only browser cache: the site serves the new icon (`cropped-site-icon-512-*.png`); a hard refresh or another browser shows it.
- Pretty Links: /linkedIn points to an expired affiliate link (left alone). Facebook, Twitter and YouTube links not yet checked.
- Open, low priority: Gravatar and sidebar photo (he does them), AdSense propagation, LinkedIn groups reposting (skipped).

## Course app (not started)
He wants a simple Android and iOS app with three sections: video courses people can buy in the app, timed test questions so users can judge their knowledge, and a library of the category-by-category carousel PDFs, each linked to its course (course-specific PDFs later). He may connect a Teachable-style account to the website later. The Revision Library and `library.json` are meant to be reusable as the app's PDF source. One-page plan written 27 Nov 2026: `app-plan.md` (waiting for his answer on selling in-app now or later).

## Working notes
- Built-in browser (browser_repl) is signed in to WordPress admin. Set a wide viewport (`setViewportSize({width:1300,height:1000})`) or admin content hides. Reuse tab variables; declaring the same name twice fails.
- Create a page through the REST API from an admin page: `fetch('/wp-json/wp/v2/pages', {method:'POST', headers:{'X-WP-Nonce': wpApiSettings.nonce, ...}})`.
- Code Snippets editor: `document.querySelector('.CodeMirror').CodeMirror.setValue(code)`, then "Save and Activate".
- A PHP snippet is linted with `php -l` before pasting; Code Snippets has safe mode if one breaks the site.

- 28 Nov: logo sizes. Header logo was 70% wide on phones (old Additional CSS rule `#masthead a img`), footer 100% (phones) and 320px max (desktop). Footer rule in Appearance > Customize > Additional CSS is now `#footer .site-logo img{width:70%;max-width:400px}`: header and footer logos are equal at phone, tablet and desktop (210 / 400 / 400 px, measured). Old footer rule was `max-width:320px`.

- 28 Nov: three checks scheduled into this chat: Sun 29 Nov 20:30 IST (auto-resume ran; build task + Make 8659415 and 8753316 on; exactly two active), Mon 30 Nov 08:30 (morning build, today.json, library entry live), Mon 30 Nov 19:20 (Make 8659415 posted to LinkedIn with first comment, Page reshare, Facebook, X). Posting channels: LinkedIn personal profile (Zernio, PDF post + first comment), LinkedIn Page reshare, Facebook and X (Buffer). Data store 262859 holds `ytfeed` (latest uploads, for video days; no view counts) and `traffic` (Search Console clicks per blog post, for `post` days only).
- Open idea (his: "look later"): feed LinkedIn engagement back into content choice. Zernio exposes LinkedIn personal-profile analytics (impressions, reach, reactions, comments, saves) for posts published through Zernio, all of ours are; no new tool needed if his Zernio plan includes it (an Analytics add-on may be required; not checked). Alternative with no cost: read the LinkedIn analytics page in his signed-in browser weekly.
