# OCA migration process (applies to OCA-side PRs)

From the [OCA maintainer-tools migration wiki](https://github.com/OCA/maintainer-tools/wiki/Migration-to-version-20.0)
(plus the 19.0 page for framework checklists). These are process rules;
technical rules live in each `transitions/` directory.

## Before migrating

- Read the latest OCA conventions; subscribe to the relevant OCA project list.
- Announce the module on the repo's GitHub issue "Migration to version X.0".
- Install pre-commit (OCA repos carry their own config).

## Preserving history (required)

```bash
git clone https://github.com/OCA/$repo -b $new
git checkout -b $new-mig-$module origin/$new
git format-patch --keep-subject --stdout origin/$new..origin/$old -- $module | git am -3 --keep
pre-commit run -a          # formatting in one [IMP] commit, --no-verify
# ... apply the transition checklist ...
git commit -m "[MIG] $module: Migration to $new"
# PR title: "[$new][MIG] <module>: Migration to $new"
```

If the target branch does not exist yet, branch from the previous version
branch (`origin/$old`) and re-base onto `origin/$new` when the bot creates it.

## Never do

- Change copyright years or original authors.
- Squash real commits (only bot/Weblate commits may be squashed).
- Vendor an unreleased dependency into a branch to make CI pass.
- Rename a module without renaming its whole commit history
  (`git filter-repo --path-rename`).

## Team rules that have cost PRs before

- One module per PR; no `.codecov.yml`, no `.github/workflows/*` changes.
- `*mig-*` branches on the fork are live PR state — no casual force-pushes.
- AI disclosure: `Assisted-by: <model>` trailer, never `Co-authored-by:`.
