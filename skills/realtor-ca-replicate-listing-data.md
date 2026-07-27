---
name: Replicate REALTOR.ca DDF listing data into your own database
description: >-
  Run CREA's documented four-step replication pattern — initial load, incremental pull by
  ModificationTimestamp, master identifier list to detect removals, then per-listing detail — using
  the Property/Member/Office Replication endpoints instead of paging the resource collections.
api: openapi/realtor-ca-ddf-web-api-docs-openapi.json
generated: '2026-07-26'
method: generated
source: https://ddfapi-docs.realtor.ca/#section/DDF(r)-Web-API/DDF(r)-clients-guide
operations:
  - POST https://identity.crea.ca/connect/token
  - GET /odata/v1/Destination
  - GET /odata/v1/Property/PropertyReplication()
  - GET /odata/v1/Property/PropertyReplication(DestinationId={DestinationId})
  - GET /odata/v1/Member/MemberReplication()
  - GET /odata/v1/Member/MemberReplication(DestinationId={DestinationId})
  - GET /odata/v1/Office/OfficeReplication()
  - GET /odata/v1/Office/OfficeReplication(DestinationId={DestinationId})
  - GET /odata/v1/Property/{PropertyKey}
---

# Replicate REALTOR.ca DDF listing data into your own database

There is no push. CREA publishes no webhooks, no event stream and no AsyncAPI — the only way to
stay current is to poll the Replication endpoints. This is the pattern CREA documents.

## 0. Token and entitlement

Mint a token (`POST https://identity.crea.ca/connect/token`, `client_credentials`,
`scope=DDFApi_Read`) and send it as `Authorization: Bearer {access_token}` on every call. Tokens
last 3,600 seconds and do not slide.

`GET /odata/v1/Destination` lists the feeds linked to your credential. If you are a Technology
Provider serving several members, you will run each step per `DestinationId`.

## 1. Initial load

Pull every active record for each resource:

- `GET /odata/v1/Property/PropertyReplication()` — or
  `GET /odata/v1/Property/PropertyReplication(DestinationId={DestinationId})` for one feed
- `GET /odata/v1/Member/MemberReplication()`
- `GET /odata/v1/Office/OfficeReplication()`

Page with `$top` (max 100) and follow `@odata.nextLink`. Use the Replication endpoints, not the
plain collections, for anything over 10,000 records — that is a documented ceiling on the
collections.

## 2. Incremental pull

On every subsequent run, filter by the modification cursor and store the high-water mark:

```
GET /odata/v1/Property/PropertyReplication()?$filter=ModificationTimestamp gt 2026-07-01T00:00:00Z&$orderby=ModificationTimestamp
```

- All timestamps are UTC and UTC is expected in the filter.
- `ModificationTimestamp` has two-decimal precision.
- For photo changes specifically, watch `PhotoChangeTimestamp` on Property, and — since the
  2026-05-07 release — the Media object's own `ModificationTimestamp`.

## 3. Detect removals

The Replication endpoints also serve as the master identifier list: pull the full set of
`ListingKey` / `MemberKey` / `OfficeKey` identifiers and reconcile against your table. Anything in
your database that is absent from the master list has gone inactive — CREA removes inactive listings
from the feed rather than flagging them.

## 4. Fetch detail

For each changed key, `GET /odata/v1/Property/{PropertyKey}` (and the Member/Office equivalents) to
pull the full 144-field payload, including the inline `Media` and `Rooms` collections.

## Model notes that bite

- **MediaKey changed type.** As of 2026-05-07 it is a string, `<ObjectID>_<MediaCategoryId>_<MediaPosition>`,
  not a bigint, and a one-time reseed reissued it for every Member object. If your schema types it
  numeric, fix that before your next sync.
- Deleting and re-uploading a photo shifts `MediaPosition` for the others — refresh the whole Media
  collection for a record, not just the changed row.
- `MapCoordinateVerifiedYN` is deprecated. Use `GeoCodeManualYN`.
- Lookup enumerations (MediaCategoryId, AreaUnits, LotSizeUnits, LinearUnits, StructureType,
  SocialMediaType) live only in `$metadata` — `GET https://ddfapi.realtor.ca/odata/v1/$metadata`,
  which requires the same Bearer token. Re-pull it after any release note that mentions lookups.
- Foreign keys to resolve locally: `Property.ListAgentKey`/`CoListAgentKey[2,3]` → `Member.MemberKey`;
  `Property.ListOfficeKey`/`CoListOfficeKey[2,3]` → `Office.OfficeKey`; `OpenHouse.ListingKey` →
  `Property.ListingKey`; `Member.OfficeKey` → `Office.OfficeKey`; `Destination.MemberKey` →
  `Member.MemberKey`.

## Operational reality

- No published rate limits — pace yourself and back off on 408/503.
- No status page. No SLA.
- Change notification is a Redoc release-notes page with no feed:
  https://ddfapi-docs.realtor.ca/releasenotes. Check it before each release cycle; breaking changes
  (like the MediaKey retype) have shipped there without a version bump.
