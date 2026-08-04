# REALTOR.ca (realtor-ca)

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
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

REALTOR.ca is the national residential property listing portal of the Canadian Real Estate Association (CREA), the cooperative trade association representing roughly 160,000 REALTORS® across some 60 member boards and associations in Canada. Unlike the United States, where roughly 500 independent MLSs each run their own data pipe, Canadian listing distribution is consolidated through CREA's REALTOR.ca Data Distribution Facility (DDF®), a single national syndication service that normalizes MLS® System data from member boards and republishes it to member websites, franchisor sites, real estate advertising websites and technology providers. CREA sits squarely in the middle of the Canadian value chain — it owns the consumer portal, the national listing pool, and the syndication rails that every downstream site depends on. Its API posture is real but closed: the DDF® Web API is a genuine OData surface at `https://ddfapi.realtor.ca/odata/v1` with public, browsable documentation and a downloadable OpenAPI 3.0.4 description, yet every endpoint (including the OData `$metadata` document) returns 401 without a Bearer token, and tokens are only issued via `client_credentials` against `identity.crea.ca` using data-feed credentials that a REALTOR® or broker owner must first create and link in the member portal. CREA is a RESO member and states its data is normalized to the RESO Data Dictionary, but no CREA entry could be confirmed in the RESO certification directory, so this profile records the DDF® Web API as RESO-aligned rather than RESO-certified. There is no self-serve developer signup, no sandbox, no open data product, and no public consumer search API for realtor.ca itself.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/realtor-ca/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/realtor-ca/refs/heads/main/apis.yml)

## Tags

- Real Estate
- Canada
- Property Listings
- MLS
- RESO
- IDX
- Listing Syndication
- PropTech
- OData
- Rentals

## Timestamps

- **Created:** 2026-07-26
- **Modified:** 2026-07-26

## APIs

### REALTOR.ca DDF Web API

The REALTOR.ca Data Distribution Facility (DDF®) Web API is CREA's national listing distribution surface, exposed as OData v4-style read endpoints over Property, Member, Office, OpenHouse and Destination resources, with per-resource Replication endpoints for full and incremental (LastUpdated) pulls. MLS® System data received from Canadian boards and associations is normalized to the RESO Data Dictionary. All endpoints, including the OData `$metadata` document, require an OAuth 2.0 Bearer access token obtained with data-feed credentials; anonymous requests return HTTP 401.

