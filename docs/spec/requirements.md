# Requirements — nordpay-isms

## 1. Purpose and context

This is a personal portfolio project. NordPay GmbH is a **fictional** 60-person
payment fintech in Berlin, running on Azure, supervised by BaFin and in scope for
DORA. Nothing here is real company data, a live system, or a certification claim.

The repo is the organisational companion to my technical evidence repo
`iso27001-evidence-as-code`, which already automates evidence for 15 of the 93
Annex A controls. That repo answers "do the controls work in the cloud?". This repo
answers "is there an ISMS running around them?" — ISO/IEC 27001:2022 clauses 4-10.

### Goal

A hiring manager for an ISB / ISMS / GRC Engineer role in Germany can see, in about
15 minutes, that I can set up and operate an ISO 27001:2022 ISMS (clauses 4-10):
define context and scope, run a risk method and register, treat risk, write policies,
run an internal audit, hold a management review, and compute KPIs — and tie all of
that back to automated technical evidence without claiming anything is "passing"
unless there is evidence for it.

### Terminology (EARS)

Requirements use EARS phrasing. Keywords:
- **WHEN** — event-driven trigger.
- **WHILE** — state the system is in.
- **IF ... THEN** — condition and response.
- **THE SYSTEM SHALL** — a mandatory behaviour. Here "the system" is the repo plus
  the `isms` CLI and its validation/build steps.

## 2. Non-functional constraints

- **NFR-1** THE SYSTEM SHALL run fully offline and free: no cloud accounts, no paid
  APIs, no network calls at build time, no real attacks on third-party systems.
- **NFR-2** THE SYSTEM SHALL NOT contain real personal data (no home address, no
  phone number) in any file.
- **NFR-3** THE SYSTEM SHALL NOT copy text from ISO/IEC 27001, 27002, 42001,
  BSI C5, IT-Grundschutz, DORA or NIS2. It MAY use control/clause IDs and short
  titles written in my own words.
- **NFR-4** THE SYSTEM SHALL NOT state "certified", "compliant" or "audit-ready" as
  a fact anywhere.
- **NFR-5** WHEN any metric appears in a README, doc or report, THE SYSTEM SHALL
  derive it from a file produced by code in this repo; IF a value is not measured,
  THEN the output SHALL show "not measured" rather than an invented number.
- **NFR-6** THE SYSTEM SHALL keep source documents as Markdown and YAML, with all
  generated artefacts written only under `out/`.
- **NFR-7** THE SYSTEM SHALL target Python 3.11+ and run on Linux CI runners.

## 3. Functional requirements

### 3.1 Context and scope — ISO 27001 cl. 4

- **R-4.1** THE SYSTEM SHALL hold the organisational context in a YAML file covering
  internal and external issues, interested parties with their needs/expectations,
  and the ISMS scope statement.
- **R-4.2** THE SYSTEM SHALL record the ISMS interfaces and dependencies, including
  the boundary with the companion `iso27001-evidence-as-code` repo and the Azure
  platform, in the same or a linked YAML file.
- **R-4.3** WHEN the scope file is validated, THE SYSTEM SHALL confirm every
  interested party has at least one stated need and one owner.

### 3.2 Risk method — ISO 27001 cl. 6.1.2

- **R-6.1** THE SYSTEM SHALL define a risk method in YAML using a 5x5 likelihood x
  impact matrix, with named bands for likelihood (1-5) and impact (1-5).
- **R-6.2** THE SYSTEM SHALL define risk acceptance criteria as score thresholds
  (e.g. low / medium / high / critical) and name the role allowed to accept each band.
- **R-6.3** THE SYSTEM SHALL name risk owners as roles, not real individuals.

### 3.3 Risk register

- **R-RR.1** THE SYSTEM SHALL hold a risk register in YAML with about 20 realistic
  risks for a payment fintech.
- **R-RR.2** Each risk SHALL have: a stable id, title, description, affected asset
  or process, threat/vulnerability, owner role, inherent likelihood and impact (1-5),
  resulting inherent score, planned treatment, residual likelihood and impact, and
  residual score.
- **R-RR.3** Each risk SHALL link to one or more Annex A control IDs, and WHERE the
  risk is relevant to DORA, it SHALL also link to the relevant DORA article number(s).
- **R-RR.4** THE SYSTEM SHALL validate the risk register against a JSON schema.
- **R-RR.5** IF a risk references an Annex A control ID that is not present in the
  SoA, THEN validation SHALL fail with a clear message.
- **R-RR.6** IF a risk's residual score is above its stated acceptance band without a
  recorded acceptance decision, THEN validation SHALL flag it.

### 3.4 Risk treatment and SoA import

- **R-SOA.1** THE SYSTEM SHALL read `soa.json` exported by the companion repo,
  copied into `data/`, as the source of control status.
