# Cross-chat items

Shared by the website chat and the email chat (rules: root `README.md`). Each chat edits only its own rows. If an item needs the other chat, add a row owned by that chat and tell Ravikiran. Remove a row when done. Last updated 28 Nov 2026.

## Shared systems: who owns what
| System | Owner | Note for the other chat |
|---|---|---|
| FluentCRM contacts, lists, tags, custom fields, campaigns, junk cleanup | Email chat | Website chat's gate creates contacts with list "Newsletter" and tags "Website signup" and "Downloaded: <category>", and writes the custom field "Signup source" or "Signup page". Do not delete or rename those tags without a row here. |
| Amazon SES, FluentSMTP sending, domain and DNS mail records | Email chat | Gate emails (PDF links) also go through it. |
| Pop-up: Code Snippets id 10 and Fluent Forms form 3, double opt-in text | Email chat | Website chat changes it only for library pages, after a row here and Ravikiran's OK. |
| Newsletter content and the personal-link smartcode use in emails | Email chat | The smartcode `{{rl.key}}` itself is provided by the website chat (snippet 12). |
| Revision Library page, PDF pages, email gate (Code Snippets ids 11 and 12), `library.json` | Website chat | Email chat links to PDF pages `https://ravikirans.com/revision-library/<slug>/`, never to GitHub. |
| Morning carousel build, Make posting jobs, LinkedIn/Facebook/X, Zernio, Buffer | Website chat | |
| WordPress theme, menus, Additional CSS, ads | Website chat | |

## Open items needing both chats
| Item | Owner | Status |
|---|---|---|
| Add `https://ravikirans.com/revision-library/?rlp={{rl.key}}` to newsletter emails and verify the key renders inside a link with a one-contact test | Email chat | Open. Smartcode is live (website chat). Not tested end to end. |
| Rename the FluentCRM custom field "Signup page" to "Signup source" (page URL for pop-up signups, category for PDF downloads) | Email chat (FluentCRM data) | Waiting for Ravikiran's choice: now or with the library. The gate already writes to either name. |
| Remove the unneeded add-on "Custom Captcha Field for Fluent Forms" (patched in `Bootstrap.php`) | Email chat | Waiting for Ravikiran's OK. Still active on 27 Nov. |
| Library pop-up: after a visitor verifies by pop-up they must not be asked for email again on Download (one verified state `rl_ok`; see `website/revision-library.md`) | Website chat | Waiting for Ravikiran's OK. Will touch snippet 10 for library pages only. |
| Carousel PDFs in the newsletter: PDF button should point to the PDF page on the website | Email chat | Not built. First real PDF Mon 30 Nov. |
| Gate email sender name "Ravikiran Srinivasulu" and delivery through SES | Website chat | Re-test on a real run (Mon 30 Nov onward). |
| Commit author: email chat commits show author "Claude"; must be Ravikiran with no co-author (rule 5) | Email chat | Fix in its next commit. |
