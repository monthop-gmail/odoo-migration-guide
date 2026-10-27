# Odoo 17 → 18 Migration Guide

Breaking changes for modules moving from 17.0 to 18.0, each checked against
odoo/odoo source. Rules: [`migration-rules.yaml`](migration-rules.yaml);
proof: [evidence.md](evidence.md); PR checklist: [CHECKLIST.md](CHECKLIST.md).

> Status: seed (8 rules, 3 verified). Grown from real 15.0 ports; coming from
> 15.0/16.0, apply `15-to-16` and `16-to-17` first.

## Highlights

- **`tree` → `list`** everywhere: `<tree>` arch roots and embedded views,
  `view_mode`, xpaths, view-type strings in Python/JS.
- **Kanban:** `t-name="kanban-box"` is legacy; new archs use
  `t-name="card"`. Once converted, `kanban_image()` and `user_context` are
  no longer in the rendering context — use an image field widget or a
  `/web/image/<model>/<id>/<field>` URL.
- **`name_get` removed** → `_compute_display_name` / `display_name`.
- **`user_has_groups` removed** → `env.user.has_groups()` / `has_group()`.
- **Products:** `detailed_type` and type `'product'` are gone →
  `type='consu'` + `is_storable=True`.
- **JS RPC:** `useService("rpc")` → `import { rpc } from "@web/core/network/rpc"`.
- `check_access_rights`/`check_access_rule` deprecated → `check_access` / `has_access`.

## Checked, not changed in 18.0

- `res.users.groups_id`, `res.groups.users` — unchanged (19.0, see `18-to-19`).
- `_sql_constraints` — unchanged (19.0).
- Public `read_group` — still available.
- QWeb rendering: `env['ir.qweb']._render` — unchanged since 16.0
  (`ir.ui.view._render` disappeared in 16.0, see `15-to-16`).
