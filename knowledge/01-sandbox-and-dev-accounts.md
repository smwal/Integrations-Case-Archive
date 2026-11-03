# Sandbox districts & developer accounts

Covers these Salesforce buckets: App Settings/Config > Sandbox District, and App Settings/Config > New Dev Account.
Source: resolved integrations@clever.com cases. Every entry is a draft until PE has reviewed it.

---

## SBX-01 · "I can't get into my sandbox district's admin account. Can you reset it or give me admin access?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Triage only. PE adds the admin.

**When to use this**
- The developer says their sandbox admin account was "deleted" or doesn't work, or asks for a password reset on a sandbox account.
- The developer asks for "admin" or "write" access to their sandbox district, for example to edit data, add users, use SFTP, or test school admin and district admin roles.
- The person who owned the sandbox has left the company.

**Answer / steps**
1. Ask for the **sandbox district ID** and the **email they're logging in with**. Then ask which of these they're trying to reach. Developers often mix them up.
   - The **app developer dashboard**, where they manage the app.
   - The **sandbox district's admin view**, where they act as a district admin.
2. If they mean the sandbox district admin, look up the sandbox and check whether that email is listed as an admin.
3. **If the email is listed**, send:
   > Your account is still an admin on the sandbox. To log in:
   > 1. Go to your sandbox's district login page (clever.com/in/<district-shortname>).
   > 2. Click the **District Admin** login link in the corner of the login window. This is the step people most often miss.
   > 3. If your invite email never arrived or you still can't get in, use "Forgot your password?" on the admin login.
   > 4. If none of that works, please send a short screen recording of the error.
4. **If the email isn't listed**, collect the details and route to PE to add it:
   > We can add you as a district admin in your app's sandbox district. Could you send us (1) the sandbox district ID, (2) your first and last name, and (3) an email address you have access to that isn't already used anywhere in Clever? Once we've added you, you'll get an email to finish setting up the account.
   - The email can't already be used on any other Clever account, including the app dashboard login. Clever doesn't allow two admin-level accounts with the same email.
   - An alias is fine, for example name+sandbox@company.com. For several sandboxes, use one alias each (`+sandbox1`, `+sandbox2`, …).
   - If the partner tried to add an admin themselves and got **"This district admin already exists"**, the email is already in use. PE re-adds it with an alias.
