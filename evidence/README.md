# Evidence

Migration rules without evidence rot. Every rule recorded in a
`transitions/*/migration-rules.yaml` needs at least one pointer to a real
reproduction: a commit, a failing-then-passing test run, or a source
reference.

## How to record

1. Add a row to the transition's `evidence.md`: rule id → commit/PR/branch
   that reproduced it (fail before, pass after).
2. Prefer permanent links: GitHub commit URLs, ai-collab discussion ids, or
   test-script paths inside repos.
3. Domain-specific evidence stays in the project repo; link it, don't copy.

## Index

- 15→16: `transitions/15-to-16/evidence.md` (10 rules, 3 verified; versions checked on odoo/odoo 15.0/16.0)
- 16→17: `transitions/16-to-17/evidence.md` (13 rules, 4 verified; checked on 16.0/17.0)
- 17→18: `transitions/17-to-18/evidence.md` (9 rules, 3 verified; checked on 17.0/18.0)
- 18→19: `transitions/18-to-19/README.md` (built from real OCA CI failures)
- 19→20: `transitions/19-to-20/evidence.md` (verified on 20.0.20260926)

`confidence: verified` = a real module failed and the fix made it pass;
`source-only` = confirmed in odoo/odoo source, no recorded fail→pass run.
Private-module evidence is cited by commit SHA only — never copy vendor code.
