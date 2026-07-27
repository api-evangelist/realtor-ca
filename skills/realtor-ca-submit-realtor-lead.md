---
name: Submit an Email REALTOR lead through the DDF Lead API
description: >-
  Route a consumer "Email REALTOR" enquiry to the right member through POST /v1/Lead/CreateLead —
  the only write operation in the DDF surface, and a mandatory one for real estate advertising
  websites, because member email addresses are withheld from the feed payloads.
api: openapi/realtor-ca-ddf-web-api-docs-openapi.json
generated: '2026-07-26'
method: generated
source: https://ddfapi-docs.realtor.ca/#section/Lead-API-Endpoint
operations:
  - POST https://identity.crea.ca/connect/token
  - POST /v1/Lead/CreateLead
  - GET /odata/v1/Member/{MemberKey}
---

# Submit an Email REALTOR lead through the DDF Lead API

## Why this endpoint is not optional

DDF payloads deliberately exclude member email addresses. If your site displays DDF listings and
offers an "Email REALTOR" form, CREA requires you to deliver that message through this endpoint —
there is no address for you to mail directly.

## 1. Check the member will accept email

Read `MemberEmailYN` on the member record (`GET /odata/v1/Member/{MemberKey}`). When it is `false`,
that REALTOR has no email address on file: **hide the Email REALTOR control for them entirely**
rather than submitting a lead that cannot be delivered.

## 2. Submit

`POST https://ddfapi.realtor.ca/v1/Lead/CreateLead` with `Authorization: Bearer {access_token}`
(same `client_credentials` / `DDFApi_Read` token as the read endpoints) and a JSON body:

Required fields:

| Field | Notes |
|---|---|
| `Culture` | `en-CA` or `fr-CA` (default `en-CA`) |
| `MemberKey` | The REALTOR the enquiry is for |
| `ListingKey` | The listing it is about |
| `SenderName` | |
| `SenderEmailAddress` | |
| `PreferredMethodContact` | `email`, `phone` or `text` |
| `Message` | |

Optional: `SenderPhoneNumber` (int64 — **required** when `PreferredMethodContact` is `phone` or
`text`, or when an extension is supplied) and `SenderPhoneExtension` (int32).

## 3. Read the response properly

The Lead API does **not** use the OData error envelope. It returns a flat body on 200, 400, 404 and
500 alike:

```json
{ "success": true, "message": "...", "code": "...", "details": "..." }
```

Check `success`, not only the HTTP status. A 200 with `success: false` is a rejected lead.

## 4. There is no idempotency

`POST /v1/Lead/CreateLead` accepts no idempotency key, no request id and no dedup token. A retry
sends a second email to a real REALTOR. Therefore:

- Never retry blindly on a timeout or a 5xx — mark the submission uncertain and surface it for
  human review, or de-duplicate on your own side before resubmitting.
- Debounce the form client-side and server-side.
- Treat this operation as human-initiated. An agent should not generate leads autonomously; it is a
  write with a real-world consequence at the other end.

## 5. Testing

There is no sandbox. The one published test affordance is a query parameter that runs the real
submission path without emailing anyone:

```
POST https://ddfapi.realtor.ca/v1/Lead/CreateLead?SuppressEmail=true
```

Use it for every integration test. It defaults to `false`, so an omitted parameter emails a live
REALTOR.

## 6. Log the event

Where you also implement CREA's analytics contract, record the interaction as an `email_realtor`
event against the ListingID and DestinationID at `https://analytics.crea.ca/LogEvents.svc` — it is
fire-and-forget, no response handling required, and duplicate events from the same UUID inside a
five-minute window are ignored.
