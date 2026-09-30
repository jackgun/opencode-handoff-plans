# OpenCode Handoff Plans

Source-available agent skill and reusable templates for planning, handing off, and independently reviewing OpenCode-driven software delivery. The planning agent can be Codex, Claude Code, or another skill-capable agent, typically working in a superpowers-style plan → execute → review workflow; the reviewer can be an agent or a person. It turns product requirements into explicit contracts, executable acceptance scenarios, and evidence-based delivery reports.

## What is included

- `skills/opencode-handoff-plans/` — installable skill for writing and reviewing implementation plans handed to coding agents.
- `skills/opencode-handoff-plans/references/exact-handoff.md` — exact-execution plans, reproducible patch steps, environment boundaries, red lines, and deviation reports.
- `skills/opencode-handoff-plans/references/parallel-waves.md` — multi-lane, multi-wave plans: contract waves, plan sync before dispatch, handoff skeleton, and guard tests.
- `skills/opencode-handoff-plans/agents/openai.yaml` — optional display metadata for Codex.
- `docs/plan-retrospective.md` — lessons from a real multi-agent delivery and acceptance cycle.
- `docs/task-template.md` — compact task and verification template.

The skill helps a team trace each important rule from its specification through ownership, production wiring, acceptance scenarios, and evidence. It is intended for complex delivery such as state machines, persistence, external integrations, and parallel work. It is not a substitute for directly fixing a small, local issue.

## Installation and use

Copy `skills/opencode-handoff-plans` into your agent's skills directory, or install it with your agent's usual skill workflow. For example, Codex reads personal skills from `~/.codex/skills/` and Claude Code from `~/.claude/skills/`.

Invoke the skill by name, or describe the task and let the agent select it:

```text
Use opencode-handoff-plans to write an implementation plan for OpenCode and an acceptance plan for the independent reviewer (an agent or a person).
```

Codex also accepts `$opencode-handoff-plans`; Claude Code also accepts `/opencode-handoff-plans`.

## License

This repository is source-available under the [PolyForm Noncommercial License 1.0.0](LICENSE.md). It permits noncommercial use, modification, and distribution under its terms. Commercial use requires a separate written license from the copyright holder.

This is not an OSI-approved open-source license because it restricts commercial use.

## Contributing

Please read [CONTRIBUTING.md](CONTRIBUTING.md). Useful contributions are evidence-backed improvements to the planning method, task template, and examples drawn from real delivery outcomes.