5. Once they're an admin, they can add more admins themselves. See the Help Center article "Clever Roles: Clever Admin".
6. **The person who has app dashboard access but not sandbox access** can also be added under **Team > Add Team Member** on the app dashboard (that's dashboard access, not sandbox admin). CS can do this: in Hall Monitor, impersonate an existing team member and add the person on the Team page (see SBX-14).

**Escalate if…**
- The sandbox has no working admin, or the developer wants a new or second sandbox, or a sandbox renamed.
- The sandbox owner has left and the partner wants **ownership** moved to someone else. PE adds a new admin and transfers sandbox ownership.
- The developer says the sandbox was deleted, or that it shows "not sharing any data" with the dev app. PE can reload the default data and fix sharing.
- They actually need access to the **app developer dashboard**, for example an app is missing from their dropdown. PE may need to combine admin emails across apps.

**Notes / conflicts**
- **Ownership transfer:** in later cases PE transferred *sandbox* ownership directly (03040735). The *production-district* ownership-transfer process (written approval from a leader at the organization), which one earlier case used, doesn't appear to be needed for sandboxes. PE to confirm.
- **Freeing up an email:** a sandbox admin can delete a sandbox from Profile > Delete district account, which frees the email. CS can't deactivate an admin account that belongs to a *real* district; that district's own admins have to do it.
- Never set a password and send it by email. Let the invite or "Forgot your password?" flow handle it. One older case had a sandbox password emailed in plain text.
- Live district data can't be copied into a sandbox.

**Links:** Sandbox login: clever.com/in/<district-shortname>, then the District Admin link · Help Center: "Clever Roles: Clever Admin" · https://dev.clever.com/docs/testing-your-sso-integration#sandbox-district
**Source cases:** 03117174, 03103581, 03116901, 03112328, 03095389, 03116602, 03086408, 03116930, 03000754, 03059271, 03086630, 03088373, 03087546, 03089754, 03090010, 03056958, 03041971, 03040735, 03027431, 03113501, 03089467, 03075437, 02986833, 03074141

---

## SBX-02 · "How do I add or change test data in my sandbox, and will changes produce Events?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Yes, for most questions.

**When to use this**
- The developer wants different test data: more users, multi-school users, school admins, new terms, demographic values, graduation year, and so on.
- Values look wrong after an upload, for example every student's grade shows as "Other".
- The developer asks for an API to create sandbox data, or for SFTP credentials.
- The developer asks whether sandbox edits produce Events API events.

**Answer / steps**
The developer has to be logged in as a **sandbox district admin**. If they aren't, use SBX-01 first. A default sandbox can't be edited from the app side: the Sharing tab is read-only, and "Request sync" only sends a request to Clever.

> You can change the data in your sandbox yourself while logged in as its district admin.
>
> **Option A: edit the files.**
> 1. Download the current files from https://schools.clever.com/sync ("Download SIS file" next to each record type).
> 2. Make your changes.
> 3. Save the files with these exact names: `students.csv`, `schools.csv`, `teachers.csv`, `staff.csv`, `enrollments.csv`, `sections.csv`.
> 4. Upload them again at https://schools.clever.com/sync/upload and complete the Field Mappings.
> 5. Allow about 15 minutes for the data to sync, then check https://schools.clever.com/data-browser
>
> **Option B: add custom data** at https://schools.clever.com/custom-data/browse/students
>
> **Option C: SFTP**, for bulk or repeated loads. Your SFTP credentials are at https://schools.clever.com/sync/settings
>
> When you upload by hand, check the **Field Mappings** screen. Make sure every column you need (for example Grade) is mapped to a Clever field. Unmapped columns won't come through.

**Common edits:**
> - **Terms:** edit `term_name`, `term_start` and `term_end` in `sections.csv`.
> - **Courses:** these are a property of the section. Edit `course_name` in `sections.csv`. For subject-based logic, use the section `subject` field rather than section or course names, since real districts name these inconsistently.
> - **Graduation year:** `graduation_year` is a standard `students.csv` field. See the SFTP file spec: https://schools.clever.com/files/clever-sftp.pdf
> - **Demographics** (for example ethnicity): add the column to `students.csv` and re-upload.

There's **no API for creating sandbox data**. Use CSV upload or SFTP.

**Events:**
> To see Events for the changes you make in the sandbox, turn on the Events API in your app's settings: apps.clever.com/<app-shortname>/settings/integration. Events can't be backfilled. To generate Events, either edit records in custom data, or download the CSVs, edit them (for example enrollments or term dates), and re-upload. Once the sync has finished (usually within about 15 minutes), the events should be available on the Events API. Docs: https://dev.clever.com/docs/events-api#syncing-events

For edge cases and ready-made Events, suggest connecting the dev app to Certification ISD instead (see SBX-03).

**Which field is the district's own ID?**
> Use the `sis_id` field. It matches the district's student information system. The long Clever ID is a separate ID that Clever creates.

For a newly created, empty sandbox, PE can load the standard demo data set into it. The developer can also upload their own files.

**Sample datasets**:
> - Default (5 schools, 33 sections, 41 users): https://assets.clever.com/devdocs/default_sandbox_data.zip
> - Larger district, Clear Water ISD (250 schools, ~286K users): https://www.clever.com/wp-content/uploads/2026/04/Clearwater_Data.zip
> - Certification ISD edge cases: https://assets.clever.com/devdocs/CertificationISD.zip
>
> Each upload must include `schools.csv`, `students.csv`, `teachers.csv`, `sections.csv` and `enrollments.csv`. `admins.csv` is optional.

Clever uses the same base data for its sandbox districts; only the size varies.

Events are only available to **Secure Sync** apps.

**Escalate if…**
- Field Mappings look correct but the data is still wrong after an upload. Also see SBX-04.
- The developer needs a feature turned on in the sandbox, such as Badges for QR-code login.
- API calls fail with a version error, for example a 403 "version not supported" on v3.1. PE has to turn on v3.1 for the dev app (VER-01).

**Notes / conflicts**
- **Confirmed:** CSV re-uploads produce Events (03014763), and custom data edits produce Events (tested Oct 21, 2026).

- The shared read-only Events test district can be connected to a dev app. It has one token per connected app (dev and production each have their own), viewable through the app dropdown (03054264).
- The shared Certification ISD can't be edited on request, since many partners use it. Use your own sandbox (02985562).
**Links:** https://schools.clever.com/sync · https://schools.clever.com/sync/upload · https://schools.clever.com/sync/settings · https://schools.clever.com/files/clever-sftp.pdf · https://dev.clever.com/docs/events-api#syncing-events · https://dev.clever.com/docs/clever-api-v31-data-schema · https://support.clever.com/hc/s/articles/202494408 (uploading data) · https://support.clever.com/hc/s/articles/360035757332 (custom data) · https://support.clever.com/hc/s/articles/220213907 (Clever Badges)
**Source cases:** 03080748, 02985562, 03007901, 03054264, 03117174, 03112526, 03112328, 03095389, 03083069, 03038842, 03079898, 03007225, 03052779, 03010802, 03089754, 03097971, 03098276, 03014763, 03056020, 03079802

---

## SBX-03 · "My sandbox disconnected or is stuck on 'Requested'. How do I fix it, and can I get another sandbox?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Triage only. PE makes the change.

**When to use this**
- The developer received the automated "<sandbox> disconnected from <app>" email, or removed the app from the sandbox and can't find it to reconnect.
- After reconnecting, the sandbox shows **"Requested"** instead of connected and has to be approved by hand.
- The developer wants a second (or third, or fourth) sandbox, or a connection to Certification ISD.

**Answer / steps**
1. Collect what PE will need:
   - the dev app's **client ID**,
   - the **sandbox district ID**,
   - for a new sandbox: the admin's **name**, a **unique admin email** that isn't used on any other Clever account (aliases are fine), a sandbox name, and whether Clever should **load demo data** or leave it empty for the partner's own uploads.
2. **Check what already exists.** Partners often already have unused sandboxes connected to the app. In several cases PE connected an existing one instead of creating a new one.
3. **"Requested" instead of connected:**
   > This happens because your dev app still has to approve the district's connection request. You can approve it under pending requests in your developer dashboard. If you'd rather have it happen automatically, for example for automated tests, we can set your dev app to auto-approve district requests. Please send us your dev app's client ID.
4. **Disconnected sandbox, or the app was removed from the sandbox:**
   > Your app isn't searchable from the district side (that's intentional), so you'll need to either invite the sandbox from your app dashboard, or we can reconnect it for you.

   If the dashboard invite doesn't find the district, route to PE to reconnect.
