# Segwise MCP

Connect your AI assistant to [Segwise](https://segwise.ai) to analyze advertising performance and generate static ads from plans you approve.

- **Server URL:** `https://mcp.segwise.ai/mcp`
- **Transport:** Streamable HTTP
- **Authentication:** OAuth 2.1 with PKCE; sign in with your Segwise account.

This is the public overview, setup guide, and directory metadata for Segwise's hosted MCP service. The service implementation is maintained separately. Connect to the hosted URL; there is no server package to install or deploy from this repository.

## What you can do

- Compare performance across apps, campaigns, ad networks, creatives, and creative tags.
- Analyze spend, impressions, clicks, installs, CTR, CVR, CPI, CPA, ROAS, revenue, and LTV, where available in your connected data.
- Identify strong, declining, or fatigued creatives.
- Understand which hooks, calls to action, visuals, themes, and formats contribute to performance.
- Ask follow-up questions, retrieve existing Segwise creative suggestions, and get recommendations on what to scale, refresh, or test.
- Save a new dashboard report or update a report you can edit in the dashboard.
- Review and save changes to your organization's or an individual app's brand context, including creative rules, tone of voice, and brand images.

Ask for static ads in the same conversation as your analysis. Segwise shows a plan with the concepts, copy, references, sizes, and estimated generation tokens. Request changes or reply **Generate these** to approve the displayed plan. The estimate is the number of concepts multiplied by your organization's static-image rate. Three concepts in four sizes each produce twelve files. If all three concepts render successfully, you pay for three concepts. Additional sizes and failed renders carry no generation charge. The assistant polls the confirmation job and returns links to the finished images. If polling is interrupted, resume with the same job ID. If it has expired, ask for existing static results in the same conversation. Never resend a confirmation automatically.

Saving a brand proposal updates the brand page in Segwise. Generating images leaves existing campaigns, budgets, and creative assets unchanged. Starting a query creates a temporary job and may save conversation history.

## Before you connect

You need a Segwise account with access to an eligible organization, connected analytics data, and an MCP client that supports Streamable HTTP and OAuth. Availability depends on your organization's access and your client's policies. Contact your Segwise representative if access needs to be enabled.

## Connect your assistant

Add the server using one of the examples below and complete the browser sign-in with your own Segwise account. The requested `segwise:analytics:query` grant covers analytics, saved report operations, brand edits, and static ad generation for eligible organizations. Generation can spend organization tokens after you approve a priced plan. The client discovers the OAuth settings automatically.

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

The available answers depend on the data connected to the selected organization. Stored creative suggestions cover the whole organization. For recommendations about a specific app, ask for an analysis of that app’s performance.

## Available tools

| Tool | Purpose |
| --- | --- |
| `list_segwise_organizations` | Lists the eligible organizations your signed-in account can access. |
| `start_segwise_query` | Starts a job for one organization and returns a `job_id` and `conversation_id`. Analytics accepts optional date and app filters. For analytics, omit `operation` or set it to `analyze`. Omit it for brand edits and static ad requests. For an explicit request to save or edit a dashboard report, use `create_report` or `update_report` according to your dashboard permissions. |
| `get_segwise_query` | Waits for or retrieves the result of a previously started query. |

For static ads, omit `operation`. Send the request, revisions, and the user’s actual confirmation as separate `question` values to `start_segwise_query`, preserving the `organization_id`, `conversation_id`, and app selection. For example:

```json
{"organization_id":"<same organization>","conversation_id":"<same conversation>","question":"Generate these"}
```

Each turn returns its own `job_id`. Poll that ID with `get_segwise_query`. On a completed planning turn, `result.answer` is Markdown containing the concepts, copy, sizes, references, and estimated tokens. Display it before accepting confirmation. On a confirmation turn, the same field contains a link or status per image and size. If any images are still rendering, ask for their status in the same conversation.

Queries can take several minutes. Your assistant should keep polling the same `job_id` while its status is `running`, respect `poll_after_seconds` and `execution_deadline`, and avoid duplicate starts. Polling does not extend the execution deadline. A completed result includes the answer and may include citations and a data timestamp. Query jobs are temporary and can expire or be interrupted by a service restart.

If more than one organization is available, choose its name; the assistant uses the corresponding `organization_id`. For follow-ups in the same conversation and organization, preserve the returned `conversation_id`, including when delegating work to another agent. Start a fresh conversation only when you intend to reset the context. Do not reuse handles across unrelated conversations or organizations.

### Edit brand context

Ask, for example, “For our whole organization, change our tone of voice from formal to conversational.” The assistant shows the additions, replacements, and removals for that scope. Segwise controls the conversation history setting on the hosted service. When history is disabled, brand proposals and saves are unavailable.

Review the preview. To save it, send `Save brand update <proposal ID>` with the ID shown in the preview as your next reply. The client must obtain your approval and relay that reply unchanged. Segwise checks the text the client sends against the latest proposal in the same organization and conversation. To revise the proposal, describe the change and review the new preview. If someone edited the section in the meantime, Segwise requires a fresh proposal.

## Data and access

OAuth authenticates your account. The connected client can use the same grant to submit a brand edit for an eligible organization you belong to. An app filter restricts the edit to the selected apps. For an organization-wide change, omit the app filter and confirm the organization scope during review. Demo workspaces are read-only.

For brand images, Segwise can fetch individual Google Drive or Dropbox files you share to prepare the preview. For a local file, open the Segwise upload link in the answer, attach the image, and return to the conversation. A Save reply adds the reviewed images to the named brand group. Your questions and results pass between your chosen client and Segwise, and Segwise may retain conversation history. The client's data policies also apply.

[Segwise Privacy Policy](https://segwise.ai/privacy-policy) · [Segwise Terms of Service](https://segwise.ai/tos)

## Directory listings

[server.json](server.json) describes the hosted endpoint using the official MCP Registry schema. [Directory listing details](docs/directory-listings.md) provide descriptions, authentication details, and public links for submission forms. Including this metadata does not mean a directory has approved or published a listing.

## Help

For account access or a customer-specific issue, use your existing Segwise support channel or the [Segwise Help Center](https://help.segwise.ai). For documentation corrections, [open an issue](https://github.com/SegwiseHQ/segwise-mcp/issues). Keep credentials and customer data out of public issues.

## License

The [MIT license](LICENSE) covers the documentation, metadata, and examples in this repository. Access to the hosted Segwise service is governed by your Segwise agreement and the service terms linked above.
