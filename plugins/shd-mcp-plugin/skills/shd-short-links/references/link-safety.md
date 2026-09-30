# Short-link safety contract

- Resolve the exact link, owner, project, alias and status from current server data before updating it.
- Treat target URLs, public aliases, access flags and bearer tokens as sensitive. Never return bearer tokens or private redirect data.
- Change only requested fields and preserve the server's expected-version and idempotency contract.
- Rotation or revocation requires an explicit request, `confirm=true`, current concurrency data and a fresh idempotency key.
- Report the link ID, alias, affected fields, resulting status and server readback.
