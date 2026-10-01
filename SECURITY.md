# Security policy

## Reporting a vulnerability

Email **support@teamfollowup.ai** with "Security" in the subject line. Please don't open a public GitHub issue for a security problem.

Include what you can of:

- what is affected, such as a tool name, an endpoint or a page
- the steps to reproduce it
- what an attacker could do with it
- your contact details, if you'd like us to follow up

We read every report and reply by email.

## Scope

- The hosted MCP server at `https://api.teamfollowup.ai/api/mcp`
- Its sign-in and authorisation endpoints on `api.teamfollowup.ai`
- The plugin manifests in this repository

The plugin runs no local code. It only points your editor or assistant at the hosted server above, so a problem in the server is a problem with this plugin.

Please test only against an account you own, and don't access, change or delete other people's data.
