---
name: Create and track a backlog item in P4 Plan
description: Authenticate to the P4 Plan (Hansoft) GraphQL API, create backlog/bug/sprint items in a project, and read them back.
api: P4 Plan (Hansoft) GraphQL API
api_docs: https://help.perforce.com/hansoft/current/Content/hansoftapi/index.html
operations:
  - login
  - projects
  - createBacklogTasks
  - createBugs
  - createSprintTasks
  - items
  - itemsByIDs
---

# Create and track a backlog item in P4 Plan (Hansoft)

Use the P4 Plan GraphQL API service, which runs against a customer-hosted P4 Plan
server. All operation names below are real GraphQL operations from the published
API reference.

## Prerequisites
- The GraphQL API service is installed and reachable on the customer P4 Plan server.
- You have a P4 Plan user with permission on the target project (P4 Plan enforces
  per-project, field-level permissions).

## Steps
1. Authenticate with the `login` mutation to establish a session.
2. Resolve the target project with the `projects` query and capture its project ID.
3. Create the work item:
   - `createBacklogTasks` for product backlog items,
   - `createBugs` for defects, or
   - `createSprintTasks` for items inside a sprint.
   Pass the project ID and the item fields the mutation requires.
4. Read the results back with `items` (list for the project) or `itemsByIDs`
   (the IDs returned by the create mutation) to confirm.

## Conventions
- Auth: session established by `login`; see `authentication/hansoft-authentication.yml`.
- Errors: returned in the GraphQL `errors[]` array (message/locations/path) — inspect
  it before treating a response as success.
- No idempotency key is documented; do not blind-retry a create mutation on timeout —
  reconcile with `items` first.
