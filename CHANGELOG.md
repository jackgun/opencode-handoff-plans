# Changelog

All notable changes to this project are documented here.

## Unreleased

- Added a self-check that every clause of a final-wave acceptance card becomes its own assertion: compound cells split into one line per clause, counts asserted per key, and states asserted as absolute values rather than "one more than before".
- Added an acceptor check and a wave-checklist item: cleanup of build directories outside a read-only repository is proven by the directory not existing, not by an empty `git status`; the handoff gives the Windows long-path delete command.

- Added rows to `references/plan-self-check.md` for user-facing text the plan fixes (literal-value tests; changes reported as deviations), reference data with effective dates (checked per applicable year), side effects after the main transaction commits (their own failure exit for every preceding step), failure-recording rows that must satisfy table constraints, and call sites in another lane's file. Added a self-check that global constraints and card examples agree.
- Added acceptor checks: run the host's real build after resolving merge conflicts, and spell out the temporary-file procedure for read-only repositories in the handoff. Added two items to the wave checklist.

- Added rows to `references/plan-self-check.md` for fields written by existing code (name the production writer and confirm it is not a placeholder), new data shapes from one lane that pass through another lane's transforms (a cross table against every tier, run after each merge), and position-based extraction (an anchor unique to the located object, tested on the same-page layout). Added self-checks that release-blocking assertions in the design are copied into cards one by one, and that tests asserting partial results first assert the overall run status.
- Added acceptor checks: confirm the worktree is clean and HEAD matches the reported commit before accepting, confirm each new check can actually reject an input (not satisfied by fixed template text or overwritten by later code), and run the real sample through every tier on each merge result. Added two items to the wave checklist.

- Added rows to `references/plan-self-check.md` for records grouped by an external identity (reject a missing identity with zero side effects), host naming rules checked only at runtime (contract constants and a guard test over every declared identifier), and follow-on phases that extend lists locked by earlier equality tests (relaxed in the contract wave, restored by a named final-wave task). Added self-checks that follow-on plans scan existing tests and that parallel lanes have isolated test resources.
- Added a test-isolation section to `references/parallel-waves.md`: one test database (or schema or directory) per lane plus one for the acceptor, created in the contract wave. Added a red line against restarting or stopping shared services and ending processes the lane did not start, an acceptor check to inspect containers and processes when acceptance runs are interrupted, and two checklist items.

## 0.8.0 — 2026-10-02

- Extended `references/plan-self-check.md` with rows for host test doubles that reproduce the real signature and failure mode (auth failures never fall back to a default identity), transformation tests whose inputs contain something to transform, cross-stage persisted fields read back with non-empty values, one event per idempotency marker with a fail → resume → succeed test, rollback before failure writes with database-level error injection, reads by frozen version id, a test per signature parameter, deletion of shared content-addressed storage objects, display labels for internal values, per-entry source verification for reference data, field-level response tables between API and UI lanes, and the host's real build or type check for built artifacts.
- Added self-checks that every placeholder or function a content card uses exists in the engine, that rules cited only by design section number carry their formula, and that golden values agree with the card's thresholds.
- Added acceptor checks: compare named tests with actual test functions, search for assertions inside conditionals, swap host doubles for real-signature versions, and run the strictest host build. Added red lines for auth fallbacks, conditional assertions, and reference data filled from memory.
- Added rows for registries filled by import side effects (an owner for loading, a fresh-process test, and no explicit loader calls in tests), engines that must not return an empty success on a missing scope input, durable writes named in every rollback requirement, a shared connection fixture that closes connections, and an owning task for every asset the design locks and ships with code.
- Added self-checks that design and plan examples run through the engine and produce output, that tests depending on a later lane's artifact have a scheduled post-merge rerun, and that rework orders get the same self-check as plans. Added acceptor practices: rerun downstream lanes with the real upstream artifact after merge, inspect database activity when tests hang, and accept only verbatim quotes from fetch or summarization tools. Added a red line against excluding failing or hanging tests from reported runs.
- Added rows for field-level edit endpoints (per-type allowed fields with explicit rejection, and edits that must reach the final output), the main entry's enqueue-and-rollback rule applied to every entry that creates a record and enqueues, deduplication treated as a race with a concurrent test, and storage bucket and key formats for data handed between lanes.
- Added self-checks that every expectation added during a plan sync is checked against a real run or the host source, that each requirement names a file inside the lane's scope, and that a rule written for one entry is written for every entry of the same shape. The acceptor now opens each named test and checks its assertions against the card.

## 0.7.0 — 2026-10-01

- Required completeness assertions (row counts or key sets) alongside sampled golden values for extraction tasks, and whole-table assertions from reconciliations in the source document (totals, identities, cross-references), with totals read from the source and mirrored as runtime data-quality warnings.
- Added rows for structural parsing rules that apply to every table and every occurrence in the source, and for a named invalid-input test per declared error type. Added a fixture-versus-real-output comparison for lanes that developed against hand-written fixtures, and an upstream-real-output probe.
- Required reports that attribute failures to the baseline to include the same failure on the baseline commit with the same command and environment; the acceptor reproduces it first.

- Extended `references/plan-self-check.md` with rows for source-set coverage in grounding validators, every exit of isolation and filtering rules with a control group, resume and retry semantics per intermediate state, round-trip fixtures that cover every contract value type, and model message sequences and input encoding. Added grounding variants and counterexamples for exemption rules.
- Extended the cross-lane contract-wave rule to shared decision logic (validators, normalizers, formatters), and required traceability tests to assert at the exit where a rule takes effect.

- Extended `references/plan-self-check.md` for computing components: a per-task null-rule table, "missing is not zero" review items assigned to every consumer of the data, behavior for keys outside closed lookup tables, the meaning of "not in the list" for truncated inputs, and host validate-and-generate runs in every task that produces host-consumed configuration.
- Required golden values to be recomputed by script from source data with their formula and inputs, a named test for every calculation rule, and literal right-hand sides in golden assertions; replacing a literal with a recomputation by the same formula is now a red line.
- Added a self-check that every produced data item has a consumer, examples for signature inputs and design narrowing, and a contract round-trip probe for computing components.
- Added plan requirements for filter and validation rules (a named adversarial sample set covering sibling rules), fail-closed handling of required compliance content, a field-destination table for serialization and rendering, and a resource-access boundary when servers parse or render untrusted content. Extended the totals self-check to require named test expectations that agree with the card's rules and defaults.

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
