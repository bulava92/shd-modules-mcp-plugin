# Discussion contract

Use the narrowest tool for the request:

- discovery: `shd_list_discussion_channels`,
  `shd_list_discussion_topics`;
- content: `shd_list_discussion_messages`, `shd_search_discussions`;
- writes: `shd_add_discussion_message`, `shd_update_discussion_message`;
- files: `shd_upload_discussion_attachment`.

If a required direct discussion tool is absent from the active MCP tool list,
search for it with `shd_mcp_search` before reporting it unavailable. If the
router finds the tool, invoke the discovered operation through
`shd_mcp_write`, using its returned schema and required fields (including
confirmation or idempotency fields when specified). If the router is not
available or does not find the operation, report that limitation; never
replace an edit with a new message.

Resolve the canonical channel/topic/message identifiers before a write. Keep
searches bounded, distinguish no results from denied scope, and retain the
server's pagination or conflict fields. A message edit or attachment upload is
a side effect: require explicit user intent and report the final server result.
After an edit, read the target message again and confirm the stored content.
If a write returns an error or times out, check the target state before retrying.
Retry only when the operation supports idempotency, reusing the same key and
payload; otherwise report the uncertain outcome and stop.
