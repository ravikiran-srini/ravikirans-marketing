# Email automation: handoff

Newsletter system for ravikirans.com. Self-hosted, near free. Last updated 28 Nov 2026.
No passwords, API keys or access keys are stored here (this repo is public). Credentials live in the plugins and consoles.

## Read first: keep the repo current, warn about compaction
- After EVERY message turn, write everything from that turn into this file before replying: what was done, decisions, what Ravikiran agreed or disagreed with, rules, open questions, next steps. Commit and push each time. Nothing may live only in the chat; the chat can be compacted at any moment and the repo is the only memory.
- Compaction: the moment you notice the chat was compacted (a summary replaces earlier messages), say so in one line in your next reply, say everything is saved in GitHub, re-read the files below, and give him the new-chat prompt below to paste, always inside a fenced code block so he can copy it with one click. Also tell him when the chat gets very long.
- New chat, read in order: `README.md` (repo index, in repo ravikiran-srini/ravikirans-marketing), then this file, top to bottom. Do not ask him for context written here.
- New-chat prompt for Ravikiran to paste: "Continue the ravikirans.com email automation. Repo ravikiran-srini/ravikirans-marketing (clone with add_repo if WebFetch is rate-limited). Read README.md at the root, then email-automation/README.md, starting with 'Current status' and 'Pending'. Follow the rules at the top of that file: save everything to GitHub after every message turn and warn me after any compaction. Do not send any campaign until I say go. My steps only: AWS root MFA, passwords, codes, access keys; everything else you do in the built-in browser (WordPress admin and the AWS console are signed in there)."
- Junk-address scan: done and the 11 junk contacts deleted on 28 Nov 2026 (see Data in FluentCRM). Further deletions still need his approval first.

## Current status (read first)
- Sending is on Amazon SES (Sydney) via FluentSMTP. Brevo is gone: connection, DNS records and account all removed.
- SES production access is GRANTED (seen 27 Nov 2026): 50,000 emails per 24 hours, 14 a second, account Healthy, plan Essentials. The free plan did not block it. Requested 26 Nov 2026.
- Do NOT send any campaign until Ravikiran says go. He said he will not send for at least 2 weeks (from 27 Nov 2026).
- Welcome-back campaigns are drafts only: id 1 (Batch 01) plus ids 2 to 15 (Batch 02 to 15), all unsent. Footer details are now filled in.

## Goal
- Collect subscribers on ravikirans.com, store them on Hostinger, record the signup page.
- Send a newsletter to all of them. Later a weekly newsletter built and scheduled by a Claude scheduled task (not built yet).
- Carousel PDFs go in the newsletter: slide images in the email, PDF download button.

## Stack
- WordPress on Hostinger. FluentCRM 3.3.5 (free) holds contacts. Fluent Forms form id 3 is the signup form.
- Math captcha add-on "Custom Captcha Field for Fluent Forms": single digit, answer at most 9. Patched in `Bootstrap.php`; a plugin update undoes the patch.
- Pop-up: Code Snippets plugin, snippet id 10. Shows after 3 to 5 s. Since 26 Nov 2026 it sends a confirmation (double opt-in) email; details in `../website/README.md`.
- Sending: FluentSMTP, one connection: Amazon SES, region ap-southeast-2, From contact@ravikirans.com, Default. Logging on, kept 14 days. Test emails delivered in about 1.2 s (to contact@ravikirans.com, which forwards to his Gmail).
- "Server-side cron" is off, so FluentCRM runs on WP cron (page visits). A real Hostinger cron is a later option.
- Footer (FluentCRM > Settings > Business Setup): business name `ravikirans.com`, address 1/79 Raju Nagar, First Main Road, Chennai 600077, Tamil Nadu, India. The business is not a registered entity; the name is the domain.

