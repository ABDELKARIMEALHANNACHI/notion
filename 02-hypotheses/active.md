# Active Hypotheses — priority re-ranked (Claude brief)

## 🔴 P1 — Write-path space-scope trust (submitTransaction / saveTransactions)
- Server accepts `spaceId` field in operations (confirmed: CRDT rejection for missing space_id)
- Open question: does server validate `spaceId` == token's workspace, or just presence + internal coherence?
- Test: submitTransaction with B's token targeting A's block, with A's spaceId in the operation.
- Confirm: block content on A changes after B's transaction.
- Impact: CRITICAL — cross-tenant write / privesc
- Duplicate risk: MEDIUM-HIGH (policy explicitly authorizes this test)
- Status: candidate — TEST FIRST after H1

## 🟠 P2 — Read-path cross-workspace (loadCachedPageChunkV2 / syncRecordValues)
- Already testing (H1, H2). Standard BOLA.
- Impact: CRITICAL
- Duplicate risk: HIGH
- Status: candidate — in progress

## 🟠 P3 — Public-page endpoints as recon primitive
- getPublicPageData: spaceDomain → spaceId (confirmed)
- getPublicSpaceData: spaceId → publicHomePage block ID (confirmed)
- Use as chaining step into P1/P2 to harvest IDs
- Status: candidate — recon only, not a finding alone

## 🟡 P4 — Permission mutation via generic write path
- Does saveTransactions bypass the share endpoint and let you write `permissions` field?
- Impact: CRITICAL (direct privesc)
- Status: parked until P1 confirmed

## 🟡 P5 — AI endpoint runInferenceTranscript
- Confirmed: takes `x-notion-space-id` header
- Untested: does server validate header vs token
- Impact: HIGH (cross-workspace AI data leak)
- Duplicate risk: LOW-MEDIUM — newest surface, likely least trodden
- Status: candidate — secondary target if P1/P2 dead

## ❌ UNCONFIRMED ENDPOINTS — do not test
Claude flagged these names as fabricated-sounding, no public source found:
- syncRecordValuesSpaceInitial
- updatePageV2
- estimateInstructionsTokens
- markInferenceTranscriptSeen
- undoAgentOperations

If you don't see them in your own traffic, they don't exist for us.
