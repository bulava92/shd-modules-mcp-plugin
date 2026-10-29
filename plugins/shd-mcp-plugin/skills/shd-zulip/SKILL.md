---
name: shd-zulip
description: Use when the user asks to search, read or manage messages in the linked SHD Zulip account.
metadata: modules=zulip
---

# SHD Zulip

- Zulip is a linked external account and is separate from SHD Discussions.
  Resolve visible streams and topics from the connected account before use.
- Search messages for cross-stream requests; use bounded reads for one stream
  or topic and fetch a specific message by its returned ID.
- Sending, editing, deleting or reacting is a write. Do it only when the user
  explicitly requested that action and the exact target and content are known.
  Never infer a recipient, topic or message text.
- Delete only the exact message the user named. Do not repeat a send after an
  ambiguous response; inspect the returned message ID and state first.
- Use the image or attachment tools for returned Zulip uploads. Do not expose
  linked-account credentials or replace a protected attachment with its raw URL.
