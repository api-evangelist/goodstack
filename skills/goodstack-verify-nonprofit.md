---
name: Verify a nonprofit organisation
description: Submit a validation request to confirm an organisation is an eligible nonprofit, then read the result.
api: openapi/goodstack-openapi-original.json
operations:
  - POST /v1/validation-requests
  - GET /v1/validation-requests/{id}
  - GET /v1/organisations/{id}
---

# Verify a nonprofit organisation

Use the Goodstack Services API (base `https://api.goodstack.io/v1`). Authenticate with your secret key in the `Authorization` header.

## Steps
1. **Create a validation request** — `POST /v1/validation-requests` for the organisation you want vetted. Add an `Idempotency-Key` header on the POST.
2. **Poll or subscribe** — `GET /v1/validation-requests/{id}` to read status, or subscribe to the `validation_request.approved` / `validation_request.rejected` webhooks instead of polling.
3. **Read the organisation** — once approved, `GET /v1/organisations/{id}` to pull the verified organisation profile (registry, country, categories).

## Rules
- A rejected request carries a `rejectionReason` (e.g. "Organisation is not a nonprofit", "didn't provide sufficient proof of nonprofit status").
- For agent-led verification flows, use `POST /v1/agent-verifications` instead, and attach documents with `POST /v1/validation-requests/{validationRequestId}/document`.
- Errors use the `{ error: { code, title, message, reasons } }` envelope.
