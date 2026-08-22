# Zillapi (zillapi)

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Independent third-party provider of Zillow-sourced U.S. residential property data via a REST API, hosted MCP server, and agent skills. Returns ~300+ fields per property including price, Zestimate, rent Zestimate, photos, schools, taxes, and full price history, plus listing search, building extraction, async batch jobs, and signed webhooks.

**APIs.json:** [https://zillapi.apievangelist.com/apis.yml](https://zillapi.apievangelist.com/apis.yml)

## Tags

- real estate
- proptech
- property data
- zillow
- zestimate
- valuation
- AVM
- listings
- MCP
- AI agent
- REST API

## Timestamps

- **Created:** 2026-07-13
- **Modified:** 2026-08-09

## APIs

### Zillapi Account API

The Account API from Zillapi — 2 operation(s) for account.

- **Human URL:** [https://zillapi.com/api/properties/](https://zillapi.com/api/properties/)
- **Base URL:** `https://api.zillapi.com`

#### Tags

- Account

#### Properties

- [OpenAPI](openapi/zillapi-account-api-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/zillapi-account-api.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/zillapi-account-api.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)
- [Documentation](https://zillapi.com/quickstart/)
- [L L Ms Txt](https://zillapi.com/llms.txt)
- [L L Ms Txt](https://zillapi.com/llms-full.txt)
- [Tool Crosswalk](mcp/zillapi-tool-crosswalk.yml)
- [L L Ms Txt](llms/zillapi-llms.txt)
- [Data Model](data-model/zillapi-data-model.yml)
- [Examples](examples/zillapi-examples.yml)

### Zillapi Buildings API

The Buildings API from Zillapi — 1 operation(s) for buildings.

- **Human URL:** [https://zillapi.com/api/properties/](https://zillapi.com/api/properties/)
- **Base URL:** `https://api.zillapi.com`

#### Tags

- Buildings

#### Properties

- [OpenAPI](openapi/zillapi-buildings-api-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/zillapi-buildings-api.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/zillapi-buildings-api.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)
- [Documentation](https://zillapi.com/quickstart/)
- [L L Ms Txt](https://zillapi.com/llms.txt)
- [L L Ms Txt](https://zillapi.com/llms-full.txt)
- [Tool Crosswalk](mcp/zillapi-tool-crosswalk.yml)
- [L L Ms Txt](llms/zillapi-llms.txt)
- [Data Model](data-model/zillapi-data-model.yml)
- [Examples](examples/zillapi-examples.yml)

### Zillapi Jobs API

The Jobs API from Zillapi — 3 operation(s) for jobs.

- **Human URL:** [https://zillapi.com/api/properties/](https://zillapi.com/api/properties/)
- **Base URL:** `https://api.zillapi.com`

#### Tags

- Jobs

#### Properties

- [OpenAPI](openapi/zillapi-jobs-api-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/zillapi-jobs-api.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/zillapi-jobs-api.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)
- [Documentation](https://zillapi.com/quickstart/)
- [L L Ms Txt](https://zillapi.com/llms.txt)
- [L L Ms Txt](https://zillapi.com/llms-full.txt)
- [Tool Crosswalk](mcp/zillapi-tool-crosswalk.yml)
- [L L Ms Txt](llms/zillapi-llms.txt)
- [Data Model](data-model/zillapi-data-model.yml)
- [Examples](examples/zillapi-examples.yml)

### Zillapi Listings API

The Listings API from Zillapi — 4 operation(s) for listings.

- **Human URL:** [https://zillapi.com/api/properties/](https://zillapi.com/api/properties/)
- **Base URL:** `https://api.zillapi.com`

#### Tags

- Listings

#### Properties

- [OpenAPI](openapi/zillapi-listings-api-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/zillapi-listings-api.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/zillapi-listings-api.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)
- [Documentation](https://zillapi.com/quickstart/)
- [L L Ms Txt](https://zillapi.com/llms.txt)
- [L L Ms Txt](https://zillapi.com/llms-full.txt)
- [Tool Crosswalk](mcp/zillapi-tool-crosswalk.yml)
- [L L Ms Txt](llms/zillapi-llms.txt)
- [Data Model](data-model/zillapi-data-model.yml)
- [Examples](examples/zillapi-examples.yml)

### Zillapi Properties API

The Properties API from Zillapi — 13 operation(s) for properties.

- **Human URL:** [https://zillapi.com/api/properties/](https://zillapi.com/api/properties/)
- **Base URL:** `https://api.zillapi.com`

#### Tags

- Properties

#### Properties

- [OpenAPI](openapi/zillapi-properties-api-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/zillapi-properties-api.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/zillapi-properties-api.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)
- [Documentation](https://zillapi.com/quickstart/)
- [L L Ms Txt](https://zillapi.com/llms.txt)
- [L L Ms Txt](https://zillapi.com/llms-full.txt)
- [Tool Crosswalk](mcp/zillapi-tool-crosswalk.yml)
- [L L Ms Txt](llms/zillapi-llms.txt)
- [Data Model](data-model/zillapi-data-model.yml)
- [Examples](examples/zillapi-examples.yml)

### Zillapi Search API

The Search API from Zillapi — 2 operation(s) for search.

- **Human URL:** [https://zillapi.com/api/properties/](https://zillapi.com/api/properties/)
- **Base URL:** `https://api.zillapi.com`

#### Tags

- Search

#### Properties

- [OpenAPI](openapi/zillapi-search-api-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/zillapi-search-api.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/zillapi-search-api.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)
- [Documentation](https://zillapi.com/quickstart/)
- [L L Ms Txt](https://zillapi.com/llms.txt)
- [L L Ms Txt](https://zillapi.com/llms-full.txt)
- [Tool Crosswalk](mcp/zillapi-tool-crosswalk.yml)
- [L L Ms Txt](llms/zillapi-llms.txt)
- [Data Model](data-model/zillapi-data-model.yml)
- [Examples](examples/zillapi-examples.yml)

### Zillapi Webhooks API

The Webhooks API from Zillapi — 3 operation(s) for webhooks.

- **Human URL:** [https://zillapi.com/api/properties/](https://zillapi.com/api/properties/)
- **Base URL:** `https://api.zillapi.com`

#### Tags

- Webhooks

#### Properties

- [OpenAPI](openapi/zillapi-webhooks-api-openapi.yml) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Postman Collection](collections/zillapi-webhooks-api.postman_collection.json) — [Postman Collection 2.1](https://schema.getpostman.com/json/collection/v2.1.0/collection.json)
- [Open Collection](collections/zillapi-webhooks-api.opencollection.json) — [Open Collection 1.0](https://schema.opencollection.com/opencollection/v1.0.0.json)
- [Documentation](https://zillapi.com/quickstart/)
- [L L Ms Txt](https://zillapi.com/llms.txt)
- [L L Ms Txt](https://zillapi.com/llms-full.txt)
- [Tool Crosswalk](mcp/zillapi-tool-crosswalk.yml)
- [L L Ms Txt](llms/zillapi-llms.txt)
- [Data Model](data-model/zillapi-data-model.yml)
- [Examples](examples/zillapi-examples.yml)

## Common Properties

- [Agent Skill](skills/zillapi-get-zestimate.md)
- [M C P Server](mcp/zillapi-mcp.yml)
- [Overlay](overlays/zillapi-openapi-overlay.yaml)
- [Domain Security](security/zillapi-domain-security.yml)
- [Agentic Access](agentic-access/zillapi-agentic-access.yml)
- [Authentication](authentication/zillapi-authentication.yml)
- [O Auth Scopes](scopes/zillapi-scopes.yml)
- [Conventions](conventions/zillapi-conventions.yml)
- [Error Catalog](errors/zillapi-error-codes.yml)
- [Lifecycle](lifecycle/zillapi-lifecycle.yml)
- [Conformance](conformance/zillapi-conformance.yml)
- [Packages](packages/zillapi-packages.yml)
- [Well Known](well-known/zillapi-well-known.yml)
- [A P I Catalog](https://zillapi.com/.well-known/api-catalog)
- [Webhooks](asyncapi/zillapi-webhooks.yml)
- [Rate Limits](rate-limits/zillapi-rate-limits.yml)
- [Plans](plans/zillapi-plans.yml)
- [Vulnerability Disclosure](security/zillapi-vulnerability-disclosure.yml)
- [Security](https://github.com/ZeroPointRepo/zillow-skills/blob/main/SECURITY.md)
- [Developer Portal](https://zillapi.com/)
- [Documentation](https://zillapi.com/quickstart/)
- [API Reference](https://zillapi.com/api/properties/)
- [Getting Started](https://zillapi.com/quickstart/)
- [Blog](https://zillapi.com/blog/)
- [GitHub Organization](https://github.com/ZeroPointRepo/zillow-skills)
- [Pricing](https://zillapi.com/pricing/)
- [Sign Up](https://zillapi.com/signup)
- [Login](https://zillapi.com/login)
- [Terms of Service](https://zillapi.com/legal/terms/)
- [Privacy Policy](https://zillapi.com/legal/privacy/)
- [Support](mailto:hello@zillapi.com)

## Maintainers

**FN:** ZeroPointRepo
**Email:** hello@zillapi.com
**URL:** https://github.com/ZeroPointRepo/zillow-skills
