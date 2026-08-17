# Direct signed-gain router gate — R3 preregistration

Date: 2026-08-08 (Europe/Bucharest).

This is the cheapest unfinished gate in
`PREREG_DIFFICULTY_ROUTER_R1_20260807.md`. It does not create a new
estimator, generate a corpus, run a physical row, open Mini100, package,
upload, or submit anything.

Evidence labels:

- held Full100 and Generated128 correlations: **broad statistical**;
- routed adjusted-score arithmetic using modeled branch multipliers:
  **projection**.

## Correct score and target

The controlling score text is `docs/CHALLENGE.md` lines 32--43:

```text
q_i = max(0.1, C_i / 272B)
s_i = final_layer_MSE_i * q_i
score = mean_i(s_i)
```

Compute is applied row by row before averaging. For the frozen m=32
diagnostic arm, define

```text
g_i = q_full * raw_full_i - q_m32 * raw_m32_i
q_full = 0.5133
q_m32 = 0.1397
```

Positive `g_i` means routing that row to m=32 improves its adjusted row
loss under this modeled price. The regression target is the sign-preserving
log-like transform

```text
y_i = asinh(g_i / median_train(abs(g)))
```

so negative gains are not collapsed to an arbitrary log floor. The routed
policy is frozen as `predicted y_i > 0`; no held-family threshold or routed
fraction is tuned.

The m=32 arm is not below the literal `27.2B` multiplier floor. This is a
signal gate for the best earlier oracle arm, not a G4 floor-arm receipt.
Even a passing result would leave a separately measured m=16/sub-27.2B arm
or a renamed non-floor routing deployment to resolve.

## Data and reference-noise correction

Training uses only the existing 500-network WhestBench research test
capture and its already-created R1/R2 target-blind weight features. The
research reference has **N=1,000,000**, not N=1e9. Its dataset card reports
an approximate final-mean target-noise floor of `5e-8`. The stored R1 router
labels did not subtract that floor, so this test must be described as a
noisy-label lower-bound screen, not training on near-exact truth.

Held reporting uses only the historically opened project Full100 and
Generated128 development banks already used by R1/R2. They remain separate
throughout reporting. Official Mini100 is untouched.

Every consumed archive, feature array, label array, preregistration, and
source is SHA-256-pinned in the receipt. Row identities and m-grid alignment
must be asserted before fitting.

## Prior-art preflight

Queries: difficulty router, direct gain regression, log gain, per-row floor,
adaptive compute, R1/R2 feature models, Full100, Generated128.

Nearest controlled work:

- `runtime/artifacts/k129_router_regressor_g1g2_r1_20260807.json` trained
  raw-error models and only evaluated direct log-gain in-family;
- `runtime/artifacts/k129_router_regressor_g1_r2_20260807.json` trained only
  raw-error models;
- `runtime/artifacts/k129_perrow_floor_routing_selector_requirement_r2_20260807.json`
  established capacity but no held direct-gain model.

Outcome: **materially new observable** at the held gate. This completes the
missing direct signed adjusted-gain evaluation; it is not a renamed raw-error
regressor. The existing controlled negatives do not contain this held
prediction.

Target ceiling: the earlier m=32 oracle projection is `9.2019e-8`, so a
predictive signed-gain model has sufficient ceiling for the requested
`1.10e-7` threshold. PASS.

## Frozen models

Primary:

- R2 580-dimensional features;
- `HistGradientBoostingRegressor(max_iter=500, max_depth=3,
  learning_rate=0.05, l2_regularization=3.0, min_samples_leaf=30,
  random_state=20260808)`;
- five deterministic training-family folds for diagnostic CV;
- fit once on all 500 training rows for held evaluation.

Diagnostics, never eligible to replace the primary after scoring:

- R2 ridge (`alpha=100`);
- R1 62-dimensional HGB with the same parameters;
- R1 ridge (`alpha=10`).

Standardization is learned from each training fold or, for the held fit,
from all 500 training rows. No coefficient, arm multiplier, fraction,
feature, or model is selected using held outcomes.

## Gates

G1 passes only if the primary model satisfies all of:

- correlation with signed-log gain `>=0.50` on Full100;
- correlation `>=0.50` on Generated128;
- correlation `>=0.60` on the concatenated held rows.

G2 passes only if the frozen `prediction > 0` policy satisfies all of:

- positive adjusted-score improvement on each held family;
- size-weighted pooled adjusted-score improvement `>=3.5%`;
- at least one and fewer than all rows routed in each family;
- routed `raw_m32/raw_full` p95 no greater than the modeled break-even
  `q_full/q_m32` in each family.

The lane continues only if both G1 and G2 pass. Otherwise the current
R1/R2 direct-feature family is killed. A failure does not by itself kill a
future equivariant encoder trained on a properly receipted larger corpus,
but it prohibits interpreting the current 750 unreceipted self-generated
rows as evidence for one.

## Outputs and prohibitions

The single output is
`runtime/artifacts/k129_router_direct_signed_gain_g1g2_r3_20260808.json`.
No prediction package, model artifact, physical receipt, upload, or remote
action is authorized.
