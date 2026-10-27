# 19→20 evidence index

Each rule in `migration-rules.yaml` / `README.md` was reproduced on the
released **20.0.20260926** build (sha1-verified official deb) during the
ThaiACC Odoo 20 work and the OCA-bridge migrations below.

| Rule | Reproduced by |
|---|---|
| `acl-csv-renamed` + `ir-rule-xml-removed` | OCA/server-ux `date_range` migration: `KeyError: 'ir.model.access'`, then CSV/XML conversion — branch `20.0-mig-date_range`, commit `1c4a46b` + rule conversion commit; 27 tests passed |; private module migration, commits `805680d` (CSV + 46 `ir.rule` → `security/ir.access.csv`) and `7b28956` (xml-id `model_ir_config_parameter` had been converted to `ir.config.parameter` by turning every `_` into `.`; the model is `ir.config_parameter`. Rows for models removed in 20.0 dropped)
| `odoo-http-package-split` | OCA/reporting-engine `report_xlsx` / `report_xlsx_helper` migrations: `ImportError: cannot import name 'content_disposition'` — branches `20.0-mig-report_xlsx` (commit `[FIX] report_xlsx: adapt controller imports`) and `20.0-mig-report_xlsx_helper` |
| `report-file-field-removed` | `report_xlsx` demo + tests: `ValueError: Invalid field 'report_file' in 'ir.actions.report'` — fixed in same branch; 6+3 tests passed after |
| `config-parameter-typed-getters` | OCA partner-contact `partner_firstname`: `AttributeError: 'ir.config_parameter' object has no attribute 'get_param'` in post-init hook — branch `20.0-mig-partner_firstname` |
| `test-tags-default-post-install` | `report_xlsx` plain `TransactionCase` collected 0 tests at install; `odoo/tests/common.py` `BaseCase.__init_subclass__` documents the `{'standard','post_install'}` default |
| `virgin-db-init-names-ignored` | Fresh-db install runs logged `WARNING invalid module names, ignored`; fixed by two-step boot in `thaiacc-odoo` `test/official_smoke_test.sh` |
| `company-registry-removed` | `grep -r company_registry` on odoo 20.0 source matches only test-file names; `odoo/tools/partner_identifiers.py` defines `TH_BRANCH_CODE`/`TH_VAT` |
| `http-interface-default-localhost` | `odoo/tools/config.py` `--http-interface my_default='127.0.0.1'` (19.0: `'0.0.0.0'`) |
| Official baseline capabilities | `thaiacc-odoo` branch `20.0` commits `b49ad6b`, `68047a8`: official l10n_th suite 15/15 + capability checks 9/9 (`test/official_smoke_test.sh`) |
| `caba-company-flag` | Shell probe on 20.0.20260926: on_payment purchase tax + payment reconcile produced 0 CABA entries with `company.tax_exigibility` off; `addons/account/models/account_move_line.py` `is_cash_basis_needed()` gates `_create_tax_cash_basis_moves()`; Thai chart enables it (`l10n_th/models/template_th.py` `_get_th_res_company`)
| `is-company-stored-compute` | ThaiACC `l10n_th_pnd_report` test: partner created with `is_company=True` classified as individual; `odoo/addons/base/models/res_partner.py` `_compute_is_company` (commercial_partner_id == partner AND has_vat)
| `env-clear-deprecated` | `l10n_th_base_sequence` test: `DeprecationWarning: Since 20.0, use transaction.clear or transaction.reset` |
| `t-esc-removed` | verified — private module migration, commit `b750cdc` (kanban arch: `Forbidden owl directive used in arch`, install passed after `t-esc` → `t-out`) and commit `4c56370` (second module, same error). A log attributed this to 17→18; source shows `t-esc` still allowed in 17.0/18.0/19.0 `_validate_qweb_directive` and removed in 20.0 |
| `stock-scrap-return-wizard-removed` | verified — private module migration, commits `06f5250` + `ea6600c` (return flow via `action_return()` / `action_return_all()`, test asserts return moves stay linked) and commit `7b28956` (ACL rows for `stock.scrap` / `stock.return.picking(.line)` dropped, install passed) |
| `default-website-xmlid` | verified — private module migration, commit `439ce98` (`External ID not found` → `base.default_website`) |
| `toggle-active-removed` | verified — private module migration, commit `40b70b3` (archive button in views failed install; replaced by `action_archive` / `action_unarchive`) |
| `partner-company-name-removed` | verified — private module migration, commit `53bd2b1` (partner form inherit referenced the removed field / button; removed) |
| `stock-move-uom-renamed` | verified — private module migration, commit `461968e` (stock.move `product_uom` → `uom_id`, install passed after) and commit `a847cf8` (second module). A log attributed this to 18→19; source shows `product_uom` still on 19.0 `stock.move` |
| `pricelist-compute-price-renamed` | verified — private module migration, commit `89b0da0` (`percentage` + `percent_price` → `discount` + `price_discount`) |
| `product-tracking-none-removed` | verified — public module `monthop-gmail/odoo-addons` `spare_parts_hub` (PR #2, branch `20.0-mig-sph`), commit `7b6b6ab` (fixture create with `tracking: 'none'` failed install; reservation guard `tracking != 'none'` refused every product) |
| `purchase-prepare-picking-removed` | verified — public module `monthop-gmail/odoo-addons` `spare_parts_hub` (PR #2, branch `20.0-mig-sph`), commit `7b6b6ab` (PO receipts lost their center: override never called; moved to `stock.move._get_new_picking_values`, test `test_receipt_rule_explicit_membership`) |
| `public-read-group-new-signature` | verified — public module `monthop-gmail/odoo-addons` `spare_parts_hub` (PR #2, branch `20.0-mig-sph`), commit `a82359a` (portal RPC `read_group(domain, fields, groupby)` → "Aggregate method is mandatory"; `formatted_read_group`) |
| `registry-clear-cache-removed` | verified — public module `monthop-gmail/odoo-addons` `spare_parts_hub` (PR #2, branch `20.0-mig-sph`), commit `7b6b6ab` (`AttributeError: 'Registry' object has no attribute 'clear_cache'` at install; `env.transaction.invalidate_ormcache()` before commit) |
| `ir-rule-compute-domain-removed` | verified — public module `monthop-gmail/odoo-addons` `spare_parts_hub` (PR #2, branch `20.0-mig-sph`), commit `3902973` (incident reference on POs used `ir.rule._compute_domain`; restriction rows of `ir.access`) |
| `group-user-regular-implied` | verified — public module `monthop-gmail/odoo-addons` `spare_parts_hub` (PR #2, branch `20.0-mig-sph`), commit `7b6b6ab` (two "grant implies nothing" tests saw `base.group_user_regular`) |
| `odoo-http-package-split` (`root`) | verified — public module `monthop-gmail/odoo-addons` `spare_parts_hub` (PR #2, branch `20.0-mig-sph`), commit `386351a` (`ImportError: cannot import name 'root' from 'odoo.http'` in every live-harness worker; `session_store()`) |
| `stock-scrap-return-wizard-removed` (scrap authority) | verified — public module `monthop-gmail/odoo-addons` `spare_parts_hub` (PR #2, branch `20.0-mig-sph`), commit `3902973` (authority override moved from `stock.scrap.action_validate/do_scrap` to `stock.move.action_scrap/_action_scrap/write(state=done)`; scrap moves start confirmed/assigned, not draft) |

## Cross-references

- ThaiACC-specific (accounting domain) findings and architecture decisions
  live in `monthop-gmail/thaiacc-odoo` → `MIGRATION-20.0.md` — linked, not
  duplicated here.
- Coordination thread: ai-collab ws-001 discussion
  `dis-b93b0f5e-167a-469f-9d9f-0d59053b7499` (this repo's transition) and
  `dis-ea38366d-b887-4bf8-9fca-9034355e42cd` (ThaiACC Odoo 20).
