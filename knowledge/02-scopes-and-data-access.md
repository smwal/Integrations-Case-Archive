# Scopes, data access & the data model

Covers these Salesforce buckets: Developer Support > Scopes and Tags, App Settings/Config > Data Access, and Developer Support > API Schema/Data Model.
Source: resolved integrations@clever.com cases. Every entry is a draft until PE has reviewed it.

---

## SCP-01 · "Can you add or remove a scope on our app (Resources, demographic fields, contacts, Secure Sync)?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Triage only. PE changes the scopes.

**When to use this**
- A partner asks to turn on Resources (`read:resources`), sensitive or demographic fields (IEP, FRL, ELL, 504, …), or Secure Sync.
- A partner wants to stop receiving a kind of data, for example contacts coming back from `/v3.1/users`.

**Answer / steps**
1. Ask for the **client ID of each app**, and whether it's dev, production, or both. Demographic fields and Resources need a Secure Sync (Clever Complete) integration. If the partner isn't an existing paying customer, run the paying-customer check first (paying goes to their ASM, otherwise application-interest@clever.com) before promising anything.
2. Check whether the scope is already on, and ask what error or missing data they're seeing. Sometimes the real problem is something else, such as a broken docs link.
3. **Resources:** confirm the client ID(s) and pass to PE. In past cases this was done the same day.
4. **Demographic fields:**
   > I'll check with our team on turning on access to additional demographic fields for your app. A few things to know: this access requires v3.1 of the Clever API, and existing district connections may need a migration before the data appears. Even after it's turned on, you'll only get a field if the district has uploaded that data to Clever and shared it with your app through their field-sharing settings.

   Ask which fields they need. New apps get **v3.0 by default**. v3.1 is turned on by request through PE, and the ASM can be looped in. The v3.1-only fields are `gifted_status`, `section_504_status`, `disability.*`, `home_language_code` and an FRL value of "Other".
5. **Removing contacts:**
   > We can remove the contact scopes from your app so contacts no longer come back from the users endpoint. We'd like to test this on your dev app and sandbox first so you can check the results, then apply it to production.

   Get the partner's written OK before PE changes production.
6. After the change, have the partner check through the Data Browser or a test API call.

**Escalate if…**
- Any scope or field needs changing. Send PE the client IDs, dev/prod, the exact scopes, and the partner's written OK for production.
- Demographic fields need a v3.1 upgrade or a migration.
- The partner asks for a role filter on `/users`. No such filter was confirmed; it was logged as Product feedback. Don't promise it.
- After the scope is on, the partner still gets 401s on LMS Connect or assignment endpoints. In one case the cause was unmatched sandbox data.
- Resources / content mapping: after PE turns it on for the app and enables the Content Assignment tab on the sandbox, an empty `/resources` response can just be a delay of at least 15 minutes (02985247).
- ELL status and home language were turned on the same day for production and dev apps (02981346). For `iep_status`, PE turns on the scope, then the partner adds the field in the app's Data Access settings (optional student fields), and the district must also share it (03048944).
- `read:terms` needs no extra scope when the app already has section access. Terms are optional data, so not every district has them (02999808).
- When a dev app is missing scopes the production app has (such as contacts or Resources), PE can match them (02969135).

- Sensitive demographic scopes and v3.1 are enabled by PE. In one case an extra engineering step was needed before the data appeared (about 2 days). Whether districts must re-authorize wasn't answered, so don't say either way (03087485).
**Links:** https://dev.clever.com/docs/secure-sync-rostering · https://dev.clever.com/docs/integration-types
**Source cases:** 03071230, 03003272, 03074408, 03087485, 03061960, 03066534, 03091929, 03073757, 03075260, 03097289, 03083152, 02985247, 02981346, 03048944, 02999808, 02969135

---

## SCP-02 · "Why can't my dev app see schools, sections, or teachers in the sandbox?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Yes.

**When to use this**
- The developer's dev app or sandbox shows no schools or sections.
- They can't find teachers to "log in as".
- They can't call the Data API.

