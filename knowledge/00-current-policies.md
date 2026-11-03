# Current policies and routing (read this first)

These are the current Developer Support policies. **They override any older guidance in other entries or in past cases.** If a case entry contradicts this file, follow this file and flag the conflict.

Last confirmed: October 21, 2026, during the Developer Support Guru review, from the public docs at dev.clever.com. POL-06 updated October 27, 2026 (v2 certification and new apps).

---

## POL-01 · Product offerings: everything new is Clever Complete
- **New integrations need a Clever Complete agreement.** That covers Secure Sync, SSO + Rostering, LMS Connect, AnySchool Rostering and SAML.
- **LMS Connect** (grade passback and LTI 1.3 SSO for Google Classroom, Canvas and Schoology) **requires Secure Sync**.
- **AnySchool Rostering** also **requires Secure Sync**. It needs extra scopes and is turned on for production apps by default.
- Docs: https://dev.clever.com/docs/integration-types

## POL-02 · No new standalone SSO-only integrations
- Clever doesn't offer **new** SSO-only integrations. New SSO comes as part of **SSO + Rostering** under Clever Complete.
- **Existing** SSO-only apps are still supported.
- Never describe SSO as "free" or as its own tier for a new partner.
- SSO data: Clever ID, full name, email address and district.

## POL-03 · Library: still a product, but no new certifications
- Clever Library is still a product, and **existing Library apps are supported**.
- Clever **isn't certifying new Library integrations**.
- **For any Library question or issue** (new requests, certification, troubleshooting, reviews, analytics, unlaunching), CS should **ask in #partner-engineering** so PE can review it.
- **Don't** send partners Library certification docs. **Don't** promise a timeline.

## POL-04 · Routing a new partner or prospect ("how do I get started?")
1. Send these docs: https://dev.clever.com/docs/getting-started, https://dev.clever.com/docs/what-is-clever, https://dev.clever.com/docs/integration-types and https://dev.clever.com/docs/certification-overview
2. Check whether they're a **paying customer**. Look for the Paying Customer tag under Scopes & Tags in Hall Monitor. A prospect with no Clever account isn't a paying customer.
   - **Not a paying customer:** loop in **application-interest@clever.com**, then solve the case.
   - **Paying customer:** loop in their **Application Success Manager (ASM)**.
3. Optional extras: https://clever.com/schema and https://clever.com/pricing

## POL-05 · Certification questions
For **any** certification question (status, how to submit, the form, feedback, LMS Connect certification):
1. **Check whether the partner is a paying customer** (Paying Customer tag under Scopes & Tags in Hall Monitor). If they **aren't**, route them using POL-04 (application-interest@clever.com).
2. **If they are a paying customer**, reply:
   > Thanks for reaching out! Certifications can take up to two weeks to review. Let me check in with our Partner Engineering team for next steps or an update.
3. **Then post in #partner-engineering in Slack** with:
   - the Salesforce case number,
   - the partner name and app ID,
   - the contact you're working with,
   - a note that they have questions about their certification.

Also:
- The only timeline CS gives is "up to two weeks" for review, and only to a partner who is waiting on a review. If a partner says they're already certified, skip that line. Check whether they already have a production app: if not, post in #partner-engineering; if so, it's not a certification issue, so troubleshoot visibility or connection first. PE handles any other timing questions.
- **Never send a survey or form link.** PE sends the right form.
- **Library certification:** see POL-03.
- **Clever ID must be the primary key.** Partners must use the Clever ID as the primary key for users in their Clever integration. An app that keys users on email or another field **won't be certified** until it switches. This is a blocker, not a caveat (confirmed Oct 22, 2026).

## POL-06 · API versions
- **v3.1 is the current version.** It only adds non-breaking changes to v3.0, mainly new student demographic fields.
- **New apps get v3.0 by default. v3.1 is turned on by request**: transfer to PE, and loop in the ASM if the partner has one.
- **v2.x is deprecated.** New apps can't use it. Existing apps are still supported, and Clever is moving partners from v2 to v3.
- **Clever no longer certifies apps built on v2.** Partners on v2 need to move to v3 before certification.
- **v2.0 and v2.1 aren't added to new apps.** Only v3.0 is added automatically. Some older apps still have v2 access. Don't offer to add v2 to a new app.
- **Don't quote deprecation or shutoff dates.** Clever hasn't published a sunset date.
- Partners choose the version in the request URL (for example `/v3.1/users`) and should always use an explicit version. The Hall Monitor "Default" column doesn't affect API calls, but it does set which Data Browser version the app sees.
- "Version not supported" means that version isn't enabled for that client ID. Confirm the client ID first, then transfer to PE.

