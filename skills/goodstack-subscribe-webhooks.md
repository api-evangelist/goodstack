---
name: Subscribe to donation and verification webhooks
description: Register a webhook endpoint to receive Goodstack donation, validation, and verification events.
api: openapi/goodstack-openapi-original.json
operations:
  - POST /v1/webhook-subscriptions
  - GET /v1/webhook-subscriptions
---

# Subscribe to donation and verification webhooks

Use the Goodstack Services API (base `https://api.goodstack.io/v1`). Authenticate with your secret key in the `Authorization` header.

## Steps
1. **Create a subscription** — `POST /v1/webhook-subscriptions` with your HTTPS endpoint URL and the list of events you want (from the 19 supported types).
2. **List/verify** — `GET /v1/webhook-subscriptions` to confirm the subscription, or `GET /v1/webhook-subscriptions/{id}` for one.
3. **Handle delivery** — return a 2xx quickly. On a non-2xx, Goodstack retries up to 4 times over ~14 hours with increasing delay. Edit with `PATCH` or remove with `DELETE /v1/webhook-subscriptions/{id}`.

## Event families
- `donation.*` — created, payment_successful, settled, disbursed, cancelled, reassigned, hosted.payment_received
- `validation_request.*` — approved, rejected
- `agent_verification.*` — pending_review, pending_user_verification, approved, rejected
- `validation_submission.*` — created, succeeded, failed, updated
- `monitoring_subscription.updated`, `eligibility_subscription.updated`

See `asyncapi/goodstack-webhooks.yml` for the full catalog.
