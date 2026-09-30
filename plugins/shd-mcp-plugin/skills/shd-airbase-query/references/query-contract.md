# AirBase query contract

- Search the resolved Base by concrete terms, then follow populated record links by their returned IDs.
- Relationship traversal is limited to four hops. Report `partial`, `truncated`, missing/failed reverse scans and depth-limit flags as limits on the result.
- A missing reverse scan or incomplete traversal is not proof that no matching relationship exists.
- Use live table and field IDs from returned schemas. Do not infer relationships from names, groups or expected wiring conventions.
- Return the shortest supported evidence chain and identify its first missing link.
