# Repository transition plan: `odoo-19-migration-guide` → `odoo-migration-guide`

Status: **proposed — awaiting review** (per handoff ho-6db40027: "Do not delete
or rename the current repo until a non-destructive migration plan is reviewed").
No rename or deletion has been performed.

## Goal

One central repo (`monthop-gmail/odoo-migration-guide`) holding all
transitions, replacing the single-version `odoo-19-migration-guide` name.

## Options considered

### Option A — rename the existing repo (recommended)

`Settings → Rename: odoo-19-migration-guide → odoo-migration-guide` on GitHub,
then restructure on a branch merged to `main`.

- Full history preserved by definition (same repository object).
- GitHub automatically redirects clones, issues, PRs and stars from the old
  name; existing `origin` URLs keep working after rename.
- The branch `transition/central-guide` on this repo already contains the new
  structure; after rename it merges to `main` and the transition is done.
- Downside: none material. Redirects break only if a NEW repo with the old
  name is later created (do not create one).

### Option B — new repo + history migration

Create `odoo-migration-guide` empty, then `git filter-repo` the old repo into
`transitions/18-to-19/` layout and push that history into it.

- Preserves commit history but rewrites it (new hashes).
- Loses stars/issues/PR history unless migrated by hand.
- Leaves two repos to clean up.
- Only worth it if the old repo must keep its name and content untouched —
  not the case here.

## Decision requested

Approve **Option A**: rename repo, then merge `transition/central-guide`
into `main`. The rename is reversible and non-destructive (GitHub keeps a
working redirect), satisfying the "safe transition path" requirement.

## What is already done on branch `transition/central-guide` (non-destructive)

- `README.md`, `CLAUDE.md`, `migration-rules.yaml`, `CHECKLIST.md` moved with
  `git mv` (history preserved) into `transitions/18-to-19/`.
- New cross-version structure created: `transitions/{17-to-18,19-to-20,20-to-21}/`,
  `shared/`, `evidence/`, plus `transitions/19-to-20/` seeded from the ThaiACC
  Odoo 20 findings (generic rules only; Thai accounting rules stay in
  `thaiacc-odoo` and are linked, not copied).
- `shared/pr-checklist.md` encodes the "migration PR is not fully done"
  working rule, including the mandatory central-guide check
  ("new rule recorded" or "no new migration rule discovered").
