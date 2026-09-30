# Service lifecycle contract

- Resolve the organization, service, version, site and publication from server results before acting.
- Read relevant service versions, usage and publication state before changing a lifecycle state.
- Keep draft, pending approval, published, failed, revoked, archived and restored states distinct. A version status alone does not prove publication succeeded.
- Editing a draft does not authorize publication. Publish, approve, archive, restore or revoke only the transition explicitly requested.
- Use fresh idempotency keys for publication/retry operations and verify the persisted publication result. Do not expose private credentials.
