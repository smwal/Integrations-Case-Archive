# Certification & onboarding

Covers these Salesforce buckets: App Onboarding > Certifications - Secure Sync, Certifications - District SSO, Certifications - LMS, Certifications - Library, and Getting Started.
Source: resolved integrations@clever.com cases. Every entry is a draft until PE has reviewed it. Always check paying customer → "up to two weeks" reply → post in #partner-engineering; no survey/form links. Library questions go to #partner-engineering.

---

## CRT-01 · "We submitted our certification. What's the status, and when will we hear back?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Triage only. CS replies, then asks in #partner-engineering; PE does the review.

**When to use this**
- The partner says they submitted the certification form (or got the automated certification follow-up email) and asks for status or an ETA, or says they've heard nothing.
- The partner says they were certified but don't see a production app.

**Answer / steps**
1. **Check whether the partner is a paying customer** (Paying Customer tag under Scopes & Tags in Hall Monitor). If they aren't, route them to application-interest@clever.com.
2. **If they are** and they're waiting on a review, reply:
   > Thanks for reaching out! Certifications can take up to two weeks to review. Let me check in with our Partner Engineering team for next steps or an update.
   If the partner says they're already certified (for example the issue is a missing production app), skip the "up to two weeks" line. Check whether the partner already has a production app. If they don't, post in #partner-engineering. If they do, the issue isn't certification, so troubleshoot it as a visibility or connection problem (see CRT-03 and APP-06) and ask in #partner-engineering if you can't resolve it.
3. **Post in #partner-engineering in Slack** with:
   - the Salesforce case number,
   - the partner name and app ID,
   - the contact you're working with,
   - a note that they have questions about their certification.
4. PE follows up from there. Don't assess the review yourself.

**Escalate if…**
- Always: every certification question goes to #partner-engineering after the reply in step 2.

**Notes / conflicts**
- The only timeline CS gives is "up to two weeks". Don't promise a specific date. In one case a promised date was missed.
- Many reviews stall because PE asked the partner for SSO test users. If PE's last message on the case is waiting on the partner, mention that in Slack.
- Never ask for passwords or secrets. Don't ask the partner for test users or a screen recording yourself for certification. Post in #partner-engineering and let PE ask for anything it needs.

**Links:** https://dev.clever.com/docs/certification-overview
**Source cases:** 03053659, 03002826, 03085951, 03081128, 03044326, 03049028, 03116382, 03033146, 03012887, 02972586, 02993813, 02986833, 03055116, 03053848, 02992053, 03009312, 02994520, 03065938

---

## CRT-02 · "How do we get certified? What does certification involve? We're ready for production."
**Status:** Draft, needs PE review · **Resolvable by CS:** Triage only. PE or the ASM sends the form.

**When to use this**
- The partner **hasn't submitted yet**. They ask what certification involves, where the form is, how to get a production app, or ask Clever to "start certification".
- The partner has several products and asks whether they need separate apps or certifications.

**Answer / steps**
1. **Check whether the partner is a paying customer**. If they aren't, route them to application-interest@clever.com. A paying customer with a brand-new product may also need a contract amendment, which their ASM handles.
2. **Don't send the form or a survey link**. After replying (step 3), post in **#partner-engineering** with the Salesforce case number, the partner name and app ID, the contact you're working with, and a note that they have questions about certification. PE sends the right form.
3. Send:
   > Thanks for reaching out! Certifications can take up to two weeks to review once submitted. Certification is the last step before you get a production app. Our Partner Engineering team reviews how your integration syncs and handles district data, then tests SSO with your test users. Before you submit, please review:
   > - Certification overview: https://dev.clever.com/docs/certification-overview
   > - Secure Sync certification guide: https://dev.clever.com/docs/secure-sync-certification-guide
   > - Testing your sync: https://dev.clever.com/docs/sync-testing
   > - Testing your SSO: https://dev.clever.com/docs/testing-your-sso-integration
   > - Going live: https://dev.clever.com/docs/going-live
   >
   > When you submit, please have ready: your dev app's client ID, a 200×200 px app icon (PNG only), the API endpoints you call, how and how often you sync, and a few sandbox test users (their Clever IDs) linked to accounts in your app, so we can test logging in from the Clever Portal. Please don't email passwords or client secrets. Let me check in with our Partner Engineering team for next steps, and they'll follow up on this thread.
4. **Which environment:** the partner can submit with a staging or dev setup, or with the integration already live in their own production. "Either flow works." They should tell PE which one they chose. The default redirect URI must be **publicly reachable** (not localhost) so PE can test a real Portal login.
5. **Several products:** if the products need separate names or icons in the Portal, PE recommends separate Clever apps in the same dashboard. Each app is certified separately. PE creates extra dev or QA apps on request (see SBX-08). The production app is only created after certification.

