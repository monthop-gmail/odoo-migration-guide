# Odoo 19 → 20 Migration Checklist

Copy this checklist into your PR description. For AI agents: use
`migration-rules.yaml` for machine-readable detect/fix patterns; full
explanations in `README.md`; proof in `evidence.md`.

## Pre-migration

- [ ] Create branch per OCA policy (`20.0-mig-<module>` on the fork, history via `git format-patch | git am -3`)
- [ ] Run the 18→19 scan once as a regression check (`transitions/18-to-19/`)
- [ ] Delete `migrations/` folder if present; no copyright-year edits

## Code changes

### Access control (biggest mechanical change)
- [ ] `security/ir.model.access.csv` → `security/ir.access.csv` (rename + `operation` column + model-name `model_id` + manifest data list)
- [ ] `ir.rule` XML records → `ir.access` records (`domain` field, group-less rows are restrictions)

### API changes
- [ ] `odoo.http` helpers: `content_disposition` → `odoo.http.stream`, `serialize_exception` → `odoo.http.dispatcher`
- [ ] `ir.config_parameter.get_param/set_param` → `get_str/set_str/...` typed variants
- [ ] `self.env.clear()` → `self.env.transaction.clear()`
- [ ] `company_registry` → `additional_identifiers` (+ `TH_BRANCH_CODE` scheme if Thai)

### Data / views
- [ ] `report_file` removed from `ir.actions.report` records and vals
- [ ] payment provider form: anchor `payment_form` group → `provider_credentials`/`provider_config*`
- [ ] website_sale template paths `views/` → `templates/`
- [ ] `_interpolation_dict` overrides keep the new `isoyear`/`isoy`/`isoweek` legends

### Tests
- [ ] Plain `TransactionCase` classes: confirm post-install timing is intended, else `@tagged('at_install', '-post_install')`
- [ ] CI/bootstrap: fresh-db installs need a two-step boot (`-i base`, then `-i <module>`)

### Ops
- [ ] Containers set `--http-interface=0.0.0.0` (or `http_interface` in odoo.conf)

## Version bump

- [ ] `__manifest__.py`: `20.0.1.0.0`
- [ ] README badges / `static/description/index.html` URLs: `19.0` → `20.0`

## Close the loop

- [ ] Central guide checked; new discoveries recorded in `migration-rules.yaml` + `evidence.md`, **or** PR states "no new migration rule discovered"
