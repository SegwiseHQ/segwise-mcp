# Segwise for Linear Agent

Use Segwise's advertising and creative-performance insights while planning work in Linear. Ask Linear Agent to analyze creative fatigue, compare campaign performance, or identify promising creative elements, then use the findings to draft experiment tasks, creative-refresh briefs, or project updates.

Segwise supplies analytics, recommendations, and user-approved static ad generation through its hosted MCP server. Linear Agent can also show a proposed change to your Segwise brand context. Saving the proposal updates the brand page for the selected organization or app. Segwise's tools do not create Linear issues themselves and cannot change ad campaigns, budgets, or existing creative assets.

## Requirements

- A Linear workspace with Linear Agent and MCP connectors enabled by an administrator.
- A Segwise account with access to an eligible organization and connected analytics data.
- Permission to connect Segwise in your Linear workspace.

## Add Segwise to your workspace

A Linear workspace administrator completes these steps:

1. Open **Settings → Administration → MCP connectors**.
2. Enable connectors if your workspace policy permits them.
3. If your workspace restricts connectors, keep **Manage connectors** set to **Only connectors in directory**.
4. Select **Add connector**, enter `https://mcp.segwise.ai/mcp`, and choose the custom URL option.
5. Leave the authentication method set to **Automatic** and add the connector.

Adding a directory entry does not connect a person's Segwise account. Each person completes the sign-in below.

## Connect your account

1. Open **Settings → Personal → Agent personalization**.
2. Under **MCP connectors**, choose **Add connector → Segwise**.
3. Sign in with your Segwise account and review the MCP grant before approving it.
4. Return to Linear and confirm that Segwise is connected.
5. Ask Linear Agent: **"List the Segwise organizations I can access."** If multiple organizations are available, choose the organization by name.

The connection uses OAuth 2.1 with PKCE. The `segwise:analytics:query` grant covers analytics, saved report operations, brand edits, and video and static generation. Generating images can spend organization tokens after you approve a priced plan. For a brand change, Linear Agent must wait for a person to review the preview and supply the exact Save reply before relaying it to Segwise. An unattended task should leave the proposal pending.

## Try a workflow

> Use Segwise to identify creatives showing signs of fatigue over the last seven complete days. Summarize the evidence and draft a creative-refresh experiment in this chat. Do not create or update any Linear records.

You can also ask:

- "Compare the performance of our top creatives last week and draft a weekly project update."
- "Which hooks and formats perform best for my app? Use the findings to draft a brief for our next creative experiment."
- "Use the Segwise findings from this conversation to suggest the success metrics for this experiment."

Review the proposed work before asking Linear Agent to create or update issues. Answers depend on the data available to your Segwise organization.

To select a reference, ask “Show my uploaded images” and choose a numbered result.

To create static ads from the findings, ask Linear Agent to plan the images in this conversation. Review the concepts, sizes, and token estimate, then request edits or reply **Generate these**. Linear Agent forwards your reply, polls the resulting job, and shows the image links. If polling stops, resume the same job. For an expired job, ask for your existing static results in this conversation before considering another generation.

## Analysis timing and follow-ups

Segwise analysis can take several minutes. Linear Agent should start the analysis once and keep retrieving the same job until it completes, respecting the tool's polling delay and deadline. Follow-up questions should preserve the returned conversation and organization context.

If the assistant stops before completion, ask it to retrieve the existing Segwise query result rather than start the same analysis again. Jobs are temporary and may expire or be interrupted by a server restart.

## Connection troubleshooting

- **Segwise is missing from the connector picker:** ask your Linear administrator to add it to the workspace connector directory.
- **OAuth registration or redirect rejection:** contact Segwise with the client name and error. Linear's displayed OAuth callback is `https://linear.app/connect/mcp/callback`; Segwise's allowed-client policy must permit it. Do not share authorization codes, access tokens, or client secrets.
- **No eligible organizations:** check that you used the correct Segwise account and have MCP access.
- **Expired sign-in:** reconnect Segwise through Linear's connector settings.

See the [OAuth guide](oauth.md) for discovery details and [Linear's MCP connection documentation](https://linear.app/docs/connect-mcp-servers) for workspace controls.

## Data and support

Questions and results are exchanged between Linear and Segwise under the connected user's permissions. Starting an analysis creates a temporary query job and may save conversation history. Linear's and Segwise's data policies apply.

Contact [shobhit@segwise.ai](mailto:shobhit@segwise.ai) for this integration, or use your existing Segwise support channel. Keep customer data and credentials out of public GitHub issues.

[Segwise Privacy Policy](https://segwise.ai/privacy-policy) · [Segwise Terms](https://segwise.ai/tos)
