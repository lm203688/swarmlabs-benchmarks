# Held-out verification suite

How we prove the 62-scenario V&V verdicts (PASS / MARGINAL / REFUTED) are **genuinely held-out** — not a fitting trick or a leaked test set.

## Why this exists

In 2026, frontier research agents (GPT-6 Astra on Insilico's DDD antibody-developability benchmark, Stanford Biomni, DrugAgent) began posting SOTA scores with no plugin and no fine-tuning. The recurring critique: a benchmark score is only meaningful if the test set is *truly unseen*. We hold ourselves to the same standard.

## Two independent checks

Both use the **noise-free ground truth** we own (virtual experiments = simulation, so truth is known exactly — no measurement noise to confuse training vs. testing).

### (A) Robustness held-out — the headline check

- Keep the **training set fixed** (deterministic seed 0 LHS).
- Draw a **fresh, independent test set** with a new seed (4242) — points the model never saw.
- Recompute R², single-sided coverage, and the verdict with the identical rule as the main suite.

**Result: 51 PASS / 3 MARGINAL / 8 REFUTED** (vs. the main suite's 53 / 4 / 5).

The verdicts reproduce under a fresh test draw. That is the proof: the 53 PASS are **not** leakage or a lucky split — they are robust to the choice of unseen test points.

### (B) Extrapolation stress — a deliberately harder variant

- Sort each scenario's points by their first coordinate (pseudo-time).
- Reserve the **later 30% as a strictly FUTURE test region**; the surrogate is fit only on the earlier 70%.

**Result: 3 PASS / 6 MARGINAL / 53 REFUTED.**

This is *expected* and *by design*: Gaussian-Process surrogates are **interpolators**, not extrapolators. Forced to predict a region it has never seen along an axis, they degrade — often with over-confident narrow intervals. We publish this result **openly** because it shows *where surrogates break*, which is precisely why a verification layer (out-of-distribution guard + calibrated uncertainty κ, never tightened) must sit between a model and any decision. It is a stress test, **not our accuracy claim**.

## Verdict rule (identical to the main suite)

- **PASS** — out-of-sample R² ≥ 0.90 and single-sided coverage ≥ 0.90
- **MARGINAL** — R² ≥ 0.70 and coverage ≥ 0.80
- **REFUTED** — below those bars

Coverage is **single-sided and conservative**: an over-confident narrow interval fails; a slightly-wide interval passes. Calibration κ only widens uncertainty, never tightens (capped at 40).

## Files

- `robustness_heldout.json` — per-scenario results of check (A): reproducible PASS/MARGINAL/REFUTED under a fresh test seed.
- `benchmarks.json` — the main 62-scenario suite (53 / 4 / 5).
- `refuted_analysis.md` — root-cause for the 5 main-suite REFUTED scenarios.

Regenerate with: `python scripts/make_held_out_benchmark.py` (lives in the SwarmLabs main repo; this file is the published artifact).
