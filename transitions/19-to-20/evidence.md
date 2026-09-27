# 19→20 evidence index

Each rule in `migration-rules.yaml` / `README.md` was reproduced on the
released **20.0.20260926** build (sha1-verified official deb) during the
ThaiACC Odoo 20 work and the OCA-bridge migrations below.

| Rule | Reproduced by |
|---|---|
| `acl-csv-renamed` + `ir-rule-xml-removed` | OCA/server-ux `date_range` migration: `KeyError: 'ir.model.access'`, then CSV/XML conversion — branch `20.0-mig-date_range`, commit `1c4a46b` + rule conversion commit; 27 tests passed |
| `odoo-http-package-split` | OCA/reporting-engine `report_xlsx` / `report_xlsx_helper` migrations: `ImportError: cannot import name 'content_disposition'` — branches `20.0-mig-report_xlsx` (commit `[FIX] report_xlsx: adapt controller imports`) and `20.0-mig-report_xlsx_helper` |
| `report-file-field-removed` | `report_xlsx` demo + tests: `ValueError: Invalid field 'report_file' in 'ir.actions.report'` — fixed in same branch; 6+3 tests passed after |
| `config-parameter-typed-getters` | OCA partner-contact `partner_firstname`: `AttributeError: 'ir.config_parameter' object has no attribute 'get_param'` in post-init hook — branch `20.0-mig-partner_firstname` |
| `test-tags-default-post-install` | `report_xlsx` plain `TransactionCase` collected 0 tests at install; `odoo/tests/common.py` `BaseCase.__init_subclass__` documents the `{'standard','post_install'}` default |
| `virgin-db-init-names-ignored` | Fresh-db install runs logged `WARNING invalid module names, ignored`; fixed by two-step boot in `thaiacc-odoo` `test/official_smoke_test.sh` |
| `company-registry-removed` | `grep -r company_registry` on odoo 20.0 source matches only test-file names; `odoo/tools/partner_identifiers.py` defines `TH_BRANCH_CODE`/`TH_VAT` |
| `http-interface-default-localhost` | `odoo/tools/config.py` `--http-interface my_default='127.0.0.1'` (19.0: `'0.0.0.0'`) |
| Official baseline capabilities | `thaiacc-odoo` branch `20.0` commits `b49ad6b`, `68047a8`: official l10n_th suite 15/15 + capability checks 9/9 (`test/official_smoke_test.sh`) |
| `env-clear-deprecated` | `l10n_th_base_sequence` test: `DeprecationWarning: Since 20.0, use transaction.clear or transaction.reset` |

## Cross-references

- ThaiACC-specific (accounting domain) findings and architecture decisions
  live in `monthop-gmail/thaiacc-odoo` → `MIGRATION-20.0.md` — linked, not
  duplicated here.
- Coordination thread: ai-collab ws-001 discussion
  `dis-b93b0f5e-167a-469f-9d9f-0d59053b7499` (this repo's transition) and
  `dis-ea38366d-b887-4bf8-9fca-9034355e42cd` (ThaiACC Odoo 20).
