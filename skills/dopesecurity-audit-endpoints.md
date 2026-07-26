---
name: Audit the endpoint fleet
description: List and search dope.swg endpoints to review status, agent versions, users, devices, and locations.
api: openapi/dopesecurity-flightdeck-openapi.yml
operations:
  - generateAccessToken
  - searchEndpoints
---

# Audit the endpoint fleet

Inventory the dope.swg endpoints reporting into your tenant.

## Steps

1. Get a bearer token (see "Authenticate to the Flightdeck API", `generateAccessToken`).
2. Call **`searchEndpoints`** — `GET /endpoints/search`. With no parameters it returns
   all endpoints, cursor-paginated and ordered by `lastSeen` (descending by default).
3. To narrow, supply exactly one search/filter parameter per request, e.g.:
   - `id` — broad case-insensitive match across device/user/email.
   - `status` — one or more of `healthy`, `error`, `dormant`, `disabled`.
   - `osVersion`, `agentVersion`, `deviceName`, `emailId`, `userId`, `locationId`, `fallbackMode`, `debugState`.
4. Page with `first` (default 50) and `after` (pass `pageInfo.endCursor` from the prior
   response); stop when `pageInfo.hasNextPage` is `false`.

## Rules

- Only one of the optional search/filter parameters may be specified per request.
- Errors come back as `{ "errors": [ { "message", "details" } ] }`; `403` means your
  client role lacks endpoint-read permission.
