# Notion Product API (notion.so/api/v3) — Endpoint Inventory

Source: Gemini research cross-checked against kjk/notionapi, NotionX/react-notion-x, jamalex/notion-py.
Status: client-observed, NOT server-verified. Ground truth = our own captures.

## Endpoints
| # | Method | Endpoint | Purpose | ID params | Auth |
|---|---|---|---|---|---|
| 1 | POST | /api/v3/loadPageChunk | Load page block hierarchy | pageId | token_v2 |
| 2 | POST | /api/v3/loadCachedPageChunk | Newer cached-page chunk | page.id | token_v2 |
| 3 | POST | /api/v3/syncRecordValues | Fetch versioned records | requests[].pointer.id + table | token_v2 |
| 4 | POST | /api/v3/getRecordValues | Older record fetch | requests[].id + table | token_v2 |
| 5 | POST | /api/v3/loadUserContent | User nav + content | none | token_v2 |
| 6 | POST | /api/v3/queryCollection | Query database view | collectionId | token_v2 |
| 7 | POST | /api/v3/search | Search pages/blocks | query | token_v2 |
| 8 | POST | /api/v3/searchPagesWithParent | Search under parent | parentId | token_v2 |
| 9 | POST | /api/v3/getPublicPageData | Public page metadata | pageId | none? |
| 10 | POST | /api/v3/getPublicSpaceData | Public space data | spaceId | none? |
| 11 | POST | /api/v3/getSignedFileUrls | Signed file URLs | fileIds | token_v2 |
| 12 | POST | /api/v3/submitTransaction | Submit record ops (write) | operations[] | token_v2 |
| 13 | POST | /api/v3/saveTransactions | Save transactions (write) | transactions[] | token_v2 |
| 14 | POST | /api/v3/saveTransactionsFanout | Transaction variant | | token_v2 |

## Auth headers (evidence-backed)
| Header | Status |
|---|---|
| Cookie: token_v2=<JWT> | Confirmed by all clients |
| Content-Type: application/json | Confirmed |
| x-notion-active-user-header | Confirmed (TypeScript client) |
| notion-client-version | Historical capture |
| x-notion-space-id | Used by some integrations |
| notion-audit-log-platform | Integration-specific |
| Origin / Referer / User-Agent | Sometimes required, not universal (403 without UA reported) |

## Key finding from research
NO public source proves or disproves cross-workspace authz checks.
NO public source confirms spaceId mismatch error behavior.
NO public source confirms whether submitTransaction enforces workspace ownership.
→ These are our hunting hypotheses.

## Client reference repos
- kjk/notionapi (Go) — client.go, api_syncRecordValues.go, api_loadCachedPageChunk.go
- NotionX/react-notion-x (TS) — packages/notion-client/src/notion-api.ts
- jamalex/notion-py (Py) — notion/client.py
- ijjk/notion-blog (JS) — createTable.js
- RadekMolenda/org-notion (Elisp) — org-notion.el
