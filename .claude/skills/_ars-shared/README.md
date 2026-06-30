# Academic Research Skills (ARS) — vendored copy

This directory vendors the **Academic Research Skills** Claude Code plugin so the
skills work in Claude Code **web sessions** (where `/plugin install` is not
available — plugins only install in the local CLI).

- **Source:** https://github.com/Imbad0202/academic-research-skills
- **Version:** 3.13.0
- **Author:** Cheng-I Wu (https://github.com/Imbad0202)
- **License:** CC-BY-NC-4.0 (see `LICENSE` / `NOTICE.md` in this folder)

## What was vendored (lean scope)

| Location | Contents |
|----------|----------|
| `.claude/skills/deep-research/` | Deep Research skill (13-agent pipeline, 8 modes) |
| `.claude/skills/academic-paper/` | Paper-writing skill (12-agent, 11 modes) |
| `.claude/skills/academic-paper-reviewer/` | Peer-review skill (5-reviewer panel) |
| `.claude/skills/academic-pipeline/` | End-to-end orchestrator (10 stages) |
| `.claude/skills/_ars-shared/shared/` | Cross-skill schemas, contracts, rubrics, references |
| `.claude/skills/_ars-shared/agents/` | The 3 plugin-shipped agents |
| `.claude/skills/_ars-shared/CLAUDE.md` | Plugin routing-discipline doc |
| `.claude/skills/_ars-shared/MODE_REGISTRY.md` | All 27 modes + trigger keywords |
| `.claude/skills/_ars-shared/POSITIONING.md` | Design philosophy / scope boundaries |
| `.claude/commands/ars-*.md` | The 16 `/ars-*` slash commands |

`_ars-shared/` has no `SKILL.md`, so skill discovery ignores it — it is plain
support material.

## Path mapping for in-skill references

The original plugin resolves relative references against the **plugin root**.
In this vendored layout, when a `SKILL.md` (or agent) references:

- `` shared/... `` → read from `.claude/skills/_ars-shared/shared/...`
- `` agents/... `` (at plugin root, the 3 shipped agents) → `.claude/skills/_ars-shared/agents/...`
  (note: each skill's *own* `agents/` subdir lives inside that skill dir and resolves normally)
- `` .claude/CLAUDE.md `` (routing discipline) → `.claude/skills/_ars-shared/CLAUDE.md`
- `` MODE_REGISTRY.md `` / `` POSITIONING.md `` → `.claude/skills/_ars-shared/`

## Lean-scope omissions

To keep the repo light, the following plugin parts were **not** vendored:

- `scripts/` — 217 Python integrity-gate / verifier scripts (~4.4 MB). These are
  *advisory* gates that the plugin runs via its own hook runtime, which does not
  load in web sessions. Skill `SKILL.md` references to `` scripts/*.py `` will
  therefore not resolve to a local file; the skills remain fully usable for their
  research / writing / review workflows without them.
- `hooks/` (SessionStart announce + PreToolUse write-scope guard) — require the
  plugin runtime.
- Translated READMEs, `tests/`, `evals/`, large `examples/`.

If you later want full fidelity, re-fetch the tarball from the source repo and
copy `scripts/`, `hooks/`, and the rest alongside this folder.

## Note on `deep-research` name

This web environment ships a built-in `deep-research` skill. The vendored
plugin skill uses the same name (kept intentionally so the `academic-pipeline`
and `academic-paper` skills can reference it via their `depends_on` /
`related_skills` metadata). The project-level skill here takes precedence.
