---
name: "integrations-case-archive"
description: "Mine Salesforce cases from the integrations@clever.com alias into topic-based markdown knowledge files for the CS team's Claude project (backfill or weekly incremental run). Use when asked to run, continue, or update the integrations case archive."
---

# Integrations Case Archive agent

Turn resolved developer-support cases from integrations@clever.com into topic-based markdown knowledge files. The files are uploaded to a shared Claude project that the Customer Support team uses to answer simple developer questions.

## Location
**Folder:** `~/Documents/Integrations Case Archive/`

If the folder isn't mounted in this session, request it with `request_cowork_directory` (path `~/Documents/Integrations Case Archive`).

| Path | Purpose |
|---|---|
| `knowledge/00-current-policies.md` | **Hand-maintained current rules (POL-01 to POL-08). Read this first on every run. Never edit or overwrite this file.** It is the only source for the rules. |
| `knowledge/NN-<topic>.md` | The topic files that get uploaded to the Claude project. |
| `knowledge/99-emerging-topics.md` | Themes from open cases. Not answers. |
| `_agent/state.md` | Topic file map, bucket status table and run log. Read it first and update it last. |
| `CHANGED_FILES.md` | Add every knowledge file you create or change under "Pending upload" (no duplicates). Also list `00-current-policies.md` if it was modified since the last upload. |
| `PROJECT_INSTRUCTIONS.md` | The project's system prompt. Change it only if the user asks. When it is edited, keep rule 6 ("Never mention policies") and don't add policy IDs. |

## Current rules win
Past cases often show outdated guidance. Older cases in particular may offer standalone SSO-only, Library certification, survey links, v2 or v3.0-only advice, a per-app sandbox limit, or routing straight to application-interest@. When a case conflicts with `00-current-policies.md`:
- write the entry so it follows the current rule,
- note the difference under **Notes / conflicts** as outdated guidance (for example, "Older cases did X, but that's no longer the process"),
- never write an entry that goes against a current rule.

If several recent resolved cases suggest a rule has actually changed, **don't edit the policy file.** List the evidence under "Possible policy changes" in your end-of-run summary only.

**The rules are internal. Never name them in output files.** Policies are internal labels. Apply them, but state each rule inline as plain guidance. The files that go to the project (`knowledge/NN-*.md`, `99-emerging-topics.md`), plus `CHANGED_FILES.md` and `PROJECT_INSTRUCTIONS.md`, must not contain policy IDs like "POL-04", the phrases "per policy" or "current policy", or the policy filename. Describe changes by topic (for example, "certification routing updated"). Policy IDs are allowed only in the end-of-run summary shown to the user and in `_agent/state.md`. This applies to Clever's internal policies only; unrelated uses of the word (for example, a device policy setting) are fine.