## POL-07 · Custom sandbox districts
- Custom sandboxes are a **Clever Complete / Secure Sync** feature.
- **There's no limit on the number of sandbox districts per app.** The old per-app limit was removed on Oct 21, 2026, and the public docs no longer mention it.
- Naming: `#DEMO <app name>`. Each sandbox needs an admin email that isn't used by any other Clever account. Aliases are fine.
- Sample datasets:
  - Default, 5 schools: https://assets.clever.com/devdocs/default_sandbox_data.zip
  - Clear Water ISD, 250 schools / ~286K users: https://www.clever.com/wp-content/uploads/2026/04/Clearwater_Data.zip
  - Certification ISD edge cases: https://assets.clever.com/devdocs/CertificationISD.zip
- **Certification ISD** is a shared, **read-only** sandbox. It rotates through 3 days of data every 3 days and is used for edge cases and Events testing. Partners request it at integrations@clever.com and include their dev app name.
- **Events are only available for Secure Sync.** In a sandbox, both CSV re-uploads and **custom data edits** create Events (custom data confirmed Oct 21, 2026).
- **No eligibility check needed.** CS doesn't have to check Clever Complete / Secure Sync before creating a sandbox for a partner (confirmed Oct 21, 2026).
- **Re-upload rule:** if the sandbox has had a full sync in the last ~6 months (180 days), re-uploading any one required file is enough. After that, upload all five required files (schools, students, teachers, sections, enrollments) together, or custom data won't fully process.
- Docs: https://dev.clever.com/docs/custom-sandbox-districts and https://dev.clever.com/docs/sync-testing

### District requests for a dev app ("District App")
A **district** (not an app partner) sometimes asks for its own developer app, usually to reach its own data through the Clever API. This is handled case by case and isn't advertised in the Help Center or dev docs. It's rare.
1. **Check the requester is a Clever Admin (owner)** on the district's team members page. If they aren't, loop in an owner for permission. Never grant this to someone who isn't a district owner.
2. **Ask how the district plans to use the app**, and get the name of the District App administrator plus an email that **isn't** the one they use for their district Clever account (an alias is fine).
3. **Get approval:** loop in the district's **School Success rep** (listed on the Info tab of the district's account in Hall Monitor) and get approval from a **CS Manager**. If the district is in Onboarding, CS still owns the app creation, and Onboarding handles everything else.
4. **After approval**, have the district sign up for a developer account at https://apps.clever.com/signup (Textblaze: `.apiaccess`), using an email not linked to any existing district admin account. Tell them:
   > Please note that you will **only** be able to connect the developer account to your own district. If in the future your district wishes to allow other districts to use the developer app, you will need to obtain a Secure Sync contract with Clever.
5. **Once they've signed up**, post the request in **#partner-engineering** with: the dev app ID, the district ID, the approving CS Manager and proof of approval (for example a Slack link), and the Salesforce case number. PE follows up in the thread.
6. **When PE is done**, tell the district the dev account is ready, and set expectations:
   > At this time, Clever Support is unable to provide troubleshooting assistance outside of the resources listed in the developer documentation. Please refer to our documentation on developer access for assistance with the Clever API: https://dev.clever.com/docs/api-overview

   PE doesn't support districts accessing their own data through the API either. You don't need to tell the district that.

**Deny the request if:**
- the requester isn't a district owner, or the owner declines when looped in; or
- the district wants to connect the app to **another** district. That needs a Secure Sync contract and a production account, so route them to **application-interest@clever.com** for the normal new-app sales process (POL-04). To find this out, ask something like: *"Could you share how your district is hoping to use the Clever data? Our team would love the feedback for product development."*

District requests for a **sandbox** are handled by CS. If the district has questions, or very specific requirements for the sandbox, CS can ask in #partner-engineering. They aren't a District App request.

Source: Guru card "Districts Creating an App to access the Clever API".

## POL-08 · Security and credentials
- Partners must never send client secrets, district-app tokens or authorization headers over email or other unsecured channels. If Clever sees one, it **resets it within 24 hours**. Tell the partner to rotate the credential right away.
- **If CS comes across a client secret, district-app token or authorization header shared over email**, notify **#partner-engineering immediately** so PE can rotate the token. Don't wait for the case to be transferred.
- **All app dashboard users must have SSO or 2FA turned on.** The three options are App Dashboard SSO (SAML), Sign in with Google, and 2FA.

## Where to ask
| Topic | Channel / contact |
|---|---|
| Anything Library | #partner-engineering |
| Secret, token or auth header seen in email | #partner-engineering, immediately |
| District asking for a District App (dev app) | School Success rep + CS Manager approval, then a request in #partner-engineering |
| District asking for a sandbox | CS handles it. Questions or very specific requirements: #partner-engineering |
| Any certification question (forms, status, timing, feedback) | #partner-engineering |
| New-integration interest (non-paying) | application-interest@clever.com |
| Existing paying partner, commercial or scope questions | Their ASM |
| Technical escalations | Transfer the SF case to PE |
