# Developer Support Guru collection: accuracy review

**Reviewed:** October 21, 2026 · **Scope:** 51 cards from the Developer Support collection · **Checked against:** dev.clever.com (current docs, v3.1.2), plus support.clever.com where its pages would load.
**Method:** I checked each card's factual claims (product names, API behavior, URLs, UI paths, requirements) against the public docs. Internal-only processes (Hall Monitor, Jira, LaunchDarkly, Slack routing) can't be verified publicly, so I only flagged them where they look stale on their face.

---

## 1. Summary

| Overall rating | Cards |
|---|---|
| 🔴 Retire / archive | 8 |
| 🟠 Major updates | 13 |
| 🟡 Minor updates | 17 |
| 🟢 Current | 13 |

Most of the outdated content comes from **four product changes the cards haven't caught up with**:

1. **Clever Library has been discontinued.** dev.clever.com: *"Clever no longer offers Clever Library integrations."* Library content shows up in **17 cards**: the 6 Library-section cards, Hall Monitor – Library, Library – District SSO Error, Certification Questions, Getting Started, Product Offerings, Special Districts, Scopes, App Icon, API v3 Migration, Clever error and API tokens.
2. **Standalone SSO-only is no longer offered.** *"Clever no longer supports new standalone SSO-only integrations… should use SSO as part of an SSO + Rostering integration."* Several cards still treat "District SSO" as its own tier, and one calls it "Free".
3. **Everything now sits under Clever Complete.** Secure Sync, SSO + Rostering, LMS Connect, AnySchool and SAML all carry *"Clever Complete Agreement Required"*. That includes custom sandbox districts. The cards still describe the older tier structure (Secure Sync / Secure Sync Lite / District SSO / Library).
4. **v3.1 is the current API version.** The API Versions, v3 Migration and Data Browser cards are written around v3.0. The docs also say there's *"not yet… a formal API sunsetting policy"*.

---

## 2. Fix first: High-severity items (CS would give partners wrong information)

