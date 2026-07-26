---
name: Configure a web policy
description: Create a dope.swg web policy, set category restrictions, and assign it to users and groups.
api: openapi/dopesecurity-flightdeck-openapi.yml
operations:
  - generateAccessToken
  - listPolicies
  - createPolicy
  - getPolicyContent
  - updatePolicyRestrictions
  - updatePolicyAssignments
---

# Configure a web policy

Stand up a new web policy and put users behind it.

## Steps

1. Get a bearer token (`generateAccessToken`).
2. Review existing policies with **`listPolicies`** — `GET /policies` (cursor-paginated).
   Each item reports `sslInspection` and `clashCount`.
3. Create the policy with **`createPolicy`** — `POST /policies/{policy_name}`.
   `policy_name` must be non-empty, <= 32 chars, and must not contain `# ! @ $ % ^ * ? . / \`.
4. Inspect current content with **`getPolicyContent`** — `GET /policies/{policy_name}/content`.
   `inheritsFromBase: true` means values come from the Base Policy.
5. Set category actions with **`updatePolicyRestrictions`** —
   `PUT /policies/{policy_name}/content/restrictions`. Only submitted categories change; send
   `{ "inheritsFromBase": true }` to reset a block back to Base. Category restriction is one of
   `ALLOW`, `BLOCK`, `WARNING` (custom categories may also be `IGNORE`).
6. Assign principals with **`updatePolicyAssignments`** —
   `PUT /policies/{policy_name}/assignments`. Each provided `users`/`groups` array fully replaces
   that list; omit a field to leave it unchanged; send `[]` to unassign all.

## Rules

- All user/group identifiers are validated against the tenant directory; unresolved entries cause a `400`.
- The Base Policy cannot be deleted. `deletePolicy` is destructive (destructive MCP tier).
- PUT operations are replace-semantics and converge on retry — safe to re-send.
