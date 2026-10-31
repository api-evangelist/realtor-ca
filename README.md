# REALTOR.ca

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

REALTOR.ca is the national residential property listing portal of the Canadian Real Estate Association (CREA), the cooperative trade association representing roughly 160,000 REALTORS® across some 60 member boards and associations in Canada. Unlike the United States, where roughly 500 independent MLSs each run their own data pipe, Canadian listing distribution is consolidated through CREA's REALTOR.ca Data Distribution Facility (DDF®), a single national syndication service that normalizes MLS® System data from member boards and republishes it to member websites, franchisor sites, real estate advertising websites and technology providers. CREA sits squarely in the middle of the Canadian value chain — it owns the consumer portal, the national listing pool, and the syndication rails that every downstream site depends on. Its API posture is real but closed: the DDF® Web API is a genuine OData surface at https://ddfapi.realtor.ca/odata/v1 with public, browsable documentation and a downloadable OpenAPI 3.0.4 description, yet every endpoint (including the OData $metadata document) returns 401 without a Bearer token, and tokens are only issued via client_credentials against identity.crea.ca using data-feed credentials that a REALTOR® or broker owner must first create and link in the member portal. CREA is a RESO member and states its data is normalized to the RESO Data Dictionary, but no CREA entry could be confirmed in the RESO certification directory, so this profile records the DDF® Web API as RESO-aligned rather than RESO-certified. There is no self-serve developer signup, no sandbox, no open data product, and no public consumer search API for realtor.ca itself.

**Areas:** Real Estate · Canada · Property Listings · MLS · RESO · IDX · Listing Syndication · PropTech · OData · Rentals

## REALTOR.ca on APIs.io

Everything profiled for REALTOR.ca is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/realtor-ca/](https://apis.io/providers/realtor-ca/)** — or go straight to one of the APIs or properties below.

### APIs (8)

- [CREA Analytics Web Service](https://apis.io/apis/realtor-ca/crea-analytics-web-service/) — A fire-and-forget listing-event logging service documented inside the DDF® Web API documentation. Sites and applications displaying DDF® listings call it to…
- [CREA Identity Server](https://apis.io/apis/realtor-ca/crea-identity-server/) — CREA's OAuth 2.0 / OpenID Connect authorization server at identity.crea.ca, which issues the access tokens every DDF® Web API call requires. Its discovery…
- [REALTOR.ca Destination API](https://apis.io/apis/realtor-ca/realtor-ca-destination-api/) — Get details about each destination linked to the Technology Provider
- [REALTOR.ca Lead API](https://apis.io/apis/realtor-ca/realtor-ca-lead-api/) — Create Lead
- [REALTOR.ca Member API](https://apis.io/apis/realtor-ca/realtor-ca-member-api/) — Get Members
- [REALTOR.ca Office API](https://apis.io/apis/realtor-ca/realtor-ca-office-api/) — Get Offices
- [REALTOR.ca Open House API](https://apis.io/apis/realtor-ca/realtor-ca-openhouse-api/) — The OpenHouse API from REALTOR.ca — 2 operation(s) for openhouse.
- [REALTOR.ca Property API](https://apis.io/apis/realtor-ca/realtor-ca-property-api/) — Get Properties

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [realtor-ca-ddf](https://apis.io/servers/realtor-ca/realtor-ca-ddf/)

#### Agentic access (1)

Which operations an agent can call, which act, and which need a human in the loop.

- [Realtor Ca Agentic Access](https://apis.io/agentic-access/realtor-ca/realtor-ca-agentic-access/)

#### Collections (2)

Postman and Open Collection request collections.

- [REALTOR.ca DDF® Web API Documentation](https://apis.io/collections/realtor-ca/open-realtor-ca-ddf-web-api/)
- [REALTOR.ca DDF® Web API Documentation](https://apis.io/collections/realtor-ca/open-realtor-ca-ddf-web-api-docs/)

#### Overlays (2)

OpenAPI overlays, including conversational phrasing for each operation.

- [Realtor Ca Ddf Web Api Docs Overlay](https://apis.io/overlays/realtor-ca/realtor-ca-ddf-web-api-docs-overlay/)
- [Realtor Ca Ddf Web Api Overlay](https://apis.io/overlays/realtor-ca/realtor-ca-ddf-web-api-overlay/)

#### Security (3)

Authentication, trust centre, and vulnerability disclosure.

- [Realtor Ca Authentication](https://apis.io/security/realtor-ca/realtor-ca-authentication/)
- [Realtor Ca Domain Security](https://apis.io/security/realtor-ca/realtor-ca-domain-security/)
- [Realtor Ca Vulnerability Disclosure](https://apis.io/security/realtor-ca/realtor-ca-vulnerability-disclosure/)

#### OAuth scopes (1)

The permissions the API asks for.

- [Realtor Ca Scopes](https://apis.io/scopes/realtor-ca/realtor-ca-scopes/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.realtor.ca/)
- [Documentation](https://ddfapi-docs.realtor.ca/)
- [Getting started](https://ddfapi-docs.realtor.ca/#section/Quickstart-Overview)
- [API reference](https://ddfapi-docs.realtor.ca/)
- [Support](https://support.crea.ca/)
- [Changelog](https://ddfapi-docs.realtor.ca/releasenotes)
- [Blog](https://www.crea.ca/news/)
- [Security](https://www.crea.ca/.well-known/security.txt)
- [Terms of service](https://www.crea.ca/ddf/member-policy-and-rules/)
- [Privacy policy](https://www.crea.ca/privacy/)

## API Evangelist rating

**Kin Score 38.8** — thin · rubric 0.24.0 · scored 2026-10-17

| Facet | Score |
|---|---|
| Discoverability | 71.4 |
| Contract quality | 42.0 |
| Contract governance | 4.5 |
| Operational transparency | 26.3 |
| Developer ergonomics | 49.4 |
| Access clarity | 21.1 |

**Agent readiness 34.7** — agent-ready

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | derived (partial credit) |
| Reversibility Documented | — |
| MCP Server | — |
| Auth Clarity | served (partial credit) |
| Idempotency | — |
| Error Semantics | verified |
| OpenAPI Examples | partial |
| Rate Limit Signal | — |
| Event Surface Described | — |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | served (partial credit) |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |

**Regulatory alignment 40.2** — Horizontal (data, software, accessibility, platform)

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/realtor-ca/](https://apis.io/providers/realtor-ca/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for REALTOR.ca: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/realtor-ca/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
