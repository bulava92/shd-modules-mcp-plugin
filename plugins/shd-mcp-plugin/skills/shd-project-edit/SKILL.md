---
name: shd-project-edit
description: Use when the user asks to inspect or change allowlisted project source, database or service targets.
metadata: modules=project-edit
---

# SHD project editing

## Identify and inspect the target

- Resolve the exact configured repository, non-production database connection
  or allowlisted service. Read the current file, schema/fingerprint or service
  status before proposing a change.
- Keep this guarded edit surface separate from Repository Code, which is
  read-only. Do not use shell or SFTP access.
- Do not guess target names or interpret a project display name as a database
  connection or repository ID.

## Guarded changes

- Source writes are limited to the configured file operation. Read the file,
  preserve its current SHA-256, change one file at a time and use the expected
  hash and required confirmation only after an explicit user request.
- Database operations must use a configured non-production target. Prefer
  schema inspection and a dry run; keep writes bound and single-operation,
  and run only listed migrations with the current target fingerprint and
  explicit confirmation.
- Inspect an allowlisted service before restarting it. Restart only the exact
  requested unit and satisfy its expected-state and confirmation fields.
- Re-read affected state after a write. Stop on stale fingerprints, conflicts,
  unsupported targets or ambiguous requests; never bypass the guard.
