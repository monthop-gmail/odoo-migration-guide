# 15→16 evidence index

Evidence comes from two private third-party module ports, cited by commit
only (no vendor code or text copied): **Migration A** (website/marketplace
module, 15.0 → 18.0 in one jump, January 2026, with a failure-by-failure log)
and **Migration B** (sale/rental module, 15.0 → 20.0, October 2026). Because
A jumped three versions and its log labels many changes "18", the
`changed_in` of every rule was re-checked on odoo/odoo branches 15.0/16.0.

| Rule | Confidence | Reproduced by |
|---|---|---|
| `view-render-moved-to-qweb` | verified | A: commit `1c72684` — `AttributeError: 'ir.ui.view' object has no attribute '_render'` on website pages, fixed with `ir.qweb._render` (log Fix 8). The log says "18"; source shows the method was already gone in 16.0. |
| `fields-view-get-removed` | source-only | 15.0 vs 16.0 `odoo/models.py`; A's 15.0 source has overrides (see hit table) |
| `inherited-view-groups-id-forbidden` | source-only | A: `5d54993` (log §3.3, groups_id removed from 7 inheriting view files) |
| `ir-translation-removed` | source-only | A: `5d54993` (log §4, ACL row dropped) |
| `stock-location-route-renamed` | source-only | A: `5d54993` (log §4, ACL row dropped) |
| `bootstrap5-data-attributes` | verified | A: commit `a27d41b` — tabs/modals/dropdowns did nothing until `data-bs-*` (log Fix 10) |
| `bootstrap5-close-button` | verified | A: commit `a27d41b` (log Fix 10) |
| `bootstrap5-renamed-utilities` | source-only | Bootstrap 4.3.1 (15.0) → 5.1.3 (16.0) bundles; Bootstrap 5 migration guide |
| `web-controllers-main-split` | source-only | A: `5d54993` (log §2.2, ensure_db import) |
| `decimal-precision-addon-import` | source-only | private module migration, commit `cab3ce1` (Migration B: import dropped in 4 files; fields already used `digits='<name>'`); `addons/decimal_precision` exists on 12.0 and is absent from 13.0 on, so the break predates 15.0 |
| `product-template-pricelist-search-field-removed` | source-only | private module migration, commit `304e7e0` (Migration B, product search view); A: `5d54993` (log §3.5, removed from search filters); 15.0 vs 16.0 `product_template.py` |
| `account-move-get-default-journal-removed` | verified | private module migration, commits `1510162` + `ea6600c` (`_get_default_journal` gone and the context default no longer drives `_search_default_journal`; sales journal now taken from a draft `new({'move_type': 'out_invoice'})` move). The log says "19→20"; source shows `_get_default_journal` only in 15.0 |
| `account-move-line-display-type-required` | verified | private module migration, commit `1510162` (deposit invoice line copied a False `display_type`; now defaults to `'product'`). The log says "19→20"; 16.0 already has `required=True` |
| `payment-acquirer-renamed-provider` | verified | private module migration, commit `ea661b6` (website controller still called `payment.acquirer`; switched to `payment.provider`) |
| `fields-view-get-removed` | verified — private module migration, commit `41d1697`: four `fields_view_get` overrides (seller product domain, picking/partner toolbars, seller bill labels) had silently stopped running since 16.0; ported to `get_view` / `get_views`, tests check each per user. The seller domain uses the seller's partner id, so it had to go in `get_view` (the `_get_view` cache key has no user) |
| `inherited-view-groups-id-forbidden` | verified — private module migration, commit `72df550` (`group_ids` on two inheriting search views → install failed with "Inherited view cannot have 'groups' defined on the record"; moved to `groups=` on the filter) |