5. **Extra sandbox:** custom sandboxes are a **Clever Complete / Secure Sync** feature, and the partner's app is expected to be on it. CS doesn't need to verify that before collecting the details in step 1. There's **no per-app limit**, so no limit check is needed either.
   - Name the sandbox `#DEMO <app name>`.

   Then choose one of these two options.
   - Clever adds them as an admin on their existing sandbox.
   - Clever creates a new sandbox with them as admin. A new sandbox starts empty, and PE can load demo data into it.

   Each sandbox needs its own unique admin email. Once it's created:
   > You'll receive an email shortly with instructions on how to complete the signup process.
6. A sandbox **can't be converted into a production** environment. Production is a separate request.

**Certification ISD:** this is a shared, **read-only** sandbox (it appears as "#DEMO Certification ISD - Events") for edge cases and Events testing. Partners request it at integrations@clever.com and include their dev app name. PE connects it, often the same day.
> This demo district is maintained by Clever and we can't provide direct access to it, because many partners rely on its current behavior.

**Escalate if…** Every request on this card needs PE, except reconnecting through the partner's own dashboard invite. CS's job is to collect the details in step 1 and set expectations. If the request involves a **production** app, escalate right away.

**Notes / conflicts**
- **Self-reconnect:** partially answered. The partner can't reconnect from the district side because the app isn't searchable. The dashboard invite is offered, but in the case reviewed it didn't find the district and PE connected it (03075819).
- **Eligibility:** CS doesn't need to check Clever Complete / Secure Sync before creating a sandbox for a partner.
- **Library partners:** in one case CS offered a custom sandbox to a Library partner and said it would "work with your Library integration" (03068720). Library topics go to #partner-engineering instead.
- **District requesters:** this entry is for app partners only. A district asking for its own sandbox isn't a Developer Support topic. A district asking for a dev app goes to SBX-09.
- No case quoted a per-app limit; partners had 4–6 sandboxes on one app. That matches the current rule of no per-app limit.
- A partner with only a staging connection who wants a dev environment may only need a second sandbox, not a new app. In one case PE created a new demo sandbox with a new admin alias and connected it (03070562).

- PE can disconnect a sandbox district from a production app when test users can't log in or the production app shows in the sandbox (03003579).
**Links:** https://dev.clever.com/docs/custom-sandbox-districts · https://dev.clever.com/docs/sync-testing · https://support.clever.com/hc/s/articles/215120718
**Source cases:** 03089255, 03003579, 03052197, 03108351, 03086226, 03112328, 03088373, 03056958, 03052628, 03042252, 03089107, 03014763, 03056020, 03075819, 03113501, 03068720, 03089467, 03075437, 03057922, 03090010, 03070562

---

## SBX-04 · "I uploaded data or added custom data, but it isn't showing up (or my test user suddenly can't log in)"
**Status:** Draft, needs PE review · **Resolvable by CS:** Triage only. PE runs or refreshes the sync.

