---
name: Call the Esri ArcGIS Location Services MCP server
description: >-
  Connect an agent to Esri's hosted MCP endpoint for ArcGIS Location Services, get a token with
  the right privileges, and understand why the tool list you see may be shorter than the one in
  the docs.
api: mcp/esri-mcp.yml
operations: [getOAuthToken]
generated: '2026-09-07'
method: generated
source: https://developers.arcgis.com/ai/mcp-arcgis-location-services/
---

# Call the Esri ArcGIS Location Services MCP server

## The endpoint

```
https://location-services-mcp.arcgis.com/beta/mcp
```

Streamable HTTP transport. **There is no stdio build** — Esri's FAQ says so explicitly. If a
config you were given uses `npx` or a local command for ArcGIS location services, it is not
Esri's server.

Protected-resource metadata is served at
`https://location-services-mcp.arcgis.com/.well-known/oauth-protected-resource` and names
`https://arcgis.com/` as the authorization server.

## Get a token

Either an ArcGIS API key credential or an OAuth 2.0 credential. Send it as:

```
Authorization: Bearer <access token>
Accept: application/json, text/event-stream
Content-Type: application/json
```

The credential needs these privileges — and this is the part that trips people up:

- `Portal service > General privileges > Apps and capabilities > Allow beta access`
- `Location services > Geocoding > Geocode (stored)` — **pay-as-you-go accounts only**
- `Location services > Routing > Simple routing`
- `Location services > Elevation > Elevation service`
- `Location services > Static maps > Static maps service`
- `Location services > Data Enrichment > GeoEnrichment service`

## Steps

1. `POST` `{"jsonrpc":"2.0","id":1,"method":"tools/list"}` to confirm the connection. Without a
   token you get `{"error":{"code":499,"message":"Token Required."}}`.
2. Call a tool. The seven published tools are `find_address_candidates`, `reverse_geocode`,
   `solve_route`, `elevation_at_locations`, `map_with_overlay`, `get_topic_fields` and
   `describe_location`.
3. `prompts/list` returns fourteen server-supplied prompt templates — use them rather than
   inventing phrasing.

## Rules

- **A short tool list is a privilege problem, not an outage.** The server filters `tools/list`
  by the privileges on your token. If `find_address_candidates` is missing, your account is not
  pay-as-you-go enabled.
- **`map_with_overlay` returns base64 PNG.** Some clients cap MCP response payload size. Reduce
  width/height, reduce geometry count, or tighten the extent before assuming a failure.
- **Data enrichment is two-stage.** Call `get_topic_fields` for a topic and country first, then
  pass those fields to `describe_location`.
- **Every successful tool call is billed** against the underlying location service. The MCP
  protocol itself is free; the work is not. An agent that loops is an agent that spends.
- **This is beta.** Fleet routing and location-allocation are not available.

## References

- mcp/esri-mcp.yml — full tool and prompt inventory
- mcp/esri-tool-crosswalk.yml — which tools map to which REST operations
- conventions/esri-conventions.yml
