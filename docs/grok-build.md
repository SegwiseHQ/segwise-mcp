# Segwise for Grok Build

The Segwise plugin adds the hosted Segwise MCP service to Grok Build. It provides advertising and creative analytics using data connected to your Segwise organization.

## Requirements

- Grok Build with HTTP MCP and OAuth support.
- A Segwise account with access to an eligible organization and connected analytics data.
- Permission to install and trust the plugin under your organization's Grok policies.

You do not need a Segwise API key or a local MCP server. Each person signs into their own Segwise account through OAuth. Client registration and callback URLs remain subject to Segwise's authorization policy.

## Install from a checkout

From a checkout of this repository that contains `.grok-plugin/plugin.json` and `.mcp.json`:

```sh
grok plugin validate .
grok plugin install . --trust
grok plugin enable segwise
```

Review the package before granting trust. It contains one HTTP MCP server and no scripts, hooks, agents, skills, LSP servers, or executable dependencies.

Start a new Grok Build session, or reload plugins with `r` in `/plugins`. Open `/mcps`, select Segwise, and press `i` to sign in. Approve the `segwise:analytics:query` scope with a Segwise account that can access your intended organization.

The xAI marketplace submission is separate from installation from a checkout. This guide does not claim the plugin is approved or available in the built-in catalog. The catalog entry pins a full commit SHA from the public `SegwiseHQ/segwise-mcp` repository.

## Verify the connection

1. Run `grok plugin details segwise` and confirm the plugin contains the `segwise` HTTP MCP server.
2. Run `grok mcp doctor` and inspect the Segwise connection. Complete OAuth if requested.
3. Ask: "List the Segwise organizations I can access."
4. Choose one organization by name, then ask: "Which creatives had the best ROAS last week? Include spend and explain the trade-offs."
5. Verify that Grok starts one analysis, polls the returned job until completion, and bases its answer on the returned results.
6. Ask a follow-up in the same organization and verify that the returned conversation ID is reused.

Available tools are `list_segwise_organizations`, `start_segwise_query`, and `get_segwise_query`. Query jobs may take several minutes; respect the returned polling delay and execution deadline. Do not start another job merely because the first is still running. See the [tool and query guidance](../README.md#available-tools).

The service supplies analytics and recommendations. It cannot edit campaigns, budgets, or creative assets. Starting an analysis creates a temporary job and may save conversation history; it is not a stateless read.

## Authentication and data

The only MCP endpoint configured by the package is `https://mcp.segwise.ai/mcp`. OAuth discovery points to `https://auth.segwise.ai/oauth/2.1`. Grok uses the authorization server's published endpoints for sign-in, registration, and token exchange. No credentials or static authorization headers are bundled in this repository.

Questions and results pass between Grok and Segwise. Segwise organization permissions limit data access. The [Segwise Privacy Policy](https://segwise.ai/privacy-policy), [Terms of Service](https://segwise.ai/tos), and your AI client's data policies apply. See [OAuth setup](oauth.md) for discovery URLs, scope, and troubleshooting.

## Verification status

On November 5, 2026, the public protected-resource and authorization-server metadata were reachable and advertised the analytics scope and authorization-code flow with PKCE (`S256`). An unauthenticated request to the MCP endpoint returned `401` with the expected OAuth metadata challenge.

These checks verify discovery only. Authenticated installation, OAuth completion, organization listing, and analysis in Grok Build have not yet been tested. The steps above are a test procedure, not recorded test results.

## References

- [Grok Build plugin guide](https://github.com/xai-org/grok-build/blob/main/crates/codegen/xai-grok-pager/docs/user-guide/09-plugins.md)
- [Grok Build MCP documentation](https://docs.x.ai/build/features/mcp-servers)
- [xAI marketplace contribution guide](https://github.com/xai-org/plugin-marketplace/blob/main/CONTRIBUTING.md)
- [Segwise support](https://help.segwise.ai)

The repository's [MIT license](../LICENSE) covers this plugin package. Hosted service access is governed by your Segwise agreement.
