# Rostering and Secure Sync

Covers these Salesforce buckets: Developer Support > Best Practices - Rostering, and Developer Support > Troubleshooting - Rostering.
Source: resolved integrations@clever.com cases. Every entry is a draft until PE has reviewed it.

---

## ROS-01 · "Can my app pause or stop syncing a district (summer, or schools that aren't using us)? What happens to the data?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Yes

**When to use this**
- A partner says their program doesn't run over the summer and asks how to pause every district.
- A partner asks what happens to their data and users when a district pauses.
- A partner asks whether disconnecting a district changes their tokens or IDs.
- A partner wants to stop receiving data for schools that don't use the app.

**Answer / steps**
- **There is no partner-side pause.** A sync pause is something a district does. When a district pauses, it stays connected to all its apps. It only pauses the sync between its SIS and Clever, so it can make updates in the SIS before syncing the new year's data. Partners aren't disconnected.
- While a district is paused, the last sync before the pause stays in place. That's the data the partner can access until the pause ends. Users already shared with the app can still log in.
- **Disconnecting is different.** When a district disconnects from an app, the district-app token is invalidated and never reused. If the district connects again later, Clever provisions a new district-app token. Depending on how the partner archives and restores records, they may need to rebuild their roster. If the same data is shared, the Clever IDs stay the same.
- To stop receiving data for schools that don't use the app, the district can change its sharing filters so only the schools using the app keep syncing.
- Partners can build their own automatic pause on their side each summer.
- A bulk pause for partners doesn't exist. It was shared with Product as feedback.

> Thanks for reaching out! Here's how pausing works. Districts can pause the sync between their SIS and Clever from their own Clever dashboard. They stay connected to your app, and the last sync stays in place until the pause ends, so you can still see that data and shared users can still log in: https://support.clever.com/hc/s/articles/205666507?language=en_US. We don't have a partner-side pause or a bulk pause, but I've passed that along as feedback to our product team. If you only want to stop receiving data for schools that aren't using your app, the district can adjust its sharing filters. If a district disconnects from your app, its old token stops working for good, and you'll get a new district token if they reconnect.

**Escalate if…**
- The partner needs behavior not listed here, such as exactly when data stops after a pause.
- The partner wants Clever to disconnect or pause a district on their behalf.

**Notes / conflicts**
- Don't describe a pause as a disconnect. A paused district stays connected; a disconnected district does not.
- Not confirmed: whether a district's pause changes anything the partner pulls through Events.
- Data from terms past `term_end_date` also stays in the API until the district deletes it or removes it from sharing (03007300).

- PE said Clever automatically pauses syncs for all districts over the summer (03079700). A partner's request to waive sync charges goes to the ASM. Quote no fees.
**Links:** https://support.clever.com/hc/s/articles/205666507?language=en_US · https://dev.clever.com/docs/data-changes
**Source cases:** 03079700, 03008192, 03076598, 03086772, 03007300
---

## ROS-02 · "How do I match our existing users, schools or sections to Clever records, and what happens when IDs change?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Triage only, PE makes any change

**When to use this**
- A partner has existing accounts and wants to link them to Clever data before or after the first sync.
- A partner asks for a file of Clever IDs for their districts, or a one-time download of student data.
- A partner asks about IDs changing after a district changes SIS (see the sync conversion note).
- A partner sees two records that look like the same person or school, or a student appearing under the wrong account.

**Answer / steps**
- Clever can't send a mapping file. Partners with Secure Sync can use **Matchmaker** (click Matchmaker in the left navigation of the Clever dashboard). They upload a CSV of their existing records, Clever compares it with the records shared with the app, and emails back a CSV with matched and unmatched records. The more identifiers the CSV includes, the better the match. Review the output before using it.
- For matching students, the docs list the most reliable fields first: SIS ID and student number, then state ID, birthdate, email, and name last. Name alone isn't reliable, since students in the same school can share a name.
- Schools are usually matched once, often by hand or with school admin help, using the MDR number or NCES ID.
- Clever doesn't recommend matching existing sections to Clever sections, because unique section identifiers rarely exist in partner data. If it's essential, narrow by school and teacher first.
- The district can also export its own data from the Data Browser and share the file with the partner.
- **Sync conversions:** if the question is about a district changing SIS, or what happens to IDs when it does, don't answer it. Sync conversions are handled only by Clever Support.
- The SIS ID and the student number are two different Clever fields. Comparing them can produce false matches.
- Two schools in one building can share one NCES ID (03075117). Don't use NCES ID as the only unique key for schools. The Clever ID is the primary key.

