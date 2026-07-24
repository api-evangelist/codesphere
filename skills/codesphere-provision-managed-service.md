---
name: Provision a Codesphere managed service
description: Discover available managed-service providers, create a service instance, and confirm it is running.
api: openapi/codesphere-openapi-original.json
operations: [managed-services-listProviders, managed-services-create, managed-services-getDetails, managed-services-list]
---

# Provision a Codesphere managed service

Authenticate with `Authorization: Bearer <PAT>`. Managed services include PostgreSQL,
Valkey, OpenSearch, RabbitMQ, DocumentDB, object storage, and virtual Kubernetes clusters.

## Steps

1. **List providers** — `GET /managed-services/providers`
   (`managed-services-listProviders`) to see available service types and plans.
2. **Create the service** — `POST /managed-services` (`managed-services-create`) with the
   provider, plan, team, and optional backup schedule. Capture the returned `id`.
3. **Check details / status** — `GET /managed-services/{id}`
   (`managed-services-getDetails`) until `status` is running.
4. **List team services** — `GET /managed-services` (`managed-services-list`) to confirm.

## Notes
- Schedule backups with `POST /managed-services/{id}/backups` (`managed-services-scheduleBackup`).
- Errors carry a `traceId`; include it when contacting support@codesphere.com.
