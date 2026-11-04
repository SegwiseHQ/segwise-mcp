# Directory listing details

Use these details when submitting Segwise's hosted MCP service to directories. This repository provides public documentation and metadata; it does not contain the hosted server implementation or a self-hosted distribution.

## Listing fields

| Field | Value |
| --- | --- |
| Name | Segwise MCP |
| Publisher | Segwise |
| Public overview repository | https://github.com/SegwiseHQ/segwise-mcp |
| Product website | https://segwise.ai |
| Documentation | https://github.com/SegwiseHQ/segwise-mcp#readme |
| Server URL | `https://mcp.segwise.ai/mcp` |
| Transport | Streamable HTTP (`streamable-http`) |
| Authentication | OAuth 2.1 with PKCE; individual Segwise account |
| Required scope | `segwise:analytics:query` |
| Prerequisites | Eligible Segwise organization, account access, connected analytics data, compatible OAuth client |
| Access and pricing | Segwise account and applicable service agreement required; contact Segwise for availability and commercial terms |
| Suggested categories | Analytics, advertising, marketing, creative performance |
| Support | https://help.segwise.ai |
| Documentation issues | https://github.com/SegwiseHQ/segwise-mcp/issues |
| Privacy policy | https://segwise.ai/privacy-policy |
| Terms of service | https://segwise.ai/tos |

## Short description

Advertising and creative-performance analytics for your Segwise organizations.

## Longer description

Segwise MCP connects AI assistants to the advertising and creative-performance data available in your Segwise organization. Compare campaigns, apps, ad networks, and creatives; analyze metrics such as spend, CPA, and ROAS; identify creative fatigue; and explore which hooks, visuals, and calls to action drive performance. Connect to the hosted Streamable HTTP endpoint and sign in with OAuth. The service provides analytics and recommendations; it cannot modify campaigns or generate creative assets. Starting a query creates a temporary job and may save conversation history.

## Tools

- `list_segwise_organizations`: Discover the organizations the signed-in account can access.
- `start_segwise_query`: Start an organization-scoped analysis with optional date and app filters.
- `get_segwise_query`: Poll a running query and retrieve its result.

## Official MCP Registry metadata

[server.json](../server.json) follows the [official remote-server format](https://modelcontextprotocol.io/registry/remote-servers). It uses the GitHub organization namespace `io.github.SegwiseHQ/segwise-mcp` and points directly to the hosted service.

The optional `repository` field is omitted because the schema describes it as the server's source-code repository. `websiteUrl` points to this public overview. For directories that accept documentation repositories, use the public overview URL in their repository field and identify its documentation-only scope. Directories requiring implementation source need a separate publication decision.

The manifest version is `0.2.0`, matching the integration version in the maintained server source when this overview was prepared. Confirm the deployed version before publishing a registry release, and update the metadata when the service contract changes.

This repository does not publish or submit itself to directories. Registry submission additionally requires publisher authentication, namespace ownership verification, and any directory-specific review. No directory approval or listing status is implied by these files.
