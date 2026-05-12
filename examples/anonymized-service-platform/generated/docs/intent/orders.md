# Orders

## Purpose

Accept inbound order requests in the API service, validate them against the shared contract, persist the canonical order record, and emit the async handoff that downstream processing depends on.

This area is not responsible for notification delivery guarantees or worker-side retry policy after the handoff completes.

## Entry Points

- HTTP route registration in `services/api/src/router.ts`.
- Order handlers under `services/api/src/orders/`.
- Shared request and event contracts in `packages/shared/orders/` and `packages/shared/events/`.

## Key Rules

- The API should only acknowledge a successful order after validation and canonical persistence succeed.
- Notification enqueue is a downstream side effect, not part of the order acceptance contract.
- Shared contract changes must be verified against both the API handler path and the worker consumers before this doc is updated.

## Dependencies

- Upstream inputs: authenticated client requests, request-shaping middleware, and order-processing config.
- Downstream effects: order events consumed by the worker and any audit or analytics sinks that subscribe to the canonical event.
- Shared dependencies: `packages/shared/` contract modules and any config used to enable or disable partner-specific order behavior.

## Verification Notes

- Reviewed code paths: `services/api/src/orders/createOrderHandler.ts`, `services/api/src/orders/orderService.ts`, and `packages/shared/events/orderEvents.ts`.
- Known boundary: notification retry policy is intentionally documented in `notifications.md`, not here.
- Review date: 2026-05-06.
