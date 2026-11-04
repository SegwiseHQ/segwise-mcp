# OAuth setup

Segwise MCP uses OAuth 2.1 authorization with PKCE. Add the hosted MCP endpoint to your client and use its browser sign-in flow.

## Sign in

1. Add `https://mcp.segwise.ai/mcp` as a Streamable HTTP MCP server.
2. Start the connection and select the client's sign-in or authentication action.
3. Sign in with the Segwise account that has access to your organization.
4. Review and approve the requested `segwise:analytics:query` scope.
5. Return to your client and ask it to call `list_segwise_organizations`.

If your client asks for a client ID, secret, or registered callback URL, contact Segwise through your existing support channel with the client name and version. OAuth client registration is subject to the service's client and redirect-URL policies. Do not reuse dashboard tokens or internal service credentials.

## Connection details

| Setting | Value |
| --- | --- |
| MCP endpoint and OAuth resource | `https://mcp.segwise.ai/mcp` |
| Transport | Streamable HTTP |
| Protected resource metadata | `https://mcp.segwise.ai/.well-known/oauth-protected-resource/mcp` |
| Authorization server issuer | `https://auth.segwise.ai/oauth/2.1` |
| Authorization server metadata | `https://auth.segwise.ai/.well-known/oauth-authorization-server/oauth/2.1` |
| Required scope | `segwise:analytics:query` |
| Authorization flow | Authorization code with PKCE (`S256`) |
| Token delivery to MCP | `Authorization: Bearer …`, managed by the client |

The client should discover authorization, token, and client-registration endpoints from the metadata instead of hard-coding them. Use the exact resource URL, including `/mcp`, when the client requests an OAuth resource value.

The public authorization metadata advertises a dynamic client registration endpoint. This does not guarantee that every client's callback URL is permitted. Registration and organization access remain subject to Segwise policy.

## Check discovery without signing in

```sh
curl --fail --silent --show-error \
  https://mcp.segwise.ai/.well-known/oauth-protected-resource/mcp
```

The response identifies the resource, authorization server, and analytics scope. An unauthenticated MCP request returns `401` with a `WWW-Authenticate` challenge pointing to this metadata. That challenge is the normal start of OAuth setup.

Discovery and the unauthenticated challenge were checked on November 4, 2026. These checks do not verify a particular user's sign-in, organization access, or analytics results. Verify your connection with an authenticated organization-list request, followed by an analysis for an organization you can access.

## Troubleshooting

| Symptom | What to do |
| --- | --- |
| `401` before sign-in | Complete the client's OAuth flow. |
| `401` after a previous successful connection | Reconnect or sign in again through the client. |
| Client registration or redirect URI rejected | Share the client name, version, and exact error with Segwise support. A client registration change may be needed. |
| Scope or resource mismatch | Use `segwise:analytics:query` and the exact resource `https://mcp.segwise.ai/mcp`, then authorize again. |
| No organizations returned | Confirm that you signed in with the correct account and that your organization has access. |
| Multiple organizations returned | Choose the organization by name before starting the analysis. |
| Query stays `running` | Keep polling the same job according to its delay and deadline; do not start duplicate jobs. |
| Job expired, missing, or interrupted | Follow the tool's recovery message. A new query may be needed. |

For a custom MCP client, follow the [MCP authorization specification](https://modelcontextprotocol.io/specification/latest/basic/authorization) and store OAuth credentials in the client's secure credential store. Tokens and authorization codes must not be committed to configuration files or posted in public issues.
