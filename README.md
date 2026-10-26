# Notion Bug Bounty

Target: Notion (HackerOne: `notion`)
Signal requirement: 0.0 — no trial-report restriction
Status: Active
Started: 2026-10-26

## Workflow
- MAP → HUNT → VALIDATE → REVIEW
- 2 mapping sessions, 3 testing sessions

## Scope
- In scope: Product API, Public API, notion.id, calendar/mail.notion.so, Notion AI, Integrations, Desktop, Frontend, Auth
- Out of scope: error-message-only findings, unpredictable-ID IDOR without impact, AI prompt injection without data leak, automated scanners

## Rules
- Test only accounts/workspaces created for this assessment
- No production data
- No destructive tests

## Status labels
`candidate` · `confirmed` · `rejected` · `parked` · `inconclusive`

## Layout
- `00-scope/` — policy, scope, exclusions
- `01-recon/` — asset map, endpoint inventory, captures
- `02-hypotheses/` — active, parked, rejected
- `03-evidence/` — dated evidence folders (request/response/screenshots)
- `04-reports/` — drafts and submitted
