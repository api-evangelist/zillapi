# auth.md

You are an agent. This document tells you how to authenticate to **Zillapi**
(a Zillow property data API) on behalf of a user, then call the API. Follow the steps
in order.

- **Resource server (the API you call):** `https://api.zillapi.com`
  — REST under `/v1/*`, MCP under `/mcp`.
- **Authorization server (where you register and get a token):** `https://api.zillapi.com`
- **Scope:** `mcp:access` (one scope; grants the full `/v1` REST + `/mcp` tool surface).

There are two ways in. Pick based on what you have:

| You have… | Use | Human needed? |
|---|---|---|
| Nothing, or you're a remote-MCP connector (e.g. Claude.ai) | **OAuth 2.1 + Dynamic Client Registration** (Method B) | Yes — to grant consent once |
| A user who can paste a key, or a server-side integration | **API key** (Method A) | Once, to create the key |

This service authenticates the agent **anonymously**: you assert no upstream
user identity. A human authorizes the connection through the consent screen
during the OAuth flow (Method B), or by issuing you a key in the dashboard
(Method A). Zillapi does **not** implement ID-JAG identity assertions
(`urn:ietf:params:oauth:token-type:id-jag`), the verified-email claim grant, or
the JWT-bearer grant — do not attempt those flows; they will be rejected.

---

## Discovery

All discovery documents are public (no auth, CORS `*`):

- Protected Resource Metadata (RFC 9728): `https://api.zillapi.com/.well-known/oauth-protected-resource`
- Authorization Server Metadata (RFC 8414): `https://api.zillapi.com/.well-known/oauth-authorization-server`
- This file: `https://zillapi.com/auth.md`
- OpenAPI: `https://zillapi.com/openapi.json`
- MCP server card: `https://zillapi.com/.well-known/mcp/server-card.json`

If you hit `/v1/*` or `/mcp` without a valid token you get a `401` whose
`WWW-Authenticate` header points at the Protected Resource Metadata:

```http
HTTP/1.1 401 Unauthorized
WWW-Authenticate: Bearer resource_metadata="https://api.zillapi.com/.well-known/oauth-protected-resource"
```

The Authorization Server Metadata carries an `agent_auth` block summarizing the
registration surface described below.

---

## Method A — API key (fastest)

Best when a human can paste a key once, or for server-side use.

1. Sign up free at `https://zillapi.com/signup` (100 credits, no card required).
2. Create a key at `https://zillapi.com/app/keys/`. It looks like `zk_` followed
   by 32 url-safe base64 characters. **It is shown once** — store it securely.
3. Send it as a bearer token on every request:

```http
GET /v1/properties/12345678 HTTP/1.1
Host: api.zillapi.com
Authorization: Bearer zk_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

Manage or revoke keys at `https://zillapi.com/app/keys/`.

---

## Method B — OAuth 2.1 + Dynamic Client Registration

This is what remote-MCP connectors use. The flow is RFC 6749
`authorization_code` + RFC 7591 Dynamic Client Registration + RFC 7636 PKCE
(S256 mandatory). There is **no client secret** — Zillapi is a public client.

### B1. Register (RFC 7591)

```http
POST /oauth/register HTTP/1.1
Host: api.zillapi.com
Content-Type: application/json

{
  "client_name": "My Agent",
  "redirect_uris": ["https://myagent.example.com/callback"]
}
```

`redirect_uris` is required (at least one; `https`, or `http` for
`localhost`/`127.0.0.1` only). Response (`201`):

```json
{
  "client_id": "...",
  "client_id_issued_at": 1717000000,
  "client_name": "My Agent",
  "redirect_uris": ["https://myagent.example.com/callback"],
  "grant_types": ["authorization_code"],
  "response_types": ["code"],
  "token_endpoint_auth_method": "none",
  "scope": "mcp:access"
}
```

### B2. Authorize — the human consent (claim) step

Generate a PKCE verifier and its S256 challenge, then send the user to:

```
https://api.zillapi.com/oauth/authorize
  ?response_type=code
  &client_id=<client_id>
  &redirect_uri=<one of your registered redirect_uris>
  &code_challenge=<base64url(sha256(verifier))>
  &code_challenge_method=S256
  &scope=mcp:access
  &state=<opaque CSRF value>
```

Zillapi redirects the user to a consent screen on `https://zillapi.com`, where
they **sign in (or sign up) and click Allow**. This is the only consent gate —
it binds the connection to a real Zillapi account. On approval the browser is
redirected to your `redirect_uri` with `?code=<auth_code>&state=<state>`. On
denial you get `?error=access_denied`.

### B3. Exchange the code for a token

```http
POST /oauth/token HTTP/1.1
Host: api.zillapi.com
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code
&code=<auth_code>
&client_id=<client_id>
&redirect_uri=<same redirect_uri>
&code_verifier=<your PKCE verifier>
```

Response (`200`):

```json
{
  "access_token": "<bearer token>",
  "token_type": "Bearer",
  "expires_in": 31536000,
  "scope": "mcp:access"
}
```

The `access_token` is long-lived (~1 year). **There are no refresh tokens** — when
it expires or is revoked, repeat from B2 (re-registration is not required; the
`client_id` is reusable).

### B4. Use the token

```http
GET /v1/properties/12345678 HTTP/1.1
Host: api.zillapi.com
Authorization: Bearer <access_token>
```

The same token works for MCP at `https://api.zillapi.com/mcp`. The connection
appears in the user's dashboard at `https://zillapi.com/app/mcp/`.

### B5. Revoke (RFC 7009)

```http
POST /oauth/revoke HTTP/1.1
Host: api.zillapi.com
Content-Type: application/x-www-form-urlencoded

token=<access_token>
```

Always returns `200`. The user can also revoke from `https://zillapi.com/app/mcp/`.

---

## Errors

- `401` with `WWW-Authenticate: Bearer …` — no/invalid token. Get one via Method A or B.
- `402` — out of credits. The user can top up at `https://zillapi.com/pricing`.
- `403` — token revoked or wrong scope.
- `429` — rate limited; see `https://zillapi.com/rate-limits/`. Honor `Retry-After`.
- OAuth errors follow RFC 6749 (`invalid_grant`, `invalid_request`, etc.) as JSON
  `{ "error": "...", "error_description": "..." }`.

## Reference

- Authentication guide: `https://zillapi.com/authentication/`
- Rate limits: `https://zillapi.com/rate-limits/`
- MCP setup guide: `https://zillapi.com/blog/mcp-server-for-zillow-data/`
- Terms: `https://zillapi.com/legal/trademark-and-affiliation/`
- Support: `hello@zillapi.com`
