# Social posting chain and analytics

Part of the website handoff (`README.md` in this folder is the hub). Last updated 28 Nov 2026.

## The daily chain (fully automatic, no manual step)
1. 07:37 IST, morning build (scheduled task, rules in `../tools/NIGHTLY.md`): picks today's item (posting days Mon, Tue, Thu, Sat), builds the 10-page carousel PDF and the cover PNG, saves both in `carousels/` in this public repo, writes `today.json` and `plan.csv`, and on `category` and `post` days appends a line to `library.json`.
2. The website reads `library.json` from GitHub (cached up to 1 hour for visitors) and draws the Revision Library page, the PDF page and the Quick Revision box by itself. No job uploads anything to WordPress; nothing is stored in the WordPress media library.
3. 19:00 IST, Make scenario 8659415 reads `today.json` and posts: LinkedIn personal profile via Zernio (the PDF as a document post, plus an automatic first comment), a reshare from the LinkedIn Page (RAVIKIRANS), Facebook and X via Buffer (cover image only, no album: his earlier choice). Make skips and emails him if `today.json` is not dated today.
- Links (changed 28 Nov, his OK): on category and post days the morning job builds `pdf_page` = `https://ravikirans.com/revision-library/<slug>/` and uses it as the link in the Facebook and X texts and as a second comment line "Free PDF: " on LinkedIn, so readers give their email on the website, never through GitHub. Course, book and video days keep the item's own link. No Make change was needed (Make posts the text fields as they are).
- Watch on the first run (Mon 30 Nov): X posts must stay under 220 characters with the longer URL.
- To verify on Wed 2 Dec: on non-posting days `today.json` stays old, so Make may send the "Daily LinkedIn post skipped" email; if it does, decide with him whether that is wanted.
- The Revision Library has PDFs only for category and post days (courses, books and videos have none).

## Data store 262859 (Make)
- Key `ytfeed`: the channel's latest uploads (RSS), used by the morning job to find videos to promote. No view counts, so videos are chosen by least recently posted, not most viewed (he once asked for most viewed; not built, would need a way to read views).
- Key `traffic`: Google Search Console clicks per blog post (from the Traffic tab of his sheet, written by his other autopilot). Used only on `post` days (when every category, course, book and video has been posted once): the highest-traffic post not yet posted.
- Make 8753316 refreshes both weekly, Mondays 06:30 IST.

## Analytics: what works, what we decided (28 Nov 2026)
- Wanted: feed LinkedIn engagement back into what the morning job chooses.
- Zernio: his plan is Free (2 accounts: LinkedIn profile + RAVIKIRANS Page, no card). Zernio bills per account (first 2 free) and includes analytics. For a personal profile it only has posts published through Zernio, and needs the `r_member_postAnalytics` scope. Make already holds a Zernio API key connection (13150349), so Make can pull `GET /v1/analytics`.
- LinkedIn's own Content analytics (personal profile, 28 days, read 28 Nov): 1,443 impressions, 636 members reached, 14 engagements (3 reactions, 5 comments, 5 reposts, 1 save), per-post impressions (top 383, 252, 208), 78% in-network, demographics, Export button. Zernio showed 6 profile posts, reach 711, 13 engagements, likes 2, comments 0 (some tiles stale until its hourly sync).
- His correction (accepted): Zernio was set up only 1 to 2 weeks ago and nothing was posted since 24 Nov, so the comparison was not like for like. No decision yet on which to use. LinkedIn's own analytics stays the reference for now (complete history).
- Fair test: after the first week of posting (30 Nov, 1, 3, 5 Dec), compare post by post. A check is scheduled into the website chat for Mon 7 Dec 10:00 IST. Note: his own automatic first comment may count in LinkedIn's comment total (not verified).
- If adopted later (not approved): weekly pull of per-post reach and reactions into `engagement.csv` or a data store key; after about 4 weeks add one short generic rule to `../tools/NIGHTLY.md` (favour post types and categories with higher reach, keep 1 in 4 posts as a fresh test). Facebook and X are not covered (Buffer only). LinkedIn's page needs his Mac on with the Claude desktop app open if read from the browser.
