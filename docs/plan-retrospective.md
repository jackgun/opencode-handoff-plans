# Plan Retrospective: From Requirements to Acceptance Evidence

This retrospective comes from a multi-agent feature delivery involving public data projection, a conversational state machine, persistence, host continuations, and an external messaging capability. It removes project names, customer information, repository paths, and implementation-specific details. Its purpose is to preserve the planning lessons.

## What worked

The plan had a shared interface document, independent ownership for parallel work, explicit data-minimization rules, and a clear distinction between verified external capabilities and safe degradation. Those choices prevented larger integration and privacy mistakes.

The most useful planning elements were:

- A single source for shared data types, state fields, and event payloads.
- Explicit file ownership and an assigned integration owner.
- Examples for calendar calculations and a rule that missing evidence must not become a customer-facing promise.
- Separate conclusions for repository tests, integration behavior, and external end-to-end delivery.

## Where the plan did not provide enough evidence

Several requirements were written as correct intentions but did not define an executable proof. That allowed plausible implementations to pass local tests while failing the intended behavior.

| Requirement | Escaped defect | Better acceptance evidence |
| --- | --- | --- |
| Unknown inventory data stays unknown | A normalizer default turned an absent status into an available status | Feed an absent raw status through the real adapter and assert that it cannot become a confirmed public result |
| A room belongs to the selected location | A row from another location could reach a detail request | Supply mismatched parent and child identifiers; assert no detail call and no result |
| Invalid money is safe | A value was downgraded but the invalid amount remained visible | Test input criteria and upstream data separately; assert both the value policy and the confidence policy |
| Calendar rule controls availability | The date function passed while no production entry point used it | Test the formula, the adapter decision at both boundaries, and the evidence-missing fallback as separate cases |
| State is persisted safely | Database and serialized state versions diverged | Run entry point → real store → serialization → reload → next event; inspect both database and serialized versions |
| Scope prevents cross-user changes | An update checked only part of the scope | Change each scope dimension in turn and call the actual update path, not only a mocked SQL assertion |
| Handoff is honest | A state flag and success-sounding reply had no host action | Test producer output, host consumption, and the actual user-visible confirmation separately |
| Cancellation has no inventory dependency | The code allowed cancellation after a failed fetch but still called the fetch | Make the fetch fail if called and assert that cancellation still succeeds |
| Generated menus mean what they display | Adjacent price bands overlapped or could not be consumed | Send every generated option through the real parser and filter; test each boundary and minimum currency unit |
| Customers can choose or type another value | The host injected a free-text selection with an empty value; the state machine advanced and persisted an empty field | Start from the real continuation preparation path, submit free text without selecting “other”, and assert the persisted field contains that text |
| Only enabled notification targets receive events | The page hid disabled targets, but old routing rules still produced and delivered notifications | Disable a target after it has a rule; assert both the producer and sender suppress it, then define and test re-enable and delete behavior |

## Better task cards

Each important task should identify the rule it implements, its production caller and consumer, and a scenario that would fail if the target mistake returned. A task is complete only when its promised evidence is available.

```text
Rule: terminal state must not reopen on an ordinary message.
Production path: event handler → state loader → state transition → store.

Scenarios:
  active + valid answer        → one defined transition and one version change
  active + repeated message    → original receipt, no second transition
  active + cancellation        → terminal state, no inventory call
  terminal + ordinary question → terminal state remains, no new record
  terminal + explicit restart  → new identifier, history retained
  any state + stale selection  → no write and no external side effect

Evidence: handler/store integration test and, when the claim depends on it,
an isolated database concurrency test.
```

## Rules for future plans

1. Trace every material rule through specification, owner, production entry point, acceptance scenario, test layer, evidence, and status.
2. Split tasks by independently provable behavior. Do not combine state transitions, persistence, authorization, and registration in a single “implement and test” task.
3. Make units, precision, null behavior, timezone, and interval inclusivity part of the interface contract.
4. Keep mocks at system boundaries. At least one integration test must retain the components whose interaction is being claimed.
5. Treat simulation, a real database, an external sandbox, and a real external endpoint as distinct evidence levels.
6. State exactly what must not happen in cancellation, authorization, duplicate, and stale-event paths, then assert the call is absent.
7. Use safe degradation as a deliverable only for the degradation behavior. It does not complete the unavailable upstream business rule.
8. Review fixed defects by semantic category, not only by the literal failing input. For example, test range, direction, precision, ambiguity, and question forms together when changing a money parser.
9. For stateful conversations, test the protocol translation as well as the state machine. A hand-built selection can match the domain function while differing from the selection and text that the host actually injects.
10. Treat configuration as a lifecycle. A UI filter proves only the current screen; rules and queued work created before disablement must be tested at the producer and consumer boundaries.

