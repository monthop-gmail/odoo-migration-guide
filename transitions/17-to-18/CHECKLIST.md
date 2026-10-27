# Odoo 17 → 18 Migration Checklist

Copy into your PR description. Machine-readable rules: `migration-rules.yaml`.
Coming from 15.0/16.0? Run the `15-to-16` and `16-to-17` scans first.

## Views
- [ ] `<tree>` → `<list>`; `view_mode` `tree` → `list`; xpath `//tree` → `//list`
- [ ] kanban `t-name="kanban-box"` → `t-name="card"`
- [ ] no `kanban_image()` / `user_context` in card templates

## Python
- [ ] `name_get` overrides/calls removed
- [ ] `user_has_groups` → `env.user.has_groups` / `has_group`
- [ ] `detailed_type` / type `'product'` → `type='consu'` + `is_storable`
- [ ] `check_access_rights` / `check_access_rule` → `check_access` / `has_access`

## Data
- [ ] `ir.cron` records: no `numbercall` / `doall`

## JavaScript
- [ ] `useService("rpc")` / `jsonRpc` → `rpc` from `@web/core/network/rpc`
- [ ] smoke-test website pages and kanban views in the browser (JS errors
      only show at runtime)
