# Notion — Scope Table (source: HackerOne, Oct 13 2026)

| Asset | Coverage | Resolved | Priority |
|---|---|---|---|
| **Product API** | notion.so/api/v3 | 100 (53%) | 🔥 #1 — IDOR/SSRF/injection/privesc |
| **Public API** | api.notion.com | 3 (2%) | #2 — token privesc |
| **Privilege Escalation** | cross-user page access | 2 (1%) | #2 — cross-workspace page access |
| Notion AI | AI model | 9 (5%) | #3 — data leak via prompt |
| Notion Frontend | notion.so/ | 21 (11%) | #3 |
| Notion Integrations | CSRF in 3rd-party integrations | 6 (3%) | #3 |
| Notion Authentication | auth flows | 5 (3%) | #3 |
| Notion Desktop App | desktop build | 4 (2%) | #4 |
| notion.id | Android app | 3 (2%) | #4 |
| mail.notion.so | domain | 3 (2%) | #4 |
| calendar.notion.so | domain | 2 (1%) | #4 |
| Github repos (makenotion) | public artifacts | 1 (1%) | #5 |
| Notion MCP | MCP | 0 (0%) | #2 (new, Oct 2026) |

## Bounty ranges (per asset, CVSS-based)
| Asset | Low | Medium | High | Critical |
|---|---|---|---|---|
| Product API / Public API / Auth / AI / Integrations / Privesc / mail / calendar | $100 | $250 | $2,000 | $5,000 |
| Notion Desktop | $75 | $150 | $1,000 | $3,000 |
| Notion Frontend | $75 | $150 | $750 | $2,500 |
| notion.id | $50 | $100 | $500 | $2,000 |
| Github repos | $50 | $100 | $500 | $2,000 |

## Stats (90 days)
- Reports received: 512
- Bounties paid: $31,950
- Reports resolved: 187
- Hackers thanked: 200
- Avg bounty: $250
- Top bounty range: $2,000–$5,000
- Response efficiency: 91%