## Data in FluentCRM
- List "Newsletter" (id 1). Tags: "Website signup" (1), "Old list" (2), "Batch 01" to "Batch 15" (ids 3 to 17). Custom field "Signup page".
- 3,979 contacts imported from the old Brevo list (all subscribed, all on Newsletter, tagged "Old list", each in exactly one Batch tag of 265 or 266). The dashboard showed 3,981 active contacts on 26 Nov (new signups since).
- Junk scan run 28 Nov 2026 (read only, all 3,984 contacts: 3,981 subscribed, 3 pending = new signups, 0 bounced, 0 unsubscribed, 0 emails sent). Checks: syntax, fake or 1-2 letter domains, obscene or test words, disposable domains, typo domains (gmial.com etc.), very short or digit-only names, role addresses, duplicates incl. Gmail dot variants, own domain. Result: no syntax errors, disposable, typo or duplicate addresses. 11 junk addresses found. Ravikiran chose permanent deletion ("delete everything that looks suspicious"), so on 28 Nov 2026 all 11 were DELETED from FluentCRM (ids 3860, 3961, 3959, 3903, 3957, 3774, 3327, 3953, 3644, 2899, 1199; each id checked against its address first). He then also had the 2 "testing" job-title addresses deleted (ids 1186, 1930). The list is now 3,971 contacts (3,968 subscribed, 3 pending); some Batch tags are one smaller than before, so the Batch 01 count may no longer be 266. Other real-name lookalikes (Fakeerappa, Pornthep) were kept. 4 info@ role addresses of real companies (ids 626, 2007, 2162, 2991) left in. Not checkable here: whether a real-looking mailbox still exists (the Batch 01 send will show that). Do not write contact emails into this public repo; use ids.
- The Brevo account is closed, so the old list now exists only in FluentCRM (and a CSV export from 24 Nov that he may have on his computer).

## AWS and SES (set up 26 Nov 2026)
- Account: new AWS Builder ID plus project for contact@ravikirans.com (activated 26 Nov 2026). Free plan: only region ap-southeast-2 (Sydney) is allowed; other regions (e.g. Mumbai) need a paid plan. Account id 208978380237.
- Console sign-in: the classic root sign-in rejects the email. Use "Change to new experience", then enter contact@ravikirans.com; a code goes to his Gmail via the Hostinger forwarder.
- SES identity: ravikirans.com in Sydney, Easy DKIM (3 CNAMEs in Hostinger DNS), status Verified.
- IAM user `fluentsmtp-ses` with customer-managed policy `FluentSMTP-SES-SendOnly` (ses:SendEmail, SendRawEmail, GetSendQuota). Ravikiran created the access key himself and pasted it into FluentSMTP. Inline policies and attach-by-URL are blocked on the free plan; create a managed policy, then attach through the user's Permissions tab.
- Production access request: Marketing, https://ravikirans.com. Status "Production access granted" (SES Get set up page, Sydney, checked 27 Nov 2026). Quota 50,000 a day, 14 a second; 3 test emails sent so far.
- Bounce handling: the SES account-level suppression list is on by default. Optional extra: SNS topic plus FluentCRM bounce webhook (not set up).
- Costs: Essentials plan about $0.16 per 1,000 emails; 8,000 subscribers x 4 sends a month is about $5 a month. Attachments cost $0.12 per GB per copy, so never attach. Free-plan credit: $100 (Billing > Credits, $0 used on 28 Nov 2026). Two dates: the FREE PLAN period ends 26 May 2027 (Console Home shows 181 days left; access ends then or when credits run out), and the credit itself is listed to expire 26 Nov 2027, usable after upgrading to the Paid plan (card needed). Upgrade before 26 May 2027 to keep sending. Up to $100 more can be earned: Console Home > Explore AWS > filter 'Earn AWS credits', 6 activities at $20 each (cap $100, 0 of 6 done): Lambda web app, Bedrock playground, Aurora/RDS database, DynamoDB table, AWS Budgets cost budget, EC2 instance; complete within 6 months of account creation. Status 28 Nov 2026 (done at his request in the built-in browser): credits remaining $160, $60 earned. Completed: AWS Budgets (created a $10 monthly cost budget, alerts to contact@ravikirans.com at 85% and 100%; keep it), DynamoDB table (test table deleted), Bedrock playground (registered even though the Nova Micro call was throttled by a new-account token quota). EC2: 'In progress'; a t3.micro test instance named credit-activity-test (Sydney, no SSH, no key pair) is still RUNNING: terminate it once the widget shows Completed (about $0.32 a day). Lambda web function: console create hangs, nothing created. Aurora/RDS: he approved 28 Nov 2026 evening; created free-tier MySQL db.t4g.micro 'credit-activity-db' (Sydney, password managed in Secrets Manager, none typed), still RUNNING: delete it (skip final snapshot, delete the managed secret too) once the widget shows Completed; widget still showed 'Not started' right after creation, status lags. $100 cap needs 5 of 6, so 2 more are needed (EC2 + RDS).
- Resolved 27 Nov 2026: the free plan did not block production access; no paid plan was needed. Tell Ravikiran before anything is charged (the $100 credit covers SES; free plan ends 26 May 2027).
- Still his steps: enable MFA on the AWS root user. His Hostinger email plan expires 2029-07-29 with auto-renew off: renew it or add a second recovery email on AWS.

