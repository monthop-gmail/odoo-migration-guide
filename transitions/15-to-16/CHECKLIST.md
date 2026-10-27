# Odoo 15 → 16 Migration Checklist

Copy into your PR description. Machine-readable rules: `migration-rules.yaml`.

## Python
- [ ] `ir.ui.view` `._render(...)` → `env['ir.qweb']._render(xmlid, values)`
- [ ] `fields_view_get` overrides → `_get_view` / `get_views`
- [ ] `from odoo.addons.web.controllers.main import ...` → new submodules
- [ ] no `odoo.addons.decimal_precision` import; `digits='<name>'`

## Data / security
- [ ] no `ir.translation` usage / ACL rows
- [ ] `stock.location.route` → `stock.route`
- [ ] inheriting view records: `groups_id` → `groups=` in the arch

## Website / frontend (Bootstrap 5)
- [ ] `data-*` → `data-bs-*` for toggle/target/dismiss/ride/slide/parent/spy
- [ ] `class="close"` → `class="btn-close"`
- [ ] directional utilities and removed components (see rule `bootstrap5-renamed-utilities`)
- [ ] test tabs, modals, dropdowns and carousels by hand