**When to use this**
- The upload "succeeded", but no data appears.
- Custom data records stay greyed out (unprocessed), can't be added to sections, or show "School not accessible".
- A `#DEMO` test user that used to work now fails SSO.

**Answer / steps**
1. **Wait first.** Syncs usually take about 15 minutes, sometimes 30–60. Ask them to check https://schools.clever.com/data-browser
2. **Check the upload:**
   - IDs must match across files (for example the school ID in `students.csv` must exist in `schools.csv`).
   - If there's been a full sync in the last ~6 months (180 days), re-uploading any one required file is enough. After that, all five required files (schools, students, teachers, sections, enrollments) need to go up together.
3. Send:
   > Custom data depends on the district's SIS/SFTP data being current, so sometimes custom records aren't processed until a fresh SIS sync has run. If your sandbox hasn't had a full sync in the last 6 months, please re-upload all five district files (schools, students, teachers, sections, enrollments) together to trigger a new sync. If it has, re-uploading any one of them is enough. Please give it 15–60 minutes and check the Data Browser.
4. If it's still missing, collect the **sandbox district ID**, the **client ID**, a few **example record IDs**, a screenshot of the upload confirmation, and the user type. Then escalate.

**Escalate if…**
- Data is still missing after 60 minutes.
- Custom data shows "School not accessible". A bad `schools.csv` may have removed schools.
- SSO fails for a test user that used to work. PE fixed this by refreshing the sync.

**Notes / conflicts**
- Sync history is kept for only 30 days, so PE can't roll back an older bad upload. Options PE offered: export the custom data to CSV, or wipe the sandbox and reload demo data (03059082).
- The 6-month / 180-day re-upload rule is confirmed.
- In two cases the fix was only described as "an issue with the sync" fixed on Clever's side.

**Links:** https://schools.clever.com/data-browser · https://support.clever.com/hc/s/articles/202494408 (uploading data) · https://support.clever.com/hc/s/articles/360035757332 (custom data) · https://schools.clever.com/files/clever-sftp.pdf
**Source cases:** 02975148, 03001007, 03059082, 03064639, 03083667, 03084843, 03094961, 03097971

---

## SBX-05 · "District admins (or staff) disappeared from the API for my sandbox"
**Status:** Draft, needs PE review · **Resolvable by CS:** Triage only. PE re-shares the admins.

**When to use this**
- The Data Browser or `/users?role=district_admin` shows zero admins for the app.
- Often happens after the partner edited sandbox data, or after moving from the dev app to a production app.

**Answer / steps**
> Clever made a change to stop sharing district admins with applications by default. Districts asked for this for security reasons. Your admin account wasn't deleted, it's just no longer shared with your app. We can re-share it for you. Could you send the client ID (dev and/or production) and the sandbox district ID?

Once re-shared:
> `/users?role=district_admin` works without OIDC. The admins just need to be shared with your app.

**Escalate if…** Always. PE re-shares admins, and staff if needed.

**Notes / conflicts**
- Sharing doesn't carry over from a dev app to a production app after certification. Ask PE to re-share for every connected demo district.
- Partner systems should re-provision accounts when they next sync from Clever.
- A dev app that can't see sandbox data can also be a data-sharing or access-tier problem. In one case PE fixed it by changing the app's access tier (03058598).

**Links:** https://dev.clever.com/docs/custom-sandbox-districts
**Source cases:** 03101819, 03090010, 03058598

---

## SBX-06 · "How do I log in as a sandbox test student or teacher? The login says there's no account for that user."
**Status:** Draft, needs PE review · **Resolvable by CS:** Yes.

**When to use this**
- The developer wants test users who can actually log in, for SSO testing or demo videos.
- Username/password login on the district login page fails, or the sample dataset's credentials return "Invalid credentials".

**Answer / steps**
> **Default sandbox:** test users log in with their ID as both username and password:
> - Student: `student_number`
> - Teacher: `teacher_number`
> - School admin: `staff_id`
>
> You can find the test users in your sandbox district from your developer dashboard. In a developer account you can also type a test user's name while viewing the district and use the **Log in as…** button in the top-right corner.
>
> If you see the school picker, you won't find your sandbox by searching school names. Add your sandbox `district_id` to the authorization URL to skip it.

For **custom sandboxes** or users the partner uploaded:
> The sample datasets are for uploading users and schools, not for logging in. Login works according to your sandbox's login method:
> 1. As the sandbox admin, go to https://schools.clever.com/instant-login/settings, click **Add Login Method** and follow the prompts. District Password is the simplest.
> 2. Find your Portal URL at https://schools.clever.com/portal/settings/url and log in as a test user there. An incognito window helps.
> 3. For users you upload, include `student_number`/`teacher_number` (or username and password fields) so credentials exist.
>
> Changes can take 30–60 minutes to process.

