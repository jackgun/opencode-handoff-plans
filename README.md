# OpenCode Handoff Plans

Source-available Codex skill and reusable templates for planning, handing off, and independently reviewing OpenCode-driven software delivery. It turns product requirements into explicit contracts, executable acceptance scenarios, and evidence-based delivery reports.

## What is included

- `skills/opencode-handoff-plans/` — installable Codex skill for writing and reviewing implementation plans handed to coding agents.
- `docs/plan-retrospective.md` — lessons from a real multi-agent delivery and acceptance cycle.
- `docs/task-template.md` — compact task and verification template.

The skill helps a team trace each important rule from its specification through ownership, production wiring, acceptance scenarios, and evidence. It is intended for complex delivery such as state machines, persistence, external integrations, and parallel work. It is not a substitute for directly fixing a small, local issue.

## Use with Codex

Copy `skills/opencode-handoff-plans` into your Codex skills directory, or install it from this repository using your usual Codex skill workflow. Then invoke:

```text
Use $opencode-handoff-plans to write an implementation plan for OpenCode and an acceptance plan for Codex.
```

## License

This repository is source-available under the [PolyForm Noncommercial License 1.0.0](LICENSE.md). It permits noncommercial use, modification, and distribution under its terms. Commercial use requires a separate written license from the copyright holder.

This is not an OSI-approved open-source license because it restricts commercial use.

## Contributing

Please read [CONTRIBUTING.md](CONTRIBUTING.md). Useful contributions are evidence-backed improvements to the planning method, task template, and examples drawn from real delivery outcomes.