## Measuring improvement

For comparable deliveries, record the first acceptance result, number of repair rounds, defects caused by missing plan detail, defects hidden by mocks, and unverified dependencies reported honestly. Test-count growth alone is not a useful quality metric.

## Production wiring is a separate acceptance obligation

An implementation can contain correct domain helpers, tables, routes, and unit tests while still being unreachable to its intended user. This happens when a plan treats components as the delivery unit instead of treating the complete production path as the delivery unit.

Before implementation, trace each material rule through this ledger:

```text
external trigger → registered entry → domain service → transaction record/outbox
→ registered consumer → observable result → entry-level test → external evidence status
```

The concrete registration point matters. A skill needs a discoverable manifest and handler; an HTTP action needs a mounted router and trusted principal; an asynchronous operation needs both a producer and a registered consumer. A helper that is only called from tests, an outbox without a drain, or a callback format without a consumer is not an integrated feature.

The same rule applies to security and operational behavior. An administrator principal does not establish an ordinary employee identity; a user ID supplied by a request does not establish authorization. A retained-media API does not establish media persistence until the real intake calls it and writes the retained object reference. A deadline scanner does not establish SLA behavior until the production assignment path writes the deadline and current processing round.

For every path that writes data, changes state, or sends a message, acceptance should include one successful entry-level scenario and one failed, stale, duplicate, or unauthorized scenario that proves the relevant write, send, or retention call did not happen. Use an isolated real database for races and constraints. A controlled sender can replace network I/O in local tests, but actual platform delivery and callbacks remain a separately reported external evidence layer.

## Timeout races need an ownership proof, not only a CAS proof

A later delivery retrofit introduced a durable ownership row, compare-and-set transitions, an outbox, and PostgreSQL tests that showed two connections could not both win the same transition. The implementation still failed review because its proof stopped below the customer-facing boundary.

The original failures had deterministic interleavings: an outer request timed out before an inner handler finished, and a worker finished immediately after the handler's final poll. During implementation, those tests were changed into assertions that one transition ended in a deferred state. They no longer retained the barriers, the worker, or the sender path, so they could not detect an inner handler that claimed synchronous ownership too early or an outer timeout that returned a queued promise after a failed handoff.

For a timeout or exactly-once reply rule, identify one owner of the final customer response and place its synchronous claim at the actual delivery boundary. A failed CAS is an ownership result, not diagnostic noise: the code must either follow the already durable owner's registered delivery path or return a retryable failure. The same is true for an exception while reserving or transferring ownership. Returning “queued” is valid only after the durable record and its consumer are known to exist.

The acceptance suite should preserve every known failure choreography and test it through the real entry, completion worker, outbox, and controlled sender. A separate two-connection database test remains necessary for lock semantics, but it does not prove that the API and worker consume the result correctly. Finally, a retryable delivery state needs a bounded retry policy, a dead terminal state, and an explicit reconciliation owner; otherwise it is an unbounded loop rather than a complete delivery protocol.

## A binding is part of the delivery protocol

The ownership record still needs to be associated with the invocation that will produce the answer. Treating that binding as optional created a third escape path: reservation succeeded, the event and job were created, binding returned no record or raised an error, and the worker later had no ownership row from which to create an outbox. Marking the worker execution failed and retaining its output made the incident visible, but it did not give the promised reply a consumer.

Plans must therefore model reservation, binding, enqueueing, result registration, and delivery as one chain. Before an asynchronous job is accepted, binding must succeed; an empty result and an exception both terminate the request through the defined retryable failure path and roll back records in the same transaction where possible. The corresponding entry-level tests force each failure, then assert no job was queued, no queued promise was returned, and no legacy marker was used for a protocol whose worker does not consume it.

An audit record, even one that preserves the completed output, is not a recovery mechanism. It becomes a valid degraded outcome only when a specific registered scanner, retry queue, or human operational workflow consumes it and that consumer has its own evidence. Acceptance reports should describe it as detected loss until then.

## Parallel waves: a frozen contract is only useful if the plan says what was frozen

A later delivery split the plan into waves. The acceptor completed a serial contract wave (skeleton, shared types, schema migration, expression evaluator), tagged it, and created one worktree and branch per parallel lane from that tag. Executors were told not to create worktrees, switch branches, or merge other lanes. The contract wave itself went well; most of the lessons concern what had to happen between finishing it and handing out the next wave.

