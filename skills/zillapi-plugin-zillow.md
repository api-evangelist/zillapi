---
name: zillow
description: Live Zillow property data via the bundled Zillapi MCP server — full property records, Zestimates, and listing search on 160M+ U.S. homes. Use when the user asks about a specific U.S. property, home values, rent estimates, comps, or wants to find for-sale/for-rent/sold listings. Do not use for addresses that merely appear in passing.
license: MIT
---

# Zillow property data (Zillapi)

This plugin bundles the hosted **`zillapi` MCP server** (`https://api.zillapi.com/mcp`). Its four tools are the only data path — never scrape zillow.com and never call the REST API directly from this skill.

**Authentication is automatic.** The MCP server uses OAuth 2.1: the first tool call prompts the user to sign in (or create a free account — 100 credits, no card). There is no API key to configure. If tools fail with an auth error, tell the user to complete the sign-in prompt in their client, or re-enable the `zillapi` MCP server in settings.

## When to use

**DO use when the user:**

- Asks what a home is worth, its Zestimate, rent Zestimate, taxes, or last sale → `get_zestimate` (if you have a zpid) or `lookup_property_by_address`
- Gives a U.S. address and asks about the property, its photos, schools, price history, or listing agent → `lookup_property_by_address` (one call returns all of it)
- Pastes a `zillow.com/homedetails/...` link → extract the zpid from the URL (the number before `_zpid`) and use `lookup_property_by_zpid`
- Asks to find homes matching criteria (area, price, beds, for sale / for rent / sold) → `search_listings`
- Asks for comps → look up the subject property, then `search_listings` with `status: "sold"` in a small bounding box around it

**Do NOT use when:**

- An address appears incidentally (signatures, unrelated documents)
- The user discusses real estate abstractly without asking for data on a specific property or search
- The user hasn't signaled they want a lookup — tool calls cost credits; when intent is ambiguous, confirm first

## Tools (exact MCP surface)

### `lookup_property_by_address` — 3 credits
- `address` (string, required): full street address with city, state, ZIP — e.g. `"17 Zelma Dr, Greenville, SC 29617"`. Partial addresses ≥6 chars usually resolve.
- Returns the **full record** (300+ fields): price, Zestimate, rent Zestimate, tax history, price history, photos, schools, agent contact, lat/lon, zpid.
- This already contains everything — don't follow it with `get_zestimate` for the same property.

### `lookup_property_by_zpid` — 1 credit (24 h cache)
- `zpid` (string, required): numeric Zillow property id, e.g. `"11026031"`.
- Same full record, cheaper. Prefer this whenever you already have a zpid (from a Zillow URL or a previous search result).

### `get_zestimate` — 1 credit
- `zpid` (string, required).
- Returns just the valuation: Zestimate, rent Zestimate. Use when you have a zpid and only need the value. If you only have an address, use `lookup_property_by_address` instead (it includes the Zestimate).

### `search_listings` — 1 credit **per result**
- `bbox` (string, required): `"west,south,east,north"` in decimal degrees, e.g. `"-82.48,34.78,-82.30,34.90"`. Derive it from the named location; for a city use roughly ±0.05–0.15°, for a neighborhood/comps radius ±0.01–0.03° around the subject's lat/lon.
- `status` (string, required): `"for_sale"` | `"for_rent"` | `"sold"`.
- `beds_min` (integer, optional), `price_max` (integer USD, optional).
- Each returned listing costs one credit — keep the bbox tight and filters set so results stay relevant and cheap. Results include zpids you can feed into `lookup_property_by_zpid`.

## Credit hygiene

- Cheapest path first: zpid lookups (1) beat address lookups (3); a focused bbox beats a metro-wide one.
- Batch reasoning: one full record answers value + schools + history + agent questions — don't re-call per question.
- Failed calls don't consume credits.

## Trademark

Zillapi is an independent service and is not affiliated with, endorsed by, or sponsored by Zillow Group, Inc. "Zillow" and "Zestimate" are registered trademarks of Zillow Group, Inc.