**Answer / steps**
1. Check the app's integration type.
   - **SSO-only** apps get only the user's Clever ID, name, email and district ID.
   - **Secure Sync** (rostering) scopes add full roster data, district-app tokens and Data API access.
2. If the app is SSO-only, send:
   > Your dev app currently has only Clever SSO scopes, which is the default for development apps. That's why you can see a user's Clever ID, name, email and district, but not school or section data. Secure Sync gives full roster data, district-app tokens and Data API access, and it's part of a Clever Complete agreement. See https://dev.clever.com/docs/integration-types.

   For the next step, check whether the partner is a paying customer. If the partner isn't a paying customer, loop in application-interest@clever.com. If they are, loop in their ASM.
3. Every dev app has a "#DEMO … (Dev) Sandbox" district. To give the partner admin access to it:
   - ask for a first name, last name and an email that isn't used on any Clever account, including the app's own login email,
   - then add them as a Clever Admin.
4. If the app already has Secure Sync and their docs link is broken, send them the Secure Sync rostering page. Mention that the docs were reorganized and to use the left-hand menu.

**Escalate if…**
- The app already has Secure Sync, the sandbox is shared, and data still doesn't appear.
- The partner wants a custom or certification district, or wants to test the Events API.
- The sandbox is linked to an LMS and API calls return 401. PE needs to match the data.

**Notes / conflicts**
- In one case, the Secure Sync link on an app's onboarding page went to a "page not found" docs URL. The Secure Sync onboarding overview is now at https://dev.clever.com/docs/onboarding (district tokens, matching strategies). If partners still report the broken link, flag it to the docs owner.
- Routing: paying customer → ASM; otherwise application-interest@clever.com.
- The Data Browser message "Your application's scopes do not give you permission to use the data browser" means the app doesn't have Secure Sync (03014590). Roster scopes can't be given to an SSO-only app (03010510, 03045885). If the partner wants them, check whether they're a paying customer: paying customers go to their ASM, everyone else to application-interest@clever.com.

**Links:** https://dev.clever.com/docs/integration-types · https://dev.clever.com/docs/secure-sync-rostering
**Source cases:** 03080901, 03074551, 03083152, 03010510, 03045885, 03067001, 03067943, 03014590

---

## SCP-03 · "How do we tell specialists, school admins, or subject teachers apart in Clever data?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Yes, for general guidance. PE for data-model design consults.

**When to use this**
- The partner wants to classify users beyond the standard roles: district vs school specialists, non-teacher staff, grouping teachers by subject or title.
- School admins are coming through as district admins.
- A field (for example home language) is present for most schools but missing for one.

**Answer / steps**
- Staff are linked to **schools**, not sections. `roles.staff.schools` lists their schools. Districts load staff themselves, and staff sub-roles are limited.
- For custom classifications, districts can add **extension fields** on staff (`roles.staff.ext`). The partner turns on `ext` as an optional staff field under **Settings > Data Access**, then gives districts one consistent field name and set of values.
- Staff have no sections, so to get students for a staff member use `/schools/{id}/users?role=student`, not `/mystudents`.
- Teacher **title** is optional free text from the SIS, so coverage is low. For subject, use the section `subject` field, which Clever normalizes to a fixed list.
- If school admins come through as district admins, the district needs to add them as staff (Custom Staff) and share them with the app.
- If one district is missing a field others have, check whether that district sends it in an extension field instead. It then depends on the district's sharing settings.

> Clever doesn't have a built-in user type for that, but there are two good options. Staff records are linked to schools (`roles.staff.schools`), and districts can add a custom extension field (`roles.staff.ext`) to carry your designation. Turn on `ext` as an optional staff field under Settings > Data Access so the values come through. For subject-based experiences, the section `subject` field is more reliable than teacher title, which is optional free text from the SIS.

**Escalate if…**
- The partner wants a new native role or field. Log it as product feedback; don't promise it.
- The partner wants a deep data-model design session.

**Notes / conflicts**
- One person can hold several roles. Check every role on the user record before assigning permissions.
- No outdated guidance found.
- Other ways partners told staff apart: use the teacher `title` field when a district can set matching titles (03019355); use extension fields for roles the data model doesn't have, such as clusters of schools or special-education staff (02976938, 03069734).

