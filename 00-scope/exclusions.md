# Notion — Hard Exclusions (do NOT submit these)

## Instant rejects
- Social engineering / phishing
- Attacks against Notion users or infrastructure
- Automated scanning tools
- Rate limiting / brute force (non-auth endpoints)
- Password policy issues
- HTTP 404 / error pages
- Banner / version disclosure
- robots.txt / .well-known presence
- Self-XSS
- Spam
- Clickjacking (without ATO or sensitive data)
- CSRF on unauthenticated or non-sensitive forms
- MITM / physical access
- Known-vulnerable libs without PoC
- CSV injection without demonstrated impact
- Missing SSL/TLS best practices
- Content spoofing / text injection without HTML/CSS mod
- DoS / stress tests
- Missing CSP, cookie flags, security headers (unless ATO)
- Outdated browsers (<2 versions behind)
- Descriptive error messages / stack traces / server errors  ⚠️
- Public zero-days patched <1 month
- Tabnabbing
- Open redirect (without extra impact)
- Notion hosting malware / spam / phishing
- Requires unlikely user interaction
- Paid feature bypass
- *.notion.site identity info
- Expected behavior per Notion docs
- JS execution on file.notion.so / notion-static.com
- AI prompt engineering for inappropriate responses  ⚠️
- Lock feature bypass on pages/databases
- In-App Contact form
- Source code / debug access
- Testing on production data accounts  ⚠️
- Testing accounts not created for engagement  ⚠️
- Cross-org/cross-workspace testing with non-engagement workspaces  ⚠️

## Platform standards Notion does NOT commit to
- Severity rating for IDOR with unpredictable IDs  ⚠️⚠️
- Multiple reports on systemic vulnerabilities
- Payment for bypassing resolved vulnerabilities
- Severity for vulnerable client network connections
- Responsible disclosure for 3rd-party components
- Severity for sensitive PII leakage
- Severity for self-sign-up flow vulns
- 3rd-party component consumption