**Escalate if…**
- The partner wants new dev or QA apps, renamed apps, or scopes added to an existing production app. Only PE does these.
- The partner wants a call.
- It's about **LMS Connect** certification (ask in #partner-engineering) or **Library** (ask in #partner-engineering only).

**Notes / conflicts**
- In these cases PE sent the form directly. That's expected for PE, but it's not something CS does.
- Two older cases (Oct 2025) had PE creating or certifying a standalone SSO-only app for an existing partner. Don't suggest a new SSO-only app.
- LMS Connect and AnySchool require Secure Sync.
- Don't link the dev docs "Certification Types" page. It still describes Library and standalone SSO certifications, which is outdated: Library isn't certifying new apps and standalone SSO-only isn't offered to new partners.
- Certification ISD testing is for the partner's own validation. They don't send results to Clever. Supporting every edge case isn't required but is strongly recommended (03053971).

**Links:** https://dev.clever.com/docs/certification-overview · https://dev.clever.com/docs/secure-sync-certification-guide · https://dev.clever.com/docs/sync-testing · https://dev.clever.com/docs/testing-your-sso-integration · https://dev.clever.com/docs/going-live
**Source cases:** 03082113, 03084290, 03081128, 03105227, 03078199, 03048588, 03045627, 03011850, 03033146, 03070862, 03037961, 02975347, 03039515, 03050218, 03053971, 03051064, 03013127, 03034828, 03048861

---

## CRT-03 · "We're certified. How do we go live? Our app isn't showing in production, or districts can't find us."
**Status:** Draft, needs PE review · **Resolvable by CS:** Yes, once PE has created the production app.

**When to use this**
- PE has confirmed certification and created the production account, and the partner asks what to do next.
- The partner is confused about the dev app vs the production app.

**Answer / steps**
1. Check the case history for PE's message saying it created a production account. If it isn't there, use CRT-01.
2. Send:
   > Congratulations on completing certification! To finish going live:
   > 1. In your Clever dashboard, use the app dropdown at the top of the page to switch to your new production app.
   > 2. Note your production credentials. They're different from your dev app's. Please keep your client secret private and never send it by email.
   > 3. Update the redirect URIs on the production app as needed.
   > 4. Click through the go-live steps. Once you finish, your app is live.
   >
   > Your dev app is for testing only, and districts can't find it. Keep it for testing changes before you release them. This guide covers connecting with districts: https://support.clever.com/hc/s/articles/215120718