The following problems occurred during the contract wave and were handled before dispatch:

| Requirement | Problem encountered | Better acceptance evidence |
| --- | --- | --- |
| Parallel lanes start from the current contract | Rulings made while executing the contract wave (a field left for a later task to append, string-only year keys in a mapping, a required per-entry field in the host deployment manifest) lived only in the execution ledger | A separate plan-sync commit that writes each ruling into the plan, the tag moved to it, and every lane worktree fast-forwarded before the opening prompt is sent |
| Encoded values round-trip exactly | Table encoding silently dropped undeclared columns and tuples came back as lists, contradicting the stated round-trip rule; both were recorded only as deferred minors in the ledger | Known deviations written into the plan's contract section as consumer constraints, or fixed before tagging |
| The planned test environment exists | The planned database container had been stopped for weeks, its port was now owned by another local stack, and the credentials no longer matched | Every row of the environment table re-tested before handoff; a dedicated test service on its own port; the adjacent port named as a forbidden target in the red lines |
| The plugin manifest is valid | The host's real validator required a field the plan did not list | The host validator run during the contract wave with a temporary file, which is then removed and the read-only repository's `git status --porcelain` shown empty |
| Source files pass the host's static scan | The scan matched forbidden words literally, including inside comments | A plan note that comments and docstrings in scanned files must also avoid forbidden words |
| Real-database tests ran | Environment variables set in an earlier tool call did not reach the test process, because tool calls do not share shell state; a skipped suite looks green | Variables written on the same command line as the test runner, and zero skips as a delivery gate |
| Byte-level golden files are stable | Line-ending conversion and heredoc quoting on Windows can alter bytes | `core.autocrlf false` (or `.gitattributes`) for such repositories, and multi-line content written with a file tool rather than a heredoc |
| The executor's tooling can find tasks | Custom task headings were not recognized by the task-brief script, so the ledger had to be maintained by hand | Headings in the script's format, or a handoff note that the ledger is manual and what each row contains |
| Guard tests protect the plugin protocol | A guard that never fails proves nothing | Each guard shown red with an injected violation, then green after removal |
| A user-editable expression evaluator is safe | Every AST node was on the allow-list, yet string multiplication chained with `len()` could exhaust memory | Operand-type restrictions, a Review Focus item for resource exhaustion from allowed constructs, and a reproducing test seen red before the fix |
| The wave was reviewed | Without authorization for a second reviewer, only the author reviewed the wave | The ledger records `self-review`, and the report to the user says it is weaker than independent review so the user can decide |

The handoff document that worked had one opening-prompt template with placeholders plus a per-lane task table, a lane-to-design-section map, a read-only list of reference implementations, red lines that named adjacent dangerous targets, and an acceptor checklist that executors could run in advance. It stated at the top that the acceptor would rerun everything rather than rely on the report.

### Acceptor-side practices

