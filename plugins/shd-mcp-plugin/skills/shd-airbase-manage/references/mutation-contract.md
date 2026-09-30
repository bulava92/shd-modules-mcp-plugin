# AirBase mutation contract

- Resolve the user's selected project and exact Base before any mutation. Use the returned project and Base identifiers.
- Read the target table schema and current records first. Use field IDs, link targets and allowed values returned by SHD.
- Validate record values before create or update. Preview bulk create/upsert with `dry_run=true` and resolve every conflict before writing.
- Bulk cell updates have no dry-run. Review every record/field/value tuple before calling the mutation tool.
- Read the action schema and field usages before schema changes. Require explicit intent and confirmation for destructive changes, public access changes and external-table detachment.
- Keep AirBase project data separate from global System Base data.