- **Human URL:** [https://ddfapi-docs.realtor.ca/](https://ddfapi-docs.realtor.ca/)
- **Base URL:** `https://ddfapi.realtor.ca/odata/v1`

#### Tags

- Property Listings
- MLS
- RESO
- OData
- Syndication
- Open House
- Agents
- Offices

#### Properties

- [OpenAPI](openapi/realtor-ca-ddf-web-api-openapi.json) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [OpenAPI](openapi/realtor-ca-ddf-web-api-docs-openapi.json) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Documentation](https://ddfapi-docs.realtor.ca/)
- [API Reference](https://ddfapi-docs.realtor.ca/)
- [Change Log](https://ddfapi-docs.realtor.ca/releasenotes)
- [Authentication](authentication/realtor-ca-crea-identity-openid-configuration.json)
- [Documentation](https://www.crea.ca/ddf/)
- [Support](https://crea.vanillacommunity.com/)

### REALTOR.ca DDF Lead API

The DDF® Lead API accepts an email lead for a REALTOR® from a downstream site. Because member email addresses are deliberately excluded from the DDF® Web API payloads, real estate advertising websites displaying DDF® listings are required to route "Email REALTOR®" form submissions through this endpoint. It is the only write operation in the documented DDF® surface, and it requires the same OAuth 2.0 Bearer access token as the read endpoints.

- **Human URL:** [https://ddfapi-docs.realtor.ca/](https://ddfapi-docs.realtor.ca/)
- **Base URL:** `https://ddfapi.realtor.ca/v1`

#### Tags

- Leads
- Email
- Real Estate

#### Properties

- [OpenAPI](openapi/realtor-ca-ddf-web-api-openapi.json) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [OpenAPI](openapi/realtor-ca-ddf-web-api-docs-openapi.json) — [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Documentation](https://ddfapi-docs.realtor.ca/)
- [Authentication](authentication/realtor-ca-crea-identity-openid-configuration.json)

### CREA Analytics Web Service

A fire-and-forget listing-event logging service documented inside the DDF® Web API documentation. Sites and applications displaying DDF® listings call it to record View, Click and email_realtor events against a ListingID and DestinationID so that agents and broker owners can see where their listings are being consumed. Requests carry ListingID, DestinationID, EventType, UUID and optional IP, ReferralURL and LanguageID arguments; no response handling is required.

- **Human URL:** [https://ddfapi-docs.realtor.ca/](https://ddfapi-docs.realtor.ca/)
- **Base URL:** `https://analytics.crea.ca/LogEvents.svc`

#### Tags

- Analytics
- Events
- Listings

#### Properties

- [Documentation](https://ddfapi-docs.realtor.ca/)

### CREA Identity Server

CREA's OAuth 2.0 / OpenID Connect authorization server at identity.crea.ca, which issues the access tokens every DDF® Web API call requires. Its discovery document is served anonymously and advertises the `client_credentials`, `authorization_code`, `refresh_token`, `implicit`, `device_code` and CIBA grants, and four scopes — `openid`, `DDFApi_Read`, `OfferManagementApi.read.write` and `BoardDataApi.read`. The documented DDF® flow is `client_credentials` with scope `DDFApi_Read`, where `client_id` and `client_secret` are the username and password of a registered DDF® Destination data feed. Tokens are valid for 60 minutes.

- **Human URL:** [https://ddfapi-docs.realtor.ca/](https://ddfapi-docs.realtor.ca/)
- **Base URL:** `https://identity.crea.ca`

#### Tags

- Authentication
- OAuth
- OpenID Connect
- Identity

#### Properties

- [Authentication](authentication/realtor-ca-crea-identity-openid-configuration.json)
- [Documentation](https://ddfapi-docs.realtor.ca/)

## Common Properties

- [Website](https://www.realtor.ca/)
- [Website](https://www.crea.ca/)
- [Documentation](https://ddfapi-docs.realtor.ca/)
- [Documentation](https://www.crea.ca/ddf/)
- [Getting Started](https://www.crea.ca/ddf/get-started/)
- [Terms of Service](https://www.crea.ca/ddf/member-policy-and-rules/)
- [Support](https://support.crea.ca/)
- [Forum](https://crea.vanillacommunity.com/)
- [Change Log](https://ddfapi-docs.realtor.ca/releasenotes)
- [Authentication](authentication/realtor-ca-crea-identity-openid-configuration.json)
- [Authentication](authentication/realtor-ca-auth0-openid-configuration.json)
- [LinkedIn](https://www.linkedin.com/company/realtor-ca/)
- [LinkedIn](https://www.linkedin.com/company/canadian-real-estate-association/)

## Access Posture

| Question | Answer |
| --- | --- |
| Developer portal | [https://ddfapi-docs.realtor.ca/](https://ddfapi-docs.realtor.ca/) — public documentation, no login |
| Self-serve signup | No |
| Access gate | Membership required — REALTOR® or broker owner, or a Technology Provider linked to their registered Destination feed |
| What you must accept | REALTOR.ca DDF® Member Policy and Rules; a feed registered at `member.realtor.ca` |
| Auth model | OAuth 2.0 `client_credentials` → `https://identity.crea.ca/connect/token`, scope `DDFApi_Read`, 60-minute Bearer token |
| RESO posture | RESO Data Dictionary-aligned; CREA is a RESO member, but no certification could be confirmed in the RESO directory |
| OData `$metadata` | `https://ddfapi.realtor.ca/odata/v1/$metadata` — HTTP 401, authentication required |
| Open data | None |
| Webhooks / events | None — polling via `LastUpdated` on the Replication endpoints |
| SDKs / Postman collection | None published by CREA |

See [review.yml](review.yml) for the full probe log, harvest provenance, and RESO / access-gate findings.

## Maintainers

- Kin Lane — kin@apievangelist.com
