# Segwise MCP

Connect your AI assistant to [Segwise](https://segwise.ai) to analyze advertising and creative performance using your organization's data.

- **Server URL:** `https://mcp.segwise.ai/mcp`
- **Transport:** Streamable HTTP
- **Authentication:** OAuth 2.1 with PKCE; sign in with your Segwise account.

This is the public overview, setup guide, and directory metadata for Segwise's hosted MCP service. The service implementation is maintained separately. Connect to the hosted URL; there is no server package to install or deploy from this repository.

## What you can do

- Compare performance across apps, campaigns, ad networks, creatives, and creative tags.
- Analyze spend, impressions, clicks, installs, CTR, CVR, CPI, CPA, ROAS, revenue, and LTV, where available in your connected data.
- Identify strong, declining, or fatigued creatives.
- Understand which hooks, calls to action, visuals, themes, and formats contribute to performance.
- Ask follow-up questions and get recommendations on what to scale, refresh, or test.

The MCP service provides analytics and recommendations. It cannot change campaigns, budgets, or creative assets, or generate new creatives. Starting an analysis creates a temporary query job and may save conversation history.

## Before you connect

You need a Segwise account with access to an eligible organization, connected analytics data, and an MCP client that supports Streamable HTTP and OAuth. Availability depends on your organization's access and your client's policies. Contact your Segwise representative if access needs to be enabled.

## Connect your assistant

Add the server using one of the examples below, complete the browser sign-in, and approve the requested analytics access. Each person connects with their own account. The client discovers OAuth settings automatically; you do not need an API key in these examples.

### Claude Code

```sh
claude mcp add --transport http segwise https://mcp.segwise.ai/mcp
```

Open Claude Code, run `/mcp`, select Segwise, and follow the authentication prompt. The command adds the server for the current project by default; add `--scope user` to make it available across your projects.

For a shared project configuration, merge [examples/claude-code.json](examples/claude-code.json) into your project's `.mcp.json`.

[Claude Code MCP documentation](https://code.claude.com/docs/en/mcp)

### Codex

```sh
codex mcp add segwise --url https://mcp.segwise.ai/mcp
codex mcp login segwise
```

Complete sign-in in the browser. Alternatively, merge [examples/codex.toml](examples/codex.toml) into `~/.codex/config.toml`, then run the login command. Start a new session after changing the configuration.

[Codex MCP documentation](https://developers.openai.com/codex/mcp)

### Cursor

Merge this entry into `.cursor/mcp.json` for your project, or `~/.cursor/mcp.json` for all projects:

```json
{
  "mcpServers": {
    "segwise": {
      "url": "https://mcp.segwise.ai/mcp"
    }
  }
}
```

Open Cursor's MCP settings and complete the Segwise sign-in prompt. A copyable configuration is available in [examples/cursor.json](examples/cursor.json).

[Cursor MCP documentation](https://cursor.com/docs/mcp)

### VS Code

1. Run **MCP: Add Server** from the Command Palette.
2. Choose **HTTP** and enter `https://mcp.segwise.ai/mcp`.
3. Use `segwise` as the server name and choose your configuration destination.
4. Complete the trust and OAuth prompts, then enable the tools in your agent session.

[VS Code MCP documentation](https://code.visualstudio.com/docs/agent-customization/mcp-servers)

### Linear Agent

Add Segwise to your Linear workspace's MCP connector directory, then connect your Segwise account through **Agent personalization**. Use the resulting analytics to draft creative experiments, briefs, and project updates with Linear Agent.

[Linear Agent setup guide](docs/linear.md)

### Other MCP clients

Add a remote Streamable HTTP server with the URL above and choose OAuth authentication. Use your client's sign-in flow. See the [OAuth setup guide](docs/oauth.md) for discovery URLs, the requested scope, and troubleshooting.

These examples follow each client's documented configuration format. Client versions, administrator policies, and OAuth client registration can affect connection availability.

## Try it

First ask: **"List the Segwise organizations I can access."** Then select an organization and ask a question such as:

- "Which creatives had the best ROAS last week? Include spend and explain the trade-offs."
- "Which hooks and calls to action perform best on Meta for my app?"
- "Which creatives are showing fatigue, and what should we test next?"
- "Compare CPA and revenue across my apps for the last 30 days."

The available answers depend on the data connected to the selected organization.

## Available tools

| Tool | Purpose |
| --- | --- |
| `list_segwise_organizations` | Lists the eligible organizations your signed-in account can access. |
| `start_segwise_query` | Starts an analysis for one organization and returns a `job_id` and `conversation_id`. Accepts optional date and app filters. |
| `get_segwise_query` | Waits for or retrieves the result of a previously started query. |

Queries can take several minutes. Your assistant should keep polling the same `job_id` while its status is `running`, respect `poll_after_seconds` and `execution_deadline`, and avoid duplicate starts. Polling does not extend the execution deadline. A completed result includes the answer and may include citations and a data timestamp. Query jobs are temporary and can expire or be interrupted by a service restart.

If more than one organization is available, choose its name; the assistant uses the corresponding `organization_id`. For follow-ups in the same conversation and organization, preserve the returned `conversation_id`, including when delegating work to another agent. Start a fresh conversation only when you intend to reset the context. Do not reuse handles across unrelated conversations or organizations.

## Data and access

OAuth authenticates your account; organization membership and service permissions determine which data you can query. Questions and results pass between your chosen AI client and Segwise, and conversation history may be retained for follow-up analysis. The client provider's own data policies also apply.

[Segwise Privacy Policy](https://segwise.ai/privacy-policy) · [Segwise Terms of Service](https://segwise.ai/tos)

## Directory listings

[server.json](server.json) describes the hosted endpoint using the official MCP Registry schema. [Directory listing details](docs/directory-listings.md) provide descriptions, authentication details, and public links for submission forms. Including this metadata does not mean a directory has approved or published a listing.

## Help

For account access or a customer-specific issue, use your existing Segwise support channel or the [Segwise Help Center](https://help.segwise.ai). For documentation corrections, [open an issue](https://github.com/SegwiseHQ/segwise-mcp/issues). Keep credentials and customer data out of public issues.

## License

The [MIT license](LICENSE) covers the documentation, metadata, and examples in this repository. Access to the hosted Segwise service is governed by your Segwise agreement and the service terms linked above.