**Links:** https://dev.clever.com/docs/extension-fields · https://dev.clever.com/docs/users · https://dev.clever.com/docs/sections · https://dev.clever.com/docs/multi-role-users-in-clever
**Source cases:** 03116679, 03115382, 03051669, 03054310, 02984989, 02976938, 03019355, 03069734

---

## SCP-04 · "Our app isn't showing to districts, or there's no Invite button"
**Status:** Draft, needs PE review · **Resolvable by CS:** Triage only, PE makes the change

**When to use this**
- The partner says districts can't find or search for their app.
- The App Dashboard has no "Invite districts" option.
- A district gets an error when trying to add the app.

**Answer / steps**
1. Ask for the client ID of each app, and whether it's dev or production.
2. If the app is certified or in production but districts can't see it, or the Invite option is missing, transfer to PE. In three cases PE found the app still in developer mode or not set to searchable, and changed it the same day.
3. If it's a new app, check that the partner has completed the go-live wizard in their dashboard. In one case that was still outstanding.
4. After PE changes it, ask the partner to confirm the Invite option shows and the district's error is gone.

> Thanks for letting us know! I've asked our Partner Engineering team to check your app's settings. Could you confirm your client ID and whether this is your dev or production app? Once they make the change, please check that the Invite option shows and let us know if the district still sees the error.

**Escalate if…**
- Any visibility, searchable or production change is needed. PE does it.
- The partner wants the app hidden or invite-only instead (see APP-01).

**Notes / conflicts**
- In two cases (02981410, 03025016) the app was already certified, so certification wasn't the blocker.
- 02981410 asked to be "listed in Library". Don't promise a Library listing. Any Library question goes to #partner-engineering.
- Tag changes may take a few hours to show on the district request page (03051820).

- An app can be missing the "searchable" config. PE enabled it and asked a district admin to retry (03085361).
**Links:** none confirmed
**Source cases:** 03085361, 02977127, 02981410, 03025016, 03016190

---

## SCP-05 · "We only need teachers and sections. Can we stop students being shared?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Triage only, PE tests scope changes

**When to use this**
- A partner only needs teachers, staff and sections, and wants no student records.
- Often raised because costs rise with student counts.

**Answer / steps**
1. Set expectations from what PE found:
   - Students can't be removed from sections. Even with the student read scope removed from the app, a call to `/sections` or `/sections/{section_id}/users` always returns at least the Clever user IDs of all the students in the section. A section's data includes its list of students (the Sections doc lists `students` as a guaranteed field), and app-side filters don't change this.
   - Removing the student scope removes the student data fields (name, email and so on), but not the student IDs.
   - The app dashboard setting for which user types can sign in with SSO controls SSO access only. It doesn't change what syncs.
   - Global sharing recommendations can't exclude students when districts share sections.
2. Options PE offered:
   - PE can remove student scopes on a dev app so the partner can test.
   - Districts share only teachers and staff, and the partner gets sections another way (teachers create their own, or the district sends a sections CSV).
   - Content mapping (see SCP-06): districts share only teachers, and admins assign content in Clever.
3. For billing or cost questions, loop in the partner's ASM. Don't discuss pricing.

> Thanks for the detail. Each section includes its list of students, so we can't send you sections and leave students out entirely. Removing student scopes only removes the data fields, not the student records. Two options we can look at together: have districts share only teachers and staff and bring in sections another way, or use content mapping so admins assign the content. I'm looping in our Partner Engineering team to go through these with you.

**Escalate if…**
- The partner wants student scopes changed or content mapping turned on (PE).
- The conversation turns to cost: send it to the ASM.

**Notes / conflicts**
- In 03045447 the partner closed the case as answered but not resolved, and the product feedback was logged.
- The case threads quoted pricing. Never repeat it.

- Global sharing recommendations only offer student and section rules, with no staff role. That's expected. For staff, recommend sharing per district (which supports staff) or use a schools rule so admins pick schools. District admins are shared automatically (03002398).
**Links:** https://dev.clever.com/docs/sections
**Source cases:** 03002398, 03045447, 03008086

---

