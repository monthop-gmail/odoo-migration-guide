# 18→19 evidence index

The rules in `migration-rules.yaml` up to `urljoin-odoo-tools` were built
from real OCA CI failures (see `README.md`). This index covers rules added
later from private module migrations; private evidence is cited by commit
SHA only.

`changed_in` was checked against odoo/odoo branches 18.0 and 19.0 (links in
`migration-rules.yaml`), because the migrations jumped 15.0 → 20.0 and
their logs attribute changes to the range where they were noticed.

| Rule | Confidence | Reproduced by |
|---|---|---|
| `product-search-location-context` | source-only | private module migration, commit `304e7e0` (custom product search view aligned with the 19.0 core context keys; no failure, the model still reads the old keys) |
| `sale-confirm-no-customer-follower` | verified | private module migration, commit `26974c1` (portal test failed: confirmed order no longer followed by the customer, so follower-based portal access matched nothing) and commit `7376633` (access rule moved to the order's customer) |
