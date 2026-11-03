# LMS Connect, Library & AnySchool

Covers these Salesforce buckets: Developer Support > Troubleshooting - LMS Connect, Best Practices - Library, and Troubleshooting - AnySchool, plus LMS Connect questions from Troubleshooting - Other.
Source: resolved integrations@clever.com cases. Every entry is a draft until PE has reviewed it.

**Library:** for any Library question (visibility, test districts, certification, analytics), ask in #partner-engineering. Don't send Library certification docs or promise a timeline.

---

## LMS-01 · "LMS Connect basics: which LTI placements, how do we know a district has LMS Connect, and what do the submission states mean?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Yes.

**When to use this**
- A partner building LMS Connect asks a basic question about launches, connection status, or submission states.

**Answer / steps**
> Districts set up LTI 1.3 to launch Clever apps from their LMS. If a district previously added your app with LTI 1.1, they'll need to remove it and set it up again with LTI 1.3.
>
> To see whether a district has LMS Connect, open the **LMS Connect** tab in your Clever app dashboard. It shows each district as connected or not connected.
>
> A submission is one student's attempt at an assignment. Its state can be **created**, **submitted**, **returned** or **reclaimed**. The accepted fields and values are in the LMS Connect API reference.

**Escalate if…**
- The LMS shows data differently from what the API wrote (for example due dates or assignees after an update).
- A documented field, such as `grading_type`, seems to be ignored.

Both of these turned out to be bugs reported to Engineering. Ask for the **assignment IDs** and the LMS involved before transferring to PE.

**Notes / conflicts**
- LMS Connect requires Secure Sync, and LMS Connect is a v3.1 feature (see VER-01).
- LMS Connect certification goes to PE.

- The LMS Connect tab shows each district as connected, or invited with the invite date. The district has to finish setup on its side. LMS Connect needs Clever Complete and PE enablement first; don't send a certification form (03090084).
- There's no grade passback between a Clever app and an arbitrary third-party app. Passback is only for Google Classroom, Canvas and Schoology (02980073).
- Displayed due-date time can differ between Canvas and Google Classroom even when the same UTC value is sent. The cause was unresolved. Escalate with logs and the LMS involved (03061720).
- LTI 1.3 SSO is documented for Canvas and Schoology only. Roadmap questions go to PE (03004551).
- A Schoology SSO link that returned 403 worked once the redirect URI moved off localhost (03003437). Certification and go-live questions go to #partner-engineering.
- PE said LTI 1.3 SSO is supported for Canvas and Schoology, not Google Classroom (02986857, an older case).
**Links:** https://dev.clever.com/docs/lms-connect-overview · https://dev.clever.com/docs/core-concepts · https://dev.clever.com/docs/api-reference · https://dev.clever.com/docs/lms-connect-lti-13-sso · https://dev.clever.com/docs/clever-lms-connect-api-errors
**Source cases:** 02986857, 03003437, 03061720, 03003970, 03004551, 03090084, 02980073, 03011938, 03002050, 02998947, 03073230

---

## LMS-02 · "How do I get demo or sandbox districts connected to my dev app to test LMS Connect (Google Classroom, Canvas, Schoology)?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Triage only, PE makes the change

**When to use this**
- A partner building LMS Connect asks for test districts on their dev app, or a test district says "Your applications are not available on LMS Connect at this time."
- A partner asks for LMS scopes on their dev app for testing.

**Answer / steps**
1. Get the dev app's client ID and which LMS they're testing (Google Classroom, Canvas or Schoology). LMS Connect requires Secure Sync.
2. Transfer to PE and ask in #partner-engineering. In the cases, PE connected a shared sandbox per LMS to the dev app, enabled the LMS scopes, and sometimes held a live call to set up the test district.

> Thanks for reaching out! Our Partner Engineering team sets up LMS Connect test districts for your dev app, so I'm passing this to them now. Could you confirm your dev app's client ID and which LMS (Google Classroom, Canvas or Schoology) you're testing first? Once the sandbox is connected, data usually shows up in about 30 minutes.