> Thanks for reaching out! To link your existing records to Clever, you can use Matchmaker in your Clever dashboard (it's in the left navigation). Upload a CSV with as many identifiers as you have, such as SIS ID, student number, state ID, name and email, and Clever will look for matches in the district data shared with your app and email you the results. We can't send a mapping file ourselves.

**Escalate if…**
- The partner reports a real mix-up of users or Clever IDs, such as one student's account shown for another. PE needs the district, the Clever IDs involved and the date it started.
- Duplicate school IDs, or a request to override an ID value. PE said a special mapping can overwrite the NCES ID, but the cases don't show how it's requested or whether it's offered to everyone.
- The partner wants a bulk export of district data. That case didn't resolve.

**Notes / conflicts**
- The Clever ID must stay the primary key for users. Matching on email or another field in Matchmaker is a one-time link only. Partners shouldn't re-key on those fields.
- The "student under the wrong account" case (03040052) wasn't resolved. PE found students whose SIS ID equalled another student's student number, and a sandbox test showed swapping SIS IDs keeps the Clever IDs but swaps who holds which. That isn't confirmed as the cause. The partner in that case was calling a v2.x endpoint, and nothing in the case quotes any deprecation date, so don't mention one.
- Sync conversions are exclusively handled by Clever Support. PE asked that this assistant not act as the expert on SIS changes or what happens to IDs. Say: "Sync conversions are exclusively handled by Clever Support. If you have additional questions about this topic, please connect with one of your teammates who are skilled in this topic." (03045513 was about this.)
- Matchmaker is Secure Sync only.

- Custom metadata fields (race, ethnicity, ELL, FRL, IEP, home language) aren't supported in the OneRoster 1.1 export. A district moving to OneRoster 1.2 is a sync conversion, so it goes to Clever Support only (03065317, 03056269).
**Links:** https://dev.clever.com/docs/onboarding · https://dev.clever.com/docs/users · https://dev.clever.com/docs/data-model-quirks-and-supported-features-district-integrations
**Source cases:** 03007656, 02987272, 03007300, 03045513, 03040052, 03075117, 03069734

---

## ROS-03 · "How do I get users by course? Why do semester classes show up as new sections? Why can't teachers see students?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Yes

**When to use this**
- A partner expected `/courses/{id}/users` (or a role filter on it) and can't find it.
- A district moves to semesters or quarters and the same class appears as new sections.
- A teacher or admin appears without any sections, so they can't see students in the app.

**Answer / steps**
- Users aren't linked to courses. They're linked to sections. A course is an optional label for what a section is, and a section is the actual class with a teacher and students. Use the section's users, for example `/v3.0/sections/{id}/users`, with `?role=teacher` to filter by role.
- Semester or term classes: how this looks depends on the district's SIS. Some SIS systems reuse a section from term to term. Others create new sections with new Clever IDs at the start of a term. Partners should handle both, by archiving old terms and reactivating them if needed.
- To show only current classes, use the term data on each section. If the district isn't sending terms, the district asks Clever Technical Support to help update its sync.
- To connect the same class across terms, PE suggested checking for a shared section number or course number.
- Teachers or admins with no sections is expected when the district hasn't set them up. The district can create sections for them or add them as staff so the partner can give extra permissions. Fixing the data in Clever is better than granting access by hand in the partner app (SCP-03 covers other staff-role needs).

> Thanks for asking. In Clever, users are connected to sections rather than courses. A course describes the offering (for example, Algebra 1), and a section is the actual class. You can get the users in a section with `/v3.0/sections/{id}/users` and add `?role=teacher` to filter by role. For classes that span semesters, some districts reuse sections and others create new ones each term, so we recommend using the term data on each section to show only current classes. To connect the same class across terms, check whether the sections share a section number or course number.

**Escalate if…**
- The partner shows two terms of the same class with different section and course numbers and can't match them.
- A district says it is sending terms, but they don't appear in the API.

**Notes / conflicts**
- All three answers were given in writing by PE, so CS can reuse them. Clever doesn't create a continuous class across terms.
- The v3.0 paths come from the cases. v3.1 should work the same, but the cases don't say so. Confirm before telling a v3.1 partner.
- Course and term IDs aren't stable: if a course or term is deleted and recreated, it gets a new Clever ID.

- For the school year start, PE recommends using the term start date (03072509).
**Links:** https://dev.clever.com/docs/sections · https://dev.clever.com/docs/courses · https://dev.clever.com/docs/data-changes · https://dev.clever.com/docs/data-model-quirks-and-supported-features-district-integrations
**Source cases:** 03072509, 03007656, 02986851, 02997238, 02967112

---

## ROS-04 · "Does Clever provide bell schedules, rotations, room assignments, or student grade/subject data?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Yes (for the confirmed answers only)

**When to use this**
- A partner asks which scheduling or segmentation data is available in Secure Sync before building.
- A partner asks how to model optional demographic fields in v3.1.

**Answer / steps**
- Course sections: yes, through the Courses and Sections objects.
- Not supported today: bell schedules, rotations or day cycles, and room or location assignments. There's no current plan. Pass it on as product feedback.
- Which fields a partner gets depends on the data schema (required versus optional fields) and on what the district chooses to share with the app.
- v3.1 disability fields: treat `disability_status` as the explicit indicator. Don't infer a disability from the type alone. The type is supplemental detail and may be empty even when the status is "Y".

> Thanks for asking! Clever supports course sections through the Courses and Sections objects. Bell schedules, rotation or day-cycle data, and room or location assignments aren't supported today. I'll share your request with our product team. You can see which fields are required and which are optional here: https://dev.clever.com/docs/clever-api-v31-data-schema

**Escalate if…**
- The partner asks about a field that isn't in the data schema.
- The partner asks about v3.1 access. New apps get v3.0 by default and v3.1 is turned on by request, so transfer to PE.
- The question is about Library. Ask in #partner-engineering.

**Notes / conflicts**
- One case answered a Secure Sync data question by pointing to the Library data table and saying only the installing teacher's data is available. That's the wrong frame for a Secure Sync partner, and Library questions go to #partner-engineering. Don't reuse that answer.

**Links:** https://dev.clever.com/docs/clever-api-v31-data-schema · https://dev.clever.com/docs/data-model · https://dev.clever.com/docs/users
**Source cases:** 03082809, 03075374, 03072466, 02991623

---

## ROS-05 · "A district's data isn't reaching us: files are missing, sections or terms are empty, or records show as filtered out"
**Status:** Draft, needs PE review · **Resolvable by CS:** Yes. Triage first. PE is only needed for widespread, integration-level problems.

**When to use this**
- A district shares only part of its data, such as users but no sections, and no OneRoster files arrive.
- Sections come back empty, or terms and courses return a 404 ("No terms found").
- A user test says the user is "filtered out by app-side filters", or the Final Data view shows 0 for a role.
- Data is missing from an app for one district, or a few.

**Answer / steps**
CS owns the first round of triage on missing data. Do it thoroughly so the partner gets a fast, useful answer.
1. Collect the client ID, whether it's the dev or production app, the district ID, which record types or fields are missing, and whether it's one district or several.
2. Check the district side. Is the district sharing the data the app needs? A common cause is a district sharing users but no sections. PE saw a missing-term-data error in that case, and no OneRoster files were produced. Asking the district to add sections to what it shares fixes it.
3. Check the app side. Look at the app's Data Access settings and any app-side filters in the App Dashboard. In one case staff records were dropped because email wasn't a required field for staff in the app's Data Access settings.
4. Check the data itself. A school with no sections returns an empty list for sections, and terms and courses return a 404. In that case the sandbox school had no sections, and PE said this is expected.
5. Check the district's sync status (see ROS-07 and ROS-01). A paused or errored district explains stale or missing data.
6. Reply with what you found and what the partner or district can change.

> Thanks for reaching out! It looks like this district is sharing only users and not sections, which means there's no term data, and that's causing an error on our side. If it's an option for the district, asking them to add sections to what they share with your app should fix it. Can you confirm the client ID and the district ID so we can double-check on our end?

> Staff records can be dropped when email isn't included as a required field for staff in your Data Access settings. Could you try adding it and let us know whether the records come through?

> A school with no sections will return an empty list for sections, and terms and courses may return a 404. Could you check the section count first and try a school that has sections?

**Escalate if…**
- It looks like an integration-level problem, for example data is missing from the app for **all** of its districts. Send PE the client ID, the districts checked, and what you already ruled out.
- You've finished the triage above and still can't explain the gap for a district. Ask in #partner-engineering with the details you collected.

**Notes / conflicts**
- For missing data, triage first. This entry takes priority over the general advice to escalate anything that looks wrong in production, unless the problem is widespread.
- A partner that needs every OneRoster file, even empty ones, may not receive them when a district shares only some record types (03001110). As far as PE recalls this hasn't been fixed, so don't tell the partner it was fixed or promise a change.
- Don't offer default term data. It isn't offered to partners.
- "Terms depend on sections" in 03071081 is PE's own explanation. Don't present it as documented behavior.

**Links:** https://dev.clever.com/docs/data-model-quirks-and-supported-features-district-integrations
**Source cases:** 02978960, 03079913, 03089005, 03106690, 03001110, 03048729, 03071081, 03114942
---

## ROS-06 · "A student field (IEP, ELL, FRL, home language, or a v3.1 field like disability status) isn't showing in the API or Data Browser"
**Status:** Draft, needs PE review · **Resolvable by CS:** Yes for the checks. PE applies scopes and turns on v3.1.

**When to use this**
- The partner expects a student field but doesn't see it in the API or the Data Browser.
- A district says it shares a field, but the partner doesn't receive it.

**Answer / steps**
1. Identify the field, then check what gates it:
   - **IEP, ELL and FRL status, and home language:** these are locked behind scopes that PE applies. If the app doesn't have the scopes, see SCP-01 and transfer to PE.
   - **v3.1 fields:** v3.1 only adds student demographic fields such as `disability_status`, the gifted and Section 504 fields, `home_language_code`, and the FRL value "Other". The app needs v3.1 turned on (see VER-01). The full list is in the dev doc below. A v3.1 field doesn't explain a missing ELL or IEP value.
2. If the scopes or version are right, check the settings. Either of these can block a field:
   - **District Dashboard (district-facing):** the district's field access settings, which control what the district shares with the app. The district also has to send the field from its SIS.
   - **App Dashboard (app-facing):** the app's Data Access settings and any app-side filters.
3. Tell the partner which setting to check, or ask the district to check theirs.

> Some student fields, such as IEP, ELL and FRL status and home language, need extra scopes on your app before they come through. Could you tell me which field you're missing and your client ID? If your scopes are in place, it's also worth checking the Data Access settings in your App Dashboard, and the district can check which fields they're sharing with your app in their District Dashboard.

**Escalate if…**
- Scopes or v3.1 need to be turned on: transfer to PE.
- The scopes, version and both sets of settings are right and the field is still missing for a district. Ask in #partner-engineering with the details you collected.

**Notes / conflicts**
- Overlaps with SCP-01 (adding or removing scopes).
- "Dashboard" can mean either the District Dashboard or the App Dashboard. Say which one you mean when you talk to a partner or district.

**Links:** https://dev.clever.com/docs/whats-new-in-api-v31 · https://dev.clever.com/docs/api-v31
**Source cases:** 03087485, 03082606, 03061960, 03066534, 03002235, 03092879, 02981346, 03048944
---

## ROS-07 · "The API says the district state is 'running' but the dashboard says 'Successful'. Where did 'in-progress' go in v3?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Triage only. PE confirms.

**When to use this**
- A partner compares the dashboard sync status with the API's `district.state` field.
- A partner built logic around a v2 "in-progress" state and sees only "running" in v3.

**Answer / steps**
> The two statuses measure different things. The dashboard "Successful" shows the result of the district's most recent SIS sync run. In the API, a district state of "running" means the sync is active and healthy, not paused or in an error state. It doesn't mean a sync is running right now. Our engineers confirmed there's no in-progress state in v3, so it's covered by "running". If you need to know when district data has changed, the `last_sync` field is a better signal.

The current `state` values are `running`, `pending` (an error the district can fix), `error` (an error Clever can fix), `paused` (the district's data is paused to reflect last year's data), and `requires-review`. The dev docs also list `success` and an empty value. If the partner asks what `requires-review` means or about any other value, hand off to PE.

**Escalate if…**
- The partner needs the list of state values confirmed, or the state looks wrong for a production district.

**Notes / conflicts**
- `requires-review` is a real, current value but isn't in the dev docs yet. What it means isn't covered in the cases, so don't explain it.

**Links:** https://dev.clever.com/docs/districts
**Source cases:** 03117945

---

## ROS-08 · "How do I get a district token programmatically? My calls send me to a login page"
**Status:** Draft, needs PE review · **Resolvable by CS:** Yes

**When to use this**
- A partner with Secure Sync can't call the Data API, and their OAuth attempts land on a login page or an "admins are not allowed" message because they're following the SSO documentation.

**Answer / steps**
1. District tokens come from `GET https://clever.com/oauth/tokens?district=<district id>`, authenticated with the app's client ID and secret. Without the `district` parameter, the call returns every token granted to the app. The district token is then used as the bearer token on Data API calls.

> District tokens come from a GET request to https://clever.com/oauth/tokens, authenticated with your app's client ID and secret. Add `?district=<district id>` to get the token for one district, or leave it off to see all the district tokens for your app. Use the district token as the bearer token on your Data API calls. Please don't email us your client secret or any token. More here: https://dev.clever.com/docs/onboarding

2. If the partner is still stuck, ask for the client ID and whether the app has Secure Sync scopes (see SCP-02).

**Escalate if…**
- The app has no Secure Sync scopes.
- A secret, token or authorization header is pasted into an email: tell #partner-engineering immediately so PE can reset it.

**Notes / conflicts**
- In the case, PE's first reply sent generic OAuth and SSO doc pages and didn't help. The partner said the rostering API access is mixed in with the SSO documentation, and PE passed on the feedback.
- District tokens are reset by PE only (see SSO-09).

**Links:** https://dev.clever.com/docs/onboarding
**Source cases:** 03043575, 03063560

---

## ROS-09 · "How do contacts (parents) work? What are the type and relationship values? Can parents use SSO?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Yes

**When to use this**
- A partner asks about parent or guardian data, contact types, or contact login.

**Answer / steps**
1. Contacts are a separate object. They come in through the student record in the district's SIS and Clever sync, and need a Secure Sync integration.
2. Each contact has a `student_relationships` object with `relationship`, `type` and `student` (a student's Clever ID).
3. A contact has only one `type`. Accepted values from the cases: Primary, Secondary, Parent/Guardian, Emergency, Family, Other, or blank. To tag a person with several types, PE's workaround was duplicate contact records or extension data.
4. Filter on `type` first, and use `relationship` as a fallback so records labeled "Other" aren't dropped.
5. A person who is both staff and a contact gets a separate Clever ID for each role.
6. Contact email exists but is optional. There's no contact username field (feedback was logged).
7. Contacts have no SSO login with Clever.

> Parent and guardian data comes through rostering as "contacts", tied to students by a `student_relationships` object. Contacts don't currently have an SSO login with Clever, and contact email is optional, so many districts won't send it. If a person is also a staff member, they get a separate Clever ID for each role.

**Escalate if…**
- The partner wants a contact username or parent SSO (product feedback, via PE).
- The district isn't sending contacts (triage per ROS-05).

**Notes / conflicts**
- Older sales replies called SSO free and listed who can use it. Don't repeat that. Only say contacts don't have SSO.

- No per-contact addresses: a contact's address sits on the student record. A Secure Sync data rule can flag non-unique contact emails. Parent SSO isn't available (02996089).
**Links:** https://clever.com/schema
**Source cases:** 02971404, 02996089, 03049600, 03055058, 03016980

---

## ROS-10 · "How do I detect changes, handle missing records, and what are the paging and rate limits?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Yes

**When to use this**
- A partner building a Secure Sync pull asks about `last_modified`, `limit`, rate limits, missing links, or records that disappear.

**Answer / steps**
1. `last_modified` is the last time the record changed in Clever. It isn't the SIS refresh time, and PE doesn't recommend it as a sync trigger. Use the Events API for incremental changes (Events are Secure Sync only).
2. `limit` has a maximum of 10,000, and larger limits are slower. Paginate with `page`.
3. The rate limit is 1,200 requests per minute per token (not per client ID). PE confirmed this is current.
4. Relational links (course, users, schools) appear only when you GET a single record, such as `/v3.0/sections/{id}`, not on list endpoints. Courses are optional.
5. If a record drops out of a pull, archive it rather than delete it. It can return with the same Clever ID, and history should stay intact.

> `last_modified` shows when a record last changed in Clever, so for incremental syncing we recommend the Events API. Relational links show up when you fetch a single record, not in list calls. If a record drops out of a pull, we suggest archiving it instead of deleting it, since it can come back with the same Clever ID. Page size can go up to 10,000, and we recommend paging for larger pulls. The rate limit is 1,200 requests per minute per token.

**Escalate if…**
- Links are missing on a single-record GET.
- Events calls hang or behave unexpectedly (one case reported a `/events` call hanging 30+ seconds past the latest event; it was a duplicate and never answered).

**Notes / conflicts**
- A v2 case showed `launchDate` briefly differing between full sync and `/events`. It resolved with no root cause. Point v2 partners to v3.
- Programmatic SSO testing: SSO uses an interactive authorization-code login, and no documented endpoint mints a code (03094001).

- Events API parameters: `starting_after`, `ending_before`, `limit`, `school` and `record_type`. There's no role filter (02983199).
- Events are kept for about 30 days. PE didn't extend retention for a partner. Pattern PE recommended (confirmed current): a regular full sync as the source of truth, with Events between syncs, and fall back to a full resync if the stored event ID is older than 30 days (03113832).
**Links:** none confirmed
**Source cases:** 02983199, 03113832, 03007901, 02975251, 03007656, 03011647, 03088183, 03094001

---

## ROS-11 · "Why are student names all uppercase? Can Clever standardize data formatting?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Yes

**When to use this**
- A partner finds inconsistent capitalization or formatting in names or other text fields and asks whether Clever cleans it up.

**Answer / steps**
1. Clever passes text values as the district's SIS provides them. It doesn't title-case or normalize them.
2. The district owns the source data. The fix is for the district to update it.
3. There's no known roadmap item. Offer to pass it on as product feedback.

> Clever shows text values like names exactly as the district's SIS provides them, so if a district sends names in all caps, we don't change them. The fastest fix is for the district to update the source data. I'm not aware of a case-normalization feature on our roadmap, but I'm happy to pass it on as feedback.

**Escalate if…**
- A district says the data is correct in their SIS but Clever shows something different.

**Notes / conflicts**
- Single case. See SCP-07 for whether fields are populated.

**Links:** none confirmed
**Source cases:** 03053836

---

## ROS-12 · "We got a users.created event for a record created a year ago. How do I know when something was shared with us?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Yes

**When to use this**
- A partner sees an Events record whose `created` date is old but the event is new, or asks for a "shared date" because records shared later are missed by `last_modified` delta pulls.
- Events are Secure Sync only.

**Answer / steps**
1. A `users.created` event is generated when a district newly shares a record with the app. "New" means new to the app, not new to Clever.
2. If the event's `created` and the record's `created` match, the district created a new record in its SIS and it met the sharing rules. If they differ, the record already existed and was newly shared.
3. There's no dedicated shared-date field. The event's `created` is the closest thing.
4. For delta syncs, use Events instead of relying on `last_modified` alone.

> A users.created event is generated whenever a district newly shares a record with your app. That can be a brand-new record or an existing record that wasn't shared before, which is why the event date and the record's created date can differ. For a "shared date", the event's created field is the closest we have, and for delta syncs we recommend the Events API rather than last-modified alone. The guide is here: https://dev.clever.com/docs/events-api

**Escalate if…**
- Expected events are missing, not just old-looking. Send to PE with the district, record IDs and dates.
- The partner asks for a shared-date field. Log it as feedback in Salesforce.

**Notes / conflicts**
- In a related case, changing sharing in a sandbox produced only `districts.changed` and `users.changed` events, with no per-school sharing field. The final answer wasn't seen.

**Links:** https://dev.clever.com/docs/events-api
**Source cases:** 03052513, 03103875, 03069061

---

## ROS-13 · "We narrowed what a district shares with us but never got delete events for the removed schools"
**Status:** Draft, needs PE review · **Resolvable by CS:** Yes

**When to use this**
- A district reduced the schools it shares and the partner expected delete events. Events are Secure Sync only.

**Answer / steps**
1. Collect the district, the date sharing changed, and what the partner expected.
2. Check the connection history yourself: the district's and the app's timelines in Hall Monitor show when the district and app disconnected from each other and reconnected. You don't need to transfer the case to PE for this. In the case seen, the district had disconnected and later reconnected the app, then shared only 2 of 17 schools. Events only cover changes made since the reconnection, so no delete events came for the other 15.
3. PE's two options: a full resync, or have the district share all schools, allow a sync, then reduce sharing and allow another sync so the removals produce delete events.

> If a district disconnected and later reconnected your app, Events only cover changes since the reconnection, so earlier removals won't appear as delete events. I checked the connection history for this district and app. The options are a full resync, or having the district temporarily share everything, allowing a sync, then reducing sharing again so the removals show up as delete events.

**Escalate if…**
- The timelines show no disconnect, or delete events are still missing after the two-step sequence (then PE).

**Notes / conflicts**
- A bearer token was pasted in this case (case number only in the maintainer notes). Never ask for tokens.

**Links:** https://dev.clever.com/docs/events-api
**Source cases:** 03036096, 03069061

---

## ROS-14 · "The Events API says our app hasn't enabled events, or it times out"
**Status:** Draft, needs PE review · **Resolvable by CS:** Yes for the toggle, triage only for timeouts

**When to use this**
- The error says the app hasn't enabled events in the settings, or Events calls time out, especially with several parameters or for one large district.

**Answer / steps**
1. Confirm the partner is on Secure Sync and check that they're looking at the right app (dev or production).
2. Check the Events toggle under Settings > Integration in the App Dashboard. In the case, the partner had the wrong app open.
3. The change is instant, but allow up to 30 minutes before retesting.
4. For timeouts, ask for the exact endpoint and parameters and transfer to PE if it continues.

> It looks like the Events toggle is off for that app. You can turn it on under Settings > Integration. The change should be instant, but please give it up to 30 minutes before retesting. If it still errors, send us the exact endpoint and parameters you're calling and we'll take a look.

**Escalate if…**
- The toggle is on and it still fails after 30 minutes, or timeouts continue for one district.

**Notes / conflicts**
- The parameter is `record_type` (singular), as the docs show (PE confirmed). One old case suggested the plural `record_types`. That's wrong.
- The "limit to 2 parameters" advice came from a 2024 v2.1 case. Don't treat it as current.
- A subscription isn't the issue. Events are Secure Sync only.

**Links:** https://dev.clever.com/docs/events-api
**Source cases:** 03073084, 03019383, 02966932

---

## ROS-15 · "A district shares far more schools than we're licensed for. What do we do with the pending request and the filters?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Yes (contract questions go to the ASM)

**When to use this**
- A pending district request or a connected district shares more schools than the partner expected, or the school count in the dashboard looks off.

**Answer / steps**
1. PE's advice: approve the request, let it sync, review the data, then add a filter.
2. Use a single filter rather than several, for example students whose school is one of the intended schools and whose grade is in the range. Sections for those students come through with them. Leave ELL/IEP filters off if they'd be set to both Y and N.
3. The school count in the dashboard's right-hand column counts each shared user's primary school only. A user with sections at other shared schools doesn't add those schools.
4. After adding a school or changing a filter, a manual sync may be needed.
5. The district can also narrow its own sharing rule and then re-approve.

> We recommend approving the request, letting the data sync, reviewing it, and then adding a filter. Try a single filter, for example students whose school is one of your intended schools and whose grade is in your range. That also brings through the sections those students are enrolled in. For questions about how schools count against your agreement, I can loop in your Application Success Manager.

**Escalate if…**
- Counts stay wrong after a manual sync or filters don't take effect, and CS can't resolve it. Escalate app-side filter problems to the ASM.
- The app is having a lot of filter issues, or has billing, contract or licensing questions: have the agent ping the ASM in #app-success on Slack.

**Notes / conflicts**
- Threads contained pricing and billing statements. Don't quote fees or billing rules.

**Links:** none confirmed
**Source cases:** 03104208, 03057121, 03108006

---

## ROS-16 · "A teacher's email changed and her classes disappeared from our data"
**Status:** Draft, needs PE review · **Resolvable by CS:** Triage first

**When to use this**
- Data for a user disappears after their email changes, and the district's sharing rule or the partner's filter matches on email.

**Answer / steps**
1. Ask which district and which user.
2. Check the district's sharing rule. In the case, a leading space before the email meant one teacher wasn't shared (5 of 6 teachers).
3. Ask the district admin to remove and re-add the email without the space. Then check the partner's own filter.

> Could you tell us which district and which user this is? We'll check the district's sharing rule first. Sometimes a stray space at the start of an email in the rule keeps a user from being shared.

**Escalate if…**
- The user is shared correctly and the data still doesn't come through after a re-sync.

**Notes / conflicts**
- The final outcome wasn't shown. See ROS-05 for other missing-data triage.

**Links:** none confirmed
**Source cases:** 03113457

---

## ROS-17 · "A student's enrollment start and end dates keep changing. Is that expected?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Triage only, PE answers

**When to use this**
- A partner uses `roles.student.enrollments` dates for reporting or attendance logic and sees them shift.

**Answer / steps**
1. Collect two or three examples and whether it affects one district or all.
2. PE told the partner that `enrollments` is built from section membership, not from an SIS admission or withdrawal record, so it can change when sections are added, dropped or their terms change. The `school` and `schools` fields carry no dates.
3. Transfer to PE if the partner needs a firm enrollment window.

> The enrollments field on the student object is calculated from the student's section memberships, not taken directly from the district's SIS admission or withdrawal dates. That means it can shift when sections are added or removed, or their terms change. I'm looping in Partner Engineering to go through your examples.

**Escalate if…**
- The partner needs a canonical enrollment window.

**Notes / conflicts**
- An earlier suggestion to use term dates didn't satisfy the partner. The Attendance API doesn't provide an enrollment window either.

**Links:** none confirmed
**Source cases:** 03065617

---

## ROS-18 · "A district mentioned 'extension fields'. What are they, and can we add a field like an IEP entry date for all our districts?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Yes

**When to use this**
- A district tells the partner it can send extra values, or a partner wants data that isn't in the Clever data model (for example an entry date for a program field, or assessment results).

**Answer / steps**
1. Extension fields are fields outside the base data model. The app and the district agree on them, the district sets them up on its side, and the values come through the Clever API. The app reads them and uses them as it needs to.
2. An app can't define one extension field that all its districts get automatically. Each district sets it up.
3. Clever doesn't add new standard fields on request. Reply "not supported today, thanks for the feedback" and log the feedback in Salesforce.

> Clever doesn't have a standard field for that value today. Districts can send values that don't come standard through extension fields, which you and the district agree on and the district sets up on their side. Details here: https://dev.clever.com/docs/extension-fields. An app can't set up an extension field for all of its districts at once, so each district configures it. We've logged your request as product feedback.

**Escalate if…**
- The district says extension fields aren't showing up in the API.

**Notes / conflicts**
- In one assessment-data case the district said it couldn't send that many extension fields and offered SFTP instead. The outcome wasn't shown.
- Extension fields don't change staff access or roles on their own (nothing in the cases says they do).

**Links:** https://dev.clever.com/docs/extension-fields
**Source cases:** 03046155, 03012100, 03004915

---

## ROS-19 · "The same parent has two IDs with the same email in the API"
**Status:** Draft, needs PE review · **Resolvable by CS:** Triage only

**When to use this**
- Two contact records (different Clever IDs) share one email, each linked to different students.

**Answer / steps**
1. Ask for the API version and a couple of example record IDs.
2. If PE confirms two separate contact records, it usually comes from how the district's SIS data was created or synced. The district should consolidate them. The district can contact Clever Technical Support if it needs help.
3. Don't tell the partner to merge on email as a Clever-supported fix.

> We confirmed these are two separate contact records in the district's data, most likely from how the contacts were created in their SIS. Please ask the district to consolidate them into one record. If they need help, they can contact Clever Support.

**Escalate if…**
- Duplicates appear across many districts or look like Clever created them.

**Notes / conflicts**
- In the case the Clever Portal showed one account while the API showed two records. Don't explain why beyond that. See ROS-09 for contacts.

**Links:** none confirmed
**Source cases:** 02971404

---

## ROS-20 · "Can Clever sync our connection request and approval dates into our CRM?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Yes

**When to use this**
- A partner wants the date a district requested or approved the connection to appear in their own CRM.

**Answer / steps**
1. Clever doesn't push connection data to third-party CRMs. Say "not supported today, thanks for the feedback" and log the feedback in Salesforce.
2. PE's workaround (OK to share with partners): a daily job on the partner's side calls the tokens endpoint with the app's credentials and compares the list of connected districts to the CRM. A new district in the list is a new connection, so the partner can stamp the date itself. The districts endpoint returns MDR and NCES IDs where available, which helps match CRM accounts. See ROS-08 for the tokens endpoint.
3. The request-sent date isn't available through the API.

> Clever doesn't push connection data into third-party CRMs today, and we've logged that as feedback. As a workaround, a scheduled job on your side can read your app's connected districts and compare them to your CRM to date new connections.

**Escalate if…**
- The partner can't get the tokens endpoint to authenticate.

**Notes / conflicts**
- The same thread covered contract minimums and rates. Don't quote any of it. Billing questions go to the ASM (#app-success).

**Links:** none confirmed
**Source cases:** 03113367
