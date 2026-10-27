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
| `sol-prepare-procurement-values-no-args` | verified | private module migration, commit `ec4797c` (override with `group_id` → TypeError on order confirmation, found by another module's tests; fixed signature, regression test `238fcea`). The log says "20.0"; the signature changed in 19.0 |
| `stock-move-group-id-picking-assignation` | verified | private module migration, commit `234b496` (copied `_search_picking_for_assignation` with `group_id` → AttributeError on delivery creation; now extends `_search_picking_for_assignation_domain`) |
| `website-sale-cart-update-removed` | verified | private module migration, commit `41d1697` (monkeypatch of `website_sale_stock.SaleOrder._cart_update` for per-seller warehouse stock never ran; replaced by `sale.order._get_free_qty`, tests check the seller warehouse limit) |
| `relational-write-needs-comodel-read` | verified | private module migration, commit `f6d9efb` (Many2many default on every partner to a marketplace-only model → users outside the marketplace groups could not create contacts/users; read granted to `base.group_everyone`, tests added) |
