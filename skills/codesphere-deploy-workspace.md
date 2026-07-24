---
name: Deploy an application to a Codesphere workspace
description: Create a workspace, then run its CI pipeline (prepare + run) and confirm the deployment is live.
api: openapi/codesphere-openapi-original.json
operations: [workspaces-createWorkspace, workspaces-startPipelineStage, workspaces-pipelineStatus, workspaces-getWorkspaceStatus]
---

# Deploy an application to a Codesphere workspace

Use the Codesphere Public API (`https://cloud.codesphere.com/api`). Authenticate every
request with `Authorization: Bearer <PAT>` — generate the Personal Access Token in
User Settings > API Keys. Errors return `{ status, title, detail, traceId }`; log the
`traceId` when reporting failures.

## Steps

1. **Create the workspace** — `POST /workspaces` (`workspaces-createWorkspace`) with the
   team, plan, and git repository. Capture the returned `workspaceId`.
2. **Run the prepare stage** — `POST /workspaces/{workspaceId}/pipeline/{stage}/start`
   (`workspaces-startPipelineStage`) with `stage=prepare` to build dependencies.
3. **Poll build status** — `GET /workspaces/{workspaceId}/pipeline/{stage}`
   (`workspaces-pipelineStatus`) with `stage=prepare` until it completes.
4. **Run the run stage** — `workspaces-startPipelineStage` with `stage=run` to boot the app.
5. **Confirm it is live** — `GET /workspaces/{workspaceId}/status`
   (`workspaces-getWorkspaceStatus`).

## Notes
- No idempotency key is supported; do not blindly retry `createWorkspace` on timeout —
  first `GET /workspaces/team/{teamId}` (`workspaces-listWorkspaces`) to check.
- Pagination on list endpoints uses `limit`/`offset`.
