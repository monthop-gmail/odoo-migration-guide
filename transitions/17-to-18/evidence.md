# 17→18 evidence index

Evidence comes from two private third-party module ports, cited by commit
only (no vendor code or text copied): **Migration A** (website/marketplace
module, 15.0 → 18.0 in one jump, January 2026, verified on 18.0-20250401 and
18.0-20260119 builds) and **Migration B** (sale/rental module, 15.0 → 20.0,
October 2026). `changed_in` was re-checked on odoo/odoo branches 17.0/18.0.

| Rule | Confidence | Reproduced by |
|---|---|---|
| `tree-view-renamed-list` | source-only | A: bulk conversion in commit `5d54993` (log §3.2: tags, view_mode, xpath); install succeeded after, pre-fix error not logged |
| `kanban-box-to-card` | verified | A: commit `1b48f01` — kanban-box deprecation warnings on 18.0-20260119 gone after 13 templates moved to `card` (log Fix 2.3) |
| `kanban-image-legacy-only` | verified | A: commit `cade613` — `TypeError: ctx.kanban_image is not a function` right after the card conversion, fixed with `/web/image/...` URLs (log Fix 3) |
| `name-get-removed` | source-only | 18.0 `odoo/models.py`; B: `9ed2ee4` (converted while passing 17) |
| `user-has-groups-removed` | source-only | A: `5d54993` (log §2.1) |
| `product-detailed-type-removed` | source-only | A: `5d54993` (log §2.1: detailed_type introduced then reverted to type for 18) |
| `check-access-rights-deprecated` | source-only | 18.0 `odoo/models.py` warning; A's log (Fix 5 notes) lists these warnings as remaining |
| `ir-cron-numbercall-doall-removed` | source-only | private module migration, commit `bd16a1c` (Migration B, 17→18 data pass); 17.0 vs 18.0 `ir_cron.py` |
| `rpc-service-to-function` | verified | A: commit `11dccf9` — website JS failed to load on 18.0; RPC calls moved to `rpc` from `@web/core/network/rpc` with the ES-module conversion (log Fix 5) |
| `http-routing-slug-moved` | verified | private module migration, commit `ea661b6` (website controllers/models imported `slug` from `http_routing.models.ir_http`, which 18.0 no longer defines; replaced by `ir.http._slug`) |

## Corrections to the old placeholder

The previous README listed `groups_id` → `group_ids` and `read_group` →
`_read_group` as 18.0 changes. Source check: `res.users.groups_id` and
`res.groups.users` still exist in 18.0 (renamed in 19.0, covered by
`18-to-19`); `read_group` is still public in 18.0 (its private `_read_group`
API changed in 17.0, see `16-to-17`).