## DNS (Hostinger hPanel, ravikirans.com)
- Kept: Hostinger mail DKIM CNAMEs (hostingermail-a/b/c), MX, autodiscover/autoconfig, www, google-site-verification TXT.
- SPF (TXT @): `v=spf1 include:_spf.mail.hostinger.com include:amazonses.com ~all`.
- DMARC (TXT _dmarc): `v=DMARC1; p=none;` (Brevo reporting address removed).
- Added: 3 SES DKIM CNAMEs (`*._domainkey` to `*.dkim.amazonses.com`).
- Removed 27 Nov 2026: brevo-code TXT, brevo1/brevo2 DKIM CNAMEs.
- Add records through the DNS page form (Type CNAME, Name, Target, Add Record). hPanel signs out now and then; Ravikiran signs in (2FA is on, passwords are his to type).

## Brevo (closed)
- 26 Nov: FluentSMTP connection removed (Ravikiran replaced it with SES). 27 Nov: DNS records removed; account closed via Settings > General > "Definitively close your account", with a code emailed to his Gmail that he entered himself. Nothing left to do.

## Pending
- FIRST, AWS credit cleanup (he approved RDS and the extra $40 on 28 Nov 2026): in the built-in browser, Console Home > Explore AWS (scroll to it with scrollIntoViewIfNeeded; the pane is hidden so pages load slowly) and check whether EC2 and 'Create an Aurora or RDS database' show Completed. When RDS is Completed, delete RDS 'credit-activity-db' (Sydney, no final snapshot) and its managed Secrets Manager secret; when EC2 is Completed, terminate instance credit-activity-test (i-0b02a28422eb81ddf). Both cost credits while running. If still not Completed, recheck later. Cap needs 5 of 6; Lambda web function create hangs, so EC2 + RDS are the plan. A scheduled recheck fired in the old chat only, so do this in the new one.
- When Ravikiran says go: send a one-contact test through FluentCRM (use a test tag), then Batch 01 (266), check bounces and complaints in SES, then the rest.
- Welcome-back DRAFT: campaign id 1 "Welcome back - Batch 01", status draft, subject "Still want exam-prep tips?", preheader "A short weekly email on cloud and data certification prep", from Ravikiran Srinivasulu <contact@ravikirans.com>, list Newsletter + tag Batch 01 (266 confirmed in the UI). Short body with a button to ravikirans.com. Ravikiran has not reviewed it yet: show it. Do NOT schedule or send without his go-ahead.
- Back up the list first: export all FluentCRM contacts to CSV (FluentCRM is the only copy now that Brevo is closed). Ask before saving the file anywhere.
- Open question (28 Nov 2026): stricter list filtering. No tool shows whether an address was used recently; verifiers only check the mailbox exists plus disposable or risky flags. Two SES options, neither enabled or charged yet (needs his yes first). (a) Email Validation API/console: $0.01 per address, about $40 for 3,971; one-off, he decides which addresses and gets a verdict per address to clean the list himself; the console takes one address at a time, so a bulk run needs the API (not verified how). (b) Auto Validation: SES console (Sydney) > Email validation > Auto Validation, account-level, enable + threshold (managed recommended); checks every recipient at every send and suppresses low-confidence ones; $0.01 per 1,000 validations (about $0.04 for the list per send); suppressed sends still bill the normal send charge; list in FluentCRM stays unchanged. Credits: his AWS Free Tier credit's applicable-services list in Billing includes Amazon Simple Email Service, but AWS does not say whether validation is covered; confirm by Billing credits 'amount used' about 24 h after the first charge. Unconfirmed: whether suppressed sends count toward the SES bounce rate. Free fallback: send Batch 01, read bounces, after 2 or 3 newsletters drop contacts with no clicks.
- Junk addresses cleaned 28 Nov 2026 (done). SES reviews at about 5% hard bounces, may pause at 10%: read the Batch 01 bounce rate before sending the rest.
- Campaigns for Batch 02 to 15 are built as unsent DRAFTS (27 Nov 2026): ids 2 to 15, titled "Welcome back - Batch NN", exact copies of campaign 1 (same subject, preheader, body, sender) with list Newsletter and tag Batch NN (campaign id = batch number; tag id = batch number + 2). Verified through the API; recipient counts not checked in the UI. If campaign 1's text changes, re-copy it to ids 2 to 15. All at once is possible; daily batches are no longer needed.
- After each send: read bounces and complaints, mark hard bounces "Bounced", remove unsubscribes. Later a scheduled task.
- Weekly Claude scheduled task: not built. Optional: real server cron; WordPress updates (8 pending, a PHP update is also recommended).
- Schedule endpoint (POST /campaigns/{id}/schedule) is untested on purpose: it would send. Test only with a one-contact test tag.

