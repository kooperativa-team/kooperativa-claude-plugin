---
name: setup
description: This skill should be used when the user wants to install, connect, or troubleshoot the Kooperativa plugin, for example when the kooperativa_* tools are missing, report a missing key, or return 401 or 402. It explains the API key and license requirements without ever handling the key in the conversation.
license: MIT
metadata:
  category: business
  author: kooperativa-team
---

# Kooperativa setup

The Kooperativa plugin starts the official MCP server (`@kooperativa_team/mcp-server`) on the user's computer. The server reads the user's API key from `~/.kooperativa/config.json`, written by its `login` command, or from a real key in the `KOOPERATIVA_API_KEY` environment variable.

## Requirements

- A Kooperativa account with an active license. Accounts are invite-only. Access requests go to https://kooperativa.io/signup.
- An API key, created by the user at https://kooperativa.io/api-keys. Keys start with `ik_live_` and are shown once.
- Node.js 18 or newer, because the server is started with `npx`.
- Claude Code, or a Cowork session that runs on the user's computer. Chat on claude.ai and on mobile does not start local servers, so the tools are not available there.

## Steps

1. Ask the user to run `npx -y @kooperativa_team/mcp-server login` in a terminal window. It asks for the key with input hidden, verifies it, and stores it with mode 600. Then run `/reload-plugins` in Claude Code, or restart the session.
2. Never ask the user to paste the key into the chat, a file in the project, or a command line. If a key is pasted anyway, tell the user to revoke it at https://kooperativa.io/api-keys and create a new one.
3. Check the connection with the `kooperativa_account_info` tool. It is free.

## Troubleshooting

| Symptom | Likely cause |
|---|---|
| No `kooperativa_*` tools | The plugin is not installed or enabled, Node.js is missing, the session is chat on claude.ai or mobile, or the plugin was not reloaded after `login` |
| "No Kooperativa API key found" | `login` has not been run, or the stored key is malformed. Run `login` again |
| 401 `API_KEY_INVALID` | The key is revoked or wrong. Create a new key and run `login` again |
| 402 `LICENSE_INACTIVE` | The workspace has no active license |
| 402 `REALTIME_REQUIRES_PAID_LICENSE` | The `*_realtime` tools need a workspace with a billing relationship |
| 429 | Per-minute rate limit reached. Wait for the `Retry-After` seconds |

Documentation: https://docs.kooperativa.io. Support: support@kooperativa.io.
