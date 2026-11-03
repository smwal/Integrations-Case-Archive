# Test results: Developer Support project (run graded 2026-11-03)

Scores are 0-2 (No policy talk and Honest about gaps are 0 or 2). Critical = wrong/invented fact, promises PE work CS can't promise, policy or POL ID mentioned, form or survey link, quoted limit/date/fee. Pass = no critical miss, Correct >= 1, Usable >= 1.

Result: **27 of 27 pass, 0 critical misses.** 22 are full marks. 5 are partial (A3, A9, B7, B10, plus watch-items on A1/A2/B5/B8/B9 that don't reduce scores but should be fixed).

| # | Correct | Usable | Owner | No policy talk | Gaps | Critical | Entries cited | Partner reply: jargon / off-allowlist links | Notes |
|---|---|---|---|---|---|---|---|---|---|
| A1 | 2 | 2 | 2 | 2 | 2 | No | SBX-01 | Link outside allowlist: `clever.com/in/<district-shortname>` | Follows invite/forgot-password flow, no password sent. Says "Both entries are still Draft" but only one entry is cited (wording slip). Doesn't tell CS it can add dashboard team members itself (SBX-14), only "an existing owner or PE" (SBX-01). |
| A2 | 2 | 2 | 2 | 2 | 2 | No | SBX-02, SBX-04 | Link outside allowlist: `apps.clever.com/<app-shortname>/settings/integration` | Both CSV and custom data create Events, Secure Sync only, 180 day / five-file rule all present. Flags Draft. |
| A3 | 1 | 2 | 2 | 2 | 2 | No | SBX-03 | None | No limit quoted (good). Missing key point: sandboxes are a Clever Complete / Secure Sync feature. Answer only says CS needn't check eligibility. |
| A4 | 2 | 2 | 2 | 2 | 2 | No | SBX-06, SBX-04 | None (schools.clever.com only) | Matches entry. No real passwords. |
| A5 | 2 | 2 | 2 | 2 | 2 | No | SSO-01 | None | No invented setting. Reply says "We can set a flag" (entry wording), mild commitment of PE work. |
| A6 | 2 | 2 | 2 | 2 | 2 | No | SSO-03, SSO-05 | None | Only entry causes; asks for client ID, timestamp, redirect_uri. |
| A7 | 2 | 2 | 2 | 2 | 2 | No | SSO-05, SSO-03 | None | No dashboard setting, Universal Links / assetlinks.json. "device policy" is the device's MDM policy, not a Clever policy. |
| A8 | 2 | 2 | 2 | 2 | 2 | No | VER-01, VER-02 | None | v3.0 default, v3.1 by request, confirm client ID, transfer to PE, loop in ASM, no dates. Reply says "We'll get v3.1 turned on" (entry wording). |
| A9 | 1 | 2 | 1 | 2 | 2 | No | SCP-01, ROS-06, VER-02 | None | PE makes the change (stated). Missing: paying-customer / Secure Sync check (rubric asks for it). Reply opens "We can turn on access..." before any check, which over-promises for a non-paying or SSO-only partner. Good: doesn't say whether districts must re-authorize. |
| A10 | 2 | 2 | 2 | 2 | 2 | No | SCP-02, SCP-08, SBX-05 | None | Correct SSO-only vs Secure Sync diagnosis, paying-customer check, no invented scopes. |
| A11 | 2 | 2 | 2 | 2 | 2 | No | CRT-03, CRT-01, SCP-04 | None (support.clever.com only) | Escalates, no go-live date. Odd but entry-driven: tells an already-certified partner "certifications can take up to two weeks" when PE hasn't created the prod app. |
| B1 | 2 | 2 | 2 | 2 | 2 | No | CRT-01 | None | Exact two-week reply, Slack post fields, no form/survey. |
| B2 | 2 | 2 | 2 | 2 | 2 | No | CRT-06, CRT-12 | None | No SSO-only offered, existing SSO-only noted, paying check. Reply never says "SSO-only isn't offered" outright. |
| B3 | 2 | 2 | 2 | 2 | 2 | No | LIB-04, CRT-06, LIB-03 | None | Ask in #partner-engineering, no Library docs, no timeline. |
| B4 | 2 | 2 | 2 | 2 | 2 | No | CRT-06, CRT-12 | None | Four docs plus paying check. Correctly drops POL-04 optional extras (clever.com/schema, /pricing) from the reply. |
| B5 | 2 | 2 | 2 | 2 | 2 | No | VER-03, VER-04 | None | No dates. Watch: agent text says "Clever hasn't published a sunset policy" (word "policy"; from VER-03). Reply says PE "can pull a report" (entry wording). |
| B6 | 2 | 2 | 2 | 2 | 2 | No | SSO-08, CRT-04, CRT-01 | None | Blocker stated. Mentions older reviews allowed caveats only to call it outdated. |
| B7 | 1 | 2 | 2 | 2 | 0 | No | SSO-09, SSO-03, ROS-08 | None | Posts to #partner-engineering immediately, rotate, 24h reset. But reply tells partner "users won't need to do anything" as fact; SSO-09 Notes says that came from PE once and was untested (and C6 itself lists it as unconfirmed). Overconfident, inconsistent with C6. |
| B8 | 2 | 2 | 2 | 2 | 2 | No | SBX-03, SBX-02 | None | No limit, `#DEMO <app name>`, unique email. Agent text mentions "a limit of 5" (framed as outdated). Better to say "an old limit was removed" without the number. |
| B9 | 2 | 2 | 2 | 2 | 2 | No | SBX-09 | Signup URL `apps.clever.com/signup` is in the steps CS relays | All approvals in order, deny rule, no troubleshooting promise. |
| B10 | 2 | 2 | 1 | 2 | 2 | No | SBX-09, SBX-03 | None | Correctly says out of scope, not a District App. Owner is fuzzy ("your team's process") because the archive has no routing for district sandbox requests. |
| C1 | 2 | 2 | 2 | 2 | 2 | No | CRT-07, ROS-10, ROS-14 | None | Test collides with CRT-07 (webhooks), so it answered from an entry. It did branch to "archive doesn't cover it" with a collect list and flagged Draft. Pick a truly uncovered topic for this test. |
| C2 | 2 | 2 | 2 | 2 | 2 | No | ROS-05, ROS-06, ROS-07, ROS-11, SCP-07, SCP-09 | None | Escalates, collect list, no cause or timeline promised. |
| C3 | 2 | 2 | 2 | 2 | 2 | No | LIB-04, LIB-03, CRT-06 | None | Answers "policy" question with no policy/POL mention. |
| C4 | 2 | 2 | 2 | 2 | 2 | No | CRT-06, CRT-01, CRT-11, SSO-11 | None | Old reply called outdated, current answer given. |
| C5 | 2 | 2 | 2 | 2 | 2 | No | none | None | Asks for details, no guessing. |
| C6 | 2 | 2 | 2 | 2 | 2 | No | SSO-09, SCP-01, ROS-07, DSH-01, DSH-04, SBX-01 | None | Examples verified against the entries. Honest, but see B7 (didn't follow its own advice on SSO-09). |

## Findings and fixes (priority order)

### 1. B7 / SSO-09: unconfirmed claim sent to partner as fact (entry content, conflicting entry)
SSO-09 step 3 and its quoted reply state "users won't need to do anything", while its Notes say this came from PE once and was untested. B7 repeated it unhedged; C6 flags the same claim as unconfirmed. Fix in `knowledge/04-sso-and-oidc.md`, SSO-09: remove that sentence from the quoted partner reply and step 3 (or reword to "we'll confirm whether anything changes on the user side"), keep the Notes flag. Add to PROJECT_INSTRUCTIONS "Style": "Never put a statement the entry's Notes mark as unconfirmed into a ready-to-send reply; hedge it or leave it out."

### 2. A9 / SCP-01: no paying-customer or Secure Sync gate; reply over-promises (entry content, instruction wording)
SCP-01 says "We can turn on access to additional demographic fields" with no check. VER-02 Notes and VER-04 say the demographic fields are Secure Sync only. Fix in `02-scopes-and-data-access.md` SCP-01 step 1/4: add "Check the partner is a paying customer with Secure Sync (Paying Customer tag). Not paying: application-interest@clever.com. Paying: loop in ASM. Only then use the reply." Soften the reply to "I'll check with our team on turning on access to additional demographic fields...". Add the Secure Sync requirement to the answer text.

### 3. A3 / SBX-03: "Clever Complete / Secure Sync only" key point dropped (entry content)
SBX-03 step 5 puts it in the same sentence as "no per-app limit" and the next line "CS doesn't need to check eligibility" overrides it. Fix: add one plain line under the "Extra sandbox" step: "Custom sandboxes are available to Clever Complete / Secure Sync apps. CS doesn't need to verify this before collecting details for PE." Same wording in B8 context (SBX-02/03 Notes).

### 4. Partner-reply links outside dev/support/schools.clever.com (instruction vs entry conflict: A1, A2, B9)
Entries that tell the agent to put these in partner text: SBX-01 reply (`clever.com/in/<district-shortname>`, also SBX-06, SSO-06), SBX-02 Events reply (`apps.clever.com/<app-shortname>/settings/integration`), SBX-09 / POL-07 (`apps.clever.com/signup`), POL-04 optional extras (`clever.com/schema`, `/pricing`), DSH-01 (`clever.com/oauth/saml/...`). The assistant obeyed the rule in B4 but not in A1/A2. Fix one way, consistently: either extend the allowlist in PROJECT_INSTRUCTIONS Style to "Clever-owned sandbox and dashboard URLs that appear in an entry's quoted reply (clever.com/in/..., apps.clever.com/...)", or rewrite those replies to describe the path ("open your app dashboard > Settings > Integration"). Recommended: extend the allowlist for those two hosts and state that clever.com/schema and /pricing stay agent-only.

### 5. B10: no routing for district sandbox requests (missing entry)
POL-07 says out of scope but not who owns it. Add a line in `00-current-policies.md` "Where to ask" (and SBX-03 Notes): "District asking for a sandbox: handled by [team/process, owner to fill in], not PE." Until then the assistant can only say "use your team's process".

### 6. Smaller wording fixes
- VER-03 (`07-api-versions.md`): change "Clever hasn't published a sunset policy" to "Clever hasn't published a sunset date" so the word "policy" never surfaces (B5).
- POL-07 / SBX-03 / B8: say "the old per-app limit was removed" without the number 5 in agent-facing text, since A3/B8 rubric treats quoting a limit as a miss.
- A1 wording: "Both entries are still Draft" when one is cited. Add to instructions: "Say 'this entry is still Draft' for the entries you actually cite."
- Test design: C1 collides with CRT-07 (webhooks); replace with a topic that has no entry. A9/B-set rubrics (TEST_SET.md) say "paying-customer check if relevant" for A9, which the entries don't support: align after fix 2.

### Cross-entry inconsistencies and entries that contradict current rules
1. **SSO-09 internal contradiction** (step 3 and quoted reply vs Notes), see fix 1.
2. **Docs link `/docs/district-getting-started`**: SCP-02 Notes says it was a "page not found" for Secure Sync partners, while SBX-09 and POL-07 send districts to it as the support resource. Confirm the URL works or give SBX-09 another link.
3. **Who can add dashboard team members**: SBX-14 says CS does it (impersonate in Hall Monitor, then Team page). SBX-01 step 6 and DSH-03 say an existing owner adds them, or PE. A1 gave only the owner/PE route. Align SBX-01 and DSH-03 with SBX-14.
4. **Library routing order**: POL-03 says just ask in #partner-engineering; LIB-04 and SBX-03 add a paying-customer check first. Answers (B3, C3) did both, which is fine, but state the order once in POL-03.
5. **CRT-01 vs CRT-03**: CRT-01 tells CS to run the paying check and send "certifications take up to two weeks" even when the partner says they're already certified without a production app. A11 followed it. Consider a one-line note in CRT-01 that for an already-certified partner the reply should say PE is checking, not quote review time.
6. **Screen-recording guidance**: CRT-09 says don't ask for a recording up front; CRT-15's reply asks for one up front.
7. **SBX-03 eligibility wording**: "Clever Complete / Secure Sync feature" plus "no eligibility check" read as contradictory (cause of A3 miss).
8. **POL-04 optional extras and POL-07/SBX-09 sign-up URL** conflict with the instructions' link allowlist (fix 4).
9. **Status lines**: several entries (SSO-01..06, ROS-*, ANY-*) lack a "Status: Draft" line although file headers say every entry is a draft. Harmless but means the assistant can't cite per-entry draft status. Add the line or keep the file-level statement and say so in the instructions.
No entry contradicts the current rules on SSO-only, Library, certification survey links, v2 dates, or the sandbox limit; those entries were already updated.