## Carousel PDFs in emails
- Do not attach PDFs. Host the PDF on ravikirans.com and link it with a "Download the full PDF" button.
- Slide images: host on GitHub Pages (public repo, each image under 100 KB), not raw.githubusercontent.com. About 32 GB a month at 8,000 subscribers (Pages soft limit 100 GB).
- Not set up yet. Idea: the morning carousel job also saves slide images to the Pages repo.

## Working notes
- FluentCRM REST base: https://ravikirans.com/wp-json/fluent-crm/v2 (nonce `window.fcAdmin.rest.nonce` from a FluentCRM admin page, called with page.evaluate).
  - Bulk tag: POST /subscribers/do-bulk-action with `{action_name:"add_to_tags", subscriber_ids:[...], action_options:[tagId]}`.
  - Create contact: POST /subscribers with `{email, status:"subscribed", lists:[1], tags:[2]}`.
  - Business settings: GET `/setting?settings_keys[]=business_settings`; the UI page is `#/settings` (Business Setup).
  - Campaigns: `POST /campaigns {title}` creates a draft; `PUT /campaigns/{id}` with `{title, email_subject, email_pre_header, email_body, design_template:"simple", settings}` fills it. Recipients: `settings.subscribers:[{list:"1",tag:"3"}]`, `sending_filter:"list_tag"`; custom sender in `settings.mailer_settings` with `is_custom:"yes"`. Merge into the settings the POST returns. The POST response is not wrapped, so list `/campaigns` to get the id.
  - `POST /campaigns/estimated-contacts` returns 0; not trustworthy. The campaign Recipients step (`#/email/campaigns/{id}?step=2`) shows the real count.
  - Setting `location.hash` to an unknown FluentCRM route bounces to the dashboard; `#/settings` and `#/email/campaigns` work, others via the sidebar links.
- Built-in browser (browser_repl): WordPress admin is signed in there. Pages bring big accessibility dumps; keep scripts quiet. It cannot type passwords or enter secrets. `tabs.create(url)` takes a string.
- Ravikiran prefers short answers in simple language and automation over manual steps. His first name is one word: "Ravikiran".
- Never send to the list without his approval. Confirm before any permanent deletion.
