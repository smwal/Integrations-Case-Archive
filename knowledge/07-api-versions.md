# API versions

Covers this Salesforce bucket: Developer Support > Versions, plus version questions from Troubleshooting - Other.
Source: resolved integrations@clever.com cases. Every entry is a draft until PE has reviewed it. New apps get v3.0, v3.1 is turned on by request through PE, and never quote deprecation dates.

---

## VER-01 · "Our calls to /v3.1 return 403 'Application does not have access to the specified API version' (or home_language is empty)"
**Status:** Draft, needs PE review · **Resolvable by CS:** Triage only. PE turns on v3.1.

**When to use this**
- A `/v3.1/...` request returns 403 with that message while the same call on v3.0 works.
- v3.1-only fields (for example `home_language`) or LMS Connect endpoints are missing or fail.
- Common cause: the dev app has v3.1 but the production app doesn't.

**Answer / steps**
1. Confirm **which client ID** is failing (dev or production).
2. Send:
   > That error means API v3.1 isn't turned on yet for that client ID. It isn't a problem with the district or your token. Could you send us the app name or client ID that's failing (the production app, if that's where you see it)? We'll get v3.1 turned on. Some fields, like home_language, are only available on v3.1.
3. Transfer to PE to turn on v3.1. Loop in the ASM if the partner has one.
4. After it's on, if a field is still empty for **one** district, PE checks whether that district sends it in an extension field instead (see SCP-03).

**Escalate if…**
- Always: PE turns on v3.1.
- The field is still empty for a district after v3.1 is on.

**Notes / conflicts**
- New apps get v3.0; v3.1 is turned on by request through PE. Partners should always put an explicit version in the request URL.
- Never quote a v2 deprecation or end-of-life date. Some older CSM/ASM emails in these cases quoted dates, and the dates didn't agree with each other. Don't repeat them.
- One older case helped a partner build Events processing on v2.1 without mentioning v3. v2 is deprecated for new apps.
- A "version not supported" (code 600) error can also mean v3.1 is turned on for the dev app but not for the production app. Confirm which client ID and token the partner is using before transferring to PE (03077144).
- PE turns on v3.1 for the app you name, so say whether it's for dev, production or both (03002235).
- Enabling v3.1 doesn't change the partner's tokens. PE enables it per app, so give PE every client ID that needs it (dev and production). Sandbox and production apps are enabled separately (03081766, 03083377, 03085732, 03055415).

- A cloned dev app returning "version not supported" on v3.1 was fixed by PE adding the v3.1 scopes (03085674). PE may reply that v3.1 is already enabled (03011569), so check before transferring.
**Links:** https://dev.clever.com/docs/whats-new-in-api-v31 · https://dev.clever.com/docs/api-v31 · https://dev.clever.com/docs/api-v3-upgrade-guide · https://dev.clever.com/docs/error-messages
**Source cases:** 03085674, 03011569, 03084169, 03108028, 03054310, 03078915, 03083390, 03079283, 03077144, 03002235, 03081766, 03083377, 03085732, 03055415

---

## VER-02 · "How do I get access to v3.1? Is it automatic, or do I have to ask?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Triage only, PE makes the change

**When to use this**
- A partner or ASM asks how to start using v3.1, or whether it's on automatically.
- The partner wants the new student fields (for example `section_504_status` or `gifted_status`) or the LMS Connect endpoints.

**Answer / steps**
1. v3.1 isn't automatic. New apps start on v3.0, and v3.1 is turned on per app by request through PE.
2. Collect the client ID of each app that needs it (dev and production).
3. Transfer to PE. Loop in the ASM if the partner has one.
4. After PE confirms, explain the field steps: add the new fields on the app's Data Access tab, and the district has to share them in its Field Access settings and have the data syncing to Clever.

> Thanks for reaching out! v3.1 isn't turned on automatically. It's enabled per app by our Partner Engineering team. Could you send the client ID for each app that needs it (dev and production, if both)? I'll get this over to them. Once it's on, you can add the new fields on your app's Data Access tab, and each district will also need to share those fields with your app in its Field Access settings.

**Escalate if…**
- Always, to turn on v3.1.
- The partner says it was already enabled and still sees errors (see VER-01).

**Notes / conflicts**
- PE told one partner the new demographic fields are only available to Secure Sync apps. Other app types can still get v3.1 but won't see those fields.
- v3.1 adds the LMS Connect APIs and the extra demographic fields. An SSO or identity-only partner can stay on v3.0 (03081977).
- One ASM asked partners to commit to v3.1 by tying it to a v2 end date. Never repeat deadline language.
- See ROS-06 for which fields are gated by scopes and which by v3.1.

**Links:** https://dev.clever.com/docs/api-v31 · https://dev.clever.com/docs/api-v3-upgrade-guide
**Source cases:** 03007759, 03009671, 03051380, 03081977, 03083377, 03116247

---

## VER-03 · "When will v2 stop working? Do we have to move to v3?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Yes for the basics. PE for migration help.

**When to use this**
- A partner on v2.0 or v2.1 asks for a deprecation or sunset date, whether v2 will keep working, or whether a v4 is coming.
- A CSM or ASM has told a partner their app still makes v2 calls and PE is asked to help.

**Answer / steps**
1. Don't give any date. Clever hasn't published a sunset date.
2. Say v2 is deprecated, new apps can't use it, existing apps are still supported, and upgrading to v3 is recommended. Clever is moving partners from v2 to v3, and it no longer certifies apps built on v2.

> Thanks for asking! v2 is deprecated, so we recommend moving to v3. Existing apps are still supported, but I can't give you a date. Our upgrade guide covers the changes, and our Partner Engineering team can help you plan the move. If you share your target timeline, they can also pull a report of the v2 calls your app is making.

