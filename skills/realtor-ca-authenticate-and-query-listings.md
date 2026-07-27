---
name: Authenticate and query REALTOR.ca DDF listings
description: >-
  Mint a DDF Web API access token from CREA's IdentityServer and run correct, paged OData queries
  against the Property, OpenHouse, Member and Office resources without tripping the 401, the
  repeated-record trap, or the 10,000-record ceiling.
api: openapi/realtor-ca-ddf-web-api-docs-openapi.json
generated: '2026-07-26'
method: generated
source: https://ddfapi-docs.realtor.ca/
operations:
  - POST https://identity.crea.ca/connect/token
  - GET /odata/v1/Property
  - GET /odata/v1/Property/{PropertyKey}
  - GET /odata/v1/OpenHouse
  - GET /odata/v1/Member
  - GET /odata/v1/Member/{MemberKey}
  - GET /odata/v1/Office
  - GET /odata/v1/Destination
---

# Authenticate and query REALTOR.ca DDF listings

## Before you start

You cannot self-serve into this API. A licensed REALTOR or broker owner must create a DDF
Destination (data feed) in the member portal and link it to you. Your `client_id` and
`client_secret` are that Destination's username and password. If you do not have them, stop —
there is no sandbox, no trial, and no anonymous read of any endpoint, including `$metadata`.

## 1. Get a token

`POST https://identity.crea.ca/connect/token` with `Content-Type: application/x-www-form-urlencoded`:

```
grant_type=client_credentials
client_id={Destination username}
client_secret={Destination password}
scope=DDFApi_Read
```

The response gives `access_token`, `expires_in` (3600), `token_type` (`Bearer`) and `scope`
(`DDFApi_Read`). The token is **not sliding** — schedule a fresh mint every hour rather than
re-minting on 401. Keep the exchange server-to-server; CREA warns that client-to-server token
requests risk exposing the feed credentials.

Send `Authorization: Bearer {access_token}` on every subsequent call.

## 2. Learn what you are entitled to

`GET /odata/v1/Destination` returns the Destinations linked to your credential. Everything else you
read is scoped to those — the token carries a `destinationid` claim, and asking for a
`DestinationId` you are not linked to returns 401, not 403.

## 3. Query a resource

`GET /odata/v1/Property?$top=100&$orderby=ListingKey&$select=ListingKey,ListPrice,City,ModificationTimestamp`

Rules that matter:

- **Always `$orderby`.** Results are unordered by default and CREA warns that paging without a sort
  can return the same record on more than one page.
- `$top` defaults to 20 and is capped at 100.
- Follow `@odata.nextLink` from the response body rather than incrementing `$skip` yourself.
- Request `$count=true` **once**, not per page — CREA flags it as a performance cost.
- `$filter` supports `eq ne gt lt ge le and or not in has`, the `any` lambda
  (`$filter=Heating/any(a: a eq 'Electric')`) and `contains` on string fields. Lookup fields must be
  filtered with the enumerated values from `$metadata`, not free text.
- There is no `$expand`. Media, Rooms and SocialMedia arrive inline on the parent record.
- All timestamps are UTC, and UTC is expected when you filter on them.

Single records: `GET /odata/v1/Property/{PropertyKey}`, `/odata/v1/Member/{MemberKey}`,
`/odata/v1/Office/{OfficeKey}`, `/odata/v1/OpenHouse/{OpenHouseKey}`.

## 4. Stop before 10,000

If your result set exceeds 10,000 records, do not keep paging — switch to the Replication endpoints
(see the "Replicate DDF listing data" skill). This is a hard instruction in CREA's documentation.

## 5. Handle errors

| Status | What it actually means | What to do |
|---|---|---|
| 400 | Invalid primary key, or an empty/null value in `$select` | Fix the key format or the field list |
| 401 | Bad/expired token **or** a DestinationId you are not entitled to | Re-mint the token; verify the Destination |
| 404 | Invalid URL, or the record does not exist in your feed | Check path and key |
| 408 / 503 | Timeout / unavailable | Back off and retry; narrow the `$filter`, drop `$count` |
| 500 | Server error | Retry with backoff, then support@realtor.ca |

Errors come back as `{"error":{"message":..,"code":..,"details":..}}` — vendor JSON, not RFC 9457,
and `code` only mirrors the HTTP status. Branch on the status line, not on prose.

## Constraints to design around

- No rate limits are published — build your own conservative pacing and honour backoff on 408/503.
- No status page exists, so a 503 has nothing to correlate against.
- Every listing you display must carry the clickable "Powered by REALTOR.ca" badge linking to the
  original listing on REALTOR.ca.
