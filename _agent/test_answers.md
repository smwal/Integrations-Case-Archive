# Test answers

## A1
**What's going on:** Developers often mix up the app developer dashboard and the sandbox district's admin view. CS can only triage this. If the person isn't already an admin on the sandbox, PE has to add them. (Both entries are still Draft until PE reviews them.)

**First, ask the partner for:**
- the sandbox district ID,
- the email they're logging in with,
- which one they mean: the app developer dashboard, or the sandbox district admin view.

Then look up the sandbox and check whether that email is listed as an admin.

**If the email IS listed as an admin, send:**
> Your account is still an admin on the sandbox. To log in:
> 1. Go to your sandbox's district login page (clever.com/in/<district-shortname>).
> 2. Click the **District Admin** login link in the corner of the login window. This is the step people most often miss.
> 3. If your invite email never arrived or you still can't get in, use "Forgot your password?" on the admin login.
> 4. If none of that works, please send a short screen recording of the error.

**If the email is NOT listed, send this and route to PE to add them:**
> We can add you as a district admin in your app's sandbox district. Could you send us (1) the sandbox district ID, (2) your first and last name, and (3) an email address you have access to that isn't already used anywhere in Clever? Once we've added you, you'll get an email to finish setting up the account.

**Collect for PE:** sandbox district ID, first and last name, and an email not used on any other Clever account, including the app dashboard login. An alias like name+sandbox@company.com is fine, one per sandbox. If the partner got "This district admin already exists", the email is already in use and PE re-adds it with an alias.

**Notes:**
- Don't set a password and email it. Let the invite or "Forgot your password?" flow handle it.
- If the original owner left and the partner wants ownership moved, or the sandbox shows "not sharing any data" or looks deleted, escalate to PE.
- If they actually need the app developer dashboard (for example an app is missing from their dropdown), an existing app owner can add them under Team > Add Team Member, or PE can help.

Entries used: SBX-01