- **R-SOA.2** THE SYSTEM SHALL produce a risk treatment plan that, for each treated
  risk, lists the chosen Annex A controls and their current status from the imported
  SoA.
- **R-SOA.3** THE SYSTEM SHALL NOT mark any control as "pass" unless the imported
  SoA shows it as passing with evidence; controls without evidence SHALL keep their
  imported status (e.g. manual, partial, not-applicable).
- **R-SOA.4** WHEN the SoA import runs, THE SYSTEM SHALL report how many controls are
  automated (have evidence/rules) versus manual, derived from the file.

### 3.5 Policy set

- **R-POL.1** THE SYSTEM SHALL contain five short policies written in my own words:
  information security policy, acceptable use, access control, supplier security,
  incident management.
- **R-POL.2** Each policy SHALL be in English and SHALL include a short German
  summary (DE Zusammenfassung).
- **R-POL.3** Each policy SHALL state an owner role, a version, a review cadence, and
  the Annex A control IDs it supports.

### 3.6 Internal audit — ISO 27001 cl. 9.2

- **R-AUD.1** THE SYSTEM SHALL contain an internal audit programme covering a
  rolling period, listing audit areas mapped to clauses and Annex A themes.
- **R-AUD.2** THE SYSTEM SHALL contain one sample internal audit report with
  findings classified (e.g. major / minor / observation) and, for each finding, a
  CAPA (corrective action, owner role, due date, status).

### 3.7 Management review and KPIs — ISO 27001 cl. 9.3

- **R-MR.1** THE SYSTEM SHALL contain a management review template following the
  cl. 9.3 inputs and outputs.
- **R-MR.2** THE SYSTEM SHALL contain one worked management review for a sample
  quarter, using only values that come from repo data.
- **R-KPI.1** THE SYSTEM SHALL compute six ISMS KPIs from the YAML/JSON data, for
  example: SoA automation coverage, share of controls with evidence, open vs closed
  risks above acceptance, number of open audit findings, CAPA on-time rate, policy
  review currency.
- **R-KPI.2** WHEN a KPI cannot be computed because its input is missing, THE SYSTEM
  SHALL output "not measured" for that KPI rather than a placeholder number.

### 3.8 CLI — `isms`

- **R-CLI.1** THE SYSTEM SHALL provide a Python CLI `isms` with a `build` command.
- **R-CLI.2** WHEN `isms build` runs, THE SYSTEM SHALL validate all YAML files
  against their schemas before computing anything.
- **R-CLI.3** IF any YAML file fails validation, THEN `isms build` SHALL stop with a
  non-zero exit code and a readable error, and SHALL NOT write partial artefacts.
- **R-CLI.4** WHEN validation passes, THE SYSTEM SHALL compute the KPIs and a risk
  heat map (count of risks per likelihood x impact cell).
- **R-CLI.5** WHEN the build completes, THE SYSTEM SHALL export one Excel workbook to
  `out/` with at least three sheets: risk register, SoA, KPIs.
- **R-CLI.6** WHEN the build completes, THE SYSTEM SHALL export a short PDF ISMS
  handbook to `out/` summarising scope, risk posture, SoA status, and KPIs.
- **R-CLI.7** THE CLI SHALL also expose a `validate` path (its own subcommand or a
  `--validate-only` flag) that runs validation without producing artefacts.

### 3.9 CI

- **R-CI.1** WHEN a pull request is opened or updated, THE SYSTEM SHALL run a GitHub
  Actions workflow that validates all YAML and runs the build.
- **R-CI.2** IF validation or build fails, THEN the workflow SHALL fail the PR check.
- **R-CI.3** THE workflow MAY upload the generated `out/` artefacts for review but
  SHALL NOT commit them to the repo.

### 3.10 Tests

- **R-TEST.1** THE SYSTEM SHALL include pytest tests for the schema validator,
  covering at least one valid and one invalid fixture per schema.
- **R-TEST.2** THE SYSTEM SHALL include pytest tests for the KPI maths and the risk
  score / heat map calculation, asserting exact expected values on fixed inputs.

## 4. Out of scope

- Real company data, real individuals, or real contact details.
- Any live cloud resource or real attack.
- Any certification, compliance, or "audit-ready" claim.
- Copying text from any standard.
- Re-implementing the technical scanners in `iso27001-evidence-as-code`; this repo
  only imports that repo's `soa.json` output.

## 5. Acceptance (definition of done for the whole spec)

- A reviewer can run `isms build` offline and get `out/` artefacts with no network.
- Every number in the handbook/README traces to a repo file or says "not measured".
- All YAML validates against schemas; CI is green on a PR.
- No standard text is copied; no certification wording; no personal data.
