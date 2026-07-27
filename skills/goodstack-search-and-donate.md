---
name: Search a nonprofit and create a donation
description: Find a verified nonprofit in the Goodstack registry and make a monetary donation to it.
api: openapi/goodstack-openapi-original.json
operations:
  - GET /v1/organisations
  - POST /v1/donations
  - GET /v1/donations/{id}
---

# Search a nonprofit and create a donation

Use the Goodstack Services API (base `https://api.goodstack.io/v1`, sandbox `https://sandbox-api.goodstack.io`).

## Auth
Send your secret key in the `Authorization` header (`sk_...`). All requests use `Content-Type: application/json` over HTTPS.

## Steps
1. **Find the organisation** — `GET /v1/organisations` with a `query` (and optionally `countryCodes`) to search the registry of verified nonprofits. Page with `pageSize` (default 25, max 100). Keep the target organisation's `id`.
2. **Create the donation** — `POST /v1/donations` with the amount, currency, and the organisation reference. Set an `Idempotency-Key` header so a retry never double-donates (keys are honoured for 21 days).
3. **Confirm** — `GET /v1/donations/{id}` to read the donation status. Goodstack requests payment from you and disburses to the nonprofit; watch the lifecycle via webhooks (`donation.created`, `donation.settled`, `donation.disbursed`).

## Rules
- Never send `sk_` keys from client-side code; use `pk_` (publishable) keys only for account identification in front-end/SDK contexts.
- Errors return `{ error: { code, title, message, reasons } }` — read `error.code` to branch.
- To reverse a not-yet-disbursed donation, `POST /v1/donations/{id}/cancel`.
