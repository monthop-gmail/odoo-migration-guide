# Odoo Migration Guide — agent instructions

You are migrating an Odoo module (or recording migration findings) using the
shared cross-version knowledge base.

## Finding the right rules

1. Identify the transition (e.g. 19→20) and open `transitions/<from>-to-<to>/`.
2. Load `migration-rules.yaml` — every rule has a `detect` regex, `severity`,
   and `auto_fix` flag.
3. Domain-specific (accounting/localization) decisions live in the project
   repo — e.g. ThaiACC accounting rules are in `monthop-gmail/thaiacc-odoo`
   (`MIGRATION-20.0.md`). Link to them; do not copy them here.

## Migrating a module

1. Run every `detect` pattern against the target module. Report rule hits.
2. Apply `auto_fix: true` rules; verify each change in context.
3. Show `auto_fix: false` matches with a proposed fix before applying.
4. Bump `__manifest__.py` version to `<target>.0.1.0.0`; update badge URLs.
5. Validate: module installs, tests pass, pre-commit passes (OCA repos).
6. Commit OCA-style: `[MIG] module: Migration to <target>.0`.

## Closing the loop (mandatory)

A migration PR is not done until the central guide was checked and either:
- each NEW discovery is recorded as a rule + evidence in this repo, or
- the PR explicitly states "no new migration rule discovered".

Record evidence per [`evidence/README.md`](evidence/README.md).

## Repo transition

The old single-version guide was moved (history preserved) into
`transitions/18-to-19/`. Do not rename the repo itself before the plan in
[`TRANSITION-PLAN.md`](TRANSITION-PLAN.md) is reviewed and approved.
