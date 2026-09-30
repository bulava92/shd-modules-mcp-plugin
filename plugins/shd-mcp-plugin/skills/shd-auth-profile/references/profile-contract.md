# Authenticated profile contract

- Read the current-user resource before changing profile data and use the server's current concurrency values.
- Change only fields explicitly requested by the user and accepted by the profile tool schema.
- Treat email, password, sessions, recovery information, access flags, tokens and OIDC subjects as sensitive or out of scope.
- Unlink OIDC only on an explicit request and after the server confirms another sign-in method remains available.
- Preserve idempotency requirements and report the persisted server readback without exposing secrets.
