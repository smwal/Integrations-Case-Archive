# Handoff: Developer Support Project (Integrations Case Archive)
*Last updated Nov 3, 2026. Replaces the Oct 22 handoff.*

## What this is
A Claude project that helps CS agents answer integrations@clever.com questions. It is fed by markdown knowledge files mined from resolved Salesforce cases (Partner Engineering / T2 Developer Support). Entries are drafts until PE reviews them.

## Where things are
Folder: `~/Documents/Integrations Case Archive/`
- `knowledge/`: files uploaded to the project (00 to 08, 11, 99).
- `PROJECT_INSTRUCTIONS.md`: pasted into the project's Instructions field.
- `CHANGED_FILES.md`: the clean upload list and steps.
- `TEST_SET.md`: 27-question test set with rubric (never upload).
- `HANDOFF.md`: this file.
- `_agent/state.md`: bucket table and a full run log with every PE answer and open question (never upload).
- `_agent/Cases_Partner_Engineering_past_2yrs.csv`: case list with PII (never upload).
- `_agent/test_questions.md`, `test_answers.md`, `test_results.md`: the last test run.
- `_agent/SKILL-copy.md`: stale reference copy of the skill. Ignore it.

Skill: `integrations-case-archive` (stored in Stephen's account; edit only with `save_skill` overwrite true, which needs the full SKILL.md). Scheduled tasks: weekly (Mondays, folds in the last 7 days of resolved cases) and a manual backfill batch.

Google files (Stephen's Drive):
- Feedback tracker sheet: https://docs.google.com/spreadsheets/d/1EG_QQwfzJk3fLlfs3ONKwDWyOhX0C5zrjunWESfYYRc/edit (tabs: Instructions, Feedback Log; Status values New, To-Do, Update In Progress, Updated).
- Older feedback tracker doc (can be trashed): https://docs.google.com/document/d/17_QMrjWSLEpGpOV21z0pubSWcTIU_E31j28eb6SVTY4/edit

## Knowledge files and ID prefixes
| File | Entries |
|---|---|
| 00-current-policies.md | POL-01 to 08 (source of truth, never cited in replies) |
| 01-sandbox-and-dev-accounts.md | SBX-01 to 16 |
| 02-scopes-and-data-access.md | SCP-01 to 11 |
| 03-app-settings-and-visibility.md | APP-01 to 08, APP-10 (APP-09 removed, IDs never reused) |
| 04-sso-and-oidc.md | SSO-01 to 13 |
| 05-rostering-and-secure-sync.md | ROS-01 to 20 |
| 06-certification-and-onboarding.md | CRT-01 to 15 |
| 07-api-versions.md | VER-01 to 05 |
| 08-lms-connect-library-anyschool.md | LMS-01 to 08, LIB-01 to 05, ANY-01 to 04 |
| 11-app-dashboard-sign-in.md | DSH-01 to 04 (the app team's own dashboard sign-in, separate from partner-app SSO) |
| 99-emerging-topics.md | empty until the weekly run fills it |

IDs are never renumbered or reused. Entry template: When to use this, Answer / steps (with paste-ready quoted replies), Escalate if, Notes / conflicts, Links, Source cases. Status line says "Draft, needs PE review".

## What was done this session
- Backfill finished from the CSV for every bucket, including the small CSV-only buckets (two batches, 59 cases) and the Troubleshooting - Other second pass (100 cases, two subagents of 50). Counts are in `_agent/state.md`.
- Applied PE answers after each run, and rewrote or removed entries when PE said so (for example APP-09 removed, SCP-11 rewritten, ANY-03 rewritten).
- Ran the 27-question test set by simulating the project (one subagent answered from the instructions and files, another graded). Result: 27 of 27 pass, 0 critical misses. Partial scores (A3, A9, B7, B10) were fixed in SBX-03, SCP-01, SSO-09 and the district sandbox routing.
- Built the feedback tracker sheet and a short Slack message for PE (review the entries, try questions, report misses).

## Rules that shape the entries (all in 00-current-policies.md and PROJECT_INSTRUCTIONS.md)
- Never mention policies or POL IDs in project replies.
- New integrations need Clever Complete. No new standalone SSO-only. Never call SSO or Library free.
- Library: still a product, no new certifications. Every Library question goes to #partner-engineering. LIB entries only collect details and route.
- New partner routing: paying-customer check (Hall Monitor Paying Customer tag). Paying goes to the ASM, otherwise application-interest@clever.com.
- Certification: paying check, then "up to two weeks" (only for partners waiting on a review), then post in #partner-engineering with case number, partner and app ID, contact. If the partner says they are already certified: check for a production app. None: post in #partner-engineering. Has one: troubleshoot visibility or connection (CRT-03, APP-06). Never send forms or survey links. CS never asks for screen recordings for certification.
- API versions: v3.0 default, v3.1 by request through PE, v2.x deprecated, no v2 on new apps, no certification of v2 apps, never quote deprecation or sunset dates.
- Sandboxes: no per-app limit. Events are Secure Sync only. Production data is not allowed in sandboxes.
- Sync conversions go to Clever Support only, with a fixed deferral sentence.
- Missing data: CS triages first. Escalate if the whole integration or all districts are affected.
- App-side filter, billing and contract problems go to the ASM (#app-success).
- Unsupported feature asks: reply "not supported today, thanks for the feedback" and log feedback in Salesforce (no PE routing). Do not advertise sharing-rule workarounds to CS.
- CS owns: icon and logo changes (Hall Monitor, app, Advanced tab, Icon, logo is right under it; PNG only, icon 200x200, logo under 500x100), adding team members to dev apps and sandbox districts (impersonate an existing member in Hall Monitor, add on the Team page, ASM via #app-success for questions), resetting rejected district invites (APP-07), district sandbox requests (questions or special requirements go to #partner-engineering).
- Links in partner replies must be Clever-owned (clever.com, apps.clever.com, dev, support, schools). Internal Guru cards are allowed for the agent only.

## What the user still needs to do
1. Replace all files in the project with the ones listed in `CHANGED_FILES.md`, and re-paste `PROJECT_INSTRUCTIONS.md` into Instructions.
2. Share the feedback sheet and project with PE, and post the Slack message.
3. Have PE review entries (all are drafts) and answer the open questions logged in `_agent/state.md`.
4. Optionally trash the older feedback doc.

## Open items worth checking
- SBX-14: CS adds team members without verifying historically. Suspicious requests go to the ASM.
- CRT-13: attendance data needs Clever Complete and v3.1. Access is by request.
- LMS-07 (Google 403 on writes) is a one-off that cleared on its own. LMS-08 required fields: title, due_date, description.
- Any PE questions still unanswered are in the latest run-log lines of `_agent/state.md`.
- Secrets seen in old cases (case numbers only): 03092064, 02969208, 03119673, 03058715, 03048873, 02970975, 03036096, 02988644. None are in the knowledge files.
- Remaining optional work: second passes on the big buckets (unread cases left: Sandbox District 48, Certifications - Secure Sync 44, Troubleshooting - SSO 42, Best Practices - Rostering 37, Troubleshooting - Rostering 37, Best Practices - SSO 31, Getting Started 18, New Dev Account 29). Likely to add case numbers more than new entries.

## How to resume
- New backfill pass: read `_agent/state.md`, use the skill, filter the CSV (channel integrations@ or integrations+cert@, product Partner Engineering or T2_Developer Support, status Closed, Solved or Resolved), exclude case numbers already cited in `knowledge/`, spread by date, prefer cases with outbound replies, one subagent per 25 to 50 cases, merge into existing entries first and only draft a new entry for a new pattern.
- After every run: update `state.md` and `CHANGED_FILES.md`, then run `grep -rnE "POL-[0-9]|per policy|current policy|current-policies"` over `knowledge/` and `PROJECT_INSTRUCTIONS.md`. The only expected hits are inside 00-current-policies.md and two intentional lines in PROJECT_INSTRUCTIONS.md.
- Re-test: run the questions in `_agent/test_questions.md` through the project and grade against `TEST_SET.md`.
- Feedback loop: read the Feedback Log tab, fix the entry or instruction, set Status to Updated, then update the upload list.