## SCP-06 · "Can you enable content mapping (Resources) on our app? Can we do it ourselves?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Triage only, PE makes the change

**When to use this**
- A partner asks for content mapping or Resources on a dev or test app, or on an existing production app.
- The partner asks whether it's self-serve.

**Answer / steps**
1. Ask for the client ID of each app and the sandbox district(s) they'll test in.
2. Transfer to PE. Only PE can enable content mapping for an app. For a sandbox or test district, PE also does a separate backend step to show the Content Assignment tab in the district dashboard.
3. For a **production** district, the district has to reach out to CS to have content mapping enabled for its account. CS can enable it once PE has enabled it on the app.
4. Tell the partner it isn't self-serve. The request for self-serve was logged as feedback.
5. If the partner asks what happens to existing users when it's enabled on a live app, don't guess. Ask PE (this was never answered in 03021263).

> I've passed this to our Partner Engineering team to enable content mapping on your app. Please share the district ID you'll test with. At the moment this can only be turned on by Clever. Once it's enabled on your app, each production district that wants to use it will need to contact Clever Support to have it turned on for their account.

**Escalate if…**
- It's being enabled on a live production app, or the partner asks about downstream effects.
- The Resources endpoint is empty after enabling (see SCP-01: it can just be a delay).

**Links:** none confirmed
**Source cases:** 03084119, 03103682, 02988070, 03021263, 03008086

---

## SCP-07 · "Is field X always populated (sis_id, email, grade, GPA, contact email)? Can you make it required?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Yes

**When to use this**
- A partner asks whether a field is guaranteed to be present, or asks Clever to make a field required for their app.

