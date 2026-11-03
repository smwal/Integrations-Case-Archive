# Developer Support project: test set (draft v1)

**Do not upload this file to the project.** Questions are written the way a CS agent would ask them, with no PE-level context. Run each one in a fresh chat in the project, then have a PE grade the answer.

## How to grade
Score each answer 0–2 on each of these. A "critical" miss is an automatic fail for that question.

| Check | 2 | 1 | 0 |
|---|---|---|---|
| **Correct** | Matches the key points | Mostly right, minor gap | Wrong or invented (critical) |
| **Usable by CS** | Explanation + paste-ready partner reply + what to collect/escalate | One of these missing | Vague |
| **Right owner** | Clear on what CS can do vs. what PE must do | Fuzzy | Wrong owner (critical if it promises PE work) |
| **No policy talk** | No mention of policies or POL IDs | n/a | Mentions a policy or ID (critical) |
| **Honest about gaps** | Flags drafts, conflicts or missing coverage | n/a | Overconfident |

Also record: which entry ID(s) the answer cited, and whether the partner reply contained any internal jargon, customer names or links outside dev.clever.com, support.clever.com or schools.clever.com.

---

## A. Questions the archive should answer (expected entry in brackets)

| # | CS agent asks | Key points a correct answer must include | Must NOT |
|---|---|---|---|
| A1 | A partner says they can't log in to their sandbox district's admin account and wants us to reset it or give them admin access. What do I do? [SBX-01] | Follow the entry's steps (password reset / invite flow). Never set a password and email it. | Set or send a password |
| A2 | Partner changed test data in their sandbox. Will that create Events? [SBX-02] | Both CSV re-uploads and custom data edits create Events, for Secure Sync only. If no full sync in ~180 days, all five required files must be uploaded together. | Say edits don't create Events |
| A3 | Partner's sandbox is stuck on "Requested." Can they get another one? [SBX-03] | Troubleshoot per entry. Extra sandbox: there's no per-app limit and no eligibility check, but sandboxes are Clever Complete / Secure Sync only. | Quote any per-app limit (e.g. 5) |
| A4 | Why does the login say there's no account for a sandbox test student? [SBX-06] | Follow the entry's steps for logging in as a test student or teacher. | Ask for or send real users' passwords |
| A5 | Why is `email_verified` false in the Clever ID token? [SSO-01] | Explanation from the entry. | Invent a setting to change it |
| A6 | Partner gets `invalid_grant` when exchanging the code. [SSO-03] | Per the entry: check the code is single-use and used promptly, redirect URI matches, correct token endpoint. Ask for the details the entry lists. | Guess at causes not in the entry |
| A7 | Partner wants Clever to open their native app instead of the website. [SSO-05] | There's no dashboard setting. The device decides: Universal Links on iOS, assetlinks.json on Android/Chromebook. | Say CS or PE can toggle a setting |
| A8 | Partner's /v3.1 calls return 403 "Application does not have access to the specified API version." [VER-01] | New apps are v3.0 by default. v3.1 is enabled on request: confirm the client ID, then transfer to PE, loop in the ASM if they have one. | Offer to turn it on ourselves; quote deprecation dates |
| A9 | Can you add a demographic field scope to this partner's app? [SCP-01] | Collect what the entry asks for. State who makes the change (PE vs CS). Route using the paying-customer check if relevant. | Promise it's done or give a timeline the entry doesn't support |
| A10 | Partner says their dev app can't see schools or sections in the sandbox. [SCP-02] | Troubleshooting steps from the entry (scopes, district connection). | Invent scope names |
| A11 | Partner is certified. Why isn't their app showing in production? [CRT-03] | Follow the entry. Escalate if production is affected. | Promise a go-live date |

## B. Current-rule questions (the right answer depends on the rules, not just past cases)

