---
name: lookup-zillow-property
description: Look up a U.S. property by address or Zillow zpid and return full property data including price, Zestimate, photos, schools, taxes, agent contact, and price history.
version: 1.0.0
---

# Lookup Zillow Property

Use this skill when you need full data on a single U.S. property: address, price, Zestimate, photos, schools, taxes, listing agent, or full price history.

## When to use

- User mentions a specific home, street address, or Zillow listing
- User gives you a numeric Zillow property id (zpid) like `11026031`
- You need 300+ structured fields about a property in a single call

## Prerequisites

- A Zillapi API key (Bearer token starting with `zk_`). Get one at https://zillapi.com/login. Free tier: 100 credits at signup, no card.
- Network access to `https://api.zillapi.com`

## Procedure

Pick the option that matches what the user gave you.

### Option A: by full address

When the user gives you a street address, prefer this. Geocoding is handled server-side.

```bash
curl https://api.zillapi.com/v1/properties/by-address \
  -G --data-urlencode "address=17 Zelma Dr, Greenville, SC 29617" \
  -H "Authorization: Bearer $ZILLAPI_KEY"
```

### Option B: by Zillow zpid

When the user gives you a numeric id, prefer this. It is faster and served from cache when fresh.

```bash
curl https://api.zillapi.com/v1/properties/11026031 \
  -H "Authorization: Bearer $ZILLAPI_KEY"
```

### Option C: by Zillow URL

When the user pastes a `zillow.com/homedetails/...` URL.

```bash
curl https://api.zillapi.com/v1/properties/by-url \
  -G --data-urlencode "url=https://www.zillow.com/homedetails/.../11026031_zpid/" \
  -H "Authorization: Bearer $ZILLAPI_KEY"
```

### Option D: via MCP (recommended for agent runtimes)

If you support the Model Context Protocol (Streamable HTTP transport), call the tool directly on the MCP server.

- Endpoint: `https://api.zillapi.com/mcp`
- Tool: `lookup_property_by_address` or `lookup_property_by_zpid`
- Auth: same Bearer token in the `Authorization` header

## Response shape

```json
{
  "data": {
    "zpid": "11026031",
    "address": { "streetAddress": "17 Zelma Dr", "city": "Greenville", "state": "SC", "zipcode": "29617" },
    "price": 295000,
    "zestimate": 305100,
    "rentZestimate": 1850,
    "bedrooms": 3,
    "bathrooms": 2,
    "livingArea": 1432,
    "yearBuilt": 1965,
    "homeType": "SINGLE_FAMILY",
    "latitude": 34.882,
    "longitude": -82.428
  },
  "request_id": "..."
}
```

## Sub-resources

If you only need a slice of the data, prefer the sub-resource endpoints. Same cache, less data over the wire:

- `/v1/properties/{zpid}/photos`
- `/v1/properties/{zpid}/zestimate`
- `/v1/properties/{zpid}/price-history`
- `/v1/properties/{zpid}/schools`
- `/v1/properties/{zpid}/tax-history`
- `/v1/properties/{zpid}/agent`
- `/v1/properties/{zpid}/nearby`
- `/v1/properties/{zpid}/open-houses`
- `/v1/properties/{zpid}/facts`

## Cost

One credit per successful (2xx) call. Sub-resource calls share the parent's cache window and are free when fresh.

## Errors

| Status | code | What to do |
|---|---|---|
| 401 | `missing_api_key` or `invalid_api_key` | Ask the user for a Zillapi key |
| 402 | `out_of_credits` | Tell the user to top up at https://zillapi.com/app/billing |
| 404 | `not_found` | The address or zpid is not in Zillow's index |
| 429 | `rate_limited` | Back off; respect the `Retry-After` header |

## Reference

- API reference: https://zillapi.com/api/properties/
- OpenAPI spec: https://zillapi.com/openapi.json
- Trademark posture: https://zillapi.com/legal/trademark-and-affiliation/
