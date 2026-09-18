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

## Measuring improvement

For comparable deliveries, record the first acceptance result, number of repair rounds, defects caused by missing plan detail, defects hidden by mocks, and unverified dependencies reported honestly. Test-count growth alone is not a useful quality metric.
