# Changelog

All notable changes to this project are documented here.

## Unreleased

## 0.3.0 — 2026-09-19

- Added timeout-race and reply-ownership planning guidance: a named final-response owner, explicit CAS `true`/`false`/error handling, fail-closed queued promises, and atomic worker result registration.
- Required plans to preserve the deterministic barriers and original interleavings of known race reproductions when turning XFAIL tests green; a store-only CAS test is no longer evidence for an API-to-worker delivery path.
- Added reusable task-card scenarios for outer timeouts, late worker completion, ownership conflicts, bounded retry, dead-letter handling, and controlled-sender verification.

## 0.2.0 — 2026-09-19

- Added acceptance guidance and reusable task scenarios for host-mediated free-text menu input, including the distinction between `text_selection` and a chosen “other” option.
- Added configuration-lifecycle checks for disabled, re-enabled, and deleted notification targets across both enqueue and delivery paths.

- Added an exact-execution handoff mode with complete file contents, unique search-and-replace anchors, sequential rehearsal, environment contracts, and explicit deviation boundaries.
- Distinguished expected results from observed evidence and assigned ownership for excluded or unavailable verification.

- Added a production-wiring ledger that requires plans to identify the external trigger, registration point, producer, durable record, consumer, observable result, and evidence status for each material rule.
- Extended task templates with concrete registration, trusted-identity, zero-side-effect, and entry-level integration-test requirements.
- Added retrospective guidance for unreachable helpers, unconsumed outboxes, missing callback consumers, and component tests that do not prove a production path.

## 0.1.0 — 2026-09-18

- First public release of the `opencode-handoff-plans` Codex skill.
- Added reusable task and verification template.
- Added retrospective on requirements traceability, integration evidence, state transitions, persistence, and external capability claims.
