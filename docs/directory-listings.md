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

Analyze ad performance, manage brand context, generate static ads, and create and edit videos.

## Longer description

Segwise MCP connects AI assistants to the advertising and creative-performance data available in your Segwise organization. Compare campaigns, apps, ad networks, and creatives using metrics such as spend, CPA, and ROAS. Review proposed brand rules, tone of voice, or images for an organization or app. Saving a reviewed proposal updates the Segwise brand page. Connect to the hosted Streamable HTTP endpoint and sign in with OAuth. Users can also generate static ads by confirming a plan with the estimated generation tokens. For videos, users can attach references, brainstorm, and revise a free written storyboard. Segwise displays the upfront illustration charge and separate draft and full-video estimates before a user chooses. The storyboard charge is refunded when a paid video is billed. Users can retrieve finished media links and approve edits or exports for videos created in the same conversation. Starting a query creates a temporary job and may save conversation history.

## Tools

- `list_segwise_organizations`: Discover the organizations the signed-in account can access.
- `start_segwise_query` runs analytics for one organization with optional date and app filters. It can also create or update a dashboard report on explicit request, subject to the signed-in user's dashboard permissions, or propose a brand edit and save it after review. Users can also plan static ads and videos, approve a priced draft or full video, and edit a storyboard or video from the same conversation.
- `get_segwise_query`: Poll a running query and retrieve its result.

## Official MCP Registry metadata

[server.json](../server.json) follows the [official remote-server format](https://modelcontextprotocol.io/registry/remote-servers). It uses the GitHub organization namespace `io.github.SegwiseHQ/segwise-mcp` and points directly to the hosted service.

The optional `repository` field is omitted because the schema describes it as the server's source-code repository. `websiteUrl` points to this public overview. For directories that accept documentation repositories, use the public overview URL in their repository field and identify its documentation-only scope. Directories requiring implementation source need a separate publication decision.

The manifest identifies version `0.5.0`, which adds conversational video generation and editing. Before publishing a registry release, compare the manifest version and description with the [hosted server card](https://mcp.segwise.ai/mcp/server-card).

This repository does not publish or submit itself to directories. Registry submission additionally requires publisher authentication, namespace ownership verification, and any directory-specific review. No directory approval or listing status is implied by these files.