3. Facts PE gave partners that you can relay:
   - Districts don't need to do anything for the app to move to v3. They may need to sync data to Clever only if the partner wants new v3 fields (such as 504 status).
   - The same credentials and tokens keep working, and there's no new app registration.
   - Contacts are `users?role=contact` on v3.
4. For a v2 call report or sandbox testing, transfer to PE.

**Escalate if…**
- The partner wants a v2 call log or help scoping migration work.
- The partner asks for written confirmation of any date. Say you can't provide one, and loop in PE and the ASM.
- The partner asks whether a v4 is planned. PE once said there were no plans, but don't repeat that as a promise.

**Notes / conflicts**
- Older cases said there was no plan to deprecate v2.1, or quoted end dates that didn't agree with each other. Neither is correct to repeat. Give the answer above only.
- One older case said only the contacts endpoints were deprecated. v2 overall is deprecated.
- Billing and contract details sit in some of these threads. Leave them out and route to the ASM.

- v2 and v3 can run side by side during migration. A partner shouldn't turn off v2 calls until the same calls work on v3, and should test in a dev app first (03045665). Enabling v3 doesn't change anything for districts (03060600).
**Links:** https://dev.clever.com/docs/api-v3-upgrade-guide
**Source cases:** 03060600, 03045665, 02972640, 02979711, 03037042, 03078710, 03078725, 03078801, 03078978, 03083590, 03090792, 03105518

---

## VER-04 · "What changes from v2 to v3? Is `legacy_id` always there, and how do teacher and admin IDs work?"
**Status:** Draft, needs PE review · **Resolvable by CS:** Yes for the points below. Triage only for the rest.

**When to use this**
- A partner migrating from v2 asks how IDs, roles or `legacy_id` behave in v3, or which version to target.

**Answer / steps**
1. v3 consolidates users into one `/users` record with multiple roles. In v2.1, teachers and admins were separate entities with separate IDs.
2. `legacy_id` is always present on v3 users. It's the v2 ID for that user's role. Students have the same `legacy_id` and `id` because a student has only one role. Other user types have a different value.
3. If a partner stays on v2.1 for now and has to match one person across roles, PE suggested email with a first and last name fallback. That's a v2 stopgap only.
4. Version choice: v3.0 is enough for identity and SSO data. v3.1 adds the LMS Connect APIs and the extra demographic fields.
5. `/v3.0/sections/{section_id}/users` supports a `role` parameter (for example `?role=teacher`) and returns only the users with that role. If a teacher and a student query look identical, check how many users the section has. In 03085732 the section only had three users.
6. Per the data model docs: custom `ext` fields sit inside each role object (for example `roles.teacher.ext`, `roles.staff.ext`, `roles.student.ext`), and `iep_status` is `roles.student.iep_status`. It's a sensitive field that needs extra scopes and Secure Sync.

> In v3, a person with more than one role shows up as a single user record. `legacy_id` is always included and holds their v2 ID for that role. Student IDs don't change. Our upgrade guide walks through the details: https://dev.clever.com/docs/api-v3-upgrade-guide

**Escalate if…**
- The partner asks something the docs don't answer. One partner asked how section teacher and student references use new versus legacy IDs, and whether `ext` fields keep the same keys and values on staff roles. The cases don't answer these, so send them to PE.

**Notes / conflicts**
- The Clever ID must be the primary key to be certified. Never recommend email as the main key.

- v2 contacts partners: in v3 contacts get the multi-role user ID, students keep their ID (the v2 ID appears as `legacy_id`), and `/users/{id}/mycontacts` works with the student ID (03000078). Point to the v3 docs and never quote dates.
**Links:** https://dev.clever.com/docs/users · https://dev.clever.com/docs/new-in-api-v3 · https://dev.clever.com/docs/api-v3-upgrade-guide · https://dev.clever.com/docs/api-v31
**Source cases:** 02997258, 03014599, 03000078, 03072620, 03083855, 03081977, 03085732, 03105518

---

## VER-05 · "Our new dev app gets 'version not supported' on /v2.1"
**Status:** Draft, needs PE review · **Resolvable by CS:** Triage only, PE makes the change

**When to use this**
- A newly created dev app calls a v2.0 or v2.1 URL and gets code 600 "version not supported", while an older app works.

**Answer / steps**
1. Explain the cause: new dev apps start on v3.0 only, so v2 calls fail.
2. Tell the partner v2 is deprecated and not for new apps. Ask them to call `/v3.0/...` with an explicit version in the URL.
3. Don't offer to add v2 to a new dev app. Only v3.0 is added automatically to new apps, and v2.0 and v2.1 aren't added automatically. If the partner insists they need v2 on an app tied to an existing v2 production app, don't promise it. Confirm the client ID and ask in #partner-engineering.

> Thanks for flagging this! New developer apps start with access to API v3.0, which is why a v2.1 call returns "version not supported". Please try the same call against v3.0. If you have an existing production app on v2 and are in the middle of moving to v3, send me the dev app's client ID and I'll check with our Partner Engineering team.

**Escalate if…**
- The partner needs v2 on a dev app tied to an existing v2 production app: ask in #partner-engineering.
- The same error appears on `/v3.1` (see VER-01).

**Notes / conflicts**
- Older cases (02977090, 02984331, 03035027) had PE add v2.0 or v2.1 to new dev apps. That's no longer the process: only v3.0 is added to new apps. Some older apps still have v2 access, v2 is deprecated, and Clever is moving partners from v2 to v3.
- Version access can't be changed in the app dashboard. Requests go through PE.
- In 02984331 the partner also saw fewer scopes than expected. That's a separate scope issue for PE (see SCP-01).

**Links:** https://dev.clever.com/docs/error-messages
**Source cases:** 02977090, 02984331, 03035027
