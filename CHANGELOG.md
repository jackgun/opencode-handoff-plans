# Changelog

All notable changes to this project are documented here.

## Unreleased

- Extended `references/plan-self-check.md` for computing components: a per-task null-rule table, "missing is not zero" review items assigned to every consumer of the data, behavior for keys outside closed lookup tables, the meaning of "not in the list" for truncated inputs, and host validate-and-generate runs in every task that produces host-consumed configuration.
- Required golden values to be recomputed by script from source data with their formula and inputs, a named test for every calculation rule, and literal right-hand sides in golden assertions; replacing a literal with a recomputation by the same formula is now a red line.
- Added a self-check that every produced data item has a consumer, examples for signature inputs and design narrowing, and a contract round-trip probe for computing components.

## 0.6.0 — 2026-09-30

- Added `references/plan-self-check.md`. For each rule that writes data or changes state, plans now derive and specify tests for: preconditions built through the production path, dedup keys for repeatedly triggered entries, check-then-write rules treated as races (lock or CAS, rowcount, two connections with a barrier), the valid range of id references, cache and completion markers written only after commit, non-trivial fixtures when data comes from a later lane, and error details that the downstream consumer needs.
- Added plan self-checks: every rule computable from the task's signature, design narrowing stated with a reason, host interface parameters honored, commit responsibility consistent, totals matching itemized lists, and cross-lane types defined in the contract wave.
- Added probe acceptance, classification of defects as plan gaps or execution deviations, and a rework-order format.
- Added a red line against faking test preconditions by direct database writes or field assignment, and matching precondition and race slots in the task template.

## 0.5.1 — 2026-09-30

- Scoped `references/parallel-waves.md` to plan and handoff content: removed the acceptor's wave report, the self-review labeling rule, and the unverified per-lane merge flow; these remain in the retrospective as acceptor-side practices and watch items.
- Named the superpowers task-brief and ledger scripts explicitly in the task-heading guidance (`### Task N:`).

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
