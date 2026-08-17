# Difficulty router — R2 addendum (repair + bounded encoder gate)

Date: 2026-08-08. Supersedes the R1 prereg's evaluation design; the
mechanism, capacity evidence, and G4 deployment ladder are unchanged.

## Corrections of record

1. The R1 prereg described test500 references as "N=1e9 near-exact".
   Wrong: the dataset's own `N` field is 1,000,000 and the per-row label
   noise floor is `post_var[:,31,:].mean()/N ≈ 5.3e-8` (row-dependent).
   The self-generated shard labels (antithetic MC, debiased, measured
   noise 2.4–3.5e-9) are therefore the PRIMARY labels; test500 is
   secondary and is row-debiased by its own `post_var/N` before use.
2. Handcrafted-feature routing (62-dim and 580-dim summaries, one- and
   two-head) is recorded as dead: held raw_full corr 0.51/0.55 does not
   carry gain information (independent check: signed-gain corr
   0.06/0.12; routing −5.98%). The raw_full signal itself is real and is
   retained as an auxiliary target.

## Bounded encoder experiment (one run before any further generation)

Data: the existing 750 self-gen nets (primary) + 500 debiased test500
(secondary, half sample-weight). No new generation before the gate.

Model: layer-token encoder (32×41 GPU tokens), heads = log raw_full and
log raw_m for m ∈ {16, 24, 32, 48}, plus a pairwise margin-ranking
objective on per-net signed gain per arm (gain_i = q_f·raw_full_i −
q_c(m)·raw_m_i with the R1 cost model; q_f = 0.5133).

Validation: early stopping on a held self-gen seed range (cross-seed),
with test500 transfer reported separately (cross-generator). Challenge
families (Full100, Generated128) are never trained on and are evaluated
once per run. Token and label archive SHA-256 are pinned in the receipt;
the receipt status field is derived from the gates, never constant.

## Gates

- CONTINUE-SCALING gate (this run): pooled held-family signed-gain
  correlation ≥0.30 with BOTH families positive, AND at least one
  (m, p) cell in the routing frontier improving projected score on both
  families simultaneously. Pass → resume generation to 4,000 and retrain.
- FINAL G1 (unchanged): ≥0.60 pooled / ≥0.50 per family, routing
  improvement ≥3.5% with kept-row tail guards.
- KILL: continue-scaling gate missed → park the routing lane; record the
  verdict; revert focus to the engineering baseline and remaining
  research lanes. No retuning on held families.
