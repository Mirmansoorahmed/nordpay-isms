# Tasks — nordpay-isms

Small tasks I can review one at a time. Each lists what it delivers and which
requirements it satisfies. No code is written until I approve this spec. Tasks are
roughly in dependency order; each should be its own small commit/PR where sensible.

## Phase 0 — repo setup

- [ ] **T0.1 Project skeleton.** Add `pyproject.toml` (deps: pyyaml, jsonschema,
  openpyxl, fpdf2, pytest; console-script `isms`), `.gitignore` (ignore `out/`,
  `.kiro/`, venvs), and empty `src/isms/` + `tests/` package layout.
  _Satisfies: NFR-6, NFR-7, R-CLI.1._
- [ ] **T0.2 Import SoA.** Copy `soa.json` from `iso27001-evidence-as-code` into
  `data/soa.json` and note the source commit in `data/README.md`.
  _Satisfies: R-SOA.1._

## Phase 1 — context and scope (cl. 4)

- [ ] **T1.1 Context YAML.** Write `context/context.yaml`: scope statement, internal
  and external issues, interested parties with needs + owner, interfaces (including
  the companion repo and Azure). _Satisfies: R-4.1, R-4.2._
- [ ] **T1.2 Context schema.** Add `schemas/context.schema.json` and the
  interested-party completeness cross-check. _Satisfies: R-4.3._

## Phase 2 — risk method and register (cl. 6.1.2)

- [ ] **T2.1 Risk method YAML.** Write `risk/method.yaml`: 5x5 likelihood/impact
  bands, acceptance bands with accept-role, covering scores 1..25.
  _Satisfies: R-6.1, R-6.2, R-6.3._
- [ ] **T2.2 Risk method schema + band-coverage check.** Add
  `schemas/method.schema.json` and the 1..25 no-gap/no-overlap cross-check.
  _Satisfies: R-6.1, R-6.2._
- [ ] **T2.3 Risk register YAML.** Write `risk/register.yaml` with ~20 realistic
  payment-fintech risks, each with inherent/residual likelihood+impact, treatment,
  Annex A IDs, and DORA articles where relevant. _Satisfies: R-RR.1, R-RR.2, R-RR.3._
- [ ] **T2.4 Risk register schema.** Add `schemas/register.schema.json`.
  _Satisfies: R-RR.4._

## Phase 3 — CLI core: loader + validation

- [ ] **T3.1 Loader.** `src/isms/loader.py` to read YAML/JSON safely.
  _Satisfies: R-CLI.2._
- [ ] **T3.2 Schema validation.** `src/isms/validate.py` validates every YAML against
  its schema. _Satisfies: R-CLI.2, R-RR.4._
- [ ] **T3.3 Cross-checks.** Add Annex-A-resolves-against-SoA (R-RR.5), residual >
  acceptance needs decision (R-RR.6), interested-party completeness (R-4.3), and
  acceptance-band coverage checks to `validate.py`.
  _Satisfies: R-RR.5, R-RR.6, R-4.3._
- [ ] **T3.4 `isms validate` command + build guard.** Wire `cli.py` with a `validate`
  subcommand/flag; `build` runs validate first and writes nothing on failure.
  _Satisfies: R-CLI.3, R-CLI.7._

## Phase 4 — SoA import and treatment plan

- [ ] **T4.1 SoA import module.** `src/isms/soa.py`: load SoA, report
  automated-vs-manual counts, never upgrade status locally.
  _Satisfies: R-SOA.3, R-SOA.4._
- [ ] **T4.2 Risk treatment plan.** Generate a treatment plan (Markdown/data) that
  for each treated risk lists its Annex A controls and their imported status.
  _Satisfies: R-SOA.2._

## Phase 5 — policies (cl. 5 operational)

- [ ] **T5.1 Five policies.** Write `policies/` — info security, acceptable use,
  access control, supplier security, incident management — EN + DE summary, each with
  owner/version/cadence/Annex A IDs. _Satisfies: R-POL.1, R-POL.2, R-POL.3._
- [ ] **T5.2 Policy index.** `policies/index.yaml` with machine-readable metadata for
  the policy-currency KPI. _Satisfies: R-KPI.1 (policy currency)._

## Phase 6 — internal audit (cl. 9.2)

- [ ] **T6.1 Audit programme.** `audit/programme.md` mapping audit areas to clauses
  and Annex A themes over a rolling period. _Satisfies: R-AUD.1._
- [ ] **T6.2 Sample audit report + findings data.** `audit/report-sample.md` with
  classified findings and CAPA; `audit/findings.yaml` for KPI maths.
  _Satisfies: R-AUD.2._

## Phase 7 — management review and KPIs (cl. 9.3)

- [ ] **T7.1 Management review template + worked quarter.** `review/template.md` and
  `review/2026-Q4-sample.md` using only repo-derived values.
  _Satisfies: R-MR.1, R-MR.2._
- [ ] **T7.2 KPI module.** `src/isms/kpis.py` computing the six KPIs, with the
  "not measured" path when inputs are missing. _Satisfies: R-KPI.1, R-KPI.2, NFR-5._
- [ ] **T7.3 Heat map module.** `src/isms/heatmap.py` returning the 5x5 residual-risk
  grid. _Satisfies: R-CLI.4._

## Phase 8 — export

- [ ] **T8.1 Excel export.** `src/isms/export_excel.py` -> `out/isms.xlsx` with
  Risk register, SoA, KPIs sheets (+ heat map block). _Satisfies: R-CLI.5, NFR-6._
- [ ] **T8.2 PDF handbook.** `src/isms/export_pdf.py` -> `out/isms-handbook.pdf`
  summarising scope, risk posture, SoA status, KPIs; prints "not measured" where
  applicable. _Satisfies: R-CLI.6, NFR-4, NFR-5._
- [ ] **T8.3 Wire `isms build` end to end.** validate -> import -> compute -> export.
  _Satisfies: R-CLI.4, R-CLI.5, R-CLI.6._

## Phase 9 — tests and CI

- [ ] **T9.1 Validator tests.** `tests/test_validate.py`: valid + invalid fixture per
  schema, asserting expected failures. _Satisfies: R-TEST.1._
- [ ] **T9.2 KPI + heat map tests.** `tests/test_kpis.py`, `tests/test_heatmap.py`
  with fixed inputs and exact expected values, including "not measured".
  _Satisfies: R-TEST.2._
- [ ] **T9.3 CI workflow.** `.github/workflows/ci.yml`: on PR, validate + pytest +
  build, upload `out/` artefact, never commit it. _Satisfies: R-CI.1, R-CI.2, R-CI.3._

## Phase 10 — docs

- [ ] **T10.1 README.** Explain the repo, the 15-minute tour, how to run `isms build`
  offline, the link to the companion repo, and the "not measured / no certification"
  honesty notes. Every number traces to a generated file. _Satisfies: NFR-4, NFR-5._

## Review checkpoints

- After **Phase 2**: review the risk method and register content before any code
  consumes it.
- After **Phase 3**: review validation behaviour on a deliberately broken file.
- After **Phase 8**: review the generated `out/` artefacts end to end.
