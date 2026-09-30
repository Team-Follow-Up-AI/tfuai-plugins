# Team Follow Up AI plugin

Connect [Team Follow Up AI](https://teamfollowup.ai) to Claude, Cursor or Grok Build and run the platform in plain language. Team Follow Up AI runs voice agents that call your leads within a minute, book the appointment, and write back what happened.

This plugin adds one thing: the official hosted MCP server at `https://api.teamfollowup.ai/api/mcp`. It doesn't install or run any local code. On first use, your editor opens a browser window where you sign in to Team Follow Up AI and approve the access it asked for. You don't need an API key or any environment variable.

## What you can do

The server exposes the same operations as the [public API reference](https://docs.teamfollowup.ai/api-reference/overview), reads and writes alike. Which tools you see depends on the scopes you approve at sign-in.

| Area | Examples |
| --- | --- |
| Agents | List, create and update voice agents and their prompts |
| Campaigns and workflows | Build campaigns, enrol leads, edit follow-up workflows |
| Contacts | Search, create and update contacts, including do-not-call flags |
| Calls | Read call records, outcomes and transcripts, place outbound calls |
| Power dialer | Start, pause and review live dialling of a lead list |
| Phone numbers | List, buy and release numbers |
| Projects, webhooks, white label, billing | Manage sub-accounts and account settings |

Example requests:

```text
Show me every lead we could not reach this week.
Which campaigns booked the most appointments last month?
Summarise the last five calls for the Acme sub-account.
Pause the power dialer on the "October reactivation" list.
Create a contact for Alex Rivera at +14165550123.
```

## Install

### Claude

Open the **Customize** page in Claude, search the directory for **Team Follow Up AI** and add it. It works in Claude on the web, desktop and mobile, in Cowork, and in Claude Code. The first tool call opens the sign-in flow in your browser.

To try it in Claude Code before the listing is live:

```text
/plugin marketplace add Team-Follow-Up-AI/tfuai-plugins
/plugin install tfuai@tfuai-plugins
```

### Cursor

Open the Cursor Marketplace, search for **Team Follow Up AI** and install it. The first tool call opens the sign-in flow in your browser.

To try it before the listing is live, copy the `plugins/tfuai` folder of this repository into `~/.cursor/plugins/local/tfuai` and restart Cursor.

### Grok Build

Open `/plugins` in Grok Build, search for **Team Follow Up AI**, then install and trust it. The first tool call opens the sign-in flow in your browser. Use `/mcps` to check the connection.

### Any other MCP client

Point the client at `https://api.teamfollowup.ai/api/mcp` over Streamable HTTP and let it run OAuth discovery. Full setup, including API key access for scripts, is in the [MCP guide](https://docs.teamfollowup.ai/mcp-actions).

## Sign-in and access

- Sign-in uses OAuth 2.1 with PKCE. The consent screen lists the exact scopes the client asked for, and anything you don't grant isn't issued.
- Access tokens last 1 hour. Refresh tokens last 30 days and rotate on every use.
- A tool your grant doesn't cover is hidden from the tool list and refused if called by name.
- Tools that charge a card are never exposed over MCP, and no OAuth client can be granted that scope.
- Revoke the connection at any time from the Team Follow Up AI dashboard.

### Keep the tool list small

A broad grant can expose a lot of tools. To send the agent only the modules you need, change the URL in `mcp.json` (Cursor) or `.mcp.json` (Claude Code and Grok Build) to add a module filter:

```text
https://api.teamfollowup.ai/api/mcp?modules=contacts,calls
```

An unknown module name returns HTTP `400` with the list of valid names.

## Writes have real effects

Write tools act on your live account, and several reach outside it. Grant only the scopes the task needs, and review each write before you approve it.

| Scope | What a call actually does |
| --- | --- |
| `calls:write` | Places a real outbound call to a real person |
| `power_dialer:write` | Starts or changes live dialling of a lead list |
| `phone_numbers:write`, `phone_numbers:release` | Spends money on a number. Releasing one can't be undone |
| `contacts:write` | Clearing a do-not-call flag can lead to a call that shouldn't happen |
| `campaigns:write` | Enrolling leads queues them to be dialled |
| `agents:write` | Changing an agent's prompt changes every call it takes from then on |
| `billing:write` | Turning on auto-recharge allows later charges to your card or a client's |

Tools carry the standard MCP hints (`readOnlyHint`, `destructiveHint`, `idempotentHint`) so your client knows when to ask first. They are hints, not a safety control. Most write operations aren't idempotent, so if a call times out, read the current state back before you retry.

## Data and privacy

Tool results can include contact details, call records, transcripts and billing data from your account. That data is processed in your editor session and by the AI provider behind it, under their terms. Only connect clients you're allowed to share that data with.

- [Privacy Policy](https://teamfollowup.ai/privacy-policy/)
- [Terms of Service](https://teamfollowup.ai/terms-of-service/)

## Support

- Email [support@teamfollowup.ai](mailto:support@teamfollowup.ai)
- [MCP guide](https://docs.teamfollowup.ai/mcp-actions)
- [API reference](https://docs.teamfollowup.ai/api-reference/overview)
- [Report a plugin issue](https://github.com/Team-Follow-Up-AI/tfuai-plugins/issues)

## License

Apache License 2.0. See [LICENSE](LICENSE). The license covers the files in this repository. It doesn't grant any right to the Team Follow Up AI name or logo.
