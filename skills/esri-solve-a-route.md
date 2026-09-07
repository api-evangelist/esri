---
name: Solve a route with ArcGIS
description: >-
  Get turn-by-turn directions and travel time between stops using the Esri ArcGIS World Routing
  Service, chaining geocoding first when you start from addresses rather than coordinates.
api: openapi/esri-routing-api-openapi.yml
operations: [getOAuthToken, findAddressCandidates, solveRoute]
generated: '2026-09-07'
method: generated
source: openapi/esri-routing-api-openapi.yml + https://developers.arcgis.com/rest/routing/
---

# Solve a route with ArcGIS

## Before you start

An ArcGIS access token with the privilege **Location services > Routing > Simple routing**.
If you also need to turn addresses into stops, the same credential needs
**Location services > Geocoding > Geocode (stored)**.

## Steps

1. **Acquire a token** — `getOAuthToken`.
2. **Resolve each stop to coordinates** — if the caller gave you addresses, run
   `findAddressCandidates` per address first and take `candidates[0].location`. Routing takes
   geometry, not prose.
3. **Solve** — `solveRoute` against
   `https://route-api.arcgis.com/arcgis/rest/services/World/Route/solve`.
   Pass `stops` as an ordered list of `x,y` pairs. Send `f=json`.
   For a departure-time-aware answer, set the start time — traffic-aware travel time differs
   materially from the free-flow default.
4. **Read the result** — route geometry plus summary attributes (total time, total distance)
   and, when requested, turn-by-turn directions.

## Rules

- **Order matters.** `stops` is an ordered list; ArcGIS solves them in the order given unless
  you explicitly ask it to optimize.
- **Advanced routing is a different product.** Fleet routing and location-allocation are not on
  this endpoint, and are explicitly out of scope for the MCP `solve_route` tool too.
- **Each solve is metered as one Route.** Retrying a failed-looking call that actually succeeded
  bills twice. Parse the body for an `error` key before retrying.
- **429 means wait 60 seconds.** No `Retry-After` header is published.

## References

- mcp/esri-mcp.yml — `solve_route` is the same operation as an MCP tool
- conventions/esri-conventions.yml
- errors/esri-problem-types.yml
