# Notifications

## Purpose

Consume canonical order-related events in the worker service and deliver external notifications without changing the source-of-truth order record owned by the API workflow.

This area is not responsible for deciding whether an order is accepted; it only owns downstream delivery attempts and provider-facing error handling.

## Entry Points

- Worker registration and consumer wiring in `services/worker/src/main.ts`.
- Notification processing code under `services/worker/src/notifications/`.
- Shared event contracts and provider payload helpers in `packages/shared/events/` and `packages/shared/notifications/`.

## Key Rules

- Delivery is best-effort with retry and backoff; a notification failure does not roll back order acceptance.
- Deduplication keys should survive retries so providers do not receive duplicate sends after transient failures.
- Channel-specific payload shaping belongs at the provider boundary, while shared event payloads remain canonical upstream.

## Dependencies

- Upstream inputs: order events emitted by the API service and worker runtime config that enables channels or retry windows.
- Downstream effects: provider adapters, delivery-status metrics, and operational alerts for repeated failures.
- Shared dependencies: `packages/shared/` event schemas, notification helpers, and any shared config that controls retry cadence or disabled channels.

## Verification Notes

- Reviewed code paths: `services/worker/src/notifications/notificationConsumer.ts`, `services/worker/src/notifications/providerRouter.ts`, and `packages/shared/notifications/dedupeKey.ts`.
- Known omission: provider-specific quota tuning stays in operator runbooks and is intentionally not duplicated in intent docs.
- Review date: 2026-05-06.
