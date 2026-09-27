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

- 18→19: `transitions/18-to-19/README.md` (built from real OCA CI failures)
- 19→20: `transitions/19-to-20/evidence.md` (verified on 20.0.20260926)
- 17→18: to be reconstructed incrementally — add the first row when a real
  17→18 case surfaces
