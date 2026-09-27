# Odoo Migration Guide — shared cross-version knowledge base

Central, machine-readable migration knowledge for the ecosystem's Odoo
modules, organized **by transition (17→18, 18→19, 19→20, 20→21)** rather
than by target version. Consumed by humans and by AI agents (Codex,
Claude, Cursor) so the same breaking-change knowledge reaches every repo.

> Renamed from `odoo-19-migration-guide` (2026-09-27, owner decision —
> GitHub rename, history and old-URL redirects preserved). The original
> files were moved — with git history preserved — into
> [`transitions/18-to-19/`](transitions/18-to-19/). The executed transition
> plan is in [`TRANSITION-PLAN.md`](TRANSITION-PLAN.md).

## Layout

```
transitions/17-to-18/    # reconstructed incrementally from real PRs (see below)
transitions/18-to-19/    # seeded from the original odoo-19-migration-guide
transitions/19-to-20/    # seeded from the ThaiACC Odoo 20 work (2026-09)
transitions/20-to-21/    # empty until 21.0 work starts
shared/                  # cross-transition rules (OCA process, PR checklist)
evidence/                # how to record evidence + index of real cases
```

Each transition directory carries:

| File | Purpose |
|---|---|
| `README.md` | Explanations and code examples |
| `migration-rules.yaml` | Machine-readable detect/fix patterns (severity, auto_fix) |
| `CHECKLIST.md` / `checklist.md` | Copy-paste checklist for PR descriptions |
| `evidence.md` | Links to real commits/PRs that prove each rule |

## Three-layer knowledge model

1. **Upstream authority** — [OCA maintainer-tools migration wiki](https://github.com/OCA/maintainer-tools/wiki) and Odoo upstream references. Canonical procedure and expectations.
2. **This repo (shared engineering knowledge)** — version-agnostic breaking-change rules, detect/fix patterns, evidence.
3. **Domain/project decisions** — stay in the project repo (e.g. ThaiACC accounting architecture in `monthop-gmail/thaiacc-odoo`). Link, don't duplicate.

## Working rule (migration PRs)

A migration PR is **not fully done** until:

- [ ] module installs on the target version
- [ ] tests pass
- [ ] pre-commit passes (OCA repos)
- [ ] history/upstream migration policy is respected (OCA: one `[MIG]` on preserved history, no copyright-year edits)
- [ ] the central transition guide was checked for known rules
- [ ] any NEW migration discovery is recorded here, **or** the PR states "no new migration rule discovered"

The machine-checkable part of this rule lives in [`shared/pr-checklist.md`](shared/pr-checklist.md).

## Evidence-growth rule

Completeness is not required before use. Older transitions (e.g. 17→18) are
reconstructed incrementally from real commits, PRs and remembered breakages —
a placeholder README plus one evidence link is a valid starting state.

## Agent quick start

Point your agent at the target transition directory:

```bash
claude "Migrate this module from Odoo 19 to 20. Use the guide at
/path/to/odoo-migration-guide/transitions/19-to-20"
```

Agent instructions per transition live in that directory's `CLAUDE.md` when
present; the generic workflow is in [`CLAUDE.md`](CLAUDE.md).
