# Ashby MCP server through Cotlet.dev

Connect Ashby to ChatGPT, Claude, Codex and Claude Code through Cotlet.dev. Work with jobs, candidate profiles, applications, interviews and offers in your signed-in Ashby account.

This is an unofficial integration, not affiliated with Ashby. Cotlet.dev hosts the connection through your Cloud Browser; this repository provides an optional Python stdio MCP bridge.

## Connect your account

1. [Request access to Ashby through Cotlet.dev](https://cotlet.dev/signup?app=ashby).
2. Open Ashby in your Cotlet.dev Cloud Browser, sign in yourself and enable it for agents.
3. Add Cotlet.dev to your agent using the connection details below.

### ChatGPT

Open Settings > Apps & Connectors > Create. Use `https://cotlet.dev/mcp/v1/connect` as the MCP server URL and choose OAuth.

### Claude

Open Settings > Connectors > Add custom connector and enter `https://cotlet.dev/mcp/v1/connect`.

### Codex

Create a Cotlet.dev API key in your console and set `COTLET_API_KEY` in your environment. Add this to your Codex configuration:

```toml
[mcp_servers.cotlet]
url = "https://cotlet.dev/mcp/v1"
bearer_token_env_var = "COTLET_API_KEY"
```

### Claude Code

```bash
claude mcp add --transport http cotlet https://cotlet.dev/mcp/v1 --header "Authorization: Bearer $COTLET_API_KEY"
```

## Recruiting tasks

Examples based on the [live Ashby integration reference](https://cotlet.dev/integrations/ashby-api-mcp-server):

- List open jobs and compare active applications.
- Find candidates by name or email, then read their applications, notes and interviews.
- List applications in a job stage and identify those waiting longest.
- Schedule a technical screen with selected interviewers and invite settings.
- Draft or send a candidate email using an available template.
- Archive an application with a configured reason and an optional rejection email.
- Create an offer from your organization's fields and send its letter after required approvals.

The live page documents these 12 operations:

- `list_jobs`
- `search_candidates`
- `get_candidate`
- `list_applications`
- `list_interviews`
- `list_archive_reasons`
- `list_email_templates`
- `schedule_interview`
- `send_email`
- `archive_application`
- `create_offer`
- `send_offer_letter`

Hosted MCP exposes these operations through `ashby_read` and `ashby_write`. Writes ask for confirmation by default; the live reference describes per-tool settings, roles and offer approval requirements. Scheduling and offers need appropriate Ashby permissions.

## Optional local stdio bridge

Python 3.10 or newer is required, with no runtime dependencies. Clone this repository and configure a client that supports stdio:

```json
{"mcpServers":{"ashby":{"command":"python3","args":["/absolute/path/to/ashby-mcp/ashby_agent.py"],"env":{"COTLET_API_KEY":"your-cotlet-key"}}}}
```

The bridge lists seven browser helpers: `ashby_guide`, `ashby_status`, `ashby_open`, `ashby_read`, `ashby_click`, `ashby_type` and `ashby_close`. These forward to Cotlet.dev's shared browser tools. They are separate from the 12 hosted Ashby operations above. Call the guide first and close the browser when finished. Tool discovery works without a key; executing a tool requires your Cotlet.dev key and connected account.

```bash
python3 ashby_agent.py connect
python3 ashby_agent.py guide
python3 ashby_agent.py status
python3 ashby_agent.py serve
```

You can also build the included Dockerfile and run it with `COTLET_API_KEY` supplied as an environment variable.

## Reference and access

- [Ashby connection guide and operation reference](https://cotlet.dev/integrations/ashby-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=ashby)
- [Request access](https://cotlet.dev/signup?app=ashby)
- [Cotlet.dev console](https://cotlet.dev/app)

Licensed under Elastic License 2.0. See LICENSE.