| # | Card | Problem | Fix |
|---|---|---|---|
| 1 | [Product – Product Offerings (App Facing)](https://app.getguru.com/card/TXza5A8c/Product-Product-Offerings-App-Facing) | Lists Library (free) and District SSO (free) as current offerings. Leaves out LMS Connect, AnySchool and SAML. | Rewrite around Clever Complete: Secure Sync, SSO + Rostering, LMS Connect, AnySchool, SAML. Remove Library and the "Free" SSO tier. |
| 2 | [Escalations – Certification Questions](https://app.getguru.com/card/iz45r7eT/Escalations-Certification-Questions) | Walks CS through Library certification. Assumes accounts without Secure Sync will certify for standalone SSO. Survey links (clever.com/certify, Airtable) may be stale. | Remove the Library content. Add LMS Connect certification. The docs say to get the certification form from the Application Success Manager. Add the "1–2 weeks after submission" turnaround. |
| 3 | [Escalations – Getting Started](https://app.getguru.com/card/TBxAerAc/Escalations-Getting-Started) | Sends partners to `/docs/classroom-vs-district` (not in the docs index) and `/docs/certification-types` (legacy page that lists Library). | Replace both with `/docs/integration-types` and `/docs/certification-overview`. CC application-interest@ on every new-integration inquiry, not just rostering ones. |
| 4 | [Scopes & Tags – API Versions](https://app.getguru.com/card/Tg8jnnBc/Scopes-Tags-API-Versions) | Written entirely around the v3.0 rollout. Doesn't say how v3.1 access is granted. | Reframe with v3.1 as current. The docs say v3.1 access goes through the ASM or integrations@. v2.x/v3.0 are deprecated but still supported for existing apps, with no published sunset date, so CS shouldn't quote dates. |
| 5 | [Scopes & Tags – Scopes](https://app.getguru.com/card/cg8jnnRi/Scopes-Tags-Scopes) | Presents InstantLogin (SSO-only) as a standalone tier "for District SSO and Library". Mentions SecureSyncLite, which the public docs don't list. | Mark standalone SSO as legacy. Remove Library. Confirm whether SecureSyncLite is still sold. Add the v3.1 demographic fields. |
| 6 | [Sandbox Districts – Create New Sandbox](https://app.getguru.com/card/iE5MBGGT/Sandbox-Districts-Create-New-Sandbox) | No eligibility check and no limit. | Add: custom sandboxes need **Clever Complete / Secure Sync**, and the limit is **5 sandboxes per app**. Update the UI steps: "Clever Passwords" is now **Clever Authentication**, sync type "Other" is now **SFTP**, and add the explicit **Launch Now** step. |
| 7 | [Sandbox Districts – Data Sets](https://app.getguru.com/card/cE5MBGei/Sandbox-Districts-Data-Sets) | The "249 School" set points to `cleverusd.zip`. | The docs' large dataset is now **Clear Water ISD** (250 schools, about 286K users). Add the official `default_sandbox_data.zip` and `CertificationISD.zip` links. |
| 8 | [Sandbox Districts – Special Districts](https://app.getguru.com/card/TR86q4ac/Sandbox-Districts-Special-Districts) | "All Library integrations must use #DEMO Library Test." | Remove the Library section. Check whether Gotham City is still maintained, since the public docs point to Clear Water ISD for large-district testing. |
| 9 | [Troubleshooting – API tokens](https://app.getguru.com/card/c48jaaxi/Troubleshooting-API-tokens) | Treats the OIDC identity token as the "SSO bearer token". Says auth codes last "a few minutes". Says non-Secure Sync district tokens always error. | Split access token and ID token: the access token is the bearer, the ID token is a JWT to decode. Auth codes are **valid for one minute, single use**. Non-Secure Sync district-app tokens can still reach `/districts`. Mark SSO-only as legacy. |
| 10 | [Troubleshooting – Clever error](https://app.getguru.com/card/iX8zMKoT/Troubleshooting-Clever-error) | Suggests the app "can launch a Library integration". Uses the path **Settings > Instant Login**. | Remove the Library suggestion. The district must connect the app and share the user. Partners can add `return_unshared=true` to get `error=unauthorized-user`. Update the settings path to the current tab. |
| 11 | [App Dash – 2FA](https://app.getguru.com/card/i54kajET/App-Dash-2FA) | Presents 2FA as optional. | The docs **require** SSO or 2FA for all app account users (since June 30, 2023). Name all three login options: App Dashboard SSO, Sign in with Google, 2FA. |
| 12 | [App Dash – New Account](https://app.getguru.com/card/ioRGRqnT/App-Dash-New-Account) | Sends partners to `apps.clever.com/signup`. | Getting Started now points to `clever.com/developer-signup`, which is a sales contact form. ⚠️ The SSO Quickstart still says apps.clever.com/signup, so the docs contradict each other. **Have PE confirm the right signup path.** Partners who need more than one app go to integrations@. |
| 13 | [Hall Monitor – Library](https://app.getguru.com/card/c68j4bbi/Hall-Monitor-Library) | A "one-stop shop for launching Library applications". Links the library-certification-guide. | Retire it. Keep only the "unlaunch" step if PE still needs it, folded into the legacy-Library card (see section 3). |

---

## 3. Library cards: kept and updated (decided Oct 21)
> **Decision:** Library is still a product. Clever is only not certifying *new* Library integrations, so all 8 cards are kept. Each one now has a banner stating this and telling CS to ask in #partner-engineering about any Library question. These edits are drafts waiting to be published. The original recommendation is kept below for reference.

### Original recommendation (superseded)

The public docs say only that Library is "no longer offered". They don't give a sunset date, a migration path, or say what happens to existing Library apps. **Recommendation:** archive these 8 cards and replace them with one internal card, **"Handling questions from legacy Library partners"**. It would cover:
- new interest: send to integration-types and application-interest@,
- existing-partner issues: routing for PE or Product to define,
- notes on the legacy `/docs/classroom-testing` page and the old Library dashboard pages.

| Card | Note |
|---|---|
| [Library – Overview](https://app.getguru.com/card/cR8z9d7i/Library-Overview) | The card already says the page "does nothing". |
| [Library – Marketing Collateral](https://app.getguru.com/card/c48jzgAi/Library-Marketing-Collateral) | Calls collateral "a requirement for Library certification". Move the icon guidance to the App Icon card. |
| [Library – Teacher Quickstart Guide](https://app.getguru.com/card/cKMxKaKi/Library-Teacher-Quickstart-Guide) | Check by hand that support article 360046090612 still exists. |
| [Library – Integration Guide](https://app.getguru.com/card/Tj8eRqrc/Library-Integration-Guide) | Links `/docs/classroom-testing`, which is orphaned and legacy. |
| [Library – Teacher Reviews](https://app.getguru.com/card/ia8jpn9T/Library-Teacher-Reviews) | Confirm the #oncall-district-dashboard channel still exists. |
| [Library – Analytics](https://app.getguru.com/card/cn8zjM4i/Library-Analytics) | Don't confuse with the main SSO Analytics page. |
| [Library – District SSO Error](https://app.getguru.com/card/i9ALE5eT/Library-District-SSO-Error) | Also refers to "apps that support District SSO AND Library". |
| [Hall Monitor – Library](https://app.getguru.com/card/c68j4bbi/Hall-Monitor-Library) | See item 13 above. |

---

## 4. Card-by-card results

### 🟠 Major updates (13)
Cards 1–12 in section 2 cover everything except **Advanced – App Icon**:

- [Advanced – App Icon](https://app.getguru.com/card/iR4LKygT/Advanced-App-Icon): remove the Library icon guidance (400×400, Marketing Collateral page). Keep the dashboard icon rule (200×200 PNG). Add that the docs tell partners to email the icon to integrations@clever.com.

### 🟡 Minor updates (17)

| Card | Recommended updates |
|---|---|
| [Escalations – App Dash SSO](https://app.getguru.com/card/TRzadnxc/Escalations-App-Dash-SSO) | Add to the template: *"After SSO verification, password logins are disabled by default"* (admins keep password access during setup). Have PE confirm the SAML EntityID/ACS values. |
| [Escalations – General](https://app.getguru.com/card/TM5n7xac/Escalations-General) | CC application-interest@ on any new integration, not only Secure Sync. Swap the named contact (darrell.stewart@) for a team alias if it's stale. Remove the trailing "#####" lines. |
| [Feature Flags](https://app.getguru.com/card/igj4dkbT/Feature-Flags) | Content Mapping is no longer described as "closed beta". It's a documented Clever Complete feature, so link `/docs/resources`. Remove the struck-through Naviance block. |
| [How to Use LaunchDarkly](https://app.getguru.com/card/cKMn7xRi/How-to-Use-LaunchDarkly) | Confirm `show-canvas-in-portal` still exists (its sibling Naviance flag was discontinued). Refresh the 2022 recordings. |
| [Sandbox Districts – Access Existing Sandbox](https://app.getguru.com/card/TAqxdyKc/Sandbox-Districts-Access-Existing-Sandbox) | In the .sand snippet, replace `/docs/il-testing` (legacy) with `/docs/testing-your-sso-integration`. Fix the signup URL once PE confirms it (see item 12). |
| [Troubleshooting – Clearing Cookies](https://app.getguru.com/card/c68BEkAi/Troubleshooting-Clearing-Cookies) | Update the Chrome Web Store URL to `chromewebstore.google.com`. Optionally add the `district_id` tip from the docs for skipping the school picker. |
| [Troubleshooting – App-side error](https://app.getguru.com/card/TE5xAnMc/Troubleshooting-App-side-error) | Note that auth codes are single-use and expire in one minute, so ask for a timestamp and user ID instead. Link `/docs/error-messages` for triage. |
| [Troubleshooting – User IDs vs Legacy IDs](https://app.getguru.com/card/cKMx8oyi/Troubleshooting-User-IDs-vs-Legacy-IDs) | The docs say the Data Browser now has a v2/v3 toggle. Add that students' `id` equals their `legacy_id`. Say "v3.x". Mention the v3.x Migration CSVs and the `sub`/`multi_role_user_id` claims. |
| [App Dash – Filters](https://app.getguru.com/card/Tx4qXjnc/App-Dash-Filters) | Add Clever Complete naming. Check the permission label and screenshot against the live UI. |
| [App Dash – Developer Onboarding Flow](https://app.getguru.com/card/cg8BGAGi/App-Dash-Developer-Onboarding-Flow) | Check the banner still appears. Link Getting Started and Going Live. |
| [App Dash – Data Browser](https://app.getguru.com/card/T8AArd5c/App-Dash-Data-Browser) | Replace the legacy `/docs/district-getting-started` link. Mention v3.1, and that v2 isn't available to new apps. |
| [Settings – Safety Thresholds](https://app.getguru.com/card/Tr4G4gMc/Settings-Safety-Thresholds) | Add Clever's default hold: a sync pauses if more than 10% of any record type is deleted, and this is relaxed at rollover. Add that **sharing changes aren't held**. |
| [Settings – General](https://app.getguru.com/card/cg88n4qi/Settings-General) | Exposed secrets: Clever **resets them within 24 hours**, and this covers tokens and auth headers too. Mark "SSO only" as legacy. |
| [Settings – SSO Integrations](https://app.getguru.com/card/iE55ApbT/Settings-SSO-Integrations) | Correct the redirect URI definition: it's where Clever sends the `code` *after* authentication. The resize handle is in the **lower right**, not the bottom left. Add Instant Login links as the third entry point. Add the note that Portal logins don't include `state`. |
| [Settings – Data Access](https://app.getguru.com/card/id88MELT/Settings-Data-Access) | **Inaccurate:** enabling extension fields on students doesn't unlock them for every record. `ext` has to be marked optional **on each record type**. |
| [Settings – API v3 Migration](https://app.getguru.com/card/ck88n8qi/Settings-API-v3-Migration) | Say "v3.x". The IDs are the same in v3.0 and v3.1. Remove the Library note. Add the support-article anchor. |
| [Scopes & Tags – App Toggles](https://app.getguru.com/card/cq84XExi/Scopes-Tags-App-Toggles) | The card is only a Confluence link. Confirm the page is current, and consider summarizing the key tags inline. |

### 🟢 Current (13)
PE Processes – Jira Issue Types · PE Processes – Jira Overview (the 2023 GIF may be dated) · Developer Support Feedback · Product – Connecting with Districts · Troubleshooting – Email Denylist · Troubleshooting – OneRoster EXPORTS (Elmer) (optional: name Secure Sync as the path; the Elmer app list looks stale) · App Dash – Analytics · App Dash – Billing · App Dash – Team · Settings – Globally Recommended Sharing · Scopes & Tags – Paying Customer · Advanced – Clone App · Advanced – Delete App

---

## 5. Problems in the public docs themselves (raise with the docs owners)

- **Legacy pages are still live but missing from the docs index.** `/docs/library-certification-guide`, `/docs/certification-types`, `/docs/classroom-testing`, `/docs/il-testing`, `/docs/district-getting-started` and `/docs/identity-api` all still resolve and describe Library or "Instant Login". CS cards and partners still link to them.
- **The docs give two different signup URLs.** Getting Started uses `clever.com/developer-signup` and the SSO Quickstart uses `apps.clever.com/signup`.
- **Some current pages still mention Library.** The OAuth Implementation page links "Secure Sync" to the legacy `/docs/district-getting-started`. Multi-Role Users and Setting up SSO still mention Library.
- **Two pages disagree on Events testing.** sync-testing says to use Certification ISD, while events-api says you need a custom sandbox.

## 6. Needs a manual check
- **Help Center articles I couldn't read.** support.clever.com pages only load in a browser, so I couldn't read these: 115003249203 (Filters), 360000508126 (Billing), 360043514192 (Sharing / v3 Migration), 360044699192 (Safety Thresholds), 360046090612 (Teacher Quickstart), 360000216886 (field sharing).
- **Links that came back empty:** `assets.clever.com/devdocs/cleverusd.zip`, `clever.com/certify` and `dev.clever.com/reference/posttokens`.
- **Internal details with a staleness risk:** named people (darrell.stewart@, Victor Hsiang in a screenshot, @emilie) and Slack channels (#oncall-classroom-experience, #oncall-district-dashboard, #oncall-discovery, #oncall-auth).

> **Update (Oct 21):** The 5-sandboxes-per-app limit no longer applies, so leave it out of every card. The public docs page (custom-sandbox-districts) was updated on Oct 21 to remove it as well.

## 7. Effect on the Claude project archive
Two of our own knowledge entries should pick up these findings:
- **SBX-03 (new sandbox):** add the Clever Complete requirement and the 5-sandboxes-per-app limit.
- **SBX-02 (test data):** add the Clear Water ISD and CertificationISD.zip datasets.

---
*Sources: dev.clever.com pages: what-is-clever, getting-started, integration-types, district-sso-1, custom-sandbox-districts, sync-testing, security, oauth-implementation, api-overview, api-v31, api-v3-upgrade-guide, error-messages, extension-fields, data-changes, setting-up-district-sso, certification-overview, secure-sync-certification-guide, district-sso-certification-guide, going-live, resources, events-api, multi-role-users-in-clever. Also clever.com/pricing.*
