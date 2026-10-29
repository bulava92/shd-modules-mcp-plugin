---
name: shd-airbase-omni-agent
description: Use when the user explicitly asks to inspect or run a registered Omni action on their connected Mac device through AirBase.
metadata: modules=airbase
---

# SHD AirBase Omni actions

Use only the authenticated AirBase Omni tools and the current device/action catalog. This skill does not grant access or bypass device ownership checks.

## Inspect and run

- For reusable steps, find manage_airbase_omni_agent through shd_prompt_search and render it with shd_prompt_get.
- Read the current shd://airbases/{base_public_id}/omni-agent/devices resource or call shd_airbase_omni_agent_devices. Use only an online device and the exact plugin_id and action_id returned for that device.
- Run only when the user explicitly requested that exact operation. Require confirm=true, pass a stable unique idempotency_key, and do not substitute a different action when one is unavailable.
- Pass action_input only when the requested registered action accepts it. When present, include its required room_ids and records; do not construct either from guesswork.
- A queued response is not completion. Poll shd_airbase_omni_agent_status with the exact returned run_id and the same device_id; report the terminal status and server result.
- Never retry a failed or ambiguous action with a new key unless the user requests a retry.

All writes and action calls remain subject to AirBase access, selected-device ownership and the server's registered-action allowlist.
