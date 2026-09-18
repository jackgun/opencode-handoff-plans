# Contributing

This project improves planning and acceptance methods through observed delivery outcomes.

Before proposing a change, describe the concrete ambiguity, escaped defect, or review failure it addresses. Keep guidance narrow and reusable: do not turn a project-specific convention into a universal rule without evidence.

For changes to the skill:

1. Preserve the distinction between requirements, evidence, and unverified assumptions.
2. Keep `SKILL.md` concise; put detailed examples and templates in `references/` or `docs/`.
3. Include a realistic scenario that would make the new guidance change an agent's decision.
4. Run the skill validator before opening a pull request.

```powershell
python "$env:CODEX_HOME/skills/.system/skill-creator/scripts/quick_validate.py" skills/opencode-handoff-plans
```

The maintainer may request that examples are generalized or that project names, customer data, credentials, and internal URLs are removed.
