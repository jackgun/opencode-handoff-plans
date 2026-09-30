# Changelog

All notable changes to this project are documented here.

## Unreleased

## 0.5.0 — 2026-09-30

- Added `references/parallel-waves.md` for multi-lane, multi-wave handoffs: an acceptor-owned contract wave that is tagged before lanes start, a separate plan-sync commit for contract-wave rulings and known consumer constraints before dispatch, fixed serialization semantics, the host's real validator run during the contract wave, a single opening-prompt template with per-lane task and design-section tables, and an acceptor checklist.
- Required guard tests to be shown red with an injected violation, added resource exhaustion from allow-listed constructs to evaluator review focus, and required `self-review` to be labeled as weaker than independent review.
- Extended the exact-handoff environment table with pre-handoff measurement and adjacent dangerous targets; added same-line environment variables with a zero-skip gate for real-database tests, Windows line-ending and heredoc guidance, and task-heading compatibility with executor scripts.
- Added reusable red lines (dependencies, containers, read-only repositories, real sends and models, golden values, sample-free model prompts, push and deploy) and report slots for red/green excerpts, checked items, and manual evidence.
- Made the skill description and default prompt independent of the reviewer's identity.
- Recorded the unverified per-lane merge and wave-advance practice as a retrospective watch item rather than a rule.

## 0.4.0 — 2026-09-20

- Added ownership-chain guidance for reservation, invocation binding, enqueueing, result registration, and outbox delivery; an empty or failed binding now requires rollback and a retryable response before any queued promise.
- Distinguished a diagnosable `failed + output + audit` record from recoverable delivery, and required a registered retry, scanner, or human consumer before claiming recovery.
- Extended the timeout-reply task scenarios with binding failure tests that assert zero queued jobs, zero customer promise, and no fallback to an incompatible legacy protocol.

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
