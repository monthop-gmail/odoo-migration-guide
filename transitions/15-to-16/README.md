# Odoo 15 → 16 Migration Guide

Breaking changes for modules moving from 15.0 to 16.0, each checked against
odoo/odoo source. Rules: [`migration-rules.yaml`](migration-rules.yaml);
proof: [evidence.md](evidence.md); PR checklist: [CHECKLIST.md](CHECKLIST.md).

> Status: seed (14 rules, 8 verified). Grown from two real 15.0 ports.

## Highlights

- **Bootstrap 4 → 5.** `data-toggle/target/dismiss/...` → `data-bs-*`
  (otherwise tabs/modals silently stop working), `close` → `btn-close`,
  `ml/mr/pl/pr-*` → `ms/me/ps/pe-*`, `*-left/right` → `*-start/end`, etc.
- **QWeb rendering:** `ir.ui.view._render()` is gone → `env['ir.qweb']._render(xmlid, values)`.
- **Views API:** `fields_view_get` → `_get_view` / `get_views`; an inheriting
  view record may not carry `groups_id` (use `groups=` inside the arch).
- **Models removed/renamed:** `ir.translation` (translations are jsonb on the
  record), `stock.location.route` → `stock.route`.
- product search views: the `pricelist_id` helper field on `product.template` is gone.
- `odoo.addons.web.controllers.main` is a deprecated shim → import from
  `home`, `utils`, ... .
- Pre-15 leftover worth scanning for: `odoo.addons.decimal_precision`
  (gone since 13.0) → `digits='<precision name>'`.

## Checked, not changed in 16.0

- `odoo.exceptions.Warning` still exists (deprecated) — removed in 17.0.
- Legacy `odoo.define(..., function (require))` still loads — removed in 17.0.
- Install hooks still get `cr` — `env` from 17.0.
- `attrs`/`states` in views still valid — removed in 17.0.
- Manifest `assets` key already exists since 15.0; nothing to change.
