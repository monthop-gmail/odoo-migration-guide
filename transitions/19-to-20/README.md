# Odoo 19 → 20 Migration Guide

Verified breaking changes and patterns for migrating Odoo modules from 19.0
to 20.0. Every item here was reproduced against the released
`20.0.20260926` build — see [evidence.md](evidence.md).

> Status: seed. Additional rules are added as real migrations surface them
> (see the evidence-growth rule in the repo README).

## 1. Access control rewritten: `ir.model.access` and `ir.rule` → `ir.access`

The biggest mechanical change of 20.0. Both models were removed and replaced
by a single `ir.access` model ("Access control records with domains"):

- `kind = 'permission'` when a `group_id` is set (former ACL semantics);
- `kind = 'restriction'` when **no group** is set (former global record-rule
  semantics).
- Rules are no longer separate records; a restriction carries its domain in
  the `domain` field (the old field name was `domain_force`).

**CSV files:** the loader infers the model from the *filename*
(`model = filename.split('-')[0]`), so:

- `security/ir.model.access.csv` → `security/ir.access.csv`
  (the old name resolves to the now-nonexistent `ir.model.access` and fails
  with `KeyError: 'ir.model.access'` at install);
- the four boolean columns (`perm_read`, `perm_write`, `perm_create`,
  `perm_unlink`) merge into one `operation` column holding a subset of
  `crud` in c-r-u-d order (e.g. `r`, `ru`, `crud`);
- `model_id` must reference the **model name** (`date.range`), not the
  `ir.model` xml-id (`model_date_range` resolves to NULL);
- `group_id:id` and `group_id/id` both still parse; prefer `/id`.

**XML rules:** convert `<record model="ir.rule">` to
`<record model="ir.access">` with `model_id`, `operation` (`crud` for
former global rules) and `domain` (the old `domain_force` expression).

An automated converter for the CSV part: replace each row
`(id,name,model_id:id,group_id:id,r,w,c,u)` with
`(id,name,<model name>,group_id/id,<op>,)`. Model names are collected from
the module's `_name` definitions.

## 2. `odoo.http` is now a package

`odoo/http.py` became `odoo/http/`. Helpers moved; core imports show the way:

```python
# 19.0
from odoo.http import content_disposition, request, route
from odoo.http import serialize_exception as _serialize_exception
# 20.0
from odoo.http import request, route
from odoo.http.dispatcher import serialize_exception as _serialize_exception
from odoo.http.stream import content_disposition
```

`request` and `route` are still re-exported from `odoo.http`.

## 3. `ir.actions.report.report_file` removed

The `report_file` field no longer exists on `ir.actions.report`; `report_name`
alone identifies the report template. Remove `report_file` from demo/data XML
and from any `create()` vals (tests included) — otherwise `ValueError:
Invalid field 'report_file' in 'ir.actions.report'`.

## 4. `ir.config_parameter` typed getters/setters

`get_param`/`set_param` are gone. Use the typed API:

```python
# 19.0
param = self.env["ir.config_parameter"].sudo().get_param("key", default)
self.env["ir.config_parameter"].sudo().set_param("key", value)
# 20.0
param = self.env["ir.config_parameter"].sudo().get_str("key", default)
self.env["ir.config_parameter"].sudo().set_str("key", value)
```

Typed variants: `get_str/get_bool/get_int/get_float`, `set_str/set_bool/
set_int/set_float`.

## 5. Test framework defaults flipped to `post_install`

Base test classes now default to `test_tags = {'standard', 'post_install'}` —
previously `{'standard', 'at_install'}`. A plain `TransactionCase` therefore
no longer runs during module installation; it runs in the post-install
phase. Classes that must run at install need an explicit
`@tagged('at_install', '-post_install')`.

Also new: on a **virgin database**, `-i <module>` names are validated against
the `ir_module_module` table during the same boot that only knows `base` —
the names are dropped with `WARNING invalid module names, ignored`. Boot in
two steps on fresh databases: first `-i base --stop-after-init`, then
`-i <module> --test-enable ...`.

## 6. `res.partner.company_registry` removed

The `company_registry` field (and its `res.company` related) is gone. Partner
company identifiers moved to `res.partner.additional_identifiers` (Json) with
metadata in `odoo/tools/partner_identifiers.py`. Core 20.0 ships
`TH_BRANCH_CODE` (5-digit Revenue Department branch code,
`th_branch_code_validate`) and `TH_VAT` schemes. Modules reading
`company_id.company_registry` must switch sources.

## 7. `env.clear()` deprecated

`self.env.clear()` → `self.env.transaction.clear()` (clears caches, pending
recomputations and cached properties — the same semantics). `env.transaction.reset()`
exists for post-commit/rollback registry changes only.

## 8. Cash-basis (CABA) is gated by a company flag

Cash-basis journal entries for `on_payment` taxes are only generated when the
**company** flag `res.company.tax_exigibility` ("Cash Basis" in settings) is
enabled — `account.move.line`'s reconciliation code checks
`any(amls.company_id.mapped('tax_exigibility'))` before calling
`_create_tax_cash_basis_moves()`. Charts enable it during application (the
Thai chart sets `tax_exigibility: True` in `_get_th_res_company`); a module
testing or depending on cash-basis behavior on a company whose chart did not
enable it will see **no CABA entries and no downstream side effects** (e.g.
payment tax invoices), with no error raised. In tests, apply the country chart
(`AccountTestInvoicingCommon.setup_country('th')`) or set the flag explicitly.

## 9. Other verified changes

- **`--http-interface` default changed** `0.0.0.0` → `127.0.0.1`: containers
  running `odoo` directly stop accepting external connections unless the
  interface is set explicitly (CLI flag or `http_interface` in odoo.conf).
- **Translated fields are jsonb columns** (`account.tax.name` is
  `{"en_US": "..."}`) — mind SQL that concatenates/casts translated fields.
- **`payment.provider` form**: `<group name="payment_form">` removed; anchor
  to `provider_credentials`/`provider_config*` on the Configuration page.
- **website_sale templates moved** `views/*.xml` → `templates/**`
  (e.g. `templates/checkout/confirmation_templates.xml`); xml ids unchanged,
  old xpaths may need file-level awareness only.
- **`ir.sequence`**: `_get_prefix_suffix(date, date_range)` signature
  unchanged; core adds `isoyear`/`isoy`/`isoweek` interpolation legends
  (modules that replace the interpolation dict must carry them); core adds a
  `not seq.id` guard in `_get_number_next_actual`.
- **res.groups privilege model** and the other framework changes listed in
  the OCA 19.0 wiki are unchanged from 19.0 — modules already on 19.0 skip
  them (see `transitions/18-to-19/`).

## 9. Still true from 19.0 (verify-only)

Domain objects, `_read_group`, `models.Constraint`, `auto_join` →
`bypass_search_access`, `groups_id` → `group_ids`, `type="jsonrpc"` routes,
`@api.returns` removal — all covered in `transitions/18-to-19/`. Run the
18→19 scan once as a regression check before starting 19→20 work.
