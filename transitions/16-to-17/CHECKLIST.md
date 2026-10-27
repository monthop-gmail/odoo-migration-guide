# Odoo 16 → 17 Migration Checklist

Copy into your PR description. Machine-readable rules: `migration-rules.yaml`.

## Views
- [ ] `attrs="{...}"` → `invisible=` / `readonly=` / `required=` expressions
- [ ] `states="..."` on buttons/fields → `invisible="state not in (...)"`
- [ ] list columns hidden by attrs → `column_invisible`

## Python
- [ ] field `states={...}` removed; readonly logic moved to views
- [ ] `name_get` → `_compute_display_name` (+ `@api.depends`)
- [ ] init/uninstall hooks: `(cr)` / `(cr, registry)` → `(env)`
- [ ] `from odoo.exceptions import Warning` → `UserError`
- [ ] `flush(...)` → `flush_all` / `flush_model` / `flush_recordset`
- [ ] `_read_group` / `_read_group_fill_results` overrides ported to new signature
- [ ] `ir.default.get` → `_get`
- [ ] website: `get_current_pricelist` → `_get_current_pricelist`; no `pricelist` in `_get_combination_info`

## Data / models
- [ ] `mail.channel` → `discuss.channel` (code, XML, ACL)
- [ ] `stock.move.quantity_done` / `reserved_availability` → `quantity`; no `stock.immediate.transfer`

## JavaScript
- [ ] `odoo.define(...)` files converted to ES modules
- [ ] tours: `registry.category("web_tour.tours").add(...)`
