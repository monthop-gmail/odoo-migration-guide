# Odoo 16 → 17 Migration Guide

Breaking changes for modules moving from 16.0 to 17.0, each checked against
odoo/odoo source. Rules: [`migration-rules.yaml`](migration-rules.yaml);
proof: [evidence.md](evidence.md); PR checklist: [CHECKLIST.md](CHECKLIST.md).

> Status: seed (14 rules, 5 verified). Grown from two real 15.0 ports.

## The big three

1. **`attrs` / `states` in views are gone.** Every `attrs="{'invisible': [...]}"`
   and `states="a,b"` must become a Python expression on the node:
   `invisible="not partner_id"`, `readonly="state != 'draft'"`,
   `invisible="state not in ('a', 'b')"`. List columns use
   `column_invisible`. The Python field parameter `states={...}` is also
   ignored now — move its readonly logic into the views.
2. **`name_get` is no longer used for `display_name`.** Override
   `_compute_display_name` (with `@api.depends`). `name_get` is removed in 18.0.
3. **Install hooks take `env`.** `pre_init_hook`, `post_init_hook` and
   `uninstall_hook` are called with `env` only (16.0: `cr` / `cr, registry`).

## Also breaking in 17.0

- `odoo.exceptions.Warning` removed (deprecated alias of `UserError` until 16.0).
- Legacy JS loader: `odoo.define('x', function (require) {...})` no longer
  loads; use ES modules (`/** @odoo-module **/` + `import`).
- `flush(fnames)` removed → `env.flush_all()` / `flush_model()` / `flush_recordset()`.
- `_read_group` / `_read_group_fill_results` new signatures (public
  `read_group` still exists through 18.0).
- `ir.default.get` → `_get`; website `get_current_pricelist` →
  `_get_current_pricelist`; `_get_combination_info` lost `pricelist`.
- `mail.channel` → `discuss.channel`; `stock.move.quantity_done` /
  `reserved_availability` → `quantity`; `stock.immediate.transfer` removed.

## Checked, not changed in 17.0

- `_sql_constraints` still works (changes in 19.0 — see `18-to-19`).
- `res.groups.users` / `res.users.groups_id` unchanged (renamed in 19.0).
