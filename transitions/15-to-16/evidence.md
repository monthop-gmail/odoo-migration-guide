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
| `decimal-precision-addon-import` | source-only | B: original 15.0 source imports the 12.0-era addon in 4 files; the addon is absent on 13.0+ |