| # | CS agent asks | Key points | Must NOT |
|---|---|---|---|
| B1 | A partner submitted certification last week and asks for status and a date. | Check whether they're a paying customer. If not, route to application-interest@clever.com. If so, reply "up to two weeks", then post in #partner-engineering with case #, partner name, app ID, contact. | Give any other timeline; send a survey, form or Airtable link |
| B2 | A new company wants an SSO-only integration, just login, no rostering. | New standalone SSO-only isn't offered. SSO comes with SSO + Rostering under Clever Complete. Then check for a paying customer and route. | Offer SSO-only or call SSO free |
| B3 | A partner asks to certify a new Library integration. | Library is still a product but no new certifications. Ask in #partner-engineering. | Send Library docs; promise a timeline |
| B4 | A prospect asks "how do I get started with Clever?" | Send the getting-started, what-is-clever, integration-types and certification-overview docs. Paying-customer check: not paying → application-interest@, then solve; paying → their ASM. | Skip the paying check |
| B5 | Partner asks when v2 will be shut down. | v2.x is deprecated for new apps, existing apps still supported. Don't give dates. | Quote any date or call v2 "valid" |
| B6 | A partner's app keys users on email, not Clever ID. Will it still be certified? | No. Clever ID as primary key is required and is a blocker. | Say older reviews allowed it with caveats |
| B7 | Partner emailed us their client secret while troubleshooting. | Post in #partner-engineering immediately. Tell the partner to rotate the secret. Clever resets exposed secrets within 24 hours. Never ask for secrets over email. | Wait for the case to be transferred |
| B8 | A partner asks if they can get a 6th sandbox on their app. | No per-app limit (the old limit was removed). Create it with a unique admin email, named `#DEMO <app name>`. | Cite a limit or say it needs approval |
| B9 | A district admin asks for their own developer app to pull their data via the API. [SBX-09] | District App process: confirm they're a district owner, get intended use, School Success rep + CS Manager approval, then signup, then #partner-engineering request. Deny if connecting to other districts (route to application-interest@). | Skip approvals; promise API troubleshooting support |
| B10 | A district asks for a sandbox. | CS handles district sandbox requests. Questions or very specific requirements go to #partner-engineering. | Handle it as a District App; send it to PE to build |

## C. Honesty and edge cases

| # | CS agent asks | Key points | Must NOT |
|---|---|---|---|
| C1 | Partner asks how to configure webhooks for a feature we haven't covered (pick a topic with no entry). | Says the archive doesn't cover it, recommends Partner Engineering, lists what to collect. | Invent endpoints, settings or behavior |
| C2 | Partner's production app is returning wrong data for several districts. | Recommends escalating to PE even if an entry partly applies. | Resolve it only from archive steps |
| C3 | "What does our certification policy say about Library?" (agent asks directly about policy) | Gives the guidance (ask in #partner-engineering, no new certifications) without citing a policy or POL ID. | Say "per policy" or "POL-03" |
| C4 | "An old case from 2025 says SSO-only certification is fine. Can I use that reply?" | Says that guidance is outdated and gives the current answer. | Reuse the old reply; cite a policy |
| C5 | Agent asks a vague one-liner: "Partner says it's not working." | Asks for the specific details to collect (what, which environment, client ID, error text) rather than guessing. | Give a confident answer to an undefined problem |
| C6 | Agent asks about something a draft entry says but marked unconfirmed in Notes. | Flags that the entry is a draft or unconfirmed before the agent promises it. | Present it as confirmed |

---

## Results log
| # | Correct | Usable | Owner | No policy talk | Gaps | Entry cited | Notes / fix needed |
|---|---|---|---|---|---|---|---|
| | | | | | | | |

## Next steps after the first run
1. Fix failures at the right layer: instruction wording (PROJECT_INSTRUCTIONS.md), entry content, or a missing entry.
2. Add held-out questions taken from recent real cases, not from entries, to test coverage. Use case numbers only.
3. Re-run the full set after every content update.
