# Producer coverage guidance: validation record

2026-11-03. Baseline skill revision: `7538c65`. This change responds to an
observed handoff escape: a universal new-event field rule was implemented for
ordinary actions, while a scheduled path wrote directly to the outbox. Public
examples passed because they did not exercise that path and the consumer could
fill the field. Both the author and acceptor had missed the path inventory.

The skill already told agents to check other entry points. The change makes the
inventory an actual task-card artifact with a code baseline, applicability,
bypasses, ownership, exit assertions and separate implementation/verification
states. It assigns discovery to the author, per-row delivery to the implementer,
and independent rediscovery/reconciliation to the acceptor. It does not mandate
a particular helper implementation or add this workflow to isolated small edits.

## Frozen scenarios

Before changing the skill, a small synthetic fixture and rubric were written:

- `api.create` and `api.finish` call a helper that already saves `public_ref`.
- `timer.poll` calls the low-level writer through an import alias and omits it.
- `recovery.retry` calls the low-level writer directly and also omits it.
- `adapter.receive_raw` uses that writer for an opaque snapshot, explicitly
  outside the notification-field contract.
- The consumer fills a missing reference from the ticket without changing the
  original event status. Its correct output can hide missing producer fields.

Three independent contexts per skill version received the same task wording:
write a handoff plan; review a claimed passing delivery; decide whether a single
button-label typo needs this skill. Agents read immutable baseline/updated skill
copies and the same fixture, without the rubric or each other's responses.

Checks: discover all four notification producers and the reasoned exclusion;
require real-entry raw-record assertions before consumer enrichment; explicitly
import the old event shape; preserve passing paths; identify ownership and an
independent coverage reconciliation; keep the label typo out of the full flow.

## Observed results

| Scenario | Baseline | Updated skill |
| --- | --- | --- |
| Plan | Found both omissions, four producer paths and raw exclusion; supplied a runnable regression. Source revision was deferred and independent path rediscovery was not explicit. | Recorded source hashes/search scope, four applicable plus one excluded path, per-path task/status and an explicit candidate write-point rediscovery against the table. |
| Delivery review | Found both omissions together, preserved passing paths and checked raw records. | Found the same two omissions, materialized the applicable/excluded inventory and explicitly assigned author, implementer and acceptor closure duties. |
| Small edit | Rejected full handoff ceremony. | Rejected full handoff ceremony. |

The coordinator independently executed the Python regression embedded in each
plan against the unchanged fixture. Both exited 1 on the two actual missing-field
paths; the updated script reported five passing checks and two failing checks.
This is expected fixture behavior, not a claim that the business bug was fixed.
The source hashes cited by the updated plan were independently verified.

`quick_validate.py` and `git diff --check` passed. Runtime changes are limited to
the skill and its existing references; no production code, deployment or external
messages are involved. The installed Codex skill points to this repository.

## Limits and reproducibility

These are single-run qualitative smoke checks, not a measured success-rate gain.
The old skill also found the simplified omissions; it would be misleading to say
it universally failed or that the new text guarantees no future escape. The
observed real-world failure motivates the structural change. The fresh checks
show that the revised structure can be applied without enlarging a small task.

Local fixture, immutable skill copies, frozen rubric, all six responses and two
execution logs are retained under `.pytest_cache/producer-coverage-eval/` in the
validation checkout; they are not runtime skill resources. The scenario above
states the reusable behavior to reproduce. No private project data is embedded
in the skill guidance.
