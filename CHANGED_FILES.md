# Files to upload to the Claude project

Last updated: Nov 3, 2026, after the full backfill and the test-set run.

Nearly every knowledge file changed, so replace them all.

## Steps
1. In the project's Files section, delete the old copy of each file below.
2. Upload the new copies from the `knowledge/` folder.
3. Open `PROJECT_INSTRUCTIONS.md`, copy everything below the line, and paste it into the project's Instructions field (replace the old text).
4. Run the questions in `TEST_SET.md` in fresh chats and have a PE grade a few.
5. Have PE review the entries. Every entry is marked "Draft, needs PE review" until then.

## Upload (knowledge/)
- 00-current-policies.md
- 01-sandbox-and-dev-accounts.md
- 02-scopes-and-data-access.md
- 03-app-settings-and-visibility.md
- 04-sso-and-oidc.md
- 05-rostering-and-secure-sync.md
- 06-certification-and-onboarding.md
- 07-api-versions.md
- 08-lms-connect-library-anyschool.md
- 11-app-dashboard-sign-in.md
- 99-emerging-topics.md (unchanged; the weekly run fills it in)

## Paste into Instructions
- PROJECT_INSTRUCTIONS.md

## Never upload
- _agent/ (case CSV with PII, state, test results)
- TEST_SET.md
- CHANGED_FILES.md