**Testing the real login:** "Log in as…" skips the password step. To test the full flow:
> Open your sandbox district's Portal URL (clever.com/in/<sandbox-shortname>) in an incognito window, sign in with the test user's `student_number` / `teacher_number` / `staff_id` as both username and password, then click your app's tile. Sandbox districts don't show up in the school picker, so use the Portal URL or add `district_id` to your authorize URL.

**Test user is asked to set a new password, then sees an error:** the sandbox's password settings have usually changed. PE can reset the password. In one case the reset switched the username to the user's email, and PE had to reset it again so `teacher_number` worked. Also check the user's role: a staff user isn't a teacher and sees different things.

**Escalate if…**
- Login still fails after the login method is set up and the sync has finished. The sync may be stale (see SBX-04).
- It's a custom user added by hand. Ask for the email on that account.
- The redirect URI is `localhost`. PE can't reproduce that, so ask for steps and screenshots first.

**Notes / conflicts**
- Sandbox districts only. Never ask for or send real users' passwords.
- "Log in as…" in the Data Browser is only for Secure Sync apps.
- Sandbox QR badges can be downloaded and printed once the sandbox login works (03021665). To test as a specific user, use "Access as Portal" under Support Tools in the Data Browser (03014479). Never paste shared test credentials into a case.

- Sandbox student usernames and passwords are the student number (the same for teachers and staff) (03059234).
**Links:** https://dev.clever.com/docs/testing-your-sso-integration · https://support.clever.com/hc/s/articles/360000406146 (user impersonation) · https://support.clever.com/hc/s/articles/202483068 (Data Browser)
**Source cases:** 03059234, 03016134, 03084304, 03113886, 03073679, 03041971, 03084843, 03059082, 03098276, 03076815, 03088510, 03000822, 02990697, 02991769, 03014479, 03021665, 03000301

---

## SBX-07 · "Can my sandbox mirror this structure? (sub-districts, empty sections, converting it to production)"
**Status:** Draft, needs PE review · **Resolvable by CS:** Yes.

**When to use this**
- The partner asks for a data shape Clever doesn't support, or wants to promote a sandbox to production.

**Answer / steps**
> **Sub-districts:** Clever doesn't support a district inside a district. The Clever account is the overarching district, and sub-district schools are just schools. We can create a sandbox with the same number of schools instead.

> **Empty sections:** Clever needs at least one enrolled student and one assigned teacher for a section to appear.

> **Sandbox to production:** an existing sandbox can't be converted into a production environment. A new production instance has to be requested.

**Escalate if…** The partner wants a custom sandbox built to match a specific structure. PE builds it.

**Notes / conflicts** Custom sandboxes are for Clever Complete / Secure Sync only, with no per-app limit.

**Links:** https://dev.clever.com/docs/custom-sandbox-districts
**Source cases:** 03072209, 03075437, 03036250, 03089467

---

## SBX-08 · "Can we get a new dev app (or Dev/QA/Stage copies) for another product?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Triage only. PE creates the app.

**When to use this**
- An existing partner wants another dev app for a new product, or extra environments cloned from an existing dev app.

**Answer / steps**
1. Ask which existing dev app to copy and what to name each new app.
2. Ask who should be added as admins. They can add others afterward.
3. If it's a **new product**, loop in the ASM. The contract may need amending before the app can be certified and go to production.

> Happy to help! Which existing dev app would you like us to copy, and what should we name the new app(s)? Who should we add as admins? Once it's created, you'll see it in the app dropdown at the top left of your developer dashboard.

**Escalate if…**
- Always (PE creates the app).
- The partner asks for a **production** app, a Library app, or a specific integration type. Route to PE and the ASM.

**Notes / conflicts**
- Don't offer or create a new SSO-only production app. One older case (03003415) did.
- New integrations need Clever Complete.
- Don't send certification form links. One case (03066914) sent one directly.
- Check for a duplicate request first; in one case the app had already been created from an earlier thread.
- Check that the partner is a paying customer and check in with #partner-engineering for next steps. PE confirms whether it's a new app that needs certification or an update to an existing app.
- PE asks which existing dev app to clone and what to name the copies. A cloned dev app gets a new client ID and secret, keeps the original's district connections, and copies team members (03069447).
- PE can't clone a production app, only a dev app (02986556). Partners can ask for the choice between reusing an existing dev app and getting a new one (03044842).
- Scopes and v3.1, or extras such as Resources/content mapping, are set by PE on request (03068580, 03084751).
- For several apps per customer, see SBX-10. For how dev and production apps relate, see SBX-11.
- In the cases, PE first asked what's different from the partner's existing apps and what the flow needs before creating anything (03026882, 03078542).