**Escalate if…**
- No production app shows in the dropdown, or the partner expected Secure Sync scopes to be added to an existing production app. PE adds the scopes or copies the dev app to production.
- The partner wants the app **invite only** (districts can be invited but can't find it in the App Gallery), or wants invite-only removed. PE sets this (see APP-01).
- The partner wants a live district connected to a **dev** app. That isn't possible; PE offered an invite-only production app instead.
- The partner wants a production app renamed, regional production apps, or isn't sure which of several apps is production.

**Notes / conflicts**
- If the partner replies with a client secret, follow the reset process and notify #partner-engineering right away.
- The cases don't say how long going live takes. Don't promise timing.
- The launch date only controls when users see the app in their Clever Portals. It doesn't affect whether data syncs or logins work (03101235).

**Links:** https://support.clever.com/hc/s/articles/215120718 · https://dev.clever.com/docs/going-live
**Source cases:** 03048873, 03082752, 03049028, 03009840, 03003347, 03002826, 03004833, 03012298, 03011868, 03004464, 02998782, 03004966, 03081128, 03085951, 03045627, 03101235, 02992053, 02994520, 03009312, 03016190, 03025016, 02981410

---

## CRT-04 · "We got automated feedback on our certification submission. What does this warning or error mean?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Triage only. PE decides what blocks certification.

**When to use this**
- The partner forwards or replies to the automated "initial feedback from your submission" email.
- A reviewer's follow-up questions confuse the partner.

**Answer / steps**
1. CS can explain what an item means. **PE decides whether it blocks certification.** Make sure the case is with PE.
2. What each automated item means:

| Automated item | What it means | What reviewers did next |
|---|---|---|
| "You have not provided valid endpoints" | The submission didn't list the Clever API endpoints the app uses | The partner replied with the endpoints and PE accepted them. For partners using a third-party integration provider, PE said endpoints weren't needed. |
| "Sync methods not recommended as best practice" | The sync is manual or on demand rather than scheduled | PE asked how mid-year or same-day roster changes get picked up |
| "Insufficient information provided for testing… valid Clever IDs" | No sandbox test users for SSO testing | PE asked for the Clever IDs of rostered test users, or a screen recording of a working Clever login (PE asks for these, not CS) |
| "Clever ID… as a key is a requirement" | The app keys users on email or another field | **Blocker**. The app won't be certified until it uses the Clever ID as the primary key. Older reviews accepted it with caveats; that no longer applies. |
| "Failure to invalidate previous sessions on shared devices" | The app doesn't end the earlier session when a new Clever login arrives | The app was still certified |

3. Reviewer follow-ups you may see:
   - **Email required:** email isn't a required field in Clever, and Clever doesn't verify it. Options PE gave: accept that some users won't be rostered, have the district sync emails, or have Support set up a custom email mapping.
   - **Required fields:** PE asks whether districts should see a data warning when a field the app needs (for example teacher email or term) is missing.
   - **Deleting records:** PE described archiving unshared or removed records, and restoring them if they come back, as required in one case and strongly recommended in another.
   - **API version:** the version on the submission didn't match the dev app, or the endpoints were v2-only.
   - **Other flags:** fields that aren't in Clever's API, and downloading CSVs instead of using the API (flagged as a security risk).
   - **Localhost redirect URIs:** these block PE's live SSO test.
4. Send:
   > Thanks for sending this over! The automated message is a first-pass check of your answers, not a final decision. A Partner Engineer will review your full submission and follow up on this thread. If you can already address the flagged items (for example, by replying with the API endpoints you use or the Clever IDs of a few sandbox test users), please reply here and your reviewer will pick it up.

**Escalate if…**
- Always ask in #partner-engineering. Apart from the Clever ID rule (a known blocker), don't tell the partner whether an item is a blocker; PE decides.
- The partner wants a call.

**Notes / conflicts**
- Don't ask the partner for test users' Clever IDs or a screen recording for certification. PE asks for those. Never ask for passwords. In one case the partner emailed passwords unprompted and nobody flagged it.
- Don't describe v2 as "valid". It's deprecated for new apps, and don't quote any dates.
- **Clever ID as the primary key** is now a certification blocker. Two May 2025 cases accepted email-keyed apps with caveats; don't treat those as precedent.

**Links:** https://dev.clever.com/docs/secure-sync-certification-guide · https://dev.clever.com/docs/users · https://dev.clever.com/docs/security
**Source cases:** 03008938, 03011868, 03008300, 03007048, 03004155, 03004966, 03045627, 03011341, 03011347, 03000418, 02998969, 02998782, 03001037

---

## CRT-05 · "What do reviewers expect from a strong Secure Sync integration (sync frequency, Events, deletions, rollover, IDs)?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Yes.

**When to use this**
- A partner who is building, or preparing for review, asks design questions: how often to sync, the Events API, deletions and archiving, whether Clever IDs change, rollover, per-school data.
- A partner asks what the "I have tested AnySchool" box on the submission means.

**Answer / steps** (based on what PE told partners)
- **Sync:** a nightly sync plus Events for changes in between, with a periodic full sync (monthly, between terms, or at rollover). Events are an optimization, not a replacement for full syncs.
- **Events API:** opt-in per app, under Settings > Integration in the app dashboard. Events can arrive out of order or more than once, so update records by Clever ID. Partners can edit sandbox data to create test events (SBX-02).
- **Deletions:** data drops out of the API only when it's deleted in Clever or the district stops sharing it. The Events API sends a "deleted" event. Archive and restore instead of hard-deleting. After a record has been gone for 300 days, Clever issues a new Clever ID.
- **Clever ID:** unique across all districts. It stays the same unless the user changes districts or their SIS ID changes. Sync conversions (a district changing SIS) are handled only by Clever Support, so send those questions to them. It **must** be the primary key; apps that key on email or another field won't be certified (CRT-04).
- **Per-school data:** use school-scoped endpoints such as `/schools/{id}/sections` and `/schools/{id}/users`. The global endpoints can't be filtered by school.
- **Section names:** these come from the SIS. If they're missing, Clever builds one from the course, teacher last name and period. A district can ask Support to turn that off.
- **Rollover:** sections get new IDs each year. Don't create duplicate accounts for returning students. Expect overlap when a district shares next year's roster early. Most districts want last year's groups archived.
- **AnySchool checkbox:** it's fine to tick if the partner knows about the feature and has read the AnySchool article. Testing it is optional.
- PE called the wider list (deprovisioning, mid-year section changes, multi-role users, pagination and backoff on large first pulls, data minimization) "gold standard", not hard requirements. There's no published rubric beyond the dev docs.

> Great questions! Here's what our reviewers look for in a Secure Sync integration:
> - Run a scheduled sync (for example nightly), use the Events API for changes in between, and still do a full sync periodically, such as monthly or at rollover.
> - Use the Clever ID as your primary key. This is required for certification. It's unique across districts and stays the same unless a user changes districts or their SIS ID changes. Questions about a district switching SIS are handled by Clever Support.
> - When a record disappears from the API (deleted or no longer shared), archive it rather than hard-deleting it, and restore it if it comes back.
> - Plan for school-year rollover: sections get new IDs each year, and returning students shouldn't get duplicate accounts.
>
> More detail: https://dev.clever.com/docs/onboarding and https://dev.clever.com/docs/data-changes

**Escalate if…**
- The partner needs Events turned on or off, an API version change, sandbox changes, or a design review or call. Send to PE.
- The partner asks which API version to build on. New apps get v3.0, and v3.1 is turned on by request. Send v3.1 requests to PE (VER-01).

**Notes / conflicts**
- One Sep 2025 case told a partner v2.1 was "still valid" with no deprecation plans. Don't reuse that. In a Mar 2026 case PE recommended v3.1 to a new app. v3.1 is still turned on by request.
- Events are only for Secure Sync. Certification ISD is the shared, read-only sandbox for edge cases and Events testing.

**Links:** https://dev.clever.com/docs/onboarding · https://dev.clever.com/docs/ss-design · https://dev.clever.com/docs/events-api · https://dev.clever.com/docs/data-changes · https://dev.clever.com/docs/users · https://dev.clever.com/docs/sync-testing
**Source cases:** 03098854, 03036713, 03070862, 03011850, 03045627, 03069295

---

## CRT-06 · "We want to build a Clever integration. How do we get started, and what do we need?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Yes

**When to use this**
- A partner or prospect asks how to start, how to get a developer account, which integration type fits, or whether they need an agreement.
- This includes people who say they only want SSO, or who ask to begin certification before they've talked to anyone.

**Answer / steps**
1. Send the starting docs, then check whether they're a paying customer. Look for the Paying Customer tag under Scopes & Tags in Hall Monitor. A prospect with no Clever account isn't a paying customer.
2. If they aren't a paying customer, loop in application-interest@clever.com and then solve the case. If they are, loop in their Application Success Manager (ASM).
3. If they ask for a new standalone SSO-only or Library integration, tell them it isn't offered for new apps. New SSO comes as part of SSO + Rostering under Clever Complete, which needs an agreement. Contract and pricing details come from Application Partnerships or the ASM. Don't quote any price or fee. For anything Library, ask in #partner-engineering.
4. Don't say how many apps a partner can have. PE decides case by case.
5. Don't speculate about whether a partner can start building before an agreement is signed. Your part ends at paying customer or not, then route based on that.

> Hi there, thanks for your interest in building with Clever! A good place to start is our developer docs: https://dev.clever.com/docs/getting-started, https://dev.clever.com/docs/what-is-clever, https://dev.clever.com/docs/integration-types and https://dev.clever.com/docs/certification-overview.
>
> New integrations with Clever are part of Clever Complete, which includes SSO and rostering together. I'm looping in our team who can go over the agreement and next steps with you, and they'll follow up soon.

**Escalate if…**
- The partner already has a Clever agreement and the question is about scopes: loop in the ASM and ask in #partner-engineering.
- It's a Library question of any kind: ask in #partner-engineering and don't send Library certification docs.
- It's about a school on a non-US SIS waiting on a form (03073110). That case needed PE and the international onboarding team, and the outcome isn't known.

**Notes / conflicts**
- Older cases (02975347, 02972586, 02993813, 03039515, 03050218) told SSO-only partners to sign up for a dev account, build, and submit for certification, with a review time of about 1.5 weeks. That's no longer the process: new apps need Clever Complete, and the only timing CS gives is "up to two weeks".
- 03100622 shows the current answer: PE told an SSO-only partner that Clever doesn't certify Library or SSO-only apps, and offered to loop in Application Partnerships.
- 03074141: Application Partnerships said District SSO is part of Clever Complete and needs a signed contract. A "Library plus District SSO" plan isn't an option for a new partner.
- 02967599: the partner had an SSO-only app and asked about Secure Sync. Secure Sync scopes are enabled after an agreement is signed. A question about extra apps should go through the new-app check (paying customer, then #partner-engineering).

**Links:** https://dev.clever.com/docs/getting-started · https://dev.clever.com/docs/what-is-clever · https://dev.clever.com/docs/integration-types · https://dev.clever.com/docs/certification-overview
**Source cases:** 03060435, 02999043, 02967599, 02970777, 02975347, 03039515, 03073110, 03074141, 03100622

---

## CRT-07 · "Can we pull the data without the API, or how often do we have to sync?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Yes for the basics. PE for any technical blocker.

**When to use this**
- A partner or prospect asks how districts normally sync data to an app, whether Clever calls them (a webhook), whether they can import files while their build isn't finished, or whether to pre-sync accounts or create them at first login.

**Answer / steps**
These are answers PE gave in these cases. CS can relay them, but have PE review before promising anything new.
- Apps call the Clever API to pull district data at least once a day, or on user login. How much more often is up to the partner.
- The Events API can be used for changes, but PE recommends the app can also pull a full batch, which is needed for a new district's first sync and for troubleshooting.
- Clever doesn't support webhooks.
- There's no way to import a district's SIS files directly.
- Pre-syncing the roster versus creating accounts at first sign-in is up to the partner. Some apps roster on demand, and others pre-sync and match on the Clever ID at login. For SSO-only apps, a first-login onboarding step isn't unusual, but consider younger students who can't type much.

> Hi there, thanks for the question! Applications pull district data from our API, at least once a day or when a user signs in, and how much more often is up to you. Clever doesn't use webhooks, and we can't import a district's SIS files directly, so the data comes through the API. If it helps, I'm happy to ask our Partner Engineering team to take a look at your plan.

**Escalate if…**
- The partner is blocked on a specific integration issue, or asks about sync holds, thresholds or deletion behavior (see Notes). Transfer to PE with the details.
- They're on a tight go-live date for a district. That needs the ASM or Application Partnerships.

**Notes / conflicts**
- 03049552: PE said districts set sync-hold thresholds, and apps can set their own "safety thresholds". During a hold, the API returns stale data and the district's state shows "pending". Removing a school shows as `schools.deleted` in Events. This is technical, so keep it with PE.
- 03050218 also sent a certification form link. Don't repeat that. PE sends any form.
- "On user login" as an alternative to the daily pull wasn't clarified beyond PE's wording, so use PE's phrasing.

- There's no real-time API. The Events API is the closest, usually within 15–30 minutes, and Events are Secure Sync only (02974258).
- Districts sync data into Clever, but the app partner controls its own pull schedule (03069671).
**Links:** https://dev.clever.com/docs/events-api · https://dev.clever.com/docs/sync-testing
**Source cases:** 03069671, 02974258, 02967599, 03004348, 03049552, 03050218

---

## CRT-08 · "How do I test the login before we go live, and what test data and accounts can I use?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Yes

**When to use this**
- A new partner asks how to try the login or SSO flow in a sandbox, log in as test users, or what to test before certification.
- If the question is only about logging in as sandbox users or sandbox admin access, use SBX-01 and SBX-06.

**Answer / steps**
1. Point them to the SSO testing doc and the sandbox district created with their developer account (the default 5-school dataset).
2. To test as a specific user, open the user in the Data Browser and use "Access as Portal" under Support Tools (03014479).
3. QR codes for sandbox users: after the sandbox login works, the badges can be downloaded and printed (03021665).
4. Test scenarios PE suggested: heavy login load, users missing required fields (such as email), and the edge cases in the data-quirks doc.
5. Certification ISD (the read-only shared sandbox): testing against it is for the partner's own validation. They don't send results to Clever. Supporting every edge case isn't required but is strongly recommended, and more can be added after certification (03053971).
6. If a test user's login fails, PE's answer in 03000301 was to use the full login URL for that district and the test user's own username and password. The partner said it still failed, so escalate.

> Hi there! To test your login, you can sign in as the demo users in the sandbox district that came with your developer account. From the Data Browser you can also open a user and choose "Access as Portal" from the Support Tools dropdown. Here are the testing instructions for SSO: https://dev.clever.com/docs/testing-your-sso-integration. Let me know if anything doesn't work and I'll bring in our Partner Engineering team.

**Escalate if…**
- A test user still can't sign in after these steps, or the partner can't get into their sandbox at all.
- The partner asks for sandbox admin access or a new dev app. Use SBX-01 and SBX-08, and the paying-customer check.
- Clever's own test of the partner's SSO fails during review (02986833, 03055116). PE may ask for a screen recording (CS doesn't).

**Notes / conflicts**
- In 03014479, shared test usernames and passwords were pasted into the thread. Don't copy that. Impersonation is the safer answer.
- 03039515 and 02967271 are about SSO-only apps. For a new partner today, see CRT-06.

- Sandbox walkthrough students log in with the student number from the walkthrough article, not a username and password (03058165). PE sends the district SSO testing page for test users: https://dev.clever.com/docs/testing-district-sso
**Links:** https://dev.clever.com/docs/testing-your-sso-integration · https://dev.clever.com/docs/data-model-quirks-and-supported-features-district-integrations · https://dev.clever.com/docs/custom-sandbox-districts · https://dev.clever.com/docs/sync-testing
**Source cases:** 03049679, 03058165, 02991266, 03004263, 03009813, 02980147, 02967271, 02986833, 03000301, 03014479, 03021665, 03039515, 03053971, 03074141, 02978791

---

## CRT-09 · "Clever couldn't log in to our app during certification. What do we need to fix?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Triage only, PE makes the change

**When to use this**
- The partner has submitted, and PE says the reviewer couldn't complete the SSO test.
- Typical causes seen: the redirect page is behind a login or isn't publicly reachable, a test user can't sign in, or the auth code is still in the URL after login.
- The partner says their test site isn't public and asks for an IP address to allow.

**Answer / steps**
1. Check whether the partner is a paying customer (Paying Customer tag under Scopes & Tags in Hall Monitor). If not, route them to application-interest@clever.com.
2. Send the standard certification reply, then post in #partner-engineering with the case number, partner name and app ID, the contact, and a note that the partner has certification questions. Never send a form link.
3. If PE has said the reviewer couldn't complete the SSO test, relay it and ask for the missing item.

> Thanks for reaching out! Certifications can take up to two weeks to review. Let me check in with our Partner Engineering team for next steps or an update.
>
> From our review notes, our team wasn't able to complete a test login to your app. Could you check the following?
> - The first redirect URI in your app's OAuth settings is publicly reachable and isn't behind a login or password.
> - After sign-in, your app exchanges the code it receives for a token (the code shouldn't still be in the URL).
> - Any test users you shared can actually sign in.
>
> Please don't email passwords or client secrets.

4. If the partner's test environment isn't publicly reachable and they ask for an IP to allow, don't give one. Post in #partner-engineering for PE to answer.

**Escalate if…**
- Always post in #partner-engineering after the reply.
- The partner sends a screen recording, new test users or updated redirect URIs. PE reviews them.
- A token call returns a generic error. In one case (02976745) the cause was a misspelled form content type. Collect the exact request (without secrets) and the error text.

**Notes / conflicts**
- Older cases had partners fill in a survey or were sent a form link. That's no longer the process.
- Don't ask for a screen recording for certification, up front or later. Post every certification question in #partner-engineering and let PE decide what it needs.
- In two cases (02978791, 02998628) the final outcome isn't visible.
- In 02992053 a production redirect URI shared with another of the partner's apps was rejected as a duplicate. The thread has no answer, so don't promise a workaround. Duplicate redirect URI issues go to #partner-engineering (see SSO-07).

**Links:** https://dev.clever.com/docs/testing-your-sso-integration · https://dev.clever.com/docs/district-sso-certification-guide
**Source cases:** 02972972, 02976745, 02978791, 02998628, 03048163, 03065938, 02992053

---

## CRT-10 · "What will the reviewer check on our District SSO form, and why did they ask us to change something?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Triage only. Route to PE up front.

**When to use this**
- A partner has submitted and asks what the reviewer will check, or why PE asked them to change something on the form or the app.
- A partner asks a detailed question about how certification works for District SSO, such as how the app should behave when a Clever user has no account in the app.

**Answer / steps**
Don't answer the details. Routing the partner to PE right away is a better experience for them, and it keeps the CS agent out of details that PE decides case by case.
1. Do the paying-customer check. If they aren't a paying customer, route them to application-interest@clever.com.
2. Send the "up to two weeks" reply and post in #partner-engineering with the case number, partner name and app ID, the contact, and the partner's question. Never send a form link.

> Thanks for reaching out! Certifications can take up to two weeks to review. Let me check in with our Partner Engineering team for next steps or an update, and they'll answer your question about the review directly.

**Escalate if…**
- Always post in #partner-engineering. PE decides what blocks certification.

**Notes / conflicts**
- Clever ID must be the primary key. A partner who keys users on email, or who matches to existing accounts by email, won't be certified until it switches (02972501). Don't tell them email can be the key.
- In the cases, PE's review comments ranged from user types on the app dashboard not matching the form, to how the app signs in a user with no email, to ending previous SSO sessions on shared devices. Don't relay these yourself. PE raises them on the case.

**Links:** https://dev.clever.com/docs/district-sso-certification-guide
**Source cases:** 03009878, 03009312, 03048861, 02972501, 02978791
---

## CRT-11 · "We're moving from SAML (or Secure Sync) to District SSO. Can we reuse our app?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Triage only, PE makes the change

**When to use this**
- An existing partner with a SAML or Secure Sync integration says they've built, or want, a District SSO or OAuth integration.
- They ask whether the new app can replace the old one, or how to get test districts into the "live" app.

**Answer / steps**
1. Do the paying-customer check. If they're a paying customer, loop in their ASM.
2. Send the "up to two weeks" reply and post in #partner-engineering with the details. Never send a form link.
3. Don't promise the partner can reuse or convert their existing app. PE usually creates a new app when a partner who has a SAML app wants OAuth/OIDC SSO, or the other way around.

> Thanks for reaching out! Our Partner Engineering team handles app changes like this, so I'm going to check in with them for next steps. To help, could you tell us which app you're using today and what you want to move to, and whether you have a dev app with the new integration set up? Please note certifications can take up to two weeks to review.

**Escalate if…**
- Always. PE decides how apps are set up.

**Notes / conflicts**
- PE doesn't reuse a SAML app for an OAuth/OIDC-based SSO app, or the other way around. It gets messy, so PE creates a new app when a partner who has one wants the other (03011939). Don't offer to reuse or convert an app, and don't say how many apps a partner can have.
- In 03038555 (Secure Sync to District SSO) PE said a live app is only created by cloning the dev app after the go-live form. The case never confirms whether the existing live app could be reused.
- In 03032496 the partner needed a dev app and sandbox before they could fill in the form, and PE created both. A sandbox admin email must be unique across district and app accounts, so PE used plus addressing.
- Standalone SSO-only isn't offered to new partners, and existing apps are still supported. New SSO comes with Clever Complete. Never describe SSO as free.
- Older cases sent a survey or form link (for example 03011939). That's no longer the process.

**Links:** https://dev.clever.com/docs/integration-types · https://dev.clever.com/docs/certification-overview
**Source cases:** 03005998, 03011939, 03038555, 03032496, 03048861, 03034828

---

## CRT-12 · "What's included in Clever Complete? Do new integrations need all of it? How does AnySchool work?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Yes for what's included, triage only for anything commercial

**When to use this**
- A partner or prospect asks what they get, whether they can buy only SSO or only rostering, or whether LMS Connect or AnySchool costs extra.
- Not for an existing paying partner asking about scope. That goes to their ASM.

**Answer / steps**
1. New integrations come as Clever Complete, one agreement covering Secure Sync, SSO + Rostering, LMS Connect, AnySchool Rostering and SAML.
2. LMS Connect and AnySchool Rostering both require Secure Sync.
3. New SSO-only integrations aren't offered. Never call SSO free or its own tier.
4. Send the starting docs, then run the paying-customer check. Paying customers go to their ASM. Everyone else goes to application-interest@clever.com, then solve the case.
5. Don't quote prices, free-school offers, discounted rates or build/review timelines. The only timeline CS gives is "up to two weeks" for a certification review.

> Thanks for reaching out! New integrations with Clever come as Clever Complete, which includes SSO and rostering together, plus LMS Connect and AnySchool, under one agreement. LMS Connect and AnySchool build on Secure Sync. These docs are a good place to start: https://dev.clever.com/docs/integration-types and https://dev.clever.com/docs/getting-started. I'm looping in the team who can walk you through the agreement and next steps.

**Escalate if…**
- They want pricing, terms or a call (ASM or application-interest@clever.com).
- They ask about an AnySchool SIS or SSO source that isn't listed in the notes: ask in #partner-engineering.

**Notes / conflicts**
- Older cases show sales replies quoting per-school prices, a free starter structure, a discounted rate and build/review timelines. Don't repeat any of it.
- AnySchool Rostering supports these SIS sources: Infinite Campus, Skyward, Blackbaud, or any SIS that supports the OneRoster API. Districts without a supported SIS can use CSV upload or SFTP. Supported SSO sources: ClassLink, Google, Microsoft Entra and ADFS, Okta, Canvas and Rapid ID (confirmed by PE).
- One thread described AnySchool as an invite flow where schools enter their SIS details and SSO credentials, and counted it as one school connection. PE didn't confirm that. Don't relay billing details.

- AnySchool data has the same format as any other district on the Data API. Mappings and extension fields aren't supported. Clever can't take a ClassLink roster upload directly, so use AnySchool with ClassLink as the source. For CSV, the district re-uploads both files each time and keeps the same user_id so Clever IDs persist. At least one student is required in users.csv (PE-stated, may be a product gap). For a non-ClassLink-partner app, ClassLink districts can export OneRoster files to SFTP (recommended) or create a custom ClassLink app. AnySchool should be on by default for production apps with Secure Sync, so check scopes before asking PE to enable it. Don't relay billing details.
**Links:** https://dev.clever.com/docs/integration-types · https://dev.clever.com/docs/getting-started
**Source cases:** 03069044, 03062675, 03066248, 03070432, 03086444, 03058452, 03108950, 03092064, 03114830, 02978264, 02997596, 03002906

---

## CRT-13 · "We're interested in Clever's attendance data. How do we get started?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Triage only, PE makes the change

**When to use this**
- A partner asks about the Attendance API or wants to join testing or give feedback.

**Answer / steps**
1. Send the two attendance docs. Attendance data requires a Clever Complete agreement, because it depends on a Secure Sync integration, so it isn't open to non-paying partners (PE confirmed). Access is by request only: existing partners ask their ASM, others contact Partner Engineering.
2. Don't confirm access, eligibility or timing. Post in #partner-engineering with the case number, the partner name and app ID, the contact, and that they're interested in attendance data (and v3.1 if mentioned). In the cases, PE then set up a kickoff call.
3. If the partner has an ASM, loop them in. v3.1 is turned on by request through PE.

> Thanks for your interest in attendance data! These docs explain how it works: https://dev.clever.com/docs/attendance-data-how-does-it-work and https://dev.clever.com/docs/attendance-data-overview. Access is by request, so I'm looping in our Partner Engineering team to follow up on next steps.

**Escalate if…**
- Any question about eligibility, access dates or testing.

**Notes / conflicts**
- The docs describe the endpoints as a preview that may change. Districts send attendance by SFTP CSV only for now, and Clever stores records for 6 months past the attendance date. Don't promise more than the docs say.
- Four of the five cases were seen only at the first message (a colleague looped in PE), so the outcomes aren't confirmed.

- The app also needs v3.1 enabled. Without it the attendance endpoint returns "app is not authorized to access API version v3.1" (03060358).
- For attendance testing, PE uploaded an attendance.csv to a partner's sandbox with a student under 90% attendance (03073025). That's a PE task.
- For attendance testing a dev app is connected to a Clever-owned attendance test district. The partner's own sandbox district can't be used (03005425).
**Links:** https://dev.clever.com/docs/attendance-data-how-does-it-work · https://dev.clever.com/docs/attendance-data-overview
**Source cases:** 03002577, 03005425, 03014378, 03061489, 03073025, 03060358, 03000751, 03000752, 03000753, 03002103, 03003915

---

## CRT-14 · "Do you have sample code or an API test page?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Yes

**When to use this**
- A partner asks for sample code in a language, says the old API test page is gone, or wants to try calls before building.

**Answer / steps**
1. Clever doesn't offer a Python sample. The recommended way to try the API is the Clever Postman collections. The old test page was replaced by them.
2. Pick the right token for each collection: an access token from the SSO login flow for the Identity API, a district-app token for the Data API. The Events API needs to be turned on under Settings > Integration in the App Dashboard.

> We don't have a Python example, but our Postman collections let you try the API with test requests: https://dev.clever.com/docs/exploring-the-clever-api. The page walks through forking the collections and choosing the right token and environment.

**Escalate if…**
- They ask about endpoints, auth or scopes beyond the docs.

**Notes / conflicts**
- A Partnerships email in one case said Clever has no SDKs but publishes a swagger definition on GitHub. Not PE-confirmed, so don't relay it.

- PE sent a JSON schema file for the API on request (03069612). Ask PE for it.
**Links:** https://dev.clever.com/docs/exploring-the-clever-api
**Source cases:** 03069612, 02974950, 02989383

---

## CRT-15 · "Our production app only has Rostering. How do we add SSO?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Triage only, PE makes the change

**When to use this**
- A partner with a working production rostering app (Clever Complete) now wants SSO.

**Answer / steps**
1. Get the production and sandbox app IDs.
2. Don't ask for a login video or screen recording. If PE needs one, PE will ask.
3. Transfer to PE. For review or certification timing, post in #partner-engineering and send no form or survey link.

> To add SSO to your production app, I'll hand this to our Partner Engineering team. Please send the app ID(s) and a short description of the SSO you'd like to add.

**Escalate if…**
- The partner isn't a paying Clever Complete customer (run the paying-customer check), or wants a separate SSO-only app.

**Notes / conflicts**
- Older cases created new SSO-only or split SSO/rostering apps as routine (03059935, 03003835, 03005998). That's no longer the process. New SSO comes with SSO + Rostering under Clever Complete. Send these to PE and the ASM and promise nothing.
- The outcome of the one case wasn't shown.

**Links:** none confirmed
**Source cases:** 03113610, 03059935, 03003835, 03005998
