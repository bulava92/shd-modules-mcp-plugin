---
name: shd-admin-operations
description: Use when the user explicitly asks to inspect or change SHD users, organizations, module settings or access configuration.
metadata: modules=transport-and-admin
---

# SHD administration

- Use only the authenticated admin tools visible to the current SHD account.
  Read the target's current settings, membership, role or policy before
  proposing a change.
- An audit or recommendation is read-only. Do not change access because a
  report identifies a risk.
- Change only the exact user, organization, module setting or policy the user
  named. Membership, invitations, roles, two-factor policy and module settings
  require an explicit request for the specific change and the tool's current
  confirmation/version fields.
- Preserve server validation and read back the resulting state. Do not broaden
  an organization or user filter, infer an administrator, or bypass a denied
  capability with another endpoint.
- Never disclose passwords, tokens, session data or integration secrets.