- For extra dev apps PE asks which existing dev app to clone from. Scopes can be enabled on dev and QA apps, and production scopes come after certification.
**Links:** https://dev.clever.com/docs/integration-types · https://dev.clever.com/docs/certification-overview
**Source cases:** 02970842, 03067465, 03071230, 03078186, 03112608, 03051058, 03006650, 03003415, 03112110, 03066914, 03069447, 02986556, 03044842, 03068580, 03084751, 03079172, 02977824, 02980740, 03026882, 03078542, 03030653, 03035049

---

## SBX-09 · "We're a district. Can we get our own developer app to use the Clever API?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Triage only. Needs CS Manager approval, then CS asks PE in #partner-engineering.

**When to use this**
- The request comes from a **district**, not an app partner, and they want their own data through the Clever API.
- For app partners, use SBX-08 (dev apps) instead. District requests for a **sandbox** aren't a Developer Support topic and aren't covered here.

**Answer / steps**
Follow the District App process. In short:
1. Check that the requester is a **Clever Admin (owner)** on the district's team members page. If not, loop in an owner.
2. Send the first reply:
   > Thanks for reaching out to Clever Support! Clever provides API access to districts by creating a "District App". When you share data with this District App (as you do with any other app), you can then log in as the District App to use our API. Clever provides this access for internal district use only.
   > To get started, could you confirm how your district plans to use this app? I'll also need the name and email of the District App administrator. This email can't be the same one you use to log into your district's Clever account. An alias or a different address works.
   > Once I have this, I'll confirm with our team that they can create an app for your district. Please note that Clever isn't able to provide tech support on District App accounts.
