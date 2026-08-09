---
name: get-zestimate
description: Fetch the current Zestimate (Zillow's automated valuation) and rent Zestimate for a U.S. property by Zillow zpid.
version: 1.0.0
---

# Get Zestimate

Use this skill when the user asks for the estimated value of a U.S. property. The Zestimate is Zillow's proprietary automated valuation; the rent Zestimate is the equivalent for rental value.

## When to use

- User asks "what is this house worth", "estimated value", "rental estimate"
- You need a sale-price valuation OR a monthly rental estimate
- You already have the zpid (or can look it up first via `lookup-zillow-property`)

## Prerequisites

- A Zillapi API key
- The Zillow zpid for the property

If you only have an address, run the `lookup-zillow-property` skill first to get the zpid, then call this one. Or use the full property lookup, which already includes the Zestimate.

## Procedure

### REST

```bash
curl https://api.zillapi.com/v1/properties/11026031/zestimate \
  -H "Authorization: Bearer $ZILLAPI_KEY"
```

### Via MCP

```
tool: get_zestimate
args: { "zpid": "11026031" }
```

## Response shape

```json
{
  "data": {
    "zestimate": 305100,
    "rent_zestimate": 1850,
    "tax_assessed_value": 250000,
    "last_sold_price": 240000,
    "currency": "USD"
  },
  "request_id": "..."
}
```

`zestimate` is the sale-price valuation. `rent_zestimate` is the monthly rental valuation. `tax_assessed_value` and `last_sold_price` are pulled from public records for context. Any field may be `null` if Zillow has no value for it.

## Accuracy

Zillow publishes a national median error rate of around 1.9% for on-market homes and around 7.2% for off-market homes (as of March 2026, per zillow.com/zestimate). Roughly 70% of Zestimates are within 5% of the eventual sale price.

The Zestimate is a model output, not a real-time appraisal. Treat it as a starting point, not a binding number.

## Cost

1 credit per call. The result is cache-served while fresh, so repeat calls for the same zpid are typically free.

## Errors

| Status | code | What to do |
|---|---|---|
| 404 | `not_found` | The zpid is invalid or the property has no Zestimate |
| 402 | `out_of_credits` | Top up at https://zillapi.com/app/billing |

## When NOT to use

- Bridge Interactive (Zillow's official MLS partner program) does not expose Zestimates to most partners. If you have Bridge access, you still need a separate path for valuation.
- Do NOT use Zestimates as the basis for a binding offer or appraisal. They are estimates, not certified valuations.

## Reference

- Zillow Zestimate documentation: https://www.zillow.com/zestimate/
- API reference: https://zillapi.com/api/properties/#zestimate
