# Revision Library: design, build state, how it works

Part of the website handoff (`README.md` in this folder is the hub). Last updated 28 Nov 2026.
No passwords, keys or tokens in this repo (it is public).

## What it is (design he approved, 26 to 27 Nov 2026)
1. A page `/revision-library/` grouped by category, newest first; each entry has cover, title, date, a link to its PDF page. Filled automatically from `library.json` (he edits nothing).
2. A "Quick Revision" box at the end of every study guide linking to that category on the library page. Shows only when the guide's category has at least one PDF in `library.json`. Applies to existing and new guides by itself.
3. A small public page per PDF `/revision-library/<slug>/` (summary, cover, download button, related guide, his Udemy courses).
4. Email gate: the download link is EMAILED (no attachment, no instant download). Clicking it confirms and subscribes the person and sets a cookie so the gate is skipped afterwards. Existing subscribers pass through personal links in newsletter emails. A rare "Already subscribed? Enter your email" fallback is fine. No gate beyond email. The PDFs are free on LinkedIn too; that is fine.
5. Reusable later as the PDF source of the course app (`app-plan.md`).

## Live state
- Page "Revision Library": WordPress page id 50437, slug `revision-library`, PUBLISHED 28 Nov 2026 on his OK, contains the shortcode `[revision_library]`. Empty state: "Quick revision PDFs are on the way". Linked from the Main Menu (after Resources, also mobile menu) and the Footer Menu since 28 Nov.
- Snippet id 11 "Revision Library (page + Quick Revision box)", active, everywhere. Source: `snippets/revision-library.php`. Provides `rl_items()` (reads `library.json` from raw.githubusercontent.com, cached 1 hour in transient `rl_library_items`; admins bypass the cache), the shortcode and the box.
- Snippet id 12 "Revision Library PDF pages + email gate", active, everywhere. Source: `snippets/revision-library-gate.php`. Needs snippet 11. If a snippet is ever lost, paste the source file back (Code Snippets editor: `document.querySelector('.CodeMirror').CodeMirror.setValue(code)`, then Save; lint first with `php -l`; Code Snippets has safe mode).
- `library.json` is `{"items": []}` until the first real PDF. Item format: `{"slug":"<date>-<slug>","title","category","category_slug","date","cover","pdf","summary","guide_url"}` (cover and pdf are raw GitHub URLs; `category_slug` must equal the WordPress category slug from `categories.csv`). The morning job appends one item per `category` and `post` day (`tools/NIGHTLY.md` step 6).
- Admin preview of the visitor view: add `?rl_visitor=1` to a PDF page. A PDF page is public only while the library page is published (drafts show only to admins).

## How the gate works
- `/revision-library/<slug>/` is served through the Revision Library page itself (rewrite to `pagename=revision-library`). The page is drawn live on each visit; nothing is stored per PDF in WordPress.
- Form (email) -> signed link emailed (valid 7 days, one PDF, nothing created yet) -> click creates or confirms the FluentCRM contact (status subscribed, list Newsletter, tag "Downloaded: <category>"; new contacts also tag "Website signup" and the signup field gets the category), sets cookie `rl_ok` (1 year), then the PDF is streamed through the site (the GitHub address is never shown). An unsubscribed contact is not resubscribed (FluentCRM guard) but still gets the PDF. Email sender name "Ravikiran Srinivasulu" (not re-tested live).
- Old subscribers: the snippet registers the FluentCRM smartcode `{{rl.key}}`; newsletter links should be `https://ravikirans.com/revision-library/?rlp={{rl.key}}` (the email chat adds them). FluentCRM's own cookie `fc_hash_secure` also counts. Fallback: same form, same email.
- Spam control: hidden honeypot, 3 emails per address and 10 per IP per hour.
- No Google ads on the library pages (sidebar AdSense widget skipped by the snippet; ads overlaid the page on his phone). The same ads still show on other pages; his call whether to change them.
- Raw GitHub PDF links stay public: accepted by him on 28 Nov (PDFs stay in the public repo, folder `carousels/`; someone who finds the GitHub link can skip the email step; the PDF is also public on LinkedIn).

## Tests so far
- 27 Nov, live, with a sample entry and a test contact: form, emailed link, confirm, subscribe, cookie, PDF streamed at original size, visitor 404 while the page was a draft, bad link, unknown slug. Test data removed afterwards.
- 28 Nov: published page shows the empty message to a visitor; unknown PDF address returns 404; phone view clean (no ads).
- NOT yet tested: the personal `?rlp=` link end to end (key format only; needs the email chat's one-contact test of `{{rl.key}}` inside a link); the sender name; a visitor with no cookie opening the PDF link or download link directly (to do with Monday's first real PDF: it must not return the PDF).

## Open: one verified identity (his rule 28 Nov 2026, NOT built)
- Rule: on the library page and its PDF pages, the pop-up collects the email. After that, Download PDF never asks for an email again, and a download works ONLY after the email is verified. Library only.
- Gap found in the code: the gate trusts only cookie `rl_ok` (set by its own emailed link or a `?rlp=` link) and `fc_hash_secure` (set when a subscriber opens a newsletter link). A pop-up signup is confirmed by FluentCRM's double opt-in link, which sets neither cookie, so such a person would be asked for their email again on Download. Not checked live: whether the pop-up (snippet 10) already skips library pages or verified visitors.
- Proposed fix (waiting for his OK): one verified state = `rl_ok`. On library pages the pop-up uses the gate's flow (signed link subscribes the contact and sets `rl_ok`); a verified visitor gets the PDF directly; an unverified visitor who clicks Download sees the same pop-up, or "Check your inbox" if the link was already sent. Other pages keep the current pop-up.

## Signup source (undecided: now, or with the library)
Pop-up form is Fluent Forms form 3 with hidden field `signup_page` = `{embed_post.permalink}` mapped to the FluentCRM custom field "Signup page". He wants ONE column called "Signup source": the page URL for ordinary pop-up signups, and the vendor or certification category for signups triggered by a PDF download. My recommendation: first source only, plus a tag "Downloaded: <vendor>" per later download. The gate already writes the category into whichever field is named "Signup source" (preferred) or "Signup page". Asked him 28 Nov; no answer yet.

## Pop-up and confirmation email
- Pop-up = Code Snippets snippet id 10, form 3. On 26 Nov 2026 the FluentCRM feed on form 3 got double opt-in turned on, the form message became "Almost done! Check your inbox...", and the double opt-in email and thank-you text were reworded (FluentCRM > Settings > Email Settings > Double Opt-in Settings).
- The math captcha add-on "Custom Captcha Field for Fluent Forms" is no longer needed; he asked about removing it; checked 27 Nov: still active.
- Delivery of confirmation emails depends on SES, handled in the email chat (`../email-automation/README.md`).
