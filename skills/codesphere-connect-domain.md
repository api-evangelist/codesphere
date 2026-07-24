---
name: Connect and verify a custom domain
description: Register a custom domain for a team, verify ownership, and route it to a workspace.
api: openapi/codesphere-openapi-original.json
operations: [domains-createDomain, domains-verifyDomain, domains-updateWorkspaceConnections, domains-getDomain]
---

# Connect and verify a custom domain

Authenticate with `Authorization: Bearer <PAT>`.

## Steps

1. **Create the domain** — `POST /domains/team/{teamId}/domain/{domainName}`
   (`domains-createDomain`).
2. **Read DNS instructions** — `GET /domains/team/{teamId}/domain/{domainName}`
   (`domains-getDomain`) to obtain the required DNS entries and verification status.
3. **Verify ownership** — `POST /domains/team/{teamId}/domain/{domainName}/verify`
   (`domains-verifyDomain`) after the DNS records propagate.
4. **Route to a workspace** — `PUT /domains/team/{teamId}/domain/{domainName}/workspace-connections`
   (`domains-updateWorkspaceConnections`) to point the domain at the target workspace.

## Notes
- `domains-getDomain` returns certificate-request and domain-verification status objects.
- Errors carry a `traceId`.
