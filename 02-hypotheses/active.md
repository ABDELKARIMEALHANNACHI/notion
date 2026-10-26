# Active Hypotheses — updated post-research

## Verified facts (from public sources)
- loadCachedPageChunk: {page:{id}, chunkNumber, limit, cursor:{stack:[]}, verticalColumns}
- loadPageChunk: {pageId, limit, cursor:{stack:[]}, chunkNumber, verticalColumns}
- syncRecordValues: {requests:[{pointer:{table,id}, version:-1}]}
- getRecordValues: {requests:[{id, table}]}
- search / searchPagesWithParent: accept spaceId
- getPublicPageData: {spaceDomain:"<sub>.notion.site"} → returns spaceId (no auth)
- getPublicSpaceData: {spaceId} → returns publicHomePage block ID (no auth)
- notion.site vs www.notion.so: different routing cells
- UA-less requests get Cloudflare 403

## H1 — Cross-workspace page read via loadCachedPageChunk
- Body: A's page.id, B's token_v2 cookie
- Confirm: block content returned
- Reject: uniform error
- Status: candidate (test now)

## H2 — Cross-workspace record read via syncRecordValues
- Body: pointer {table:"block", id:<A's block>}, version -1, B's cookie
- Confirm: record returned
- Reject: null/error
- Status: candidate

## H3 — Cross-workspace user metadata via loadUserContent
- Body: empty/minimal, B's cookie — does it return A's workspace info?
- Status: candidate

## H4 — Public page ID resolution abuse
- getPublicPageData returns spaceId from subdomain.
- Question: does getPublicPageData accept a pageId directly (not a spaceDomain)?
- If yes: can we enumerate spaceId from arbitrary pageId?
- Status: candidate

## H5 — Link-shared page without link
- Page shared "Anyone with a link"
- Attacker knows pageId (from URL screenshot, etc.) but not the link token
- Does www.notion.so/api/v3/loadCachedPageChunk with attacker's token return content?
- Status: candidate (privesc target)

## H6 — submitTransaction cross-workspace write
- Only test AFTER H1 confirms read leakage
- Status: parked (depends on H1)

## H7 — UA bypass
- Requests without UA → 403 (Cloudflare)
- Question: with UA + valid token + foreign spaceId — what happens?
- Status: lower priority, expected behavior (Cloudflare)