Do not copy rule summaries into this skill or into your own notes from memory. Read `00-current-policies.md` each run and apply what it says. It covers Clever Complete, SSO-only, Library, new-partner routing, certification (including the paying-customer check, the "up to two weeks" reply, the #partner-engineering post and no survey links), API versions, sandboxes (including the District App process, which is handled separately from sandbox requests), and security and credentials.

## Connector
Salesforce, using the `soqlQuery` tool.

## Case scope
- `Origin IN ('Email - Integrations@clever.com','Email - integrations+cert@clever.com')`
- `Primary_Issue_Category__c IN ('Partner Engineering','T2_Developer Support')`
- **Knowledge source:** `Status IN ('Closed','Solved','Resolved')`. Skip Closed - Spam, Closed - Merged, Auto-Solved and Transfer.
- **Open signal:** `Status IN ('New','Open','In Progress','Pending','On-hold - App','On Hold - Eng','On Hold - District CC')`
- **Buckets:** `Primary_Issue_Type__c` + `Primary_Issue_Detail__c`. The file map in `state.md` says which topic file each bucket goes to.

## Modes

### BACKFILL (one run = the next 2 buckets)
1. Read `00-current-policies.md` and `state.md`. Pick the 2 largest buckets with Status TODO. Treat PILOT buckets as TODO, but merge into their existing entries.
2. Start one general-purpose subagent per bucket and give it the extraction brief below.
   - Sample up to **25 cases** if the bucket has 60 or fewer, otherwise **40**.
   - Use `CreatedDate = LAST_N_DAYS:730`, spread across that range (`ORDER BY ClosedDate DESC` with OFFSET pages).
   - Give the subagent the current rules (text, not just IDs) so it can flag conflicts.
3. Merge the results into the bucket's topic file (see Writing entries).
4. Update `state.md`: set the bucket to DONE, list its entry IDs and add a run-log line. Then update `CHANGED_FILES.md`.
5. Run the scrub check (see Hard rules).

### WEEKLY (incremental)
1. Read the policies. Pull in-scope resolved cases with `ClosedDate = LAST_N_DAYS:7` and group them by bucket.
2. For each case, do one of three things:
   - **(a) It matches an existing entry:** add its case number to that entry's Source cases, and update the answer only if the behavior is new or changed and doesn't conflict with a current rule.
   - **(b) It's a new pattern:** add a new entry if you see it in at least 2 cases, or in 1 case that's clearly simple and reusable.
   - **(c) It's a one-off or account-specific:** skip it.
3. Pull open-signal cases with `CreatedDate = LAST_N_DAYS:7`.
   - Use the Subject and a truncated first inbound email to spot recurring themes.
   - Add or update lines in `99-emerging-topics.md`.
   - **Never write an entry from an unresolved case.**
4. When an emerging theme now has resolved cases, write its entry and remove the theme line.
5. Add a run-log line to `state.md` and update `CHANGED_FILES.md`.
6. Run the scrub check (see Hard rules).

## Extraction brief (give this to each subagent)
1. **List the cases:** `SELECT Id, CaseNumber, Subject, ClosedDate FROM Case WHERE <scope> AND Primary_Issue_Type__c='<type>' AND Primary_Issue_Detail__c='<detail>' ...`
2. **Read each case's emails, one case at a time:** `SELECT Incoming, MessageDate, TextBody FROM EmailMessage WHERE ParentId='<id>' ORDER BY MessageDate ASC LIMIT 6`
   - The bodies are large. If a result is saved to a file, use Grep or Read to pull out the first inbound question and the Clever replies (`Incoming=false`).
   - Ignore quoted chains and signatures.
   - If the ending matters, also pull the last 2 messages (`ORDER BY MessageDate DESC LIMIT 2`).
3. **Return:**
   - a table: CaseNumber | question | resolution | simple vs needs PE/Eng | conflicts with a current rule? (describe briefly in words; policy IDs are for the end-of-run summary only, never for entries)
   - 2–4 entries for the recurring patterns, in the entry template below.

## Entry template (inside a topic file)
```
## <PREFIX>-<NN> · "<question in the developer's words>"
**Status:** Draft, needs PE review · **Resolvable by CS:** Yes | Triage only, PE makes the change

**When to use this**
**Answer / steps**   (include paste-ready > quoted replies)
**Escalate if…**
**Notes / conflicts** (state the current rule in plain words. Never cite POL-xx IDs, "policy" or the policies filename. If an old case conflicts, write e.g. "Older cases did X, but that's no longer the process.")
**Links:** dev.clever.com / support.clever.com / schools.clever.com only
**Source cases:** case numbers
```

## Writing entries
- Separate entries with `---`. Use the next free ID number for the file's prefix and the next free number for new file names. Never reuse or renumber an ID, because CS cites them. Edits made only to change wording keep the ID and Status unchanged.
- The only two Status values are `Draft, needs PE review` and `Reviewed`. Once PE has reviewed an entry, its status becomes "Reviewed". Never downgrade a Reviewed entry. If new cases contradict it, add a note under Notes / conflicts instead.
- Before adding an entry, search all the knowledge files for one that already covers the same question. If you find one, merge into it.
- If a topic file doesn't exist yet, create it with a `# Title` and a header line that lists its buckets.
- Keep each topic file under about 40 entries. If a file grows past that, split it and update the file map.

## Hard rules
- **No PII or customer identifiers.** That means no names, emails, phone numbers, district names, partner or app names, client IDs or district IDs. Use "the partner" or "the district" instead.
- Only describe behavior the cases actually show. Put anything unconfirmed, outdated or contradictory under Notes / conflicts.
- **Never contradict `00-current-policies.md`, and never edit it.**
- **Scrub check (end of every run):** run `grep -rnE "POL-[0-9]|per policy|current policy|current-policies" knowledge/ CHANGED_FILES.md PROJECT_INSTRUCTIONS.md`. Expect hits only in `00-current-policies.md` and in two places in `PROJECT_INSTRUCTIONS.md`: the line that names the policy file as the source of truth, and rule 6 ("Never mention policies"). Both are intentional; leave them alone. Fix any other hit before finishing. Note any scrub work in the run log.
- Be honest about "Triage only": spell out what CS should collect and how to set expectations.
- **Budget:** at most 2 backfill buckets, or one weekly pass, per run.
- **End-of-run summary.** Always finish with a summary that covers:
  - the buckets and cases read,
  - the entries added or updated (by ID),
  - new emerging topics,
  - any "Possible policy changes" with evidence,
  - the list of files to re-upload from `CHANGED_FILES.md`.
  Policy IDs may be used in this summary and in `_agent/state.md`, never in files that get uploaded.