3. If the partner got the "not available on LMS Connect" message, tell PE the app probably needs the LMS Connect tag (see Notes).
4. PE's advice to partners in the cases: these sandboxes are shared with other partners, so use the specific test users and sections PE recommends.
5. Google Classroom: PE can grant the Google Classroom Roster Import app on a sandbox. It syncs only section and course data, not users, so demo user emails must already match emails in the partner's own Google Workspace.

**Escalate if…**
- A test district still shows "not available" after the app has been set up for LMS Connect (PE handled the one case seen on a live call).
- The partner asks for a demo teacher's password or login. Don't send credentials by email. Ask PE.

**Notes / conflicts**
- Don't promise timing beyond "about 30 minutes after PE connects it".
- "Your applications are not available on LMS Connect at this time" means the app isn't configured for LMS Connect. PE has to apply a tag to the app before it can be used with LMS Connect. In case 03073230 the tag hadn't been applied. CS can't add it, so collect the client ID and LMS and transfer to PE, saying the app likely needs the LMS Connect tag.
- If the partner mixes in certification or go-live questions, route as in the certification entries (CRT-10).
- Partners starting out on LMS Connect: check paying-customer status, loop in the ASM for commercial questions (including cost), and route the technical parts to PE.

- If a test district vanishes from the Data Browser or launches suddenly error, PE may find the dev app's sandbox connection was disconnected and reconnect it (03106338). Ask for screenshots first, since one district reappeared on its own (03070364). Older cases emailed a shared demo password. That's no longer the process, so ask PE.
**Links:** https://dev.clever.com/docs/lms-connect-overview
**Source cases:** 03058715, 03070364, 03106338, 03013876, 03070506, 03073230, 03078735, 03079974, 03081504, 03070243

---

## LMS-03 · "Our Google Classroom assignment POST returns 404 'Section not found in LMS' (or a Google 'notFound'), but Canvas and Schoology work"
**Status:** Draft, needs PE review · **Resolvable by CS:** Triage only, PE makes the change

**When to use this**
- A partner posts an assignment for a section and gets a 404 on a Google Classroom district while the same payload works on other LMSs.

**Answer / steps**
1. Most likely cause (PE): the section isn't active in Google Classroom, or it doesn't exist there. Google returns a 404 in those cases. Ask the partner to check that the section's class is active in Google Classroom (not archived, suspended or still provisioned) and that it exists.
2. If it's active and still fails, collect the section ID, district ID and dev app client ID. Don't ask for secrets or tokens.
3. Transfer to PE. In the case, PE had a new Google Classroom sandbox connected and calls then returned 200. Some sections there still returned a Google "notFound" even though matching showed 100% for sections.

> Thanks for the details! I'm sending this to our Partner Engineering team with the section ID and district ID so they can check how that section maps in the LMS. Please keep your request and response logs handy in case they need them.

**Escalate if…**
- The 404 reproduces on a section PE believes is matched.

**Notes / conflicts**
- The inactive-or-missing-section cause comes from PE, not from the case thread (the case never stated a cause). Google's Classroom docs list the course states (active, archived, provisioned, declined, suspended) and say an archived course can't be modified, but they don't explicitly say that an inactive course returns a 404. Present it to partners as the most likely cause and ask them to check the class state in Google Classroom.
- Google Classroom isn't LTI. Passback goes through Google's own API and uses the same district-app token as rostering.
- Schoology grade passback: a Schoology assignment must have a grading category for the grade to show in the Schoology gradebook (confirmed by PE, from case 03018439). If a partner says grades aren't appearing in Schoology, check this first.
- See LMS-01 for the general LMS data-mismatch escalation.

- Other Google Classroom errors seen: 503 backendError and a 400 for more than 20 attachments (03110704). Collect the district and section IDs.
**Links:** https://dev.clever.com/docs/clever-lms-connect-api-errors
**Source cases:** 03110704, 03079974, 03018439

---

## LMS-04 · "We got an email asking us to approve an LTI 1.3 upgrade for a district's Schoology. Do we need to do anything?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Triage only, PE makes the change

**When to use this**
- A partner replies to or asks about an older Clever email titled "Action needed: Approve your LTI 1.3 upgrade".

