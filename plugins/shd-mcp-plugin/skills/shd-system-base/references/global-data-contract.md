# System Base contract

- System Base is global and uses an allowlisted table set. Resolve table, record and field IDs from the server.
- Read the schema and current record/cell before an update. Preserve the current expected value and `record_version`.
- Update only the explicitly requested cell and use a fresh idempotency key.
- Deletion requires an explicit request, `confirm=true`, the current expected version and persisted readback.
- Keep System Base separate from project AirBase data; do not invent table names or broaden filters.
