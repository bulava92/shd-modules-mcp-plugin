---
name: shd-routing
description: Use for any request that asks SHD for data, a project operation, a module workflow, or a change to SHD records. Route the request to the narrowest existing SHD MCP tool and preserve server-side permissions.
metadata: modules=*
---

# SHD routing and safety

SHD MCP is the source of truth for live data, tool schemas, permissions,
organization/project scope, conflicts, idempotency, audit and persisted
readback. Do not invent fields, statuses, identifiers, routes or capabilities.

When the connected server lists shd_prompt_search and shd_prompt_get, use
them to find and render reusable workflow instructions. When it lists
shd_resources_search and shd_resources_get, use them to find and read current
read-only context. If a helper pair is unavailable, use the host's native MCP
prompt or resource catalog and read methods when exposed. Prompts guide steps,
resources return current context, and tools perform operations; a prompt or
resource never authorizes a write.

## Route the request

- Project list, project status, deadlines or project context: use the Projects
  Core tools and the `shd-active-projects` or `shd-project-status` workflow.
- AirBase project-data questions and relationship lookups: use AirBase tools
  and `shd-airbase-query`. For duplicates, broken links or a broader
  data-quality audit, use `shd-airbase-audit`.
- AirBase saved dashboard inspection, queries, creation or changes: use
  `shd-airbase-manage` and the matching prompt; dashboard queries are separate
  from ordinary record queries.
- AirBase schema, record, cell, AI-computed field, external table, public table
  access or import changes: use `shd-airbase-manage`.
- AirBase Omni device actions: use shd-airbase-omni-agent; run only an exact registered action explicitly requested by the user.
- Tasks, task projects, task status or task dates: use Tasks tools and the
  `shd-task-workflow` workflow.
- Project files and paths: use Files tools and the shd-project-files workflow.
- Allowlisted project source, database or service changes: use Project Edit tools and
  shd-project-edit; keep guarded writes separate from read-only repository inspection.
- Allowlisted repository code search, file reads or diffs: use read-only Repository Code
  tools and shd-repository-code.
- Discussions, channels, topics, messages or attachments: use Discussions tools and the shd-discussions workflow.
- Zulip streams, topics or messages: use the linked-account Zulip tools and
  shd-zulip; keep Zulip separate from SHD Discussions.

- Documents, templates, revisions, PDFs or publication links: use Documents tools and the `shd-documents` workflow.
- Organizations, members, invitations, roles or two-factor policy: use Organizations tools and the `shd-organizations-acl` workflow.
- Notifications, preferences or read state: use Notifications tools and the `shd-notifications` workflow.
- Services, versions, approvals or site publications: use Services tools and
  the `shd-services` workflow.
- Short links, public aliases, rotation or revocation: use Short Links tools
  and the `shd-short-links` workflow.
- Authenticated profile or OIDC link changes: use Profile tools and the
  `shd-auth-profile` workflow.
- Inventory, stock, assets, procurement or stocktakes: use Inventory tools and the `shd-inventory` workflow.
- Agents, controllers, diagnostics, tunnels or network access: use Agents tools and the shd-agents-operations workflow.
- Grafana dashboards, data sources, queries, alerts or incidents: use project-scoped
  Grafana tools and shd-grafana; do not bypass the project filter with raw API calls.
- SHD users, organizations, module settings or access policy: use admin tools and
  shd-admin-operations only for the explicitly requested target.
- Controller devices, current states, presence/history or sensor statistics: use the controller-integrations source tools; in a UI-capable host call the matching controller render tool after the data tool and before the final response (`shd_render_controller_devices_widget`, `shd_render_controller_live_state_widget`, `shd_render_controller_history_widget` or `shd_render_controller_statistics_widget`). For one statistics series, render the final result once; for several explicitly requested series, collect them into one `data.series` collection and render one summary widget, not one widget per retry, metric field or inventory match.
- Status-page monitors, history, groups or incidents: use Status Page tools and the `shd-status-page` workflow.
- Realtime sessions, event cursors or historical activity: use Realtime tools and the `shd-realtime-activity` workflow.
- Terms documents, revisions, completeness or contract discussions: use Terms tools and the `shd-terms-contracts` workflow.
- Linked Gitea issues, labels, comments or attachments: use Gitea tools and the `shd-gitea` workflow.
- Finance or CRM records: use the specific module tools and the
  `shd-finance-crm` workflow; use project context for a high-level overview.
- Finance reconciliation, balances, settlements or audit history:
  `shd-financial-account-audit`.
- Proposals, estimates, quotes or estimate-document contracts:
  `shd-estimate-management`.
- Scheduling, event types, slots or bookings: `shd-event-positions`.
- Wiki, notes, pages, revisions or note databases: `shd-wiki-management`.
- Entity matching or possible duplicates: `shd-entity-resolution`.
- Analytics, comparisons, trends or reports: `shd-analytics`.
- A multi-record change: use `shd-safe-bulk-change` before any mutation.

Use `shd_capabilities` only when the available module or permission boundary is
unclear. If a tool or module is unavailable, report that fact instead of
falling back to an undocumented endpoint.

## Read-first rule

For every write, first read the target and its relevant schema or metadata.
Only perform a mutation when the user explicitly requested that mutation. A
request to inspect, summarize, compare, audit or suggest is not permission to
write.

Preserve the exact server result. Separate confirmed values from inference,
and include affected identifiers, conflicts, validation errors and final
readback when the server returns them.

## Scope and secrets

Never ask for or expose access tokens, passwords, cookies, private keys, raw
public-link bearer tokens or integration secrets. Never use a user-provided
project name as an identifier when SHD has not resolved it to a returned
project code or ID.

See `references/module-map.md` for the current high-level routing map.
