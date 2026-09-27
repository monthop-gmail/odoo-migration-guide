# Migration PR checklist (working rule)

A migration PR is **not fully done** until every box below is checked.
Copy this into the PR description.

```markdown
## Migration PR checklist

- [ ] Module installs on the target version (fresh db, not upgrade-only)
- [ ] Tests pass (`--test-enable --test-tags /<module>`), 0 failed 0 errors
- [ ] pre-commit passes (OCA repos) or repo lint equivalent
- [ ] History/upstream migration policy respected (OCA: [MIG] on preserved history; no copyright-year edits)
- [ ] Central transition guide checked (`transitions/<from>-to-<to>/migration-rules.yaml` scan run)
- [ ] New migration discoveries recorded in the central guide, **or** stated here:
      "no new migration rule discovered"
```

The last two items are the loop-closer: either knowledge flows back into this
repo, or the PR explicitly says nothing new was learned. An unchecked guide is
a review-blocking defect, not a formality.

Scanning helper (adjust paths):

```bash
grep -rEn "$(python3 -c "
import yaml; print('|'.join(r['detect'] for r in yaml.safe_load(open('transitions/19-to-20/migration-rules.yaml'))['rules']))
")" <module-dir>/ --include='*.py' --include='*.xml' --include='*.csv' --include='*.js'
```
