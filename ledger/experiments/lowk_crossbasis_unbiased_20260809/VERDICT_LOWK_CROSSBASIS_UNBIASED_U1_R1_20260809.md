# Verdict: K12 cross-basis finite-sample Gaussian correction U1/R1

Date: 2026-08-09

Status: **kill the finite-sample plug-in-bias family**.

Evidence label: **component development diagnostic** on the fixed public
offline 160/48/48 pair-teacher bank.  No official Mini100 row, FlopScope
session, physical row, package, upload, submission, or remote action occurred.

## What was tested

The target-free seal contained 17 exact formulas built from the endpoint-
shifted first and raw second preactivation moments of the 12 complete basis
units:

- the ordinary Gaussian plug-in readout;
- an exact cross-basis U-statistic for `mu^2`, removing the downward
  finite-sample bias of `q_bar - mu_bar^2`;
- complete delete-group Richardson/jackknife extrapolations at subset sizes
  `4,6,8,10,11`, for both moment rules;
- midpoints of the paired extrapolations.

Every subset at each size was averaged.  One global scalar per formula was
fit on 160 networks; one formula was selected on 48 development networks;
the held 48 were then opened once.

## Result

The selected `plugin_j11` formula failed completely:

```text
direct K12 held raw MSE          3.15352662099e-6
selected held raw MSE            3.15320052670e-6
selected/direct ratio            0.999896593773
held row p95                     6.72922580578e-6
held rows improved               25 / 48
required raw MSE                 <=1.0e-6
breakthrough raw MSE             <=8.0e-7
```

The development split actually regressed (`1.000361x`).  The result misses
the primary raw gate by more than threefold.

## Why it failed

The finite-sample corrections are minuscule relative to the missing common
mode:

```text
RMS(U-statistic full - plug-in full)      1.793e-6
RMS(delete-11 plug-in - plug-in full)     1.314e-6
RMS(plug-in proposal - direct K12)        1.029e-3
RMS(exact teacher - direct K12)            2.052e-3
```

On held output coordinates, the ordinary plug-in direction correlates only
`0.00247` with the actual target residual.  The exact high-sample teacher
direction correlates `0.88157` with that residual.  In contrast, the plug-in
direction correlates `0.46822` with the teacher direction.  Therefore the
failure is not the `O(1/K)` nonlinear plug-in bias addressed by jackknife or
the biased estimate of `mu^2`; it is the much larger failure of the selected
12 Kerdock bases to observe the signed high-sample moment common mode.

The near-unity correlation between absolute plug-in and teacher predictions
is irrelevant because both are dominated by the common positive output
level.  Correction-direction correlation is the controlling statistic.

## Boundary

This closes:

- cross-basis U-statistic correction of the final variance;
- delete-group/jackknife/Richardson correction of the same Gaussian moment
  functional;
- further tuning of their subset sizes or scalar blends.

It does not close an estimator with a genuinely new observation of the
missing final-preactivation common mode, such as a deterministic collective
Price/Wick contraction, an off-design exact pilot, or a different structured
measure whose moment error is correlated with the target defect.

## Evidence

- preregistration:
  `PREREG_LOWK_CROSSBASIS_UNBIASED_U1_R1_20260809.md`
- target-free builder:
  `build_lowk_crossbasis_unbiased_u1_r1_targetfree_20260809.py`
- scorer:
  `score_lowk_crossbasis_unbiased_u1_r1_postseal_20260809.py`
- target-free seal:
  `runtime/artifacts/lowk_crossbasis_unbiased_u1_r1_targetfree_20260809.npz`
  (`0bf336a9540a28c33866a441ee88088665bf557db1e2f0c1fa162f294484ddc6`)
- post-seal receipt:
  `runtime/artifacts/lowk_crossbasis_unbiased_u1_r1_postseal_20260809.json`
- frozen prediction seal:
  `runtime/artifacts/lowk_crossbasis_unbiased_u1_r1_prediction_seal_20260809.npz`

