# 16→17 evidence index

Sources of evidence (private repository, cited by commit only; no vendor code
or text is copied here):

- **Migration A**: a third-party website/marketplace module ported 15.0 → 18.0
  in one jump (January 2026), with a migration log of each failure and fix.
- **Migration B**: a third-party sale/rental module ported 15.0 → 20.0
  (October 2026), mechanical 15→17 pass in commit `9ed2ee4`.

`changed_in` of every rule was checked against odoo/odoo branches 16.0 and
17.0 (links in `migration-rules.yaml`), because Migration A jumped several
versions and its log attributes changes to "18".

| Rule | Confidence | Reproduced by |
|---|---|---|
| `view-attrs-states-removed` | source-only | A: bulk conversion in commit `5d54993` (log §3.1, ~215 attributes); B: `9ed2ee4`. Install succeeded after; the pre-fix error was not logged. |
| `field-states-param-removed` | verified | A: commit `1b48f01` — "states ... no longer supported" warnings gone after moving readonly logic to views (log Fix 2.2); B: `9ed2ee4` |
| `name-get-to-compute-display-name` | source-only | B: `9ed2ee4` |
| `init-hooks-receive-env` | source-only | B: `9ed2ee4` (pre-init check hook `(cr)` → `(env)`) |
| `exceptions-warning-removed` | source-only | A: `5d54993` (log §2.2); B: `9ed2ee4` |
| `legacy-odoo-define-removed` | verified | A: commit `11dccf9` — `UncaughtClientError: Module dependencies should be a list of strings` on 18.0, fixed by ES-module conversion (log Fix 5) |
| `flush-fnames-removed` | source-only | A: `5d54993` (log §2.1) |
| `read-group-private-api-changed` | source-only | A: `5d54993` (log §2.1: fill-results overrides dropped) |
| `ir-default-get-private` | source-only | A: `5d54993` (log §2.1); private module migration, commit `92200d6` (Migration B, 7 calls in 4 files). Migration B's log files this under "19→20"; source shows 16.0 `def get` and 17.0–20.0 `def _get` only, so the rule stays in 16→17 and no 19→20 rule is added |
| `website-current-pricelist-private` | verified | A: commit `dc5b87e` — shop pages broken, fixed by the rename |
| `combination-info-pricelist-arg-removed` | verified | A: commit `e487380` — `TypeError: ... unexpected keyword argument 'pricelist'` on every product page, fixed (log Fix 4). The log says "18"; source shows the argument was already gone in 17.0. |
| `mail-channel-renamed` | source-only | A: `5d54993` (log §4, ACL row) |
| `stock-move-quantity-done-removed` | source-only | A: `5d54993` (log §3.4/§4) |
| `sale-pricelist-optional` | verified | private module migration, commit `c12fea0` (invoice creation: `NotNullViolation` on currency when the order had no pricelist; fixed by reading `sale.order.currency_id`) and commit `b7b4b10` (wizard currency related to the order). The log says "19→20"; source shows `required=True` dropped in 17.0 |

Detect hit counts against the two original 15.0 sources are in the PR
description of `transitions/15-to-17`.
