---
name: search-zillow-listings
description: Search U.S. property listings inside a geographic bounding box, filtered by status (for_sale, for_rent, sold), price ceiling, and bedroom count.
version: 1.0.0
---

# Search Zillow Listings

Use this skill when the user wants a list of properties matching geographic and price criteria. For example: "what's for sale in Austin under 600k", "show me 3-bedroom rentals in San Francisco under 4000 a month", "what sold in zip 90210 last quarter".

## When to use

- User specifies a region (city, neighborhood, ZIP, lat/lon box) AND a status (for_sale, for_rent, sold)
- User has a price ceiling, bedroom minimum, or other filter
- You need a paginated list of properties, not a single property

## Prerequisites

- A Zillapi API key. Get one at https://zillapi.com/login.
- A bounding box in `west,south,east,north` decimal lat/lon format. If the user gave you a city, look up the bbox first or use a geocoding service before calling.

## Procedure

### Sync (small results, fast path)

```bash
curl "https://api.zillapi.com/v1/listings?bbox=-83.0,34.7,-82.0,35.0&status=for_sale&beds_min=3&price_max=600000" \
  -H "Authorization: Bearer $ZILLAPI_KEY"
```

`bbox` is required. `status` is required (`for_sale`, `for_rent`, or `sold`). Other filters are optional.

### Async (large regions or many filters)

For state-wide or country-wide searches, dispatch async and consume via webhook or polling.

```bash
curl https://api.zillapi.com/v1/search \
  -X POST \
  -H "Authorization: Bearer $ZILLAPI_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "filters": { "bbox": [-125, 32, -114, 42], "status": "for_sale", "price_max": 600000 },
    "limit": 5000
  }'
```

Returns `{ "data": { "job_id": "...", "status": "queued" } }` with `202`. Poll `/v1/jobs/{id}` or set up a webhook.

### Via MCP

```
tool: search_listings
args: {
  "bbox": "-83.0,34.7,-82.0,35.0",
  "status": "for_sale",
  "beds_min": 3,
  "price_max": 600000
}
```

## Response shape

Sync:
```json
{
  "data": [
    { "zpid": "...", "price": 295000, "address": {...}, "bedrooms": 3, ... },
    ...
  ],
  "meta": { "limit": 50, "offset": 0, "has_more": true },
  "request_id": "..."
}
```

## Output formats

Append `?format=csv` or `?format=ndjson` for stream-friendly output. NDJSON is the right choice for piping into BigQuery, Snowflake, or DuckDB.

## Cost

- Sync: 1 credit per page returned
- Async: 1 credit per result item, billed when the job completes

## Errors

| Status | code | What to do |
|---|---|---|
| 400 | `invalid_bbox` | Verify bbox format: `west,south,east,north` |
| 400 | `invalid_status` | Use `for_sale`, `for_rent`, or `sold` |
| 402 | `out_of_credits` | Top up at https://zillapi.com/app/billing |
| 429 | `rate_limited` | Back off |

## Tips for agents

- Before calling for a state, get the state's bounding box. Calling with a too-large bbox returns thousands of results and uses many credits.
- Cache aggressively. Listing data changes daily, not minute-to-minute, so a short cache on your side is appropriate for most use cases.
- If you need only a count, use `limit=0` to get just the meta block.

## Reference

- API reference: https://zillapi.com/api/listings/
- Search filters: https://zillapi.com/api/search/
