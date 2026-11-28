# Site facts and working notes

Part of the website handoff (`README.md` in this folder is the hub). Last updated 28 Nov 2026.

## Site
- WordPress on Hostinger, Olsen Light theme. Plugins: FluentCRM 3.3.5 free, Fluent Forms 6.2.17 free, FluentSMTP 7.1.5, Code Snippets, Yoast SEO, Pretty Links, Wordfence, Akismet, Ninja Forms, Reading Progress Bar, WPFront Scroll Top, Advanced Editor Tools, Fixed Widget. WordPress and several plugins have updates pending.
- Study guides are normal posts (Gutenberg, `.entry-content`, category plus tags). Example guide id 50370. Structure: entry-meta-top, title, entry-meta-bottom, featured image, entry-content, tags, utils, related, comments.
- Menus (Appearance > Menus): Main Menu (also the mobile menu): My Udemy Courses, Contact, Resources, Revision Library, Study Guides (submenu of categories). Footer Menu: Resources, Contact, Privacy Policy, Icons, Images by Freepik, Revision Library.
- Logos: header and footer logos are equal at every width (210 px phone, 400 px tablet and desktop). Rules live in Appearance > Customize > Additional CSS: `#masthead a img` (70% wide) and `#footer .site-logo img{width:70%;max-width:400px}` (the old footer rule was `max-width:320px`). The old logo in the browser tab is only browser cache.
- Google ads (AdSense widget in the sidebar, plus auto ads) show on other pages and can overlay the page on phones; they are switched off only on the Revision Library pages.
- Pretty Links: /linkedIn points to an expired affiliate link (left alone). Facebook, Twitter and YouTube links not yet checked.
- Open, low priority: Gravatar and sidebar photo (he does them), AdSense propagation, LinkedIn groups reposting (skipped).

## Course app (not started)
He wants a simple Android and iOS app: video courses people buy in the app, timed test questions, and a library of the category-by-category carousel PDFs linked to their courses. The Revision Library and `library.json` are meant to be reusable as the app's PDF source. One-page plan: `app-plan.md` (waiting for his answer on selling in-app now or later).

## Working notes
- The built-in browser (browser_repl) is signed in to WordPress admin, Zernio and LinkedIn. Set a wide viewport (`setViewportSize({width:1300,height:1000})`) or admin content hides; reset it with `setViewportSize(null)` after phone-size tests. Reuse tab variables; declaring the same name twice fails.
- REST calls from an admin page need `wpApiSettings.nonce` (not present on every admin page, for example the Menus page): `fetch('/wp-json/wp/v2/pages/<id>', {method:'POST', headers:{'X-WP-Nonce': wpApiSettings.nonce, 'Content-Type':'application/json'}, body: ...})`.
- Update a snippet from this repo: in the Code Snippets editor fetch the raw file at a commit SHA, `CodeMirror.setValue(text)`, click "Save Snippet".
- Menus: in the Menus screen use the item's "Menu Order" dropdown to place an item; "Up one" can drop it into a submenu.
- Test as a logged-out visitor with `fetch(url, {credentials:'omit'})` from the browser (the sandbox shell cannot reach ravikirans.com).
