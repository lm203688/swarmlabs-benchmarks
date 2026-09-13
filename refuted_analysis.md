# REFUTED scenarios — root-cause analysis

> Honest disclosure of the 5 scenarios where the Gaussian-Process surrogate did NOT reproduce the underlying model within stated uncertainty. These are kept on display, not hidden. All numbers are measured out-of-sample values from the live report index (2026-09-12/13).

## Common root cause
All five are **inverse parameter-identification** problems in microbial kinetics: the surrogate is asked to recover parameters it was never directly given, from sparse noisy observations. This is a known-hard ill-conditioned class — not a coding bug. Coverage stays high only because the honest uncertainty band admits "I don't know."

## micro_competition_2d

- **Domain:** biology
- **Published model:** (inverse-parameter fit; no closed form)
- **Dim / bounds:** 1 / [[1.0, 20.0]]
- **Train / test:** 30 / 40
- **Measured R²:** 0.563  (R²_core: 0.159)
- **Coverage:** 0.950  | RMSE_rel: 0.664
- **Calibration scale κ:** 13.601
- **Verdict rationale:** R²=0.563 < 0.7（拟合不足）

## micro_competition_ecoli_yeast

- **Domain:** biology
- **Published model:** (inverse-parameter fit; no closed form)
- **Dim / bounds:** 1 / [[0.5, 20.0]]
- **Train / test:** 30 / 40
- **Measured R²:** 0.440  (R²_core: -0.795)
- **Coverage:** 0.950  | RMSE_rel: 0.750
- **Calibration scale κ:** 11.858
- **Verdict rationale:** R²=0.440 < 0.7（拟合不足）

## micro_doe_ecoli_monod

- **Domain:** biology
- **Published model:** (inverse-parameter fit; no closed form)
- **Dim / bounds:** 2 / [[0.001, 10.0], [25.0, 45.0]]
- **Train / test:** 50 / 60
- **Measured R²:** 0.161  (R²_core: -25.787)
- **Coverage:** 0.917  | RMSE_rel: 0.910
- **Calibration scale κ:** 7.8593
- **Verdict rationale:** R²=0.161 < 0.7（拟合不足）

## micro_mle_ecoli_monod

- **Domain:** biology
- **Published model:** (inverse-parameter fit; no closed form)
- **Dim / bounds:** 1 / [[0.001, 5.0]]
- **Train / test:** 30 / 40
- **Measured R²:** 0.125  (R²_core: -41.983)
- **Coverage:** 0.925  | RMSE_rel: 0.933
- **Calibration scale κ:** 15.9761
- **Verdict rationale:** R²=0.125 < 0.7（拟合不足）

## micro_pirt_ecoli

- **Domain:** biology
- **Published model:** (inverse-parameter fit; no closed form)
- **Dim / bounds:** 1 / [[0.001, 10.0]]
- **Train / test:** 30 / 40
- **Measured R²:** 0.312  (R²_core: -16.100)
- **Coverage:** 0.950  | RMSE_rel: 0.831
- **Calibration scale κ:** 17.7984
- **Verdict rationale:** R²=0.312 < 0.7（拟合不足）

## Why we keep them

Removing these would inflate the pass rate to 100% and make the other 53 numbers meaningless. A verification layer's value is knowing *where not to trust the surrogate* — that signal lives in the REFUTED set.