## A2
**What's going on:** Yes. Custom data edits in a sandbox create Events, and so do CSV re-uploads (confirmed Oct 21, 2026, though the entry is still Draft). Events are only available to Secure Sync apps, and they can't be backfilled. The partner must be logged in as the sandbox district admin to change data (if they can't, see the sandbox admin steps first, SBX-01).

**Ready-to-send reply:**
> Yes, changes you make in your sandbox can produce Events. To see them, turn on the Events API in your app's settings (apps.clever.com/<app-shortname>/settings/integration). Events can't be backfilled, so turn it on before you make changes. To generate Events, either edit records in custom data (https://schools.clever.com/custom-data/browse/students), or download the CSVs from https://schools.clever.com/sync, edit them (for example enrollments or term dates), and re-upload them at https://schools.clever.com/sync/upload. Once the sync has finished, usually within about 15 minutes, the events should be available on the Events API. More here: https://dev.clever.com/docs/events-api#syncing-events

**Extra tips:**
- If the last full sync was within about 6 months (180 days), re-uploading any one required file is enough. After that, upload all five (schools, students, teachers, sections, enrollments) together.
- For ready-made edge cases and Events, suggest connecting the dev app to Certification ISD. It's shared and read-only and can't be edited.

**Escalate if:** Field Mappings look right but data is still wrong after an upload, or nothing appears after 15 to 60 minutes (SBX-04: collect sandbox district ID, client ID, example record IDs, screenshot of the upload confirmation).

Entries used: SBX-02, SBX-04

## A3
**What's going on:** "Requested" means the dev app still has to approve the district's connection request. CS can only triage. PE makes any change. There's no per-app sandbox limit, and CS doesn't need to check Clever Complete or Secure Sync eligibility before arranging a sandbox.

**Ready-to-send reply for "Requested":**
> This happens because your dev app still has to approve the district's connection request. You can approve it under pending requests in your developer dashboard. If you'd rather have it happen automatically, for example for automated tests, we can set your dev app to auto-approve district requests. Please send us your dev app's client ID.

**If they also want another sandbox, collect for PE:**
- the dev app's client ID,
- the sandbox name (it's named `#DEMO <app name>`),
- the admin's name and a unique admin email not used on any other Clever account (aliases are fine),
- whether Clever should load demo data or leave it empty for the partner's own uploads.

First check what already exists. Partners often have an unused sandbox connected, and PE has sometimes connected an existing one instead of creating a new one. Then transfer to PE. Once created, the partner gets an email with signup instructions:
> You'll receive an email shortly with instructions on how to complete the signup process.

Don't promise timing. A sandbox can't be converted to production.

Entries used: SBX-03

## A4
**What's going on:** For the default sandbox, test users log in with their ID as both username and password. Custom sandboxes and uploaded users need a login method set up first.

**Ready-to-send reply:**
> **Default sandbox:** test users log in with their ID as both username and password:
> - Student: `student_number`
> - Teacher: `teacher_number`
> - School admin: `staff_id`
>
> You can find the test users in your sandbox district from your developer dashboard. In a developer account you can also type a test user's name while viewing the district and use the **Log in as...** button in the top-right corner.
>
> If you see the school picker, you won't find your sandbox by searching school names. Add your sandbox `district_id` to the authorization URL to skip it.
>
> **If it's a custom sandbox or users you uploaded:** the sample datasets are for uploading users and schools, not for logging in. As the sandbox admin, go to https://schools.clever.com/instant-login/settings, click **Add Login Method**, and follow the prompts (District Password is the simplest). Find your Portal URL at https://schools.clever.com/portal/settings/url and log in as a test user there, ideally in an incognito window. For users you upload, include `student_number`/`teacher_number` (or username and password fields). Changes can take 30 to 60 minutes to process.

**Escalate if:** login still fails after the login method is set up and the sync has finished (the sync may be stale, see SBX-04), it's a custom user added by hand (ask for the email on that account), or the redirect URI is `localhost` (ask for steps and screenshots first). Never ask for or send real users' passwords, and don't paste shared test credentials into a case.

Entries used: SBX-06, SBX-04

## A5
**What's going on:** By default Clever returns `email_verified: false` because Clever doesn't verify the emails districts put on user records. PE can set a flag per app so it returns `true`, so CS can only triage. Some apps already return `true`, so don't tell the partner an app is definitely false.

**Ready-to-send reply:**
> By default Clever returns `email_verified: false`, because Clever doesn't verify the email addresses districts put on user records. We can set a flag for your application so it returns `true`. This is done app by app, so please send the client ID for each app you want changed (production, dev and demo are separate apps). Once it's set, you don't need to change anything else in Clever. Could you also tell us what value you're currently seeing for each app?

**Collect and escalate:** the client ID for every app they want changed, plus the value they see per app. Transfer to PE to set the flag. Also escalate if an app that was already flagged still returns `false`. For dev apps, the setting applies to every connected user in every sandbox district on that dev app. PE also recommends keying users on the Clever ID since Clever doesn't verify emails.

Docs for the partner: https://dev.clever.com/docs/oauth-oidc-overview

Entries used: SSO-01

## A6
**What's going on:** `invalid_grant` almost always means the authorization code was used more than once (each code works once), or the `redirect_uri` sent at `/oauth/tokens` doesn't exactly match the one used at `/authorize` and registered in the dashboard (trailing slash included). If it's intermittent or device-specific, think of double handling on iOS, where Safari and the native app both handle the callback. If a dev app gets no `id_token` back, OIDC scopes may be missing, and PE can turn them on.

**Ready-to-send reply:**
> Thanks for the details! invalid_grant almost always means the authorization code was sent to /oauth/tokens more than once (each code works only once), or the redirect_uri in the token request doesn't exactly match the one registered in your dashboard (trailing slashes count). Could you check your logs for duplicate token calls with the same code, and make sure your redirect_uri is a fixed value rather than built from the incoming URL? If it still fails, send us your client ID, a timestamp, and the redirect_uri you're sending (no secrets, please) and we'll take a look.

**Also check:** that the request actually includes the `code` (a missing code returns `invalid_request`, not `invalid_grant`), the grant type and content type header, and that the code is exchanged right after the redirect.

**Escalate to PE if:** there are no duplicate calls and the redirect_uri matches, or the `id_token` is missing. If the partner pastes a secret or token, tell #partner-engineering right away.

Docs: https://dev.clever.com/docs/oauth-implementation and https://dev.clever.com/docs/oidc-implementation

Entries used: SSO-03, SSO-05

## A7
**What's going on:** There's no app dashboard setting for this. The device decides whether the redirect URI opens the app.

**Ready-to-send reply:**
> There isn't a dashboard setting for this. The device decides whether your redirect URI opens your app. For iOS, set up Universal Links (the apple-app-site-association file plus Associated Domains) and have your app exchange the authorization code itself. For Android and Chromebooks, host assetlinks.json and check that the device policy allows "Open supported links". Details: https://dev.clever.com/docs/il-native-ios and https://dev.clever.com/docs/ios

**Tips:** Browser cookies aren't shared with the app. Watch for the iOS double callback (Safari and the app both handling it), which shows up as `invalid_grant`. A "Launch app" button with a custom URL scheme can be a fallback on Android.

**Escalate to PE if:** it used to work and broke for many users with no change on the partner's side. In one case a Clever update caused it and was rolled back, and PE involves Engineering. Questions about Clever's iOS app and third-party SSO extensions or Managed App Configuration also go to PE.

Entries used: SSO-05, SSO-03

## A8
**What's going on:** That error means API v3.1 isn't turned on for that client ID. It's not a problem with the district or the token. A common cause is that the dev app has v3.1 but the production app doesn't. CS can only triage. PE turns v3.1 on. New apps get v3.0 by default, and v3.1 is on by request.

**Collect:** which client ID is failing (dev or production, or both). PE enables per app. Check first that PE hasn't already enabled it, as sometimes it already is.

**Ready-to-send reply:**
> That error means API v3.1 isn't turned on yet for that client ID. It isn't a problem with the district or your token. Could you send us the app name or client ID that's failing (the production app, if that's where you see it)? We'll get v3.1 turned on. Some fields, like home_language, are only available on v3.1.

**Next:** transfer to PE, and loop in the ASM if the partner has one. After it's on, if a field is still empty for one district, PE checks whether that district sends it in an extension field. Remind the partner to put an explicit version in the request URL. Don't mention any v2 dates.

Docs: https://dev.clever.com/docs/api-v31 and https://dev.clever.com/docs/error-messages

Entries used: VER-01, VER-02

## A9
**What's going on:** CS can't add scopes. PE changes them. Demographic fields need v3.1, and existing district connections may need a migration before the data appears. Even once on, the partner only gets a field if the district has uploaded the data and shared it through its field-sharing settings.

**Ask the partner for:**
- the client ID of each app, and whether it's dev, production or both,
- which exact fields they need.

Check whether the scope is already on and what they're seeing. The v3.1-only fields are `gifted_status`, `section_504_status`, `disability.*`, `home_language_code` and an FRL value of "Other". IEP, ELL, FRL and home language are scope-gated, and for `iep_status` the partner also adds the field in the app's Data Access settings.

**Ready-to-send reply:**
> We can turn on access to additional demographic fields for your app. A few things to know: this access requires v3.1 of the Clever API, and existing district connections may need a migration before the data appears. Even after it's turned on, you'll only get a field if the district has uploaded that data to Clever and shared it with your app through their field-sharing settings. Could you tell me which fields you need and the client ID of each app (dev, production, or both)?

**Escalate:** transfer to PE with client IDs, dev/prod, exact scopes or fields, and the partner's written OK for any production change. Loop in the ASM if there is one. Don't say whether districts must re-authorize (unknown), and don't promise timing. Afterwards, have the partner check in the Data Browser or with a test call.

Entries used: SCP-01, ROS-06, VER-02

## A10
**What's going on:** The most likely cause is that the dev app only has SSO scopes, which is the default for dev apps. SSO-only apps get just the user's Clever ID, name, email and district ID, not schools or sections. Rostering data needs Secure Sync, which is part of a Clever Complete agreement.

**Steps:**
1. Check the app's integration type.
2. If it's SSO-only, send the reply below, then run the paying-customer check (Paying Customer tag under Scopes & Tags in Hall Monitor). If they aren't a paying customer, loop in application-interest@clever.com. If they are, loop in their ASM.
3. If they need admin access to the "#DEMO ... (Dev) Sandbox" district, collect a first and last name and an email not used on any Clever account (including the app's login email), then add them as a Clever Admin.

> Your dev app currently has only Clever SSO scopes, which is the default for development apps. That's why you can see a user's Clever ID, name, email and district, but not school or section data. Secure Sync gives full roster data, district-app tokens and Data API access, and it's part of a Clever Complete agreement. See https://dev.clever.com/docs/integration-types.

**If the app already has Secure Sync:** check the docs link isn't broken (send https://dev.clever.com/docs/secure-sync-rostering and mention the docs were reorganized, use the left-hand menu). Also check whether a teacher has no sections, or the sandbox "holds" page has pending changes to approve (SCP-08), and whether admins were un-shared (SBX-05).

**Escalate to PE if:** Secure Sync is on, the sandbox is shared, and data still doesn't appear, or the sandbox is linked to an LMS and calls return 401. The Data Browser message "Your application's scopes do not give you permission to use the data browser" means no Secure Sync. Roster scopes can't be given to an SSO-only app.

Entries used: SCP-02, SCP-08, SBX-05

## A11
**What's going on:** First check the case history for a message from PE saying it created the production app. Since the app is certified but not showing in production, treat it as affecting the partner and escalate.

**If PE has NOT said a production app was created:** run the paying-customer check (not paying goes to application-interest@clever.com). If they are paying, send:
> Thanks for reaching out! Certifications can take up to two weeks to review. Let me check in with our Partner Engineering team for next steps or an update.

Then post in #partner-engineering with the Salesforce case number, partner name and app ID, the contact, and a note they have questions about their certification. Don't send a form or survey link, and don't promise a date.

**If PE HAS created the production app, send:**
> Congratulations on completing certification! To finish going live:
> 1. In your Clever dashboard, use the app dropdown at the top of the page to switch to your new production app.
> 2. Note your production credentials. They're different from your dev app's. Please keep your client secret private and never send it by email.
> 3. Update the redirect URIs on the production app as needed.
> 4. Click through the go-live steps. Once you finish, your app is live.
>
> Your dev app is for testing only, and districts can't find it. Keep it for testing changes before you release them. This guide covers connecting with districts: https://support.clever.com/hc/s/articles/215120718

**Escalate to PE if:** no production app shows in the dropdown, scopes weren't carried over from dev, districts can't find or invite the app (it may still be in developer mode or not set to searchable, SCP-04; ask for the client ID and whether it's dev or production), or the partner wants invite-only changed. Also check the go-live wizard was completed. Don't promise how long going live takes.

Entries used: CRT-03, CRT-01, SCP-04

## B1
**What's going on:** This is a certification status question. CS doesn't assess the review. The only timeline CS gives is "up to two weeks".

**Steps:**
1. Check the Paying Customer tag under Scopes & Tags in Hall Monitor. If not paying, route to application-interest@clever.com.
2. If paying, send:
> Thanks for reaching out! Certifications can take up to two weeks to review. Let me check in with our Partner Engineering team for next steps or an update.
3. Post in #partner-engineering with: the Salesforce case number, the partner name and app ID, the contact you're working with, and a note that they have questions about their certification.

**Notes:** Don't promise a specific date, don't send a survey or form link, and don't ask for passwords or secrets. If PE's last message is waiting on the partner (often for SSO test users' Clever IDs), say so in Slack. PE sets the right form and decides what blocks certification.

Entries used: CRT-01

## B2
**What's going on:** New standalone SSO-only integrations aren't offered. New SSO comes as part of SSO + Rostering under Clever Complete, which needs an agreement. (Existing SSO-only apps are still supported, so check whether this company already has one.) Never call SSO free or its own tier, and don't quote prices, fees or timelines.

**Steps:** send the starting docs, check whether they're a paying customer (Paying Customer tag in Hall Monitor; a company with no Clever account isn't). Not paying: loop in application-interest@clever.com, then solve the case. Paying: loop in their ASM.

**Ready-to-send reply:**
> Hi there, thanks for your interest in building with Clever! A good place to start is our developer docs: https://dev.clever.com/docs/getting-started, https://dev.clever.com/docs/what-is-clever, https://dev.clever.com/docs/integration-types and https://dev.clever.com/docs/certification-overview.
>
> New integrations with Clever are part of Clever Complete, which includes SSO and rostering together. I'm looping in our team who can go over the agreement and next steps with you, and they'll follow up soon.

**Notes:** Older cases told SSO-only partners to sign up for a dev account, build and submit for certification. That's outdated. Don't speculate about whether they can start building before an agreement is signed. Don't say how many apps a partner can have.

Entries used: CRT-06, CRT-12

## B3
**What's going on:** Clever isn't certifying new Library integrations. Don't offer it as an option, don't send Library certification docs, forms or a DSA, and don't promise a timeline. For any Library question, ask in #partner-engineering.

**Steps:**
1. Run the paying-customer check. Paying customers go to their ASM. Everyone else goes to application-interest@clever.com.
2. Collect: case number, partner name and app ID, the contact, whether they have a Library integration today, and anything they say they've already submitted (plus any other names or emails they may have used).
3. Post in #partner-engineering and wait for PE. Never offer an exception (PE decides).

**Ready-to-send reply:**
> Thanks for reaching out! Clever isn't currently certifying new Library integrations, so I'm checking with our Partner Engineering team on your situation and will follow up. I can't promise a timeline, but I'll update you here as soon as I hear back.

If they're a new partner, the Clever Complete path may come up. Follow the new-partner routing (CRT-06). Don't call Library "free" or discuss cost. If they already have a live Library app and want to test changes, see LIB-03 (PE only).

Entries used: LIB-04, CRT-06, LIB-03

## B4
**What's going on:** This is a new-partner routing question. Send the starting docs, then check whether they're a paying customer.

**Ready-to-send reply:**
> Hi there, thanks for your interest in building with Clever! A good place to start is our developer docs: https://dev.clever.com/docs/getting-started, https://dev.clever.com/docs/what-is-clever, https://dev.clever.com/docs/integration-types and https://dev.clever.com/docs/certification-overview.
>
> New integrations with Clever are part of Clever Complete, which includes SSO and rostering together. I'm looping in our team who can go over the agreement and next steps with you, and they'll follow up soon.

**Routing:** check the Paying Customer tag under Scopes & Tags in Hall Monitor. A prospect with no Clever account isn't a paying customer. Not paying: loop in application-interest@clever.com, then solve the case. Paying: loop in their Application Success Manager.

**Notes:** Optional extras for you are the schema sheet and pricing page, but partner replies may only link dev.clever.com, support.clever.com or schools.clever.com, so don't put those in the reply. Don't quote prices, timelines or app counts. Don't say how many apps they can have. Don't offer SSO-only or Library as an option.

Entries used: CRT-06, CRT-12

## B5
**What's going on:** Don't give any date. Clever hasn't published a sunset policy. You can say v2 is deprecated, new apps can't use it, existing apps are still supported, and upgrading to v3 is recommended. Clever is moving partners from v2 to v3 and no longer certifies apps built on v2.

**Ready-to-send reply:**
> Thanks for asking! v2 is deprecated, so we recommend moving to v3. Existing apps are still supported, but I can't give you a date. Our upgrade guide covers the changes, and our Partner Engineering team can help you plan the move. If you share your target timeline, they can also pull a report of the v2 calls your app is making. Upgrade guide: https://dev.clever.com/docs/api-v3-upgrade-guide

**Facts you can relay:** districts don't need to do anything for the app to move to v3 (they may need to sync data only if the partner wants new v3 fields); the same credentials and tokens keep working with no new app registration; v2 and v3 can run side by side during migration, so test in a dev app first and don't turn off v2 calls until the same calls work on v3; contacts are `users?role=contact` on v3.

**Escalate to PE (and loop in the ASM) if:** they want a v2 call report, migration help, written confirmation of any date (say you can't provide one), or ask whether a v4 is planned (don't repeat anything as a promise). If older cases or emails mention dates, they're not reliable. Don't repeat them.

Entries used: VER-03, VER-04

## B6
**What's going on:** No. Partners must use the Clever ID as the primary key for users. An app that keys on email or another field won't be certified until it switches. This is a blocker, not a caveat. Older reviews accepted email-keyed apps with caveats, but that's outdated.

**Ready-to-send reply:**
> Thanks for asking! To be certified, your app needs to use the Clever ID (the `id` field) as the primary key for users. It's a globally unique identifier for every Clever user. We can't certify an app that keys users on email or another field, so that would need to change first. When someone logs in who already has an account on your side, most partners match on the Clever ID and email and offer the user a chance to merge. If you need to keep accounts separate, store a flag for which sign-in method created the account. Let me check in with our Partner Engineering team on next steps. https://dev.clever.com/docs/users

**Steps:** post in #partner-engineering (case number, partner name and app ID, contact, and that they key on email). If they're heading for certification, run the paying-customer check (not paying goes to application-interest@clever.com), and use the standard "up to two weeks" reply if they're asking about review status. Don't send a form link. Don't promise the app can be certified.

Entries used: SSO-08, CRT-04, CRT-01

## B7
**What's going on:** This is a security issue. Notify #partner-engineering immediately so PE can rotate the credential. Don't wait for the case to be transferred. If Clever sees a secret shared over email, it resets it within 24 hours, and the partner should rotate it right away.

**Steps:**
1. Post in #partner-engineering now with the case number, the app and the partner. Don't repeat the secret.
2. Redact the secret from the email thread if you can.
3. Ask which app it is (dev or production) and the client ID.
4. Don't ask them to send any secret, token or authorization header again.

**Ready-to-send reply:**
> Thanks for reaching out. For security, please don't send client secrets, tokens or authorization headers over email. Because the secret was shared, please rotate it right away. Your app admins can roll the client secret in the API Credentials section of the Settings page in your app dashboard (it works for dev and production apps). Once you've updated your code with the new value, users won't need to do anything. If you can't roll it yourself, or district tokens also need resetting, send us the client ID (dev and/or production) and the district IDs and our Partner Engineering team will help.

**Escalate:** always. District-app tokens can only be reset by PE. If you can't roll it, an existing admin can adjust permissions on the app dashboard's Team page.

Entries used: SSO-09, SSO-03, ROS-08

## B8
**What's going on:** There's no limit on the number of sandbox districts per app. Older cases or docs mentioning a limit of 5 are outdated. Custom sandboxes are a Clever Complete / Secure Sync feature, and CS doesn't have to check eligibility first. PE creates the sandbox, so CS only collects details.

**Steps:**
1. Check what already exists. The partner may have an unused sandbox connected, and PE sometimes connects an existing one instead.
2. Collect: dev app client ID, admin first and last name, a unique admin email not used on any other Clever account (aliases like name+sandbox2@company.com are fine), the sandbox name (named `#DEMO <app name>`), and whether Clever should load demo data or leave it empty for the partner's own uploads.
3. Transfer to PE. Don't promise timing.

**Ready-to-send reply:**
> Happy to help! We can set up another sandbox district for your app. Could you send us (1) your dev app's client ID, (2) the admin's first and last name, (3) an email address that isn't used on any other Clever account (an alias like name+sandbox2@company.com works), and (4) whether you'd like us to load demo data or leave it empty for your own uploads? You'll receive an email with instructions on how to complete the signup once it's created. More on sandboxes: https://dev.clever.com/docs/custom-sandbox-districts

**Notes:** A new sandbox starts empty unless PE loads demo data. Sandboxes can't be converted to production. If the request involves a production app, escalate right away. Don't advertise limits on number of apps.

Entries used: SBX-03, SBX-02

## B9
**What's going on:** District requests for a developer app ("District App") are handled case by case. They're rare and aren't advertised, so don't offer this proactively, but follow this process now that they've asked. CS needs CS Manager approval, then PE creates the app.

**Steps:**
1. Check the requester is a **Clever Admin (owner)** on the district's team members page. If not, loop in an owner for permission. Never grant it to a non-owner.
2. Send the first reply (below). Find out how they plan to use it.
3. Loop in the district's School Success rep (Info tab of the district in Hall Monitor) and get approval from a CS Manager.
4. After approval, have the district sign up for a developer account at https://apps.clever.com/signup using an email not linked to any district admin account (Textblaze `.apiaccess`). Tell them the dev account can only connect to their own district.
5. After they've signed up, post in #partner-engineering with the dev app ID, district ID, approving CS Manager and proof of approval (for example a Slack link), and the case number. PE follows up in the thread.
6. When PE is done, tell the district the account is ready and set expectations on support (reply below).

**First reply:**
> Thanks for reaching out to Clever Support! Clever provides API access to districts by creating a "District App". When you share data with this District App (as you do with any other app), you can then log in as the District App to use our API. Clever provides this access for internal district use only.
> To get started, could you confirm how your district plans to use this app? I'll also need the name and email of the District App administrator. This email can't be the same one you use to log into your district's Clever account. An alias or a different address works.
> Once I have this, I'll confirm with our team that they can create an app for your district. Please note that Clever isn't able to provide tech support on District App accounts.

**Once PE is done:**
> At this time, Clever Support is unable to provide troubleshooting assistance outside of the resources listed in the developer documentation. Please refer to our documentation on developer access for assistance with the Clever API: https://dev.clever.com/docs/district-getting-started

**Deny if:** the requester isn't an owner (or the owner declines), or the district wants to connect the app to other districts. That needs a Secure Sync contract and a production account, so route them to application-interest@clever.com. To find out, you can ask how they hope to use the Clever data. Never promise API troubleshooting.

Entries used: SBX-09

## B10
**What's going on:** The archive doesn't cover this. District requests for a sandbox aren't a Developer Support topic and PE doesn't handle them, so don't transfer this to PE or create a sandbox yourself, and don't make up a process.

**What to do:**
- Handle it through the process your team uses for district requests, or ask a teammate who knows it.
- First ask the district what they're trying to do. If it turns out they want their own developer app or API access to their own data, that is a different, rare request with its own approval process (SBX-09). Don't offer it proactively, and follow that process only if that's what they want.
- If they're really a company that builds an app for districts (an app partner), then a sandbox comes with a developer account. Check whether they're a paying customer: paying customers go to their ASM, everyone else to application-interest@clever.com.

**Possible clarifying reply:**
> Thanks for reaching out! Could you tell me a bit more about what you're hoping to do with a sandbox, for example whether you're testing an app, training staff, or something else? That will help me point you in the right direction.

Entries used: SBX-09, SBX-03 (no entry directly covers district sandbox requests)

## C1
**What's going on:** If the partner means webhooks in general: Clever doesn't support webhooks. Apps pull data from the API at least once a day or at sign-in, and the Events API is the closest option for changes (Events are Secure Sync only, and usually arrive within about 15 to 30 minutes). That's PE's wording and the entry is still Draft, so have PE review before promising anything new.

**Ready-to-send reply if it's about webhooks generally:**
> Thanks for the question! Clever doesn't use webhooks. Applications pull district data from our API, at least once a day or when a user signs in, and how much more often is up to you. If you're building on Secure Sync, the Events API is the closest option for picking up changes in between. Docs: https://dev.clever.com/docs/events-api. If it helps, I'm happy to ask our Partner Engineering team to take a look at your plan.

**If they mean webhooks or notifications for some specific feature or product that isn't in the archive:** the archive doesn't cover it. Don't guess at endpoints, settings or behavior. Escalate to Partner Engineering and collect: partner name, client ID (dev or production), which feature they mean, what they're trying to achieve, what they've tried, any docs links they're following, and the case number.

Entries used: CRT-07, ROS-10, ROS-14

## C2
**What's going on:** A production app returning wrong data for several districts is a partner-affecting production problem, so recommend escalating to PE, even though some entries cover parts of it. This is wrong data, not missing data, so the missing-data triage-first rule doesn't apply. Because it's several districts, it may be integration-level.

**Collect before the handoff (so PE can act in one step):**
- client ID (production), partner name and the contact,
- the district IDs affected, and whether any districts look correct,
- which records and fields are wrong, with a few example record IDs, what they see versus what they expected,
- API version and endpoint, and when it started,
- whether the Data Browser shows the same wrong values,
- anything you've ruled out (district sharing, the app's Data Access settings and filters, sync status).

**Quick checks while you collect (don't hold the handoff for these):** whether it's field availability or source data (the district's SIS or a stale sync, see ROS-07 for district state), a v3.1-only field on an app without v3.1, or the primary school versus `schools` difference (SCP-09). Clever passes text values like names exactly as the district's SIS provides them (ROS-11).

**Ready-to-send reply:**
> Thanks for letting us know, and I'm sorry for the trouble. I'm sending this to our Partner Engineering team now. To help them move quickly, could you send your production client ID, the IDs of the affected districts, a few example records (IDs, the value you're seeing, and what you expected), the API version and endpoint you're calling, and when you first noticed it? Please don't include any secrets or tokens.

**Escalate:** transfer the case to PE and post in #partner-engineering. Don't promise a cause or timeline.

Entries used: ROS-05, ROS-06, ROS-07, ROS-11, SCP-07, SCP-09

## C3
**What's going on:** Here's how to handle Library questions. Clever Library is still a product, and existing Library apps are supported, but Clever isn't certifying new Library integrations.

**What to do:**
- For any Library question (new requests, certification, troubleshooting, reviews, analytics, unlaunching), ask in #partner-engineering so PE can review it.
- Don't send partners Library certification docs, forms or a DSA, and don't promise a timeline or an exception.
- Don't offer Library as an option to a new partner. Run the paying-customer check (paying goes to the ASM, everyone else to application-interest@clever.com).
- Don't call Library "free" or discuss cost.
- Older cases that sent the Library certification guide or promised gallery placement are outdated.

**If you need to reply to a partner:**
> Clever isn't currently certifying new Library integrations, so I'm checking with our Partner Engineering team on your situation and will follow up. I can't promise a timeline, but I'll update you here as soon as I hear back.

Entries used: LIB-04, LIB-03, CRT-06

## C4
**What's going on:** No, don't reuse that reply. The older 2025 case is outdated. New standalone SSO-only integrations are no longer offered, and older cases that told partners to build and certify SSO-only aren't the process now. New SSO comes as part of SSO + Rostering under Clever Complete, which needs an agreement. Existing SSO-only apps are still supported, so check whether this partner already has one. Also don't send survey or form links, and don't call SSO free.

**What to do:**
1. Run the paying-customer check. Paying customers go to their ASM. Everyone else goes to application-interest@clever.com.
2. If they have an existing app and ask about certification status, use the standard "up to two weeks" reply and post in #partner-engineering with the case number, partner name and app ID, and contact.
3. Don't promise they can reuse or convert an app (PE decides).

**Ready-to-send reply (new partner):**
> Thanks for reaching out! Clever isn't offering new standalone SSO-only integrations. New integrations come as part of Clever Complete, which includes SSO and rostering together. I'm looping in our team who can go over the agreement and next steps with you. In the meantime, these docs are a good place to start: https://dev.clever.com/docs/integration-types and https://dev.clever.com/docs/getting-started.

Entries used: CRT-06, CRT-01, CRT-11, SSO-11

## C5
**What's going on:** There isn't enough detail to match this to anything in the archive, and I can't guess at Clever behavior. You need to ask the partner some clarifying questions first.

**Ask the partner:**
- What exactly isn't working: logging in with Clever (SSO), an API call, missing or wrong data, a sandbox, or the dashboard?
- Which app (client ID) and whether it's dev or production.
- The exact error message or HTTP status, plus the full URL or endpoint (no secrets or tokens).
- Which district and which user, if it's data or login related.
- When it started, whether it ever worked, and what changed.
- Screenshots or a short screen recording.

**Ready-to-send reply:**
> Thanks for reaching out, and sorry you're running into trouble. To help us track this down, could you tell us: (1) what you're trying to do and what happens instead, (2) the exact error message or status code, (3) your app's client ID and whether it's your dev or production app, (4) the district ID and a sample user if it's about a specific district, and (5) when it started and whether it worked before? Please don't send passwords, secrets or tokens. A screenshot or short screen recording is helpful too.

**Escalate:** if you learn it's a production app, data that looks wrong in production, or anything security-related, recommend escalating to PE. Once you have details, ask me again with the specifics.

Entries used: none

## C6
**What's going on:** Every entry is a Draft until PE reviews it, so be careful about promising anything that rests on an unconfirmed note. You didn't say which entry, so here's how to handle it. Check the entry's "Notes / conflicts" section. If a statement is marked unconfirmed, untested, or "PE to confirm", relay it only as general background or don't relay it at all, and don't promise it as a firm commitment.

**Examples of unconfirmed items in the archive:**
- Rotating a client secret: "users see no change" came from PE once and wasn't tested (SSO-09).
- Whether districts must re-authorize after sensitive demographic scopes or v3.1 are enabled wasn't answered, so don't say either way (SCP-01).
- What the district state value `requires-review` means isn't covered, so don't explain it (ROS-07).
- Whether admin lists can be bulk-uploaded for dashboard SSO, or PE's turnaround time (DSH-01, DSH-04).
- Sandbox ownership transfer without written approval is "PE to confirm" (SBX-01).

**What to tell the partner:** use hedged wording and loop in PE:
> Based on what we've seen so far, [the general behavior]. I want to double-check this with our Partner Engineering team before confirming, and I'll follow up on this thread.

**Escalate:** post the question in #partner-engineering with the case number and exactly what the partner wants confirmed. If you tell me which entry or topic you mean, I can be more specific.

Entries used: SSO-09, SCP-01, ROS-07, DSH-01, DSH-04, SBX-01
