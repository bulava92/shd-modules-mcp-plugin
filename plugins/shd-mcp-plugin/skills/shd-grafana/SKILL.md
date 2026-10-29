---
name: shd-grafana
description: Use when the user asks to inspect or manage project-scoped SHD Grafana dashboards, data sources, queries or alerting.
metadata: modules=grafana
---

# SHD Grafana

## Project context

- Resolve the exact SHD project code before using project-scoped Grafana
  operations. Dashboard and data-source results are limited to that project.
- Search dashboards and list data sources first. Use only a data-source UID
  returned for the same project. Read the dashboard summary or exact panel
  queries before interpreting or changing it.
- Choose Prometheus, Loki, Pyroscope or raw query fields from the current tool
  schema. Report returned time ranges, units and errors; do not invent a
  metric, query result or dashboard dependency.
- Use shd_resources_search and shd_resources_get for current dashboard,
  data-source and summary context when it is useful.

## Changes and operations

- Dashboard edits, annotations, alert rules, routing, silences and incidents
  are stateful. Read current state and invoke only the exact transition the
  user requested.
- Preserve unrelated dashboard content. Do not delete a dashboard, alert
  configuration or snapshot without an explicit request for that exact target.
- Respect Grafana project and role checks. Never use a broad/raw API request
  to bypass project scoping or reveal datasource secrets.