3. Loop in the district's **School Success rep** (Hall Monitor > Info tab) and get approval from a **CS Manager**.
4. After approval, have the district sign up at https://apps.clever.com/signup (Textblaze `.apiaccess`), and tell them the app can only connect to their own district.
5. Once they've signed up, post the request in **#partner-engineering** with the dev app ID, district ID, approving CS Manager and proof of approval, and the case number.
6. When PE is done, tell the district the account is ready and that support is limited to the developer docs (https://dev.clever.com/docs/api-overview).

**Escalate if…**
- Always: CS Manager approval, then a request in #partner-engineering.
- **Deny** if the requester isn't an owner (or the owner declines), or if the district wants to connect the app to **other** districts. That needs a Secure Sync contract, so route to application-interest@clever.com.

**Notes / conflicts**
- Rare, and not advertised in the Help Center or dev docs. Don't offer it proactively.
- Never promise API troubleshooting to a district. Neither CS nor PE supports districts reaching their own data through the API beyond the docs.

**Links:** https://apps.clever.com/signup · https://dev.clever.com/docs/api-overview
**Source cases:** 02440803, 02594776 (examples from the Guru card)

---

## SBX-10 · "Can I have a separate app for each of my customers or districts?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Triage only, PE makes the change

**When to use this**
- An existing partner (often a reseller) asks for another app for each new customer or district they serve, usually with an app name and logo.
- The partner may call it a "new client application".
- This is not for a first-time partner. For a brand-new partner, send the getting-started docs and use the new-partner routing instead.

**Answer / steps**
1. Check that the partner is a paying customer (Paying Customer tag under Scopes & Tags in Hall Monitor). If they aren't, route them to application-interest@clever.com.
2. If the partner has an Application Success Manager (ASM), loop them in. Contracts and fees are the ASM's area, not CS's.
3. Collect what PE needs: the app name for each requested app, the logo file, and who should be the admin.
4. Post in #partner-engineering with the case number and these details to get next steps, and transfer the case to PE. PE checks whether this is a new app that needs certification or an update to an existing app. Don't create the app and don't promise a timeline.

> Thanks for reaching out! I've passed this to our Partner Engineering team, who set up new apps. They'll follow up here once it's ready. To help them move faster, could you confirm the exact app name(s) you'd like and the logo file(s)?

When PE finishes, PE sends the app details. In older cases PE told the partner to note the new production credentials, update redirect URIs, and click through the go-live wizard so the app goes live.

**Escalate if…**
- Always. Only PE creates apps. After the paying-customer check, post in #partner-engineering for next steps. PE checks with the app team whether this is a truly new app that needs certification or just an update to an existing app.
- The partner isn't a paying customer: route them to application-interest@clever.com.
- The request involves Library or a new integration type: ask in #partner-engineering for Library, and loop in the ASM for a new integration type.

**Notes / conflicts**
- Eight reseller cases were handled by PE directly. PE confirms it checks that the partner is a paying customer, and checks with the app team whether the app is new or an update, so CS should do the paying-customer check and ask in #partner-engineering before promising anything.
- In two cases the partner gave an app name and a logo in the first email, and PE didn't ask for anything else.
- In one case (03091089) only an acknowledgment is visible, so the app creation isn't confirmed.
- Don't quote fees. One case (03079172) showed an ASM explaining extra Secure Sync connection charges; that's the ASM's conversation.

- Users can't be shared between apps. Separate apps can connect to the same districts (02971534). Don't advertise app limits.
- PE won't create a duplicate app name. It asks whether it's the same company or for another name (03022189).
**Links:** https://dev.clever.com/docs/integration-types · https://dev.clever.com/docs/certification-overview
**Source cases:** 03084119, 03022189, 03103682, 02971534, 02968654, 03013350, 03016259, 03033459, 03088263, 03091089, 03096917, 03101956, 03012738

---

## SBX-11 · "Can I have one account for dev/testing and another for production? Where do I create a new app?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Yes (explanation only). PE makes any change.

**When to use this**
- The developer can't find a "create app" button in the dashboard.
- They ask how dev and production apps relate, or whether a dev app can connect to live districts.
- They ask whether there's a limit on the number of apps.

**Answer / steps**
> Here's how it works:
> - You build and test in a **dev app**, which connects to sandbox (demo) districts. A dev app can't be connected to live district data.
> - When your integration is ready, you submit it for certification. Once it's approved, our team clones your dev app into a **production app** that districts can connect to.
> - New apps can only be created by our Partner Engineering team, which is why you don't see a create button in your dashboard.
> - A cloned dev app gets its own client ID and secret, keeps the original app's district connections, and copies over the team members. You can rename the app in your dashboard settings and change redirect URIs under the OAuth settings.
>
> More on certification: https://dev.clever.com/docs/certification-overview

Redirect URIs, if asked:
> Each line in the redirect URI list is a separate URI. Only the first one matters for where users land when they click your app in the Clever Portal. The others can be used in instant login links.

If the partner wants a new dev app or a clone, use SBX-08 to collect the details and transfer to PE.

**Escalate if…**
- The partner wants a new app, a clone, or an extra sandbox: transfer to PE.
- Certification questions: for paying partners, send the standard "up to two weeks" reply and post in #partner-engineering. Never send a survey or form link.

**Notes / conflicts**
- Older cases (02977824, 02980740) had PE send a Secure Sync certification form link directly. That's not CS's job. PE sends the form.
- PE said it can't clone a production app, only a dev app (02986556). In the same case PE copied the production app's scopes onto the new dev copy and said district users can't search for a dev app.
- **App limit:** there is no hard limit on the number of apps, but it is "no limit within reason", not unlimited. Don't advertise this. PE decides case by case. If a partner asks, don't quote a number or say "unlimited". Say PE reviews each request, and collect what PE needs (SBX-08).
- The dashboard menu names in the partner reply come from PE replies (03069447, 02996471), not from a published doc, so PE should confirm them if a partner can't find a setting.

- A dev app is tied to a sandbox. A production account is created after certification, and the separation is required for security (02978264).
**Links:** https://dev.clever.com/docs/certification-overview · https://dev.clever.com/docs/getting-started
**Source cases:** 03081955, 02978264, 02996471, 02986556, 03069447, 03077750, 03070562, 03049483

---

## SBX-12 · "Can we split our app into separate SSO and Rostering apps, or make a region-specific app?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Triage only, PE makes the change

**When to use this**
- A partner wants one app for SSO and another for rostering.
- A partner wants a separate app for a different region or country (for example, a server hosted in another country).
- A partner asks for a "new SSO app" with its own settings.

**Answer / steps**
1. Ask why they want to split the app. PE asked this first in both cases, because a district requirement or a hosting region changes the answer.
2. Explain the redirect URI requirement:

> Clever requires every production SSO app to have its own unique redirect URI. Can your setup support a separate SSO entry point with its own redirect URI for this new app?

3. Ask whether the new app will include rostering (Secure Sync) or be SSO only, and whether they need a new dev app or want to reuse one.
4. Check that the partner is a paying customer, then post in #partner-engineering with the case number and these details to get next steps, and transfer to PE. Don't promise that a new app will be created.
5. Remind them that new production apps go through certification, and PE sends the form.

**Escalate if…**
- Always, to PE. Loop in the ASM if the partner has one.
- The partner wants a **new SSO-only app**: that isn't offered. New SSO comes as SSO + Rostering under Clever Complete. Route to the ASM, or to application-interest@clever.com if they aren't a paying customer.

**Notes / conflicts**
- An older case (03007602) showed PE creating a new SSO-only dev app and sending an SSO certification link. Older cases did that, but it's no longer the process.
- In one case (03082211) PE recommended a region-specific SSO and Rostering app and said other partners had done the same. PE also asked whether the new app would use the same integration methods and code base as the existing one.
- In another case (03073417) PE suggested unpaid users could sign in another way (for example Google) through an external link teachers add to their portal. The final outcome isn't visible, so treat it as an observation, not a rule.
- One thread (03082211) included a deprecation date quoted by an ASM newsletter. Never repeat dates.

**Links:** https://dev.clever.com/docs/integration-types · https://dev.clever.com/docs/certification-overview
**Source cases:** 03059935, 03003835, 03073417, 03082211, 03007602, 03074461

---

## SBX-13 · "We need our internal or test district in production so we can send an invite and record it"
**Status:** Draft, needs PE review · **Resolvable by CS:** Triage only, PE makes the change

**When to use this**
- A partner needs to show the invite flow (for example for a review) and can't find a district to invite.

**Answer / steps**
1. Get the sandbox district ID and the production app ID.
2. Post the request in #partner-engineering on Slack with the case number, the sandbox district ID and the app ID.
3. Update the partner when you hear back. Don't tell them what PE will do or promise timing.

> Thanks for reaching out! I'm checking with our team on how to set this up for your sandbox district and will follow up as soon as I have an answer.

**Escalate if…**
- The partner needs this for certification: post in #partner-engineering and send no form or survey link.

**Notes / conflicts**
- One case.

**Links:** none confirmed
**Source cases:** 03041309

---

## SBX-14 · "Our developer account owner left. Can you add me or my colleague to our app account?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Yes

**When to use this**
- A partner can't get into the developer or app account, or wants another admin or developer added to a dev app or sandbox district.

**Answer / steps**
1. Get the client ID or app name and the exact emails to add.
2. CS adds the new team members. In Hall Monitor, impersonate one of the existing team members, then add the new members on the Team page.
3. Each admin email must be unique across all Clever district and app accounts. If someone already has another Clever account, use an alias (plus addressing is fine).
4. For an app that isn't certified, remind them that real districts can't see it until certification.
5. If you have questions, or anything about the request looks suspicious, ask the ASM in #app-success on Slack to confirm it's a valid request.

> We've added the email addresses you sent to your app account. Each email has to be unique across Clever accounts, so an alias works if someone already has another Clever login. Let us know if you have any trouble signing in.

**Escalate if…**
- The request looks suspicious: ask the ASM in #app-success before adding anyone.

**Notes / conflicts**
- Historically Clever hasn't verified app partners for this, because they usually just need dev access. For adding people on the dashboard Team page, see DSH-03.

**Links:** none confirmed
**Source cases:** 03048589, 02979057, 03012621

---

## SBX-15 · "Can we load our real customer data into a sandbox to test before upgrading them?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Yes

**When to use this**
- A partner wants production district data in a sandbox for a beta or comparison test.

**Answer / steps**
1. Production data isn't allowed in the sandbox (PE). A separate sandbox district can be created for testing instead.
2. For a sandbox shaped like a specific district, see SBX-07.

> We don't allow testing with live production data in the sandbox environment, but we can set up a separate sandbox district for your testing. Let me know your dev app name and what you'd like it to include.

**Escalate if…**
- The partner wants a custom sandbox shaped like a particular district.

**Notes / conflicts**
- The partner asked for 24-hour turnaround. Don't promise timing.

**Links:** none confirmed
**Source cases:** 03074646

---

## SBX-16 · "I filled out the developer sign-up form but got no access, and now it says my email can't be used"
**Status:** Draft, needs PE review · **Resolvable by CS:** Yes (CS troubleshoots; PE has little insight into sign-up form issues)

**When to use this**
- A new partner submitted the sign-up, didn't get sandbox access, retried, and is told the email is already used while "forgot password" says there's no account.

**Answer / steps**
1. Ask which email was used and what the app was named, then troubleshoot it yourself. Don't transfer it to PE, since PE doesn't troubleshoot sign-up form problems.
2. Run the paying-customer check: not paying goes to application-interest@clever.com, paying goes to their ASM.
3. If you can't resolve it, ask a teammate who knows the sign-up process.

> Could you tell me the email address you used on the sign-up form and the app name you entered? I'll look into it for you.

**Notes / conflicts**
- The case shows only PE's first question, not a fix. The archive doesn't have detailed sign-up troubleshooting steps.

**Links:** https://apps.clever.com/signup
**Source cases:** 03082190
