---
name: Manage users and projects in P4 Plan
description: Authenticate to the P4 Plan (Hansoft) GraphQL API and administer users, groups, and projects.
api: P4 Plan (Hansoft) GraphQL API
api_docs: https://help.perforce.com/hansoft/current/Content/hansoftapi/index.html
operations:
  - login
  - users
  - userGroups
  - createNormalUser
  - updateNormalUser
  - deleteUser
  - projects
  - createProject
  - updateProject
  - cloneProject
---

# Manage users and projects in P4 Plan (Hansoft)

Administer accounts and projects through the P4 Plan GraphQL API service. Every
operation name below is a real GraphQL operation from the published API reference.

## Prerequisites
- The GraphQL API service is installed on the customer P4 Plan server.
- You authenticate as an administrator (user/project administration requires it).

## Provision a user
1. `login` to establish a session.
2. Check for an existing account with the `users` query.
3. `createNormalUser` to add the account; `updateNormalUser` to change it;
   `deleteUser` to remove it.
4. Use `userGroups` to review group membership for permissioning.

## Stand up a project
1. `createProject` for a new project, or `cloneProject` to copy an existing one
   (backlog structure, workflow, columns) as a template.
2. `updateProject` to adjust settings.
3. Confirm with the `projects` query.

## Conventions
- Auth: session from `login`; see `authentication/hansoft-authentication.yml`.
- Permissions: P4 Plan enforces per-project, field-level permissions — responses
  and mutations are scoped to what the authenticated admin may see and change.
- Errors: surface in the GraphQL `errors[]` array; check it before proceeding.
- No documented idempotency key — verify with `users` / `projects` before retrying.