**Answer / steps**
1. This outreach is no longer active (PE confirmed it was resolved and shouldn't come up again). Don't treat it as a pending request.
2. If a partner still asks about it, post in #partner-engineering with the case number, the partner and the district so PE can check, and don't promise anything. If the partner asks what LTI 1.3 launches look like, see LMS-01.

**Escalate if…**
- The partner says launches from the district's Schoology fail.

**Notes / conflicts**
- Fee or contract questions go to the ASM.

**Links:** https://dev.clever.com/docs/lms-connect-lti-13-sso
**Source cases:** 03089430, 03089885 (same email template: 03089445, 03089497, 03089513, 03089634, 03089764)

---

## LMS-05 · "What 'state' values can I set on a submission, and what do I send when a teacher grades?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Yes

**When to use this**
- A partner patching submissions asks which `state` to send when a student turns in work or a teacher grades.

**Answer / steps**
1. Accepted `state` values: created, submitted, returned, reclaimed.
2. Submission state is set automatically based on triggers. For example, when a student submits, the state becomes submitted. Sending `grade_points` on a submission PATCH sets it to returned, so the partner doesn't need to send `state` when grading.
3. Per PE, Schoology and Canvas ignore a `state` you send. The returned and reclaimed states are only supported by Google Classroom. Google Classroom allows manual `state` updates.
4. Attachments are required in the request to submit a student's attempt, not to grade.

> The accepted submission states are created, submitted, returned and reclaimed, but the state is set automatically. For example, it becomes submitted when a student submits. When you PATCH a submission with `grade_points`, Clever sets the state to returned, so you don't need to send `state` when grading. Schoology and Canvas don't support changing the state through the API, so leave `state` out for those. Returned and reclaimed are only supported for Google Classroom. Attachments are required when submitting a student's attempt, but not when grading.

**Escalate if…**
- Grades are published but don't show in Schoology after the assignment has a grading category (see LMS-03). Collect section ID, assignment IDs and student ID, then transfer to PE.
- The partner asks what returned and reclaimed mean. Both apply to Google Classroom only. In Google's own docs, returned means the teacher returned the work to the student, and reclaimed means the student turned the work in and then unsubmitted it. PE confirms Clever maps the values the same way. Point to the LMS Connect API reference for the exact fields.

**Notes / conflicts**
- PE confirmed: Google Classroom is the only LMS that supports updating `state` through a PATCH, and Canvas and Schoology don't support returned or reclaimed. Clever maps the values the same way Google does.
- A Schoology display issue in the same case went to Engineering and its end state wasn't recorded.

**Links:** https://dev.clever.com/docs/api-reference
**Source cases:** 03003437

---

## LMS-06 · "How do I unassign a student or change who an assignment is assigned to?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Yes

**When to use this**
- A partner wants to remove a student from an assignment already created in the LMS.

**Answer / steps**
1. Assignees are managed on the assignment, not by patching each submission.
2. PATCH `/sections/{id}/assignments/{id}` with `assignee_mode` and `assignee_ids`. To remove a student from an "all" assignment, set `assignee_mode` to `individuals` and send every student who should stay assigned.
3. Both fields must be sent together, and only when changing assignees. `individuals` needs at least one student ID. An empty array errors.

> Unassigning is done on the assignment, not the submission. PATCH `/sections/{id}/assignments/{id}` with `assignee_mode` set to `individuals` and `assignee_ids` listing every student who should stay assigned. Both fields need to be sent together, and the list needs at least one student ID, since an empty list returns an error.

**Escalate if…**
- The LMS shows different assignees than the API wrote (see LMS-01).

**Notes / conflicts**
- The exact error text wasn't captured, so don't quote it.

**Links:** https://dev.clever.com/docs/api-reference
**Source cases:** 03003437

---

## LMS-07 · "Google Classroom writes (update, delete, turn-in, grade) suddenly return 403 ProjectPermissionDenied, but reads and create work"
**Status:** Draft, needs PE review · **Resolvable by CS:** Triage only, PE makes the change

**When to use this**
- A partner's Google Classroom grade passback worked before and now every write returns 403 while the district shows connected and matched.

**Answer / steps**
1. Collect the dev app's client ID, the district ID, the request and response of one failing call, and when it started. Don't ask for tokens or secrets.
2. Transfer to PE and post in #partner-engineering, noting that grade passback is blocked.
3. In the one case seen, PE couldn't reproduce it, asked the partner to retry, and calls succeeded about 10 minutes later. No cause was given.

> I'm sending this to our Partner Engineering team now. Could you share the request and response for one failing call, the district ID and the time it started? Please don't include any tokens or secrets.

**Escalate if…**
- It still fails after a retry.

**Notes / conflicts**
- PE never gave a cause, and the issue cleared on its own without PE changes. It's the only case of this kind in the sample, so treat it as a one-off. Don't promise a reason or timeline.

**Links:** https://dev.clever.com/docs/clever-lms-connect-api-errors
**Source cases:** 03079120

---

## LIB-01 · "Our Library app wants the user's school or district ID"
**Status:** Draft, needs PE review · **Resolvable by CS:** Triage only, PE makes the change

**When to use this**
- A Library partner (teacher-initiated installs) asks for `district_id` or `school_id` of the signed-in user.

**Answer / steps**
1. Library questions go to #partner-engineering. Post the case number and the question. Don't promise access or a timeline.
2. What PE told the partner in the case (background): school and district IDs aren't part of Library data, since it's a teacher-initiated install for that teacher's class. Getting them takes Data API calls, which need a Clever Complete agreement. PE suggested matching the teacher's email domain against the partner's own district list as a workaround.

> I'm checking with our Partner Engineering team on this and will follow up. For now: Library is a teacher-initiated install for an individual class, so school and district IDs aren't part of that data. Getting that data requires a Clever Complete agreement.

**Escalate if…**
- Always (all Library questions).

**Notes / conflicts**
- Don't discuss price or tiers, and don't use the word "free". The email-domain workaround was one person's suggestion, not a Clever feature.

- Library installs aren't connected to a district, so apps don't get district information for that teacher. Users shared with the app at the district level can't use the Library integration (PE, older cases).
**Links:** none confirmed
**Source cases:** 02990718, 03053254, 03074537

---

## LIB-02 · "Can we use OIDC with Library?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Triage only, ask in #partner-engineering

**When to use this**
- A partner asks whether OIDC or an `id_token` works for a Library app.

**Answer / steps**
1. Don't answer on your own. Post in #partner-engineering with the case number and question. Don't promise a timeline, and don't send Library certification docs.
2. Library integrations don't support OIDC (PE confirmed). OIDC needs a token that Library doesn't provide, and this isn't expected to change since Clever isn't certifying new Library integrations. In an older case, PE suggested using OIDC for SSO and the `/userinfo` endpoint for Library, or OAuth for the whole integration, but treat that as background only and let PE advise.

> Library integrations don't support OpenID Connect. I'm checking with our Partner Engineering team on the best option for your setup and will get back to you.

**Escalate if…**
- Always.

**Notes / conflicts**
- Clever isn't certifying new Library integrations. Don't call Library or SSO "free".

- PE said Library needs OAuth 2.0 SSO (SAML isn't supported) and at least a teacher experience.
**Links:** https://dev.clever.com/docs/oidc-implementation
**Source cases:** 03053869, 03077051, 03073263, 03003713, 03061491, 03068702

---

## ANY-01 · "The AnySchool setup link says it was already used or has expired"
**Status:** Draft, needs PE review · **Resolvable by CS:** Yes (regenerating the link); triage only for anything else

**When to use this**
- A district sees "This one-time access link has already been used" or an expired-link message.
- A partner asks to extend the link or allow several CSV uploads from one link.

**Answer / steps**
1. Setup links expire 48 hours after creation and work once. Some partners found a clipboard manager or email scanner opened the link first.
2. Have the partner generate a new link from the district's page in the App Dashboard (AnySchool Rostering Configuration, then Send Email) and send it only to the person doing setup.

> AnySchool setup links expire 48 hours after they're created and can only be opened once. If a browser extension or email scanner opened the link, it shows as already used. Please generate a new link from the district's page in your App Dashboard and send it directly to the person completing setup. Let us know if it still doesn't work.

**Escalate if…**
- A fresh link fails again.
- The partner needs a district's SSO choice changed. CS can't reset it, so transfer to PE.
- The partner asks for longer expiry or multiple uploads. The expiry can't be extended at this time. Generate a new link instead.

**Notes / conflicts**
- AnySchool districts have no partner-side disconnect. Ask Clever Support or PE. A duplicate school (already on Clever, also connected through AnySchool) was removed by PE.

**Links:** none confirmed
**Source cases:** 03070863, 03071239, 03073555

---

## ANY-02 · "Our AnySchool ClassLink district shows a sync error or no data"
**Status:** Draft, needs PE review · **Resolvable by CS:** Triage first, PE/Clever Support releases holds

**When to use this**
- A ClassLink district connected through AnySchool shows a sync error, no data, or only the previous school year's term.

**Answer / steps**
1. Confirm the district's ClassLink credentials were entered on the district's page in the App Dashboard. In one case data flowed as soon as they were.
2. A new connection can show a sync error briefly. In one case it cleared within hours with no action. Ask the partner to recheck before escalating.
3. If the message is "no active academic sessions in your ClassLink instance", the district's ClassLink data must be rolled over to the new school year with the term active. Clever exposed the data only once the term dates arrived.
4. If the district wants data sooner, it can export OneRoster files from ClassLink to SFTP or upload CSV files.
5. A data hold appears when many records change at once. Ask the partner to confirm the changes are expected, then ask PE or Clever Support to release it.

> A couple of things to check. Please confirm the district's ClassLink credentials were entered on the district's page in your App Dashboard. Also, if the district's new school year hasn't started in ClassLink, Clever won't sync that data until the term is active. If they'd like data sooner, they can export OneRoster files from ClassLink to SFTP or upload CSV files. I'm checking with our team and will update you shortly.

**Escalate if…**
- Credentials are in, the term is active, and it still fails.
- A hold needs releasing.

**Notes / conflicts**
- See ROS-05 for missing data in general. The "no active academic sessions" message was only visible internally.

- If a customer says setup is done but no data arrives, first check that the email invite was actually sent to the district contact (03087006).
**Links:** none confirmed
**Source cases:** 03087006, 03089005, 03108215, 03071239

---

## ANY-03 · "ClassLink district admins aren't showing up as District Admins in Clever" (and other "do you support X in AnySchool?" questions)
**Status:** Draft, needs PE review · **Resolvable by CS:** Yes

**When to use this**
- A partner sees district-administrator users in ClassLink but no matching District Admins in Clever.
- A partner asks whether AnySchool supports a feature, field or source that isn't supported today.

**Answer / steps**
1. Users with the "administrator" role in a ClassLink OneRoster feed come into Clever as non-instructional staff. Syncing users into the District Admin role through the ClassLink sync isn't supported today.
2. Reply that it's not supported today, thank them for the feedback, and log the feedback in Salesforce. Routing it to PE wouldn't change the answer, because PE would only log the feedback too.
3. Don't share or promise any timeline, and don't offer a workaround.
4. Admin records reach an app through sharing, so if a record is missing, also check sharing.

> Thanks for flagging this! At this time, users with the administrator role from a ClassLink OneRoster feed come into Clever as non-instructional staff, not District Admins, and we don't support syncing them as District Admins yet. I've logged this as feedback for our product team, and we really appreciate it.

**Escalate if…**
- The partner reports something that looks broken rather than unsupported (for example a feature that's documented but isn't working).

**Notes / conflicts**
- In the case the partner matched users on ClassLink sisId rather than Clever ID. Clever ID must be the primary key.
- The case ended without a final fix. PE had built a one-off workaround for one district. Don't offer it.

**Links:** none confirmed
**Source cases:** 03106690

---

## LIB-03 · "We already have a Library app in production. Can we get a dev or test version to test the Library login?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Triage only, PE makes the change

**When to use this**
- An existing Library partner wants to test Library login changes in development, or says test teachers log in "by district" when they try to test Library.
- Not for partners who want to build a new Library integration (see LIB-04).

**Answer / steps**
1. Confirm the partner already has a Library app live. If not, use LIB-04.
2. Collect: the dev app name or ID that should get Library test access, the name or ID of their production Library app, what they want to test, the symptom if any, and the case number.
3. Post it all in #partner-engineering. Don't enable anything yourself, and don't email demo logins. Ask PE how to share test access.

> Since you already have a Library app in production, I'm passing your request to our Partner Engineering team so they can look at your development setup. Could you confirm which development app you'd like to use, what you plan to test, and what you're seeing right now? Please don't include any secrets or tokens.

**Escalate if…**
- Always. PE enables the Library settings on the dev app and sets up the test path.

**Notes / conflicts**
- In the cases PE enabled Library on the existing dev app and sent a direct login link to a Library sandbox district. Library apps don't show in Library search for test districts. PE removed the old Library test-plan docs, so don't link a Library testing page.
- Library installs aren't connected to a district. A test teacher showing as "by district" means the district flow is being used, not the Library flow. This is background for PE.
- One older case emailed shared demo logins. Don't copy that.

**Links:** none confirmed
**Source cases:** 03056302, 02995946, 03112719, 02990718

---

## LIB-04 · "How do I get my app into Library or listed there? My collateral page is done but 'View live page' doesn't work"
**Status:** Draft, needs PE review · **Resolvable by CS:** Triage only, PE makes the change

**When to use this**
- A partner wants to start a new Library integration, or asks about requirements, timeline or cost.
- A partner finished the Library marketing collateral page and can't view the live page, or says they already submitted something.
- A partner asks about a listing or teacher reviews without a Library integration.

**Answer / steps**
1. Don't send Library certification docs, a form or a DSA, and don't give a timeline. Don't call Library "free" or discuss cost.
2. Run the paying-customer check: paying customers go to their ASM, everyone else to application-interest@clever.com.
3. Collect: case number, partner name and app ID, the contact, whether the app has a Library integration today, what the Library/Marketing page shows, and any other emails or names the partner may have used if they say they already submitted.
4. Post in #partner-engineering and wait for PE.

> Thanks for reaching out! The Library marketing page only applies to apps that have a Library integration, and completing that page doesn't submit an app for review. Clever isn't currently certifying new Library integrations, so I'm checking with our Partner Engineering team on your situation and will follow up. I can't promise a timeline, but I'll update you here as soon as I hear back.

**Escalate if…**
- Always. The partner asks for an exception, a timeline or a form. Only PE decides.

**Notes / conflicts**
- In one case PE found no submission record for a partner who said they'd completed certification. Give PE every name and email the partner might have used.
- PE can grant exceptions in rare cases. Never offer one.
- Older cases sent the Library certification guide, DSA link and form, promised gallery or newsletter placement, and said partners would be searchable after certification. That's no longer the process.
- Don't mention legal review or a "pause" beyond "Clever isn't currently certifying new Library integrations."
- PE suggested Clever Complete to one partner. Follow the new-partner routing if it comes up.

**Links:** https://dev.clever.com/docs/integration-types
**Source cases:** 03004252, 03069335, 03048592, 03007684, 03089169, 03093397, 03110778, 03023483, 03001956, 03016257

---

## LIB-05 · "How does a Library app handle co-teachers, district precedence, SAML or native apps?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Triage only, PE answers

**When to use this**
- An existing Library partner asks how Library behaves: data for a co-teacher on a section, what happens when the district also has the app, whether SAML works, native app launches, or how to tell a Library login from a district login.

**Answer / steps**
1. Don't answer. Collect the case number, the exact question, which login type the app uses (OAuth or SAML), whether it also has a District SSO or Secure Sync integration, the section ID if relevant, and the endpoint and response (no tokens or secrets).
2. Post in #partner-engineering.

> I'm sending this to our Partner Engineering team, who handle questions about how Library integrations behave. To help them, could you share the exact endpoint you're calling and the response you get, plus whether your app uses OAuth or SAML for login? Please leave out any tokens or secrets.

**Escalate if…**
- Always.

**Notes / conflicts**
- Background confirmed by PE: Library supports OAuth 2.0 SSO and not SAML, and needs at least a teacher experience. A district-level connection takes precedence over Library for the same users, and `authorized_by` on `/me` shows which path a login used. A co-teacher's data comes in like the primary teacher's. After a teacher approves, the app can access all of that teacher's section data and students, and the teacher can't pick sections.
- One ASM reply said no full SSO is needed for Library, and another said a separate app is needed. PE corrected or contradicted both. Don't repeat them.

**Links:** none confirmed
**Source cases:** 03053254, 03077051, 03077219, 03053869

---

## ANY-04 · "Our customer isn't on Clever. How do we roster them, or remove an AnySchool district we created by mistake?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Yes for guidance, PE deletes districts

**When to use this**
- A district that isn't on Clever needs rostering for a paying partner, or a partner created a test or duplicate AnySchool district and wants it removed.

**Answer / steps**
1. Run the paying-customer check: paying customers go to their ASM, everyone else to application-interest@clever.com.
2. If the district is already on Clever, it connects normally. A district changing its SIS is a sync conversion, which goes to Clever Support only.
3. To invite a district: in the App Dashboard, Home > Invite Districts > Add an AnySchool district > enter the name > Create new district > Send email invite. The district admin chooses their SSO method during setup.
4. The "OneRoster API" source in the setup flow is ClassLink only. Other SISs use SFTP with OneRoster files. ClassLink certification is still required to connect ClassLink districts through that route, but AnySchool also supports an SFTP option, so the app and district can choose SFTP instead. See CRT-12 for the supported SIS and SSO sources.
5. To remove a test or duplicate district, get the district ID and transfer to PE. PE deleted them in both cases seen. Confirm first that the district isn't actually using AnySchool.

> You can invite a district without a Clever account from your dashboard: Home > Invite Districts > Add an AnySchool district > Create new district > Send email invite. The district admin chooses their SSO method during setup.

**Escalate if…**
- A district is duplicated or already syncing and needs to be removed.

**Notes / conflicts**
- Don't quote billing or cost details for AnySchool connections.
- ClassLink certification is still required (PE confirmed). Some replies said it isn't needed because the district can use the SFTP option instead, which is true only because SFTP is an alternative route. Don't tell a partner they can skip ClassLink certification while using the ClassLink route.
- PE said no change is needed to a partner's existing Sign in with Clever flow.
- For an AnySchool source or feature that isn't supported, say it's not supported today and log the feedback (see ANY-03).

**Links:** none confirmed
**Source cases:** 03101536, 03103705, 03095117, 03096446, 03098173, 03099783, 03073238

---

## LMS-08 · "LMS Connect assignment update returns 'UUIDs must be in v4 or v5 format' even though my UUID is valid"
**Status:** Draft, needs PE review · **Resolvable by CS:** Triage only, PE makes the change

**When to use this**
- A PATCH to an LMS Connect assignment fails with that message while the assignment ID in the path is a valid UUID.

**Answer / steps**
1. PE's explanation: only assignment and submission IDs are UUIDs. Section, term and user IDs are Clever IDs (24-character hex).
2. PE's suggestion: remove `term_id` from the PATCH body, GET the assignment first to confirm the ID and section, and retry with a minimal body.
3. If it still fails, collect status, headers and body (no tokens) and transfer to PE.

> Please remove `term_id` from the PATCH body, GET the assignment first to confirm the ID and section, and retry with a minimal body. If it still fails, send the full request and response (without any token) and I'll pass it to our Partner Engineering team.

**Escalate if…**
- It still fails.

**Notes / conflicts**
- For assignment creation the only required fields are title, due_date and description. Everything else is optional unless the specific LMS requires it (PE). The API reference lists them: https://dev.clever.com/docs/api-reference
- The partner found the call works when the assignment ID is also in the body as `id`. That came from the partner, not Clever, so don't present it as required.

**Links:** none confirmed
**Source cases:** 03080004
