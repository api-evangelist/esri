---
name: Geocode an address with ArcGIS
description: >-
  Turn a street address or place name into coordinates using the Esri ArcGIS World Geocoding
  Service, including the token you need first and the traps that make ArcGIS look like it
  succeeded when it did not.
api: openapi/esri-geocoding-api-openapi.yml
operations: [getOAuthToken, findAddressCandidates, suggestAddresses]
generated: '2026-09-07'
method: generated
source: openapi/esri-geocoding-api-openapi.yml + https://developers.arcgis.com/rest/geocode/
---

# Geocode an address with ArcGIS

## Before you start

You need an ArcGIS access token. Either an **API key** created in the ArcGIS Location Platform
dashboard, or an OAuth 2.0 token from `getOAuthToken`
(`POST https://www.arcgis.com/sharing/rest/oauth2/token`, `grant_type=client_credentials`).

The credential must carry the privilege **Location services > Geocoding > Geocode (stored)**.
There is no OAuth scope to request — ArcGIS authorizes by privilege, not scope. If you get
`{"error":{"code":403}}`, the credential is missing a privilege, not the token.

## Steps

1. **Acquire a token** — `getOAuthToken`. Cache it; it carries `expires_in` and re-minting on
   every call is wasted latency.
2. **(Optional) autocomplete** — `suggestAddresses` against
   `/World/GeocodeServer/suggest` for type-ahead. It returns `text` plus a `magicKey`.
   Pass that `magicKey` straight back into `findAddressCandidates`; it is far more accurate
   than re-sending the raw string.
3. **Geocode** — `findAddressCandidates` against
   `https://geocode-api.arcgis.com/arcgis/rest/services/World/GeocodeServer/findAddressCandidates`.
   Send either `SingleLine` (whole address in one string) or the parsed
   `address`/`city`/`region`/`postal` set — not both.
   **Always send `f=json`.** The default response format is HTML.
4. **Read the result** — `candidates[]`, each with `address`, `score` (0-100), `location.x`,
   `location.y` and an open `attributes` bag. Treat `score` as your confidence gate; do not
   assume `candidates[0]` is right.

## Rules

- **Parse the body before trusting the status line.** ArcGIS REST endpoints can return HTTP 200
  with `{"error": {"code": 498, "message": "Invalid token"}}` in the body. Check for an `error`
  key on every response.
- **Retries are billable.** Each successful geocode is metered as a stored geocode. There is no
  idempotency key, and there is nothing to undo — a retry is a second charge, not a second
  attempt at the same charge.
- **Rate limits give you nothing to work with.** Exhaustion is HTTP 429 with a one-minute
  cooling-off period, no `Retry-After` and no `RateLimit-*` headers. Wait a fixed 60 seconds.
- **Prefer the MCP tool if you have one.** `find_address_candidates` on
  `https://location-services-mcp.arcgis.com/beta/mcp` is the same operation with a typed
  interface. See `mcp/esri-mcp.yml`.

## References

- errors/esri-problem-types.yml — the error envelope and the code list
- conventions/esri-conventions.yml — the `f` parameter, pagination, rate-limit signalling
- rate-limits/esri-rate-limits.yml