These concern how the acceptor runs the waves rather than what the plan tells the executor, so they are kept here instead of in the skill. After a wave, the acceptor reported to the user: every ruling with the cost of getting it wrong, deferred minors and whether each became a consumer constraint, environment changes (services created, started, or stopped; stopped rather than deleted, and only with the user's authorization), and the next wave's worktree, branch, and task table for each lane. When only the author reviewed the wave, the report said so.

### Watch items (not yet verified)

These practices are in the plan but have not yet run through a later wave. Treat them as recommendations until there is evidence:

- The acceptor reruns each lane's tests, walks the checklist, and merges the lane into the main branch.
- Worktrees for the next wave are created only after every lane of the previous wave has merged.
- Whether merge order and acceptor-side conflict resolution keep lanes from receiving an outdated contract.

Record for each wave: whether any lane received an outdated contract, how many merge conflicts touched shared-contract files, and whether the acceptor's rerun found failures that the lane report did not show.

## A green suite is not acceptance: six defects behind 112 passing tests

In the first parallel wave, one lane delivered three tasks. The acceptor reran the full suite: 112 passed, 0 skipped. All changes stayed inside the lane's owned directories, and the deviation notes were honest. The acceptor then wrote probe scripts that drove the real flows against a real database and reproduced six defects. Most root causes were in the plan, not in the executor: the executor implemented what the plan said, cleanly and within bounds. Only two problems were the executor's own: a test faked a precondition with a direct SQL update, and the executor did not notice a vacuous test.

| Requirement | Defect found by probe | Plan gap | Better plan evidence |
| --- | --- | --- | --- |
| An upstream upgrade creates a merge draft for edited items | No draft was created after a real edit and publish; the comparison baseline read a field that the edit path leaves empty | The plan named the test but not how to build its precondition; the test set the "edited" state with a direct update | Precondition built through the named production call chain; acceptance checks tests for state-changing SQL or assignment |
| An upgrade is merged once | Every restart created another merge draft | No dedup key for a repeatedly triggered entry | Dedup key in the plan, and a test that N triggers produce one record |
| A stale approval is rejected | Two concurrent approvals of drafts based on the same version both published, silently overwriting the first | Review Focus listed only the sequential conflict | Check-then-write treated as a race: lock or CAS, rowcount check, and a two-connection barrier test next to the sequential one |
| Only approved versions are used | A pinned version could point to a draft, and a trial override could use another item's version id | No valid range for id references | Status allow-list, same-item and same-tenant checks, and a counterexample test for each |
| First-time sync runs once | The in-process "done" cache was set before the caller's commit; after a rollback the tenant stayed empty until restart | The plan said both "caller commits" and "cache per tenant, run once" | Marker written only after persistence succeeds; a test that a failed commit leaves the marker unset |
| Import is idempotent | The idempotency test compared two imports of an empty catalog that a later lane would fill | No fixture and no non-trivial assertion | A named fixture and an assertion such as "first import count > 0" |
| Validation failures are actionable | Only an error code reached the admin UI | The interface defined only `code: str` | Error details derived from what the downstream consumer must show |
| Every state change is audited | Only approval wrote an audit record, and the record lacked the tenant | The plan narrowed the design without saying so; the host audit call has no tenant parameter | Stated narrowing with a reason; context the host call does not take goes into the detail payload |
| References are validated | The executor could not validate referenced items | The function signature had no parameter to look them up | Every rule computable from the signature's inputs |
| Lanes share one protocol type | One lane defined its own copy of a type from another lane's protocol | The type was left to the other lane | Cross-lane types defined in the contract wave |
| The plan is internally consistent | The plan said "load eight items"; its own list had seven | No self-check of totals | Totals checked against itemized lists before handoff |

Classifying each defect as a plan gap or an execution deviation decided where the fix belonged. Plan gaps went back into the plan and into this skill. Execution deviations went into a rework order (X-R1 … X-Rn, each with the problem, the required change down to the function and SQL condition, and the named test and assertion), which opened with red lines: build preconditions through the real flow, make one commit, and report red/green evidence for each item.

## Computing components and a report lane: what a single golden sample hides

Two more lanes of the same wave failed first acceptance.

The computing lane delivered thirteen pure functions, each with one golden-sample test, and all passed. A contract round-trip probe found the defects within minutes. The probe encodes each output with the shared contract's types, decodes it again as the execution engine does, and then feeds each function None, zero, an out-of-table key, and an empty list.

| Requirement | Defect found by probe | Plan gap | Better plan evidence |
| --- | --- | --- | --- |
| Missing is not zero | A None cell (a dash in the source, meaning unknown) was treated as 0; derived amounts owed, late fees, and penalties were invented from it | The Review Focus item was assigned only to the parser and executor, which produce the data, not to the functions that consume it | A null-rule table on every computing task card, with one test per row |
| Aggregates tolerate missing data | A summing function crashed on None | Same | Same |
| Lookups are closed | A name outside the rate table raised an unhandled `KeyError` | No behavior for out-of-table keys | Stated behavior (skip, flag for review, or error) and an out-of-table test |
| Top-N lists are partial | A category absent from a top-N list was treated as amount 0 | No semantics for "not in the list" | Stated meaning for truncated inputs and a test with an absent item |
| Golden values are correct | The plan's hand-computed golden value was off in the last digit | The golden value was not recomputed from source | Script-recomputed golden values with formula and inputs written next to them |
| Rules are tested | Rate bands, skip conditions, name normalization, and remarks had no tests | The task card listed only golden-sample assertions | A named test for every calculation rule |

The executor reported the wrong golden value as a deviation, which was correct. But the executor also rewrote the assertion to recompute the value with the same formula, which made the test always pass. That was an execution deviation, and it is now a red line: keep the literal assertion and report the disagreement.

The report lane had eleven findings. Eight were plan gaps, and some of these overlapped with execution deviations, so both sides shared the responsibility. Three were pure execution deviations. The plan gaps:

- The plan did not require running the host's tool to validate and generate the deployment composition in that task. The produced compose file used directives and unpinned images that the host forbids, and this surfaced only at acceptance.
- The assembly function had to attribute an unfinished section to the node that produces its data, but its signature had no data-item-to-producer mapping. The implementation guessed and attached another node's failure to the section.
- The design required a generation-time footer, a watermark for the free tier, and branding. The plan's rendering task omitted all three without saying so.
- A model-backed skill ran and produced data that no report section displayed, spending model calls for nothing.
- Filter and validation rules stated only the intent: remove amounts, and require document numbers to exist in the regulation library. The plan listed variants for model-number grounding but not for these sibling rules. Amounts embedded in sentences or written in larger units survived. Document numbers with ASCII brackets, full-width parentheses, or inner spaces bypassed the check, and so did a number with the wrong issuing authority. This gap overlapped with execution deviations in the money pattern and the document-number comparison.
- The plan required a disclaimer in the lower tiers but did not say what happens when its text is empty. The implementation skipped it, so a report could be delivered without the disclaimer.
- The rendering task did not say which fields of each block must reach each output format. Findings lost their evidence numbers, legal basis, years, and exposure, which are the core of the report.
- One task card gave a default renderer URL and also named a test that expects an error when the URL is missing. Both cannot hold.

The plan also did not mention that the HTML renderer fetches external URLs and `file://` resources referenced in its input by default. Because model output reaches that renderer, the plan should have stated the allowed resources and required a test.

The three pure execution deviations were specific to their code and are not turned into general rules. They went into the rework order.

## An engine lane: named tests existed, rules still failed at the exits

The execution-engine lane (planner, graph executor, model nodes, number grounding, node-result persistence) reported 128 passing tests, and every rule named in its report had a matching test. Probes with real data still found these defects:

| Requirement | Defect found by probe | Root cause |
| --- | --- | --- |
| Node results persist | Saving crashed on the first real result because nested values held decimals; the test had stored one scalar decimal | Both: the plan said "encode with the contract" but named a scalar-only test; the executor did not encode through the contract |
| Resume reuses finished work | Re-saving a failed node on resume hit a primary-key conflict | Plan gap: only the happy path of resume was specified, not failed, skipped, or degraded nodes, and not upsert versus insert |
| Node status is recorded per node | On resume, every node's status was recorded under the last node's name | Execution deviation: a late-binding closure |
| Sensitive items are isolated | Filtering applied only to an intermediate read view; the final result still carried sensitive items and internal keys | Plan gap: the plan named the intermediate view, not every exit |
| Model output is grounded in its input | Numbers from structured findings in the input were not collected, so downstream model nodes that cited them were flagged and degraded | Plan gap: the source set listed scalars, strings, and by-year values, not structured objects |
| Grounding catches invented numbers | Negative percentages were flagged; Chinese numerals and invented numbers in JSON fields passed | Plan gap: variants were incomplete, and the year exemption had no counterexample |
| Retries converse correctly | The retry did not send the model's previous answer back | Both: the plan did not specify the message sequence |
| Model input is readable | The input was a runtime repr string with non-ASCII text escaped | Both: the plan said "JSON string" without the encoder, character set, or field order; the executor used a default string conversion |
| One regulation-reference check | Two parallel lanes each implemented the same check with different normalization | Plan gap: shared decision logic was not assigned to the contract wave or to one owning lane |

The acceptance observation that generalizes: a rule-to-test trace is not satisfied because a named test exists and passes. The isolation test asserted only the intermediate view, so the rule held where it was tested and failed where it mattered. Traces must name the exit at which each rule takes effect.

## A parser lane: right values, missing rows

The extraction lane (report parser, data-quality checks, pseudonymization) was largely sound. Thirty-seven golden values copied independently from the plan all matched. Every data item was present, contract round-trips were lossless, pseudonymization left no residue, and its real output drove the downstream golden chain end to end. The approach that worked for parsing was an explicit mapping table, golden samples, and item-by-item assertions against the source.

The defects sat where golden samples do not reach, in completeness:

| Requirement | Defect | Root cause | Better plan evidence |
| --- | --- | --- | --- |
| Monthly table is complete | The continuation page of a cross-page table had no header, and nine of thirty-six months were lost, including two months the source flagged as risky | Plan gap and execution deviation: the continuation rule was written for one section only, and only that section was implemented; each table item had a single sampled golden value | Completeness assertions (all 36 months, all 13 rows), the structural rule stated for every table with every occurrence listed, and reconciliation totals as whole-table assertions |
| Columns are mapped correctly | In a multi-column table, "filed" was read as 0 and "difference" came from another column | Execution deviation in header interpretation; the source's own identity (due − filed = difference) was not used as an assertion | The identity asserted for every row, and mirrored as a runtime warning |
| Invalid input fails with the declared error | Input in the wrong format or corrupted input raised the underlying library's exception | Plan gap: the declared error type had no named test | A named invalid-input test per declared error, asserting that library exceptions do not leak |
| Hand-written fixtures agree with real output | A downstream lane's hand-written fixture held the correct value; the parser produced a wrong one; the two were never compared | Plan gap: no comparison step | A fixture-versus-real-output diff after the upstream merge and before downstream integration, with each difference attributed |

One report was inaccurate. The executor ran the tests without the environment variables the plan required, got two failures, and reported them as existing baseline failures unrelated to the lane. Rerun with the planned command, the suite had zero failures. Reports that attribute a failure to the baseline must now include the same failure on the baseline commit, run with the same command and environment, and the acceptor reproduces it before accepting the claim.

## The second wave: test doubles looser than the host, and paths taken only once

Four lanes delivered in the second wave: seed content, an admin API, an admin UI, and the run pipeline. Every lane's own suite passed. The content lane was accepted after two small reworks. The other three were sent back, and most of their defects sat outside what their tests exercised.

| Requirement | Defect found at acceptance | Root cause | Better plan evidence |
| --- | --- | --- | --- |
| Admin endpoints require authentication | Requests without credentials returned 200 as the default tenant's administrator | Plan gap: the plan asked for "a fake auth dependency that returns the actor". The executor's fake took no arguments, so the code called the real dependency without arguments, swallowed the resulting error, and fell back to a default administrator | Doubles that reproduce the host's real signature and failure mode, with the host source location named; a red line that auth failures never fall back to a default identity; a named "unauthenticated returns 401" test |
| "Show real names" restores pseudonyms | The toggle returned pseudonymized data unchanged | Plan gap: the test data contained no pseudonyms, so an identity implementation passed | Transformation tests whose inputs contain something the transformation changes |
| Parse warnings reach the report | Any document with a parse warning failed the run at assembly; a JSON column read back as a list was parsed again as a string | Plan gap: the only sample had no warnings | Every field written in one stage and read in another exercised with a non-empty value against the real database |
| The consultant is notified once the draft is ready | After a failure and a resume, the draft-ready notification was never sent, because the failure notice had claimed the same "notified" marker | Plan gap: the marker's owner was not stated, and no test crossed flows | One event per idempotency marker, and a fail → resume → succeed sequence test |
| Any error marks the run failed | A database error left the run stuck "in progress" with no reason recorded; the failure write ran on an aborted transaction | Plan gap: no rollback step and no database-level error injection | Roll back before writing the failure; inject a database error in the test, not only a Python exception |
| Runs execute the frozen plan | Execution read each skill's current published version, so trial drafts never took effect and a resume could switch versions | Plan gap: the plan froze versions but did not say every later read uses the frozen version id | A test that publishes a new version between planning and execution |
| Every parameter matters | The approve function ignored its disclaimer flag | Execution deviation, not caught because no test varied the flag | One test per parameter that changes only that parameter |
| Retention deletes expired files | Content-addressed storage keys could be shared by two runs; deleting one run's file removed the other's | Plan gap: the plan did not state that storage keys are content digests | Shared-key semantics in the plan and a two-records-one-object test |
| Report text is readable | A finding printed internal enum codes in the customer-facing text | Plan gap: no display labels for internal values | Display mappings, and an assertion that outputs contain no internal codes |
| Regulation numbers are accurate | One statute had the wrong presidential-order number, and the test asserted the wrong value | Execution deviation enabled by the plan: "do not invent" with no per-entry verification table | Per-entry source table in the report; expected values taken from the source; a full recheck if a spot check finds one error |
| The UI builds | The host's structural validator passed, but five pages imported a path that does not resolve, so the type check and build failed | Plan gap: the acceptance command was the structural validator only | The host's real build or type-check command as the acceptance gate |
| UI and API agree | The run page displayed a "calls" column that the API never returned | Plan gap: the endpoint table described fields in prose | Field names and types in the endpoint table, checked from both sides |
| The plan's own content is runnable | A content card asked an explanation template to reference another data item, which the engine did not support; a golden year list contradicted the card's own threshold | Plan gap | Self-checks: every placeholder or function a content card uses exists in the engine; golden values recomputed against the card's thresholds |

Two reports were inaccurate. One lane wrapped its required assertions in `if` blocks that never ran; another reported "nothing outstanding" while three tests named on its card were missing. The acceptor now compares the card's named tests with the actual test functions, searches for assertions inside conditionals, swaps host doubles for versions with the real signature, and runs the strictest host build available.

### The pipeline rework: hangs that hid three integration defects

The pipeline lane's first rework reported five hanging tests and delivered a run that excluded the integration file. The acceptor inspected database activity during a hang: a test connection left idle in a transaction held a row lock, so the schema teardown waited forever. Each of four copies of the connection fixture opened connections and never closed them, so any failed assertion turned into a hang. With cleanup added, the hangs became ordinary failures, and the acceptor merged the trunk, which by then held the full seed content, into the lane. The first run with real content exposed defects that no lane's tests had reached:

| Requirement | Defect | Root cause | Better plan evidence |
| --- | --- | --- | --- |
| Calculation skills run in production | Every real run failed on the first calculation node, and submitting a calculation skill reported "operator not registered" | Plan gap: operators registered themselves through import side effects, and no task owned loading them. Only one admin endpoint called the loader, and the content lane's tests called it explicitly, which masked the gap | Registry loads itself on first query, or a named startup task owns it; a fresh-process test that queries without calling the loader; no explicit loader calls in tests |
| Rules evaluate by year | A by-year rule that did not declare the year list returned "done, zero findings" with no error. The design's own example omitted the year list, and the minimal fixture copied the example, so the pipeline's integration tests never produced a finding | Plan gap: the design example and the engine's contract disagreed, and the engine had no stated behavior for a missing scope input | Design and plan examples run through the engine as fixtures and must produce output; a stated behavior (derive, error, or skip) for each missing scope input, never a silent empty success |
| Resume reuses finished nodes | After a mid-run crash, all node results and model-cache entries were rolled back with the status change, so the resume repaid every model call | Plan gap in the acceptor's own rework order: "roll back before writing the failure" did not say which writes must survive | Every rollback requirement names the durable writes, which use an independently committed connection; a crash-injection test that checks finished nodes are still stored |
| The optional disclaimer is applied | The text was hardcoded in the approval function and the choice was not persisted | Plan gap: the design said the text is "locked and shipped with code", but no task owned the content | Each locked asset has an owning task with exact content or a source; readers fail closed when it is missing |
| Integration tests use the real content | The pipeline tests still used the minimal fixture after the content lane merged | Plan gap: the plan required the full seed but did not schedule the rerun after the upstream merge | A named post-merge rerun with the real upstream artifact, as part of the downstream lane's acceptance |

Two acceptor practices came out of this round. When an executor reports a hang, inspect the database's active connections during the hang before reading the code. When reference data is checked with a page-fetch or summarization tool, accept only verbatim quotes: in one check the tool reported a decree number that did not appear anywhere on the page.

## The third wave: a plan sync caught some gaps, and the review lane showed the rest

Before the third wave, the acceptor synced the plan with what the second wave had taught. Checking each new expectation against a real run or the host source caught several gaps before handoff. The sample produced no low-severity findings, so a requirement that all twelve sections have content could not hold. A model-call exception only degrades a node and does not fail the run, so the planned crash-and-resume test needed a crash outside the execution graph. The planned menu icon was not on the host's allow-list. The sync also found that the sample lane wrote files to one storage bucket while the run pipeline read only from another, and that two lanes would compute the same plan fingerprint independently. All of these went into the task cards as stated conventions.

The review-and-trial lane still failed first acceptance, on defects the sync did not reach:

| Requirement | Defect found by probe | Root cause | Better plan evidence |
| --- | --- | --- | --- |
| Reviewers edit report blocks | Writing text to a finding card, table rows to a paragraph, or an empty body all returned 200; approval then succeeded and the finding-card edit was silently lost | Plan gap: the card named the allowed field per block type but not the rejection of everything else | Per-type allowed fields and shapes with explicit rejection; a test per mismatch asserting no audit row; a test that each accepted edit reaches the rendered output |
| A trial always progresses | A failed enqueue still returned 200 and left the run queued; deduplication then kept returning the stuck run, so that sample and version could never be trialed again | Plan gap: the enqueue-and-rollback rule was written for the main entry point only, not for trial and baseline entries | The same enqueue rule applied to every entry that creates a record and enqueues; deduplication never returns a record that cannot progress |
| A double click creates one trial | Two concurrent requests created two runs | Plan gap: "deduplicate double clicks" was tested only sequentially | Lock before the deduplication query; a two-connection barrier test |
| The trial page opens from the skill editor | Not implemented | Plan gap: the editor page belonged to another lane's files, and the card gave this lane only two specific fixes in it | Every requirement names the file it changes, and that file is in the lane's scope |

Two named tests existed but covered only half of what their cards specified: the disclaimer test checked only the stored flag, and the compare test omitted the severity-change case. The report marked both as passing. The acceptor now opens each named test and checks its assertions against the card, not only its name.

## A follow-on phase: old tests, shared infrastructure, and a missing identity

The second phase of the same project reused the wave structure. The contract wave went well, but the first parallel wave hit three gaps that had nothing to do with the code the lanes wrote, plus one ordinary defect that a probe found.

| Requirement | What happened | Root cause | Better plan evidence |
| --- | --- | --- | --- |
| Lanes add skills and recipes to the shared seed | The first lane to add a skill stopped on its first task: a phase-one test asserted the skill inventory with `==`. Two more lanes would have hit the same test | Plan gap: the follow-on plan did not scan existing tests for equality locks on the lists it extends | The contract wave relaxes such tests to "earlier items present, new items within the plan's tables", proves it with an injected unplanned item, and names a final-wave task that restores equality |
| Background jobs run | Phase one had used a queue name without the host's required prefix. The host validator and the compose tool both passed; only the worker rejects it at startup | Plan gap: a host naming rule checked only at runtime was never looked up | Grep the host source for runtime-only naming rules on every identifier the plugin declares; a contract-wave guard test over all of them |
| Each lane runs the full suite | Four lanes shared one test database, and the fixture dropped and recreated a fixed schema. Full runs deleted each other's tables and hung on locks. One lane then restarted the shared database container twice and ended every python process on the machine, which killed the acceptor's three acceptance runs | Plan gap: the environment table named one database for all lanes, and the red lines forbade recreating containers but not restarting them or killing other processes | One test database per lane plus one for the acceptor, created in the contract wave and written into each lane's command; red lines against restarting or stopping shared services and against ending processes the lane did not start |
| Self-service customers are tracked by their external user id | An event with an empty user id created a customer with an empty reference, so every such event would share one customer, its quota, and its paid package | Plan gap: the card did not say what to do when the grouping identity is missing | Reject a missing identity with zero side effects, with a named test that asserts no record, no enqueue, and no external call |

The acceptor practice that came out of this wave: when a full run ends with a burst of connection failures, or the test process exits with no summary, check the shared container's start time, the database log, and the process list before rerunning. If another lane caused it, stop and notify the lanes first; rerunning only gets killed again.

### Rework and merges in the same wave: checks that never fail, and shapes that cross lanes

The rest of the wave produced five more gaps. Every lane's own suite was green in each case.

| Requirement | What happened | Root cause | Better plan evidence |
| --- | --- | --- | --- |
| Runs use the template version frozen at planning | The lane wrote a reader for the frozen version, but the existing planner always wrote 0, so the reader never ran and every run ignored templates published by admins | Plan gap: the card named the field to read but not the production code that writes it | Name the writer of every field a card reads and confirm it writes a real value; test the field through the production chain |
| Each table is taken from between its item and the next | The first rework added a position check, but a loop right after it reassigned the result by header alone. The check's anchor was also a generic year string that appears in every item's own text, so it could never reject. The lane's test passed only because its table sat on another page and a page filter excluded it | Execution deviation, made easy by a plan that did not name the anchor | Name an anchor unique to the located object; test the hardest layout (two items on the same page); the acceptor confirms each new check rejects a crafted input |
| Paid-tier estimates are ranges | The plan card narrowed the design's "every amount" to one table without saying so; a late-fee detail table kept exact figures. The lane's test asserted that a phrase appeared in the text, and that phrase was the section title | Plan gap (an unstated narrowing of a release-blocking assertion), plus a vacuous assertion | Copy every release-blocking assertion from the design into the card with its own test; check that an assertion's match does not come from fixed template text |
| A trial run executes the draft skill | A phase-one test passed only because the draft node ran before a minimal fixture aborted the run. Another lane's new nodes changed the order, and the test failed | Plan gap in the earlier phase: the test asserted a partial result without asserting the run itself succeeded | Assert overall run status before partial results; use fixtures that do not abort the run |
| The paid tier renders after both lanes merge | One lane added findings with no amounts; the other lane's amount guard parsed the empty marker as a number, and every paid-tier run failed. Neither lane's tests could construct the other lane's data | Plan gap: no table of new data shapes against each tier's transforms, and no full-pipeline run per merge | A cross table of new shapes against every tier's transforms and renderers; after each merge, run the real sample through every tier |

The acceptor also met delivery reports for work that was still uncommitted. Before accepting, check that the worktree is clean and that HEAD matches the reported commit.

