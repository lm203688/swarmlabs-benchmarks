# SwarmLabs Benchmark Suite

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

A **public, honesty-first benchmark** for the SwarmLabs virtual-experiment engine:
62 physics-informed scenarios, each with a 10-chapter Verification & Validation (V&V)
report, and a machine-checkable verdict. We publish **every** verdict — including the
5 that failed.

> In AI-for-science, everyone shows the wins. We show the 5 models that failed, and
> explain exactly why. That is the point.

## Verdict distribution (measured, 2026-09-13)

| Verdict   | Count | Meaning |
|-----------|-------|---------|
| PASS      | 53    | Out-of-sample R² ≥ 0.90 and one-sided coverage ≥ 0.90 |
| MARGINAL  | 4     | R² ≥ 0.70 and coverage ≥ 0.80 |
| REFUTED   | 5     | Below those bars — surrogate does not reproduce the model within stated uncertainty |
| ERROR     | 0     | Engine or report generation failure |

**85% PASS · 8% REFUTED.** The 5 REFUTED are inverse-parameter-identification
limits of sparse data, not coding bugs. See [`refuted_analysis.md`](refuted_analysis.md).

## What is in this repo

- `benchmarks.json` — the 62 scenario definitions + measured metrics + verdicts.
  Curated from the live `swarmlabs.tools/v3/report-index`.
- `refuted_analysis.md` — deep-dive on the 5 REFUTED scenarios and why they are kept
  on display.
- Each scenario's full 10-chapter report (HTML / JSON / DOCX) is served at
  [swarmlabs.tools/report-gallery](https://swarmlabs.tools/report-gallery).

## Methodology (the part that makes the numbers mean something)

1. **Noise floor is fixed at 3%.** We do not lower injected observation noise to make
   a fit look tighter. The floor is a red line.
2. **Coverage is one-sided and conservative.** A prediction interval must actually
   contain the truth ≥ 90% of the time. Over-confident narrow bands *fail* — they are
   not rewarded.
3. **Calibration κ only widens uncertainty, never tightens it.** The variance-scaling
   factor is capped at 40 and can only be raised — the model is never allowed to
   "discover" it was secretly more certain than it reported.

The reports align with **ASME V&V 10-2019** and **FDA CM&S** expectations: problem
statement, model description, code & data provenance, numerical accuracy, uncertainty
quantification, sensitivity, calibration, limitations, and a reproducibility manifest.

## Honest scope boundary

These are **virtual experiments** validated against published analytic solutions and
literature benchmarks (e.g. Verhulst logistic growth, Monod kinetics). They are a
**verification asset**, not a substitute for wet-lab measurement. We have **not yet**
run them against our own bench data — that is the next milestone, and the REFUTED set
defines exactly where the surrogate should not be trusted until then.

## Reproduce

```bash
# The scenario definitions and metrics are in benchmarks.json (no heavy deps to read it):
python -c "import json; d=json.load(open('benchmarks.json')); print(d['counts'], 'scenarios:', len(d['scenarios']))"
```

The engine and report generator live in the (private) `swarmlabs` repository; this
public repo is the **auditable artifact** — the definitions, the verdicts, and the
failures.

## License

MIT — use the definitions and metrics freely; cite the suite if it informs your work.