**Answer / steps**
1. Point to the data schema sheet (https://clever.com/schema) for which fields are required. From the cases:
   - **Always populated:** `sis_id` (student, teacher, section and staff records).
   - **Not guaranteed:** user email, student grade, `weighted_gpa` and `unweighted_gpa` (just a float with no fixed scale), and contact email.
2. Clever can't make a field required for districts. An app scope request can't make email required either, so don't offer a scope change for this.
3. If a field is missing for a record, the district can fix it in their SIS.

> Clever only guarantees the fields marked as required in our data schema. `sis_id` is always populated. Email, student grade, GPA and contact email are optional, so some districts won't send them. We recommend your app handle missing values. If a field is missing for a student your app needs it for, the district can update it in their SIS.

**Escalate if…**
- The partner says a field marked required is missing (PE).
- Values look wrong in production (see ROS-05 for triage).

**Notes / conflicts**
- Race values: one partner saw single-letter values in the dashboard Data Browser while the docs list full text. PE asked where it was seen and for the district ID. The Data Browser display is separate from the API, and the correct v3 endpoint is `/v3.0/users`. No final answer, so leave this with PE.
- K and pre-K values for `low_grade`: PE only pointed to the schema. Don't state values.
- Section grade is calculated from the grades of the students in the section, and the majority wins. For example, a section with 6 students in grade 1 and 4 in grade 2 gets grade 1 (confirmed by PE).

- `email` appears both at the top level and in the student role by data-model design, and the two values don't differ (02970386).
**Links:** https://clever.com/schema
**Source cases:** 03073936, 02970386, 03000811, 03000931, 03007656, 03097786, 03049600, 03018651, 03059352

---

## SCP-08 · "The sandbox schools list is empty, or a teacher has no schools"
**Status:** Draft, needs PE review · **Resolvable by CS:** Triage only, PE makes the change

**When to use this**
- A partner gets a successful response with an empty schools list, or a teacher with an empty `schools`, in a sandbox.

**Answer / steps**
1. Check whether the teacher has any sections. In the case seen, a teacher with no sections showed no school.
2. Check the district's pending data changes on the "holds" page in the App Dashboard. Changes may need approval before data appears. The same step was needed in a second case.
3. If that doesn't fix it, PE can add a custom section, or give a partner contact sandbox admin access so they can edit the SFTP files. Admin access needs an email that isn't used by another Clever account (aliases are fine).

> It looks like the demo teacher doesn't have any sections yet, so there's no school tied to them. Could you check the "holds" page for your sandbox district in the App Dashboard and approve any pending changes? If that doesn't fill it in, we can ask our Partner Engineering team to add a custom section.

**Escalate if…**
- PE needs to add the custom section or admin access.

**Notes / conflicts**
- Only one clear case. PE should confirm whether this is the general cause. Also see SCP-02.

**Links:** none confirmed
**Source cases:** 03017129, 03066534

---

## SCP-09 · "The app dashboard shows fewer students at a school than the district says it shared"
**Status:** Draft, needs PE review · **Resolvable by CS:** Yes

**When to use this**
- A partner compares a student count in the App Dashboard with a number the district gave them, and the dashboard number is lower.

**Answer / steps**
1. The dashboard counts students by their primary school. A student who takes a section at one school but is assigned to another building is shared with the app but doesn't count toward the first school's total.
2. In the API, `school` is the user's primary school and `schools` lists every school they're tied to.
3. If specific students are missing or can't access the app, collect the client ID, district ID and a few example user IDs, and transfer to PE.

> The most likely reason for the difference is that the district is sharing students who take classes at that school but whose primary school is a different building. The dashboard counts students by primary school. In the API, `school` is the user's primary school and `schools` lists every school they're associated with. If specific students are missing or can't access your app, send me your client ID, the district ID and a few example student IDs and I'll have our Partner Engineering team take a look.

**Escalate if…**
- Specific students are missing from the API, not just from the school-level count.
- Totals still don't line up after the primary-school explanation.

**Notes / conflicts**
- One case confirmed this explanation (PE-resolved).

- A student has one primary school (`school`, what the dashboard shows) and can have sections at other schools (`schools`). If the app needs the student's primary school, use `school`.
**Links:** none confirmed
**Source cases:** 02966629, 03096214

---

## SCP-10 · "Students get an Unauthorized error because the student email isn't sent to our app"
**Status:** Draft, needs PE review · **Resolvable by CS:** Yes (a suggestion only; PE confirms any change on the app)

**When to use this**
- A partner's logins fail for a user type and they traced it to a field that's optional in Data Access (for example student email), often in production but not in dev.

**Answer / steps**
1. Optional fields are sent only when the app asks for them and the district shares them. For districts that are already connected, the district's Field Access settings for the app decide what comes through, so the district may need to share the field there. In the app's Data Access settings in the App Dashboard, the partner can add the field (such as email) as an optional field.
2. Have them add it and retest. It's optional, so some districts still won't send it, and the partner's code shouldn't depend on it.
3. Ask for the client ID and whether it's dev or production.

> You can add student email as an optional field in your app's Data Access settings in the App Dashboard. Could you try that and let me know if it helps? Keep in mind it's optional, so some districts may still not send it. If it's still not coming through, send me your client ID (dev or production) and a district ID and I'll have our Partner Engineering team take a closer look.

**Escalate if…**
- The partner adds the field and still doesn't receive it.
- A field still isn't coming through for an already-connected district. What reaches the app depends on the Field Access settings for that district-app connection, and the district manages them. The district may need to explicitly share the field in Field Access.

**Notes / conflicts**
- The partner later said their own platform fixed the issue, so the actual cause is unconfirmed. See SCP-07 for field availability and ROS-06 for field gating.

**Links:** none confirmed
**Source cases:** 03107151

---

## SCP-11 · "A district only wants SSO but accidentally shared data. How do we go back to sharing nothing?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Triage only

**When to use this**
- An existing app is connected to a district that wants sign-in only but shared school data.

**Answer / steps**
1. The district controls what it shares, so the partner can't undo it. Explain that.
2. Don't suggest specific sharing rules or workarounds for a district. Changing a district's sharing is risky, and PE doesn't want CS advertising rule tricks.
3. Post in #partner-engineering with the case number, the app and the district, and what the district wants to stop sharing.

> Since the district controls what's shared, they would need to change their sharing settings. I'm checking with our team on the best way to help and will follow up.

**Escalate if…**
- Always.

**Notes / conflicts**
- One edge case, applying to an existing SSO-only app. Never offer SSO-only to a new partner.

**Links:** none confirmed
**Source cases:** 03047343
