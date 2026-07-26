---
name: Authenticate to the Flightdeck API
description: Obtain an OAuth 2.0 Client Credentials bearer token for the dope.security Flightdeck API and use it on subsequent calls.
api: openapi/dopesecurity-flightdeck-openapi.yml
operations:
  - generateAccessToken
---

# Authenticate to the Flightdeck API

All Flightdeck calls except token generation require a bearer token.

## Steps

1. Have an admin create API client credentials in the dope console
   (Settings → API Client Credentials). Copy the Client ID and Token Secret
   immediately — the secret is shown only once.
2. Call **`generateAccessToken`** — `POST /partner/oauth/token` with a JSON body:
   `{ "grant_type": "client_credentials", "client_id": "...", "client_secret": "..." }`.
   This is the only unauthenticated operation.
3. Read `access_token`, `token_type` (`bearer`), and `expires_in` (seconds) from the response.
4. Send `Authorization: Bearer <access_token>` on every other call.
5. Track `expires_in` and refresh before expiry — the token is short-lived.

## Rules

- The OAuth `scopes` parameter is ignored; access is governed by Flightdeck RBAC
  tied to your client's role, so a call may still return `403` even with a valid token.
- On failure the token endpoint returns `{ "error": "..." }` (401 for bad credentials).
