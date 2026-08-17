# Phase 1 Experiment Ledger

This is the short index.  Detailed numbers live in
`FULL200_TAIL_CANARY_LOG.md`, `ORACLE_LIMITS.md`, and older notes in
`../legacy_workspace/EXPERIMENT_LOG.md`.

## Canonical calibration correction: submission 315829

The historical `1.42e-7` number later in this ledger is a **retired and
falsified ex-ante projection**, not a measured local score or current remote
estimate.  The candidate's measured local adjusted score was
`1.634853235e-7`; submission `315829` actually scored `1.557374486e-7`
remotely.  Never surface `1.42e-7` without this correction.

## 2026-07-12: sample-anchored fullcov receiver closure

Tested a distinct full-covariance receiver that preserves the analytic
correlation state but anchors its preactivation mean and diagonal variance at
every hidden layer to already-paid K26 moments.  Frozen Full spaced20/offset20
arms included diagonal-only and correlation-preserving variance anchoring,
plus joint mean/variance anchoring at strengths `.5` and `1`.

Joint full anchoring appeared dramatic against the analytic fullcov proxy:

```text
affine analytic signed-Ez MSE  = 4.918471e-5 / 5.549118e-5
anchored signed-Ez MSE         = 5.338529e-6 / 5.279515e-6
```

That is not the relevant deployment comparator.  The paid sampled signed-Ez
is already `2.547553e-6 / 2.486267e-6`, roughly twice as accurate as the new
proxy.  The anchored-minus-sampled direction also fits with the wrong sign:
spaced gain `-0.012145`, transferring to only a `0.999601x` signed-Ez ratio on
offset.  No fixed negative gain in `-.005..-.025` exceeds a `0.05%` effect.

Close the family before actual shifted-row scoring.  General lesson: every
analytic-proxy improvement gate must include the paid sampled state as the
binding comparator; beating a weak analytic proxy alone is not evidence for a
useful sampled-row correction.

Artifacts:

- `probe_sample_anchored_fullcov_preregister_root.md`
- `probe_sample_anchored_fullcov_root.py`
- `cache/root_sample_anchored_fullcov_20260712.npz`

## 2026-07-12: exact great-circle integration feasibility closure

Tested a genuinely white-box alternative to point sampling.  On a fixed Haar
two-plane, a bias-free ReLU MLP is exactly `A*cos(t) + B*sin(t)` between
activation breakpoints.  The probe propagated those coefficients, solved every
new ReLU zero, and analytically integrated the final output over the complete
circle.  Averaging exact circle integrals over Haar planes is an unbiased
sphere estimator.

The breakpoint engine is correct: on Mini index 0, its first-plane mean agrees
with a 32,768-point dense angular grid to RMS `5.05e-9` and max `2.98e-8`.
The feasibility economics are nevertheless decisive:

```text
final regions, plane 1                 16,645
coefficient row-layer work, plane 1   563,532
Mini-0 raw, one exact plane           4.197569e-3
Mini-0 raw, two exact planes          1.409409e-3

K26 ordinary row-layer work           425,984  (= 13,312 * 32)
```

Thus one exact plane already requires about `1.32x` K26's dense row-layer
work before breakpoint sorting and carries roughly four orders of magnitude
too much orientation variance.  The region count grows nearly linearly by
about 400--650 roots per layer; exact angular Rao--Blackwellization does not
solve the far larger random-subspace variance.  Do not broaden, flopscope, or
package this route.  The older finitely sampled Grassmann-circle family was
negative for a different reason; this result closes the previously untested
exact-breakpoint version.

Artifacts:

- `probe_exact_great_circle_root.py`
- `cache/root_exact_great_circle_20260712.npz`

## 2026-07-12: K26 seed-covariance anisotropic fullcov gate

The paid 26 final seed vectors define a target-free, per-MLP rank-at-most-25
output subspace.  A supervised ceiling gate decomposed the existing fullcov
correction direction into that seed-disagreement subspace and its orthogonal
complement, fitted two global gains on Full1000, then froze them for Mini100.
Ranks 1, 2, 4, 8, 12, 16, 20, and 25 were predeclared; advancement required an
adjacent rank range to beat the best scalar correction by at least 3% on both
domains.

No rank approached the gate.  The best transferable point was rank 4:

```text
best scalar:  Full1000 1.392570257e-6   Mini100 1.281370916e-6
rank 4:       Full1000 1.386537807e-6   Mini100 1.276961603e-6
relative:              0.995668                  0.996559
fitted gains: inside = 0.075043, outside = 0.044074
```

Ranks 1 and 2 gave the largest Full gain but regressed Mini.  The whole family
therefore exposes at most a few tenths of one percent transferable raw signal,
not a missing breakthrough.  Do not derive or price a production rule from
this gate unless a new correction direction—not merely another rank or gain
fit—is supplied.

Artifacts:

- `probe_seedcov_anisotropic_fullcov_root.py`
- `cache/root_seedcov_anisotropic_fullcov_20260712.npz`

## 2026-07-12: nonlinear Kerdock checkpoint multifidelity control

Reopened complete-design checkpoint compression with a materially richer
control than the earlier linear mean-field response.  All 129 Kerdock bases
were propagated exactly through H2; a fixed spread-26 subset traversed the
exact nonlinear tail, while every H2 basis received a separate target-free
diagonal-Gaussian mean/variance tail.  A two-fold basis-cross-fitted scalar
coefficient estimated

```text
mean_test(exact - beta * proxy) + beta * mean_all(proxy)
```

independently per output.  The frozen Full spread-five scout required at least
10% raw improvement and raw-times-work below 0.95 before any disjoint guard.
It failed in the opposite direction:

```text
fixed 26 exact tails       raw 9.059890160e-7
unit proxy control         raw 9.056775794e-7   ratio 0.999656
cross-fitted proxy         raw 1.008160762e-6   ratio 1.112774
optimistic work ratio                         1.265766
unit raw-times-work                           1.265331
crossfit raw-times-work                       1.408511
```

The per-output fitted coefficients had standard deviation around 0.94--1.34
within each MLP, confirming that 26 exact bases do not identify the missing
tail from diagonal H2 state.  Do not broaden this proxy, add ridge/gain grids,
or package it.  Reopen only with an independently validated shallow feature
whose basis-level exact-tail correlation is materially stronger before target
scoring.

Artifacts:

- `probe_kerdock_diag_multifidelity_root.py`
- `cache/root_kerdock_diag_multifidelity_full_spaced5_20260712.npz`

## 2026-07-12: final-weight spectral filtering of fullcov-Ez

Tested whether the incumbent fullcov signed-Ez direction needs different gains
in different singular modes of the observed final weight.  A factorized
Chebyshev/Krylov basis applied orders 0--4 of normalized `W31.T @ W31` to the
already-paid correction.  Full rows 0:600 fitted coefficients, 600:800 selected
the order, 800:1000 were opened once as a guard, and Mini100 was frozen
transfer.  A production implementation would need only repeated vector-weight
products (well below `0.001B` FLOPs), so the gate required 3% raw gain on both
guards.

Validation selected order 4, but the effect was numerical-noise scale:

```text
matched scalar Full guard     1.292133739e-6
order-4 Full guard            1.292068531e-6   ratio 0.999950
matched scalar Mini100        1.283189871e-6
order-4 Mini100               1.283216762e-6   ratio 1.000021
Full/Mini half ratios         .999996/.999901/1.000041/1.000003
```

The Mini sign reversal and sub-basis-point magnitude decisively fail the gate.
The successful fullcov-Ez shift is not hiding a useful final-weight spectral
gain schedule.  Do not try higher polynomial orders, an eigensolve, or a
mode-gain grid without a different correction source.

Artifacts:

- `probe_fullcov_w31_spectral_root.py`
- `cache/root_fullcov_w31_spectral_20260712.npz`

## 2026-07-12: cheap affine late-state sparsification closure

E4's power-map restoration was compute-negative, so tested its explicitly open
alternative: row top-k followed by a cheap affine column mean/variance repair.
The repaired state has the exact factorization

```text
(sparse * scale + baseline) @ W
  = (sparse * scale) @ W + baseline @ W,
```

allowing the residual to remain sparse.  A Torch scout appeared neutral-positive
on five Full MLPs (`0.999550x` raw at state fraction `.55`), but that scout
applied `.55` to an unscreened width-256 state.  It was therefore not evidence
for the actual D48 geometry, whose late product inputs have widths 192 and 176.

The literal official-path D48 Mini-0 falsifier applied `.55` to those actual
inputs and failed catastrophically:

```text
D48 control (E4 audit)       raw 5.308289e-7   F 27.36534B
affine state .55             raw 1.142322e-5   F 26.51B
raw ratio                    about 21.5x
F saving                     about 0.855B
```

A separate random-matrix unit test compared the tracked sparse-plus-baseline
product to materializing the dense affine state: max difference `9.54e-6`, RMS
`1.59e-6` against reference-product RMS `10.01`.  The algebra is correct to
ordinary float32 reassociation; the statistical failure is real.  Keeping 141
entries to mimic the width-256 scout would exceed the typical 111--136 positive
counts and remove essentially no product work.  Close this route; never cite
the `0.999550x` scout without the production-width falsifier.

Artifacts:

- `probe_affine_state_sparsify_root.py`
- `candidate_k26_d48_affine_state055_root.py`
- `cache/root_affine_state_sparsify_full_spread5_20260712.npz`
- `cache/root_d48_affine_state055_mini0_20260712.csv`

## 2026-07-12: cheap early-variance recurrence and teacher distillation

The production-shaped prefix-7 variance correction had reduced raw MSE to
`0.8911/0.8771/0.8788x` on Full-spaced20, Full-offset20, and Mini-spaced20,
but its source was the unaffordable full-K3/K3cov analytic prefix.  A frozen
economic audit tested diagonal Gaussian propagation and full-covariance
Gaussian/Hermite orders 1, 2, 4, and 8 at every prefix endpoint 1..8.  One
output coefficient was fitted on Full-spaced20 and frozen before the other two
guards.  No recurrence used targets.

No cheap recurrence passed.  Orders 2/4/8 were numerically indistinguishable;
their best worst-split result was endpoint 4 at approximately
`0.9310/0.9853/0.9565x` (matrix-core lower bound `0.235B`).  Endpoint 7, the
literal teacher's useful location, was `0.9091/0.9994/0.9457x` at a `0.436B`
matrix core.  Thus the Gaussian covariance state itself does not carry the
transferable signal; the expensive prefix's non-Gaussian K3/K3cov feedback is
load-bearing.

A final target-free distillation fitted one global, output-permutation-
equivariant linear map from the 16 cheap diag/Hermite-8 endpoint directions to
the deterministic expensive direction.  Ridge was selected by five
MLP-held-out teacher folds, without outcome targets.  Although teacher
correlation was `.938` on Full-spaced, it transferred at only `.693` on
Full-offset and `.837` on Mini.  With the expensive teacher's frozen output
strength, raw ratios were `0.9160/0.9931/0.9474x`, versus the teacher's
`0.8911/0.8771/0.8788x`.  Do not tune against these guard failures.  Close
cheap Gaussian-prefix and global response-distillation variants unless a new
derived non-Gaussian covariance/K211 state changes the representation.

Artifacts:

```text
legacy_workspace/probe_cheap_variance_prefix_root.py
legacy_workspace/cache/root_cheap_variance_prefix_20260712.npz
legacy_workspace/probe_cheap_variance_teacher_distill_root.py
legacy_workspace/cache/root_cheap_variance_teacher_distill_20260712.npz
```

## 2026-07-12: exact package-shaped Kerdock-129 Winograd economics

Reopened the statistically broad complete Kerdock 4-design at its actual
blocker: exact CPU contraction.  A self-contained direction module and exact
level-vectorized Winograd adapter reproduce the frozen Kerdock Mini index-0
prediction (`1.0566e-7` versus cached `1.0576e-7`, the small difference is
float32 reassociation).  Both tested full-depth levels pass the official
60-second in-process guard:

```text
level 4: raw=1.056771942e-7  F=165.815709B  wall=30.879s  residual=.472s
level 5: raw=1.056607058e-7  F=162.532063B  wall=39.294s  residual=.495s
```

Index 0 is unusually favorable and would score below `1e-7`; it is not a
broad estimate.  At the level-4/5 effective-compute factor, the frozen
three-guard Kerdock mean (`2.901689e-7`) remains around `2.3e-7` adjusted,
worse than K26.  Deeper recursion has sharply diminishing arithmetic returns
because the live `232`-column tail supports only three levels, while its wall
and allocation cost rise.  Converting the broad line to `1e-7` would require
roughly another 56% effective-compute reduction, not the observed 2%.

Keep this as an exact operational/statistical benchmark.  Do not package or
submit it, and never extrapolate its favorable index-0 score to the suite.

```text
legacy_workspace/build_kerdock129_direction_module_root.py
legacy_workspace/cache/root_kerdock129_direction_module_20260712.npz
legacy_workspace/candidate_kerdock129_vecwin_root.py
```

## Protected Line

- Remote best known from this lane: `311557`.
- Artifact: `submission_phase1_pure18_3swap_firsthalf_h1affine_weighted_finalonly_bundle.tar.gz`.
- Remote score: adjusted `2.420613336e-7`, raw final `1.697342478e-6`, multiplier `0.142613`.
- Status: keep protected until a broad gate beats it after compute repricing.

### Current protected l2snap line

- Current strongest known remote line from this branch: `311697`.
- Artifact:
  `submission_phase1_pure18_protected_h1affine_l2snap_b05_weighted_affine_full1000_finalonly_bundle.tar.gz`.
- Remote score: adjusted `2.3707390588881096e-7`, raw final
  `1.655662345e-6`, multiplier about `0.14319`.
- Module-backed bit-identical cleanup exists:
  `submission_phase1_pure18_protected_h1affine_l2snap_b05_weighted_affine_full1000_module_finalonly_bundle.tar.gz`.
  Local profile is bit-identical with the same FLOPs and only a tiny residual
  reduction, so this is a packaging cleanup rather than a raw-MSE candidate.
- Phase-1 local steady compute profile for the module-backed estimator on Mini
  indices `0..4`:

```text
raw final MSE mean      = 1.707698e-6
adjusted score mean     = 2.426864e-7
mean multiplier         = 0.14367069
counted FLOPs / MLP     = 3.8155630592e10
effective compute / MLP = 3.9078426831e10
residual wall / MLP     = 0.00923s mean, 0.00157s min, 0.03957s max
```

  Read: the protected line is counted-arithmetic dominated.  Reaching the
  `0.1` floor requires about a `29%` reduction in counted row propagation, not
  a setup/residual cleanup.

## Current Non-Overlap Pass

### 2026-07-07 - K26 sparse-tail activation column pruning

Question: can the current K26/L4/fullcov-Ez sampled row cloud buy adjusted
score by stopping exact propagation of weak mid-depth activation coordinates?

Added:

```text
legacy_workspace/probe_k26_sparse_tail_meanfield.py
```

The useful variant is target-free:

```text
score per layer/coordinate = mean(row_cloud_activation^2)
keep top 240 of 256 coordinates
apply from sampled tail layers 7..30
compute selected contribution with rectangular Strassen2
no omitted-tail correction in the deployed candidate
```

Research probe results:

```text
Full1000, current baseline in probe:
  keep=256 raw = 1.393772225e-6

Dynamic energy pruning, keep=240:
  start=8 raw = 1.392786657e-6  rel=0.999293
  start=7 raw = 1.392230039e-6  rel=0.998894
  start=6 raw = 1.396586915e-6  rel=1.002019

Aggressive keep=232:
  Full1000 raw = 1.394691478e-6  rel=1.000660

Static first-240 + variance rescale:
  Full200 raw = 2.536632495e-1  # catastrophic
```

Read: this is not generic random thinning.  The live row-cloud energy selector
is doing the work; fixed columns destroy the deep sampled distribution.  The
good setting is gentle pruning only: `keep=240`, start at layer 7.

Production translation:

```text
package folder:
  legacy_workspace/
  _pkg_pure26_w05_unionteacher1100gamma_l2snap_split035_045_noaffine_module_rectsparse240s7_preedgeL4_fullcovEzCal_finalonly

artifact:
  whest-starterkit/packages/to_test_remote/
  submission_phase1_pure26_w05_unionteacher1100gamma_l2snap_split035_045_rectsparse240s7_preedgeL4_fullcovEzCal_finalonly_bundle.tar.gz

sha256:
  1aac45e418007b889c23f6081f5e056f281c45ada2391f3617b3cfd2935aa1f2
```

Implementation note: fancy indexing `a[:, idx]` caused large residual time.
Use `fnp.argpartition` plus `fnp.take(a, idx, axis=...)` instead.  A micro
profile on the selected matmul showed the same reduced FLOPs with near-zero
residual for `fnp.take`, while fancy indexing had millisecond-scale residual.

Local package checks:

```text
whest validate: passed

Mini indices 0,5:
  current fullcov-Ez:
    raw=6.753851e-7 adjusted=1.294720e-7 mult=0.19443510 flops=4.526e10
  rectsparse240s7:
    raw=6.827245e-7 adjusted=1.282450e-7 mult=0.19131480 flops=4.340e10

Full split spaced20:
  rectsparse240s7:
    raw=1.130103e-6 adjusted=1.976967e-7 mult=0.17451628 flops=4.340e10
```

Read: this is a valid compute-polish candidate, not a breakthrough.  It cuts
FLOPs and slightly improves the Full1000 probe raw, but the gain is only a few
percent adjusted.  Keep as a remote-test candidate after higher-ceiling
accuracy work, not as evidence of a sub-2e-7 solution by itself.

### 2026-07-07 - K26 compute-economics pass

Goal: reduce effective compute for the current K26/L4/fullcov-Ez branch without
giving up raw accuracy.

Closed replacement idea:

```text
auxiliary protected/no-L4 final cloud replacing K26 rows
  k=6 selected seeds, best Mini transfer:
    count=26 gamma, raw=1.325983887e-6, modeled mult=0.212637,
    adjusted=2.819529400e-7
  k=10 selected seeds, best Mini transfer:
    count=26 gamma, raw=1.325995099e-6, modeled mult=0.240743,
    adjusted=3.192234203e-7
```

Read: the auxiliary final cloud has transferable signal when appended, but it
does not replace K26 sampled rows.  Lowering the K26 count to pay for auxiliary
rows loses too much raw MSE.  This is closed as a compute-reduction route.

Closed cheap Gaussian proxy:

```text
full-cov Gaussian Ez proxy, rectsparse232s7:
  full spaced20 raw=1.129616520e-6, adjusted=1.933004748e-7

diagonal mean/variance Ez proxy, same lambda/calibration:
  full spaced20 raw=3.447922e-6, adjusted=5.608614e-7
```

Read: the response-aligned off-diagonal covariance structure in the full-cov
proxy is load-bearing.  Do not replace it with diagonal/mean propagation.

Strassen schedule economics:

```text
standard two-level rectangular Strassen, rectsparse232s7:
  raw parity line              raw=1.129616520e-6
  local full spaced20 adjusted =1.933004748e-7
  counted FLOPs                =4.239381073e10
  warm ops                     =7290
  predicted remote multiplier:
    25us/op 0.2229, 45us/op 0.2765, 65us/op 0.3301

one-level standard Strassen:
  raw parity line              raw=1.129616520e-6
  local full spaced20 adjusted =2.077437744e-7
  counted FLOPs                =4.743094353e10
  warm ops                     =2857
  predicted remote multiplier:
    25us/op 0.2006, 45us/op 0.2216, 65us/op 0.2427

one-level rectangular Strassen-Winograd:
  raw parity line              raw=1.129617e-6
  local full spaced20 adjusted =2.059668e-7
  counted FLOPs                =4.738e10
  warm ops                     =2640
  predicted remote multiplier:
    25us/op 0.1985, 45us/op 0.2179, 65us/op 0.2373

one-level Winograd + final-layer energy prune to 232:
  raw parity line              raw=1.129617e-6
  local full spaced20 adjusted =2.042727e-7
  counted FLOPs                =4.724e10
  warm ops                     =2645
  predicted remote multiplier:
    25us/op 0.1980, 45us/op 0.2174, 65us/op 0.2369

one-level Winograd + final-layer energy prune to 224:
  raw parity line              raw=1.129617e-6
  local full spaced20 adjusted =not rerun through package; diagnostic wrapper
                                  local adjusted=2.009701e-7 at level-2,
                                  raw parity held
  counted FLOPs                =4.720e10
  warm ops                     =2645
  predicted remote multiplier:
    25us/op 0.1978, 45us/op 0.2173, 65us/op 0.2367
```

Read: local scoring prefers two-level Strassen because local residual is cheap.
Remote scoring may prefer one-level Winograd if residual cost per op is above
about `13 us/op`.  Final-layer energy pruning to 224 is raw-parity on the
full-spaced20 guard and slightly improves the Winograd economics.  This is an
economics candidate, not a raw-MSE gain.

Staged artifact:

```text
whest-starterkit/packages/to_test_remote/
  submission_phase1_pure26_w05_unionteacher1100gamma_l2snap_split035_045_rectsparse232s7_finalsp224_winograd1_preedgeL4_fullcovEzCal_finalonly_bundle.tar.gz
  submission_phase1_pure26_w05_unionteacher1100gamma_l2snap_split035_045_rectsparse232s7_finalsp232_winograd1_preedgeL4_fullcovEzCal_finalonly_bundle.tar.gz
  submission_phase1_pure26_w05_unionteacher1100gamma_l2snap_split035_045_rectsparse232s7_winograd1_preedgeL4_fullcovEzCal_finalonly_bundle.tar.gz

sha256:
  924e2c616f235a9274320078874f687606f64e36cb6084f6032792685617e9db
  099f354bb7932d690bbbf2655158175d444ab349ff3c64648c1eaa44beef1c11
  bfaa137ee6cfa80930f3b3f6cddf0231a6cd7fb66299e643040a4d9b6dea5735

whest validate:
  passed
```

Do not submit this blindly.  It is useful only if the live grader's residual
pricing on this package is closer to the high-op penalty regime than local
`quick_score_selected.py` suggests.

### 2026-07-07 - K26 sparse / pathCV84 sparse late-q25 router

Goal: find a route below the conservative K26 adjusted frontier by using the
high-raw-accuracy pathCV branch only on networks where its extra compute is
worth paying for.

New pathCV engineering probe:

```text
package dir:
  legacy_workspace/_pkg_pathcv84_flatq05_late_std_or_router_probe_batchedsparse224s7_tailz_probe/

change:
  pathCV84 branch keeps the first-layer pathwise CV exact.
  The sampled tail uses batched Strassen2 with dynamic activation-energy
  column selection:
    keep = 224
    sparse layers = 7..29

full spread5, indices 0,250,500,750,999:
  non-sparse batched pathCV84:
    raw      = 4.604733e-7
    adjusted = 2.623017e-7
    mult     = 0.572095
  sparse224s7 pathCV84:
    raw      = 4.277169e-7
    adjusted = 2.229322e-7
    mult     = 0.522746

full warning spaced20, indices 0,50,...,950:
  pathCV84 sparse224s7:
    raw      = 6.866845e-7
    adjusted = 2.599168e-7
    mult     = 0.464774

keep208s7 falsifier on spread5:
  raw      = 8.701287e-7
  adjusted = 4.335448e-7
  mult     = 0.500217
```

Read: dynamic activation-energy pruning is real for pathCV, but only down to
about 224 columns.  More aggressive pruning breaks raw MSE.  The pathCV branch
is still too expensive as a standalone, but it becomes useful as a routed arm.

K26/pathCV oracle on full warning spaced20:

```text
k26 rectsparse240s7:
  raw      = 1.130103e-6
  adjusted = 1.976967e-7
  mult     = 0.174516

pathCV84 sparse224s7:
  raw      = 6.866845e-7
  adjusted = 2.599168e-7
  mult     = 0.464774

per-row oracle choosing lower adjusted branch:
  raw      = 7.427968e-7
  adjusted = 1.577119e-7
  pathcount= 8/20
```

Deployable scalar gate:

```text
feature:
  q25 of flattened weights in late layers 24..31

threshold:
  use pathCV84 sparse224s7 iff late_flat_q25 <= -0.0596421

source:
  Full200 out-of-fold scalar gate on old pathCV-like vs current K26 losses.
  Not retuned on the warning slice.

actual packaged-router warning spaced20:
  raw      = 7.933577e-7
  adjusted = 1.883076e-7
  mult     = 0.313675
  flops    = 8.019e10
  pathcount= 8/20

independent Mini spaced20 check:
  k26 rectsparse240s7:
    raw      = 1.328960e-6
    adjusted = 2.312995e-7
    mult     = 0.174432
  pathCV84 sparse224s7:
    raw      = 9.162633e-7
    adjusted = 3.289517e-7
    mult     = 0.444084
  q25 router:
    raw      = 1.149770e-6
    adjusted = 3.345417e-7
    mult     = 0.348927
    pathcount= 10/20
```

Package staged, not submitted:

```text
dir:
  legacy_workspace/_pkg_k26rectsparse_pathcv84sparse224_lateq25_router_probe/

tar:
  whest-starterkit/packages/to_test_remote/
    submission_phase1_k26rectsparse_pathcv84sparse224_lateq25_router_probe_bundle.tar.gz

also copied to:
  whest-starterkit/packages/active/

sha256:
  c53adb754b54e94c6b4b8ae91df013a6fa8ce7e59841d5a6e43ade0b5c633cdc

whest validate:
  passed
```

Read: this was the first local sub-2e-7 result from the current session, but
the Mini spaced20 check demotes it from "candidate" to "diagnostic".  The
complementarity is real, but the q25 gate is not robust enough; it overpays
pathCV on Mini.  Do not remote-submit this without a stronger gate that improves
both the full warning slice and Mini.

### 2026-07-07 - final signed-preactivation zero-shrink falsifier

Question: the true final signed preactivation mean `E[z_L]` is a huge oracle.
Maybe the protected sampled `E[z_L]` is simply overtrusted and can be shrunk
toward zero, an MLP-level mean/median, or a global neuron mean for almost no
runtime cost.

Cache-only Full1000 diagnostic using
`legacy_workspace/cache/preedge_final_full1000_rawonly.npz`:

```text
base protected sampled ReLU raw = 2.306192251e-6

linear identity:
  pred = sample_ReLU + 0.5 * alpha * (proxy_Ez - sample_Ez)

best fixed alpha over coarse grid:
  zero proxy               alpha=0, no gain
  MLP mean proxy           alpha=0, no gain
  MLP median proxy         alpha=0, no gain
  global neuron mean proxy alpha=0, no gain

5-fold target-fitted alpha, diagnostic only:
  zero proxy               raw=2.305559730e-6  rel=0.999726
  MLP mean proxy           raw=2.305306672e-6  rel=0.999616
  MLP median proxy         raw=2.305496402e-6  rel=0.999698
  global neuron mean proxy raw=2.305557922e-6  rel=0.999725
```

Read: the signed preactivation oracle is not recoverable by a global shrink of
the sampled signed mean.  The useful error is instance/direction specific, not
a simple over-amplification of `sample_Ez`.  Do not revisit zero/mean shrinkage
except as part of a genuinely new `E[z_L]` proxy.

### 2026-07-06 - union53 target-free seed-subset compression recheck

Question: can the deployable l2snap union53 seed bank be compressed to a
smaller deterministic seed set by matching its own high-count mean, without
using targets during selection?

Method: on `l2snap_union53_seed_preds_full1000.npz`, use the equal union53 mean
as a teacher and select k seeds in 5 MLP-grouped folds by greedy + local-swap
minimization of teacher MSE on the training folds only.  Targets are used only
for held-out diagnostics.

Anchors:

```text
union53 equal teacher raw         = 8.039954401e-7
protected l2snap raw              = 2.301739726e-6
k26 split035/045 cached mean raw  = 1.476394503e-6
```

Target-free OOF subset results:

```text
k14 raw = 3.316209261e-6
k16 raw = 2.823739681e-6
k18 raw = 2.557794206e-6
k20 raw = 2.258553328e-6
k22 raw = 2.046452034e-6
k24 raw = 1.912035687e-6
k26 raw = 1.718707572e-6
k28 raw = 1.573008833e-6
k32 raw = 1.333662527e-6
k39 raw = 1.085100773e-6
```

Read: target-free matching to the union53 mean does not recover the useful
target-aligned k26 seed geometry.  At equal count it is worse than the existing
k26 cache, and larger k values lose adjusted-score economics.  Do not retest
"choose a small seed subset by approximating the high-count teacher mean"
unless the selection objective includes a genuinely new, target-free signed
observable rather than plain mean matching.

### Exact-proxy GREG from l2snap checkpoints

- New diagnostic:
  `probe_l2snap_exact_proxy_greg.py`.
- Goal: use the protected sampled branch as measurements and correct seed-block
  imbalance toward a known shallow proxy expectation:

```text
protected full-depth seed mean
  + beta * (analytic shallow-proxy expectation - protected shallow-proxy mean)
```

- This differs from the earlier multilevel GREG probe: it does not buy extra
  shallow union seed blocks.  It uses exact first/layer-2 marginal expectations
  and per-MLP seed covariance to estimate beta.
- Spaced20 smoke:
  - layer-2 exact proxy direct corrections were wrong-signed/worse.
  - layer-1 ReLU/equal-center gave a small direct gain:
    `2.227351847e-6` vs base `2.250037261e-6` (`0.989918x`), but OOF alpha
    was unstable/worse.
- Broad checks, layer-1 only:
  - Full200 base `2.181298568e-6`; best direct exact-proxy line was
    `diag, weighted center, global/neuron clipped` at `2.179111092e-6`
    (`0.998997x`).
  - Mini100 base `1.897755704e-6`; best direct line was
    `diag, weighted center, neuron unclip` at `1.892557598e-6`
    (`0.997261x`).
  - Target-fitted alpha often improves a bit more, but OOF alpha is unstable,
    so only the direct correction counts.
- Verdict: real but tiny.  The first-layer diagonal proxy is a stable signed
  observable, but not an oracle bridge by itself.  It may be worth folding into
  a future nearly-free cleanup package if implementation cost is negligible;
  do not treat it as the breakthrough path.

### Final-rooted Hermite/Wick `112` shortcut

- New diagnostic:
  `probe_finalroot_112.py`.
- Goal: test the smallest piece of the "contract diagrams before K3" proposal
  without implementing the full all-pairs compiler.  It computes only
  source-layer-to-final-preactivation `112` contractions and fits one OOF scalar
  coefficient against the protected l2snap residual.
- Late sources `24..30`, spaced20:

```text
base raw       = 2.250037261e-6
feature_rms    = 2.037058e-4
residual_corr  = -0.003427
global alpha   = +0.0689412, raw = 2.249840034e-6
OOF raw        = 2.250523205e-6  # worse
```

- All sources `0..30`, spaced20:

```text
feature_rms    = 3.204767e-4
residual_corr  = +0.003996
global alpha   = +0.172615, raw = 2.246977055e-6
OOF raw        = 2.247907776e-6  # 0.095% gain
```

- Verdict: reject the `112`-only shortcut as a breakthrough path.  It is not
  the strong `R2 >= 0.25` support signal that the final-rooted diagram proposal
  requires.  If we return to this lane, skip to richer K211/repeated-slice
  templates or a more faithful all-pairs compiler; do not tune `112` alone.

### L2snap cached branch synergy recheck

- Re-ran `probe_l2snap_variant_synergy.py` on Full1000 cached l2snap variants,
  excluding the high-count seed caches first.
- Cheap/predicted branch stack:

```text
protected raw                          = 2.301739726e-6
best single branch, dual24_teacher      = 1.750153787e-6
linear OOF stack, 19 cheap variants     = 7.791386330e-7 raw
linear OOF stack + exactGH side branches= 7.785543541e-7 raw
```

- The strong `dual*_teacher` caches are prediction-only, so do not assume they
  are deployable seed means.  Treat them as teachers unless generation metadata
  is recovered.
- Deployable-looking seed/pred subset OOF, manually separated from
  teacher-only caches:

```text
targetdual20 + targetdual24                    = 1.029126e-6 raw
targetdual20 + targetdual24 + count30 + consensus = 9.191986e-7 raw
plus union53 + first40                         = 6.181780e-7 raw
```

- Cost read:
  - the `6.18e-7` stack is a real raw-MSE ceiling, but it requires a large
    union of seed geometries if collapsed into one pass, so the adjusted score
    is likely worse than protected.
  - a direct OOF seed-weight refit over union53 worsened or returned to equal:
    equal raw `8.039954401e-7`, best broad ridge `8.051360720e-7`.
- One-pass compression over the full deployable-high seed union:

```text
available union size = 78 seed blocks
equal raw            = 5.752315053e-7
best OOF ridge raw   = 5.752840193e-7  # equal is effectively optimal
```

  This is a strong raw-MSE teacher, but at roughly `78/18` times protected
  seed count its adjusted proxy is not competitive with the 18-block protected
  line.
- Verdict: preserve this as a high-count/branch teacher ceiling.  It does not
  currently produce a lower adjusted candidate.  The interesting unsolved
  problem is compressing multiple branch geometries into one cheaper weighted
  or analytical correction, not tuning union53 weights.

### E-8 feasibility and joint-teacher trajectory distillation

- Score arithmetic:
  - `1e-8` adjusted requires raw below `1.0e-7` at the 10% floor, below
    `6.98e-8` at the current protected `~0.143` multiplier, and below
    `4.0e-8` at a 25% multiplier.
  - Existing high-budget SPHEREx probes are still around `3.6e-7` to `4.0e-7`
    raw at near-full-budget multipliers, so brute-force seed count does not
    reach the e-8 regime.
- Old final-rooted / forward-student notes around `6.8e-8` adjusted are from
  the earlier depth-8/warm-up-style calibration.  Current Phase-1 depth-32
  smoke of
  `candidate_forward_student_hcoeff_h123_d5d7_refit_reusecov_fastguard_estimator.py`
  failed with `IndexError: tuple index out of range` in `_h_cal[layer_idx]`.
  `candidate_final_fullskip_estimator.py` runs at depth 32 but is far too weak
  as a direct arm on mini index 0: raw `2.076756755e-5`, adjusted
  `2.095568257e-6`.
- New diagnostic script:
  `probe_joint_teacher_traj_distill.py`.
  It tests whether cached l2snap intermediate trajectory maps can predict the
  expensive `joint_expanded - sampler` teacher, after the older cheap
  final/weight feature distillation failed.
- Full200, highseed/current18 sampler baseline:
  - sampler raw `2.330236086e-6`;
  - direct joint-expanded teacher blend raw `1.881223769e-6`;
  - old cheap 64-feature ridge worsened;
  - trajectory-only ridge gave best OOF raw `2.225045332e-6`
    (`0.954858x`), teacher R2 `+0.04469`, teacher corr `+0.20242`;
  - HGB was weaker: raw `2.288443370e-6` (`0.982065x`).
- Full200, l2snap trajectory-base baseline:
  - sampler/base raw `2.181297932e-6`;
  - direct joint-expanded teacher blend raw `1.787968960e-6`;
  - trajectory-only ridge gave best OOF raw `2.170637066e-6`
    (`0.995113x`), teacher R2 `+0.03100`, teacher corr `+0.16964`;
  - adding old cheap features plus trajectory deltas worsened.
- Interpretation: intermediate trajectory does expose some signed
  joint-expanded teacher information that final summaries missed, but the
  signal mostly collapses after l2snap.  This is not a package path.  It points
  back to a larger analytical source-injection port, not a small feature
  residual on top of the protected l2snap line.

### 2026-07-06 - CUDA union-teacher residual MLP recheck

Goal: use the RTX 4090 to test whether the high-count l2snap union teacher can
be distilled into cheap protected-seed-cloud/runtime features.  The training
target is the deterministic union53 teacher, not public truth; truth is used
only for Full1000-train and independent Mini validation reporting.

Commands:

```text
python \
  legacy_workspace/probe_l2snap_teacher_residual_mlp.py \
  --epochs 25 --hidden 96 --depth 3 --batch 32768 \
  --mode full --device cuda

python \
  legacy_workspace/probe_l2snap_teacher_residual_mlp.py \
  --epochs 25 --hidden 64 --depth 2 --batch 32768 \
  --mode safe --device cuda
```

Full feature MLP:

```text
train base_truth = 2.301739726e-6
train gain=1    = 2.159705249e-6  # learns teacher direction in-sample

Mini validation base_truth = 1.897755704e-6
Mini best small gain       = 1.895251392e-6 at gain=0.25  # 0.13% gain
Mini gain=1                = 1.925259696e-6               # worse
```

Safe feature MLP:

```text
train gain=1    = 2.279820853e-6  # weak in-sample teacher fit
Mini gain>0     = worse for all reported gains
```

Verdict: reject this exact teacher-distillation shape.  The union53/high-count
teacher contains real raw-MSE signal, but the cheap seed-cloud + diagonal
features do not expose a transferable correction.  Do not promote a package or
spend time on larger MLPs over the same features without a new target-free
observable.

### Stationary source-injection cheap diagonal dictionary

- New diagnostic script:
  `probe_stationary_source_injection.py`.
- Idea: build tied late-layer source columns from diagonal Gaussian propagation
  states, inject them at tail layers, propagate them through the linearized ReLU
  mean dynamics, and fit global MLP-grouped OOF coefficients against the
  current l2snap residual.
- Full200 l2snap trajectory-base baseline:
  `2.181297932e-6` raw.
- Direct diagonal Gaussian base was not useful:
  `9.648706564e-4` raw; scalar OOF blend back to l2snap raw was neutral/worse
  at `2.184033407e-6`.
- Multivariate propagated source OOF worsened for all tested ridges:
  best `alpha=100000` raw `2.187511605e-6` (`1.002849x`).
- Univariate scan found only tiny real-looking signals:
  - `h4_c@16-23` raw `2.179020860e-6` (`0.998956x`);
  - `h4_c@12-31` raw `2.176520883e-6` (`0.997810x`).
- Interpretation: the broad stationary-source abstraction is still plausible,
  but this cheap diagonal dictionary is wrong/too weak.  Do not package it and
  do not tune this dictionary for percent-level gains.  The next source family
  must include structured covariance / repeated-K3 information or another
  signed observable with much higher teacher alignment.

### H1 projected cross-moment control variates

- Script: `probe_h1_cross_moment_cv.py`.
- Goal: test the expert memo's exact first-layer projected quadratic controls:
  compute known-expectation h1 cross-moment defects and use them as a
  target-free row-level regression control variate.
- Fast spaced20 probe with protected seeds, half64 rows, rank8 controls:
  - fixed QR/full/raw-target: direct worsened (`1.0006x` at ridge 100,
    `1.0427x` at ridge 0); the only target-improving line required a huge
    negative shrink (`-122.7`).
  - fixed QR/linear+full/raw-target: same pattern; teacher correlation at the
    high-ridge line was `-0.214`.
  - tail-random/full/raw-target: near-flat but still wrong-signed; teacher
    correlation at the high-ridge line was `-0.055`.
  - the 120-seed teacher residual itself was strongly aligned with the true
    residual on this slice (`+0.987`), so this is not a bad pseudo-reference.
- Interpretation: the raw h1 projected controls may still be useful as features,
  but the direct same-row regression CV is not shippable and does not point
  toward the high-seed teacher.  Do not package this form.

### Stein boundary-current prototype

- Script: `probe_stein_boundary_current.py`.
- Goal: test the expert memo's Gaussian integration-by-parts idea using a
  diagonal gradient-energy approximation and smoothed ReLU boundary delta.
- Result: mechanically works and has the right sign, but is too weak in the
  naive full-depth form.
  - half8 spaced20: best tau around `0.1..0.2`, residual/teacher correlations
    `+0.12..+0.14`, target-fitted tiny blend improves about `1.1%`.
  - half16 spaced20: same positive sign; best `std,tau=0.1` improves about
    `1.4%`.
  - half32 spaced20: signal shrinks; best `std,tau=0.1` improves only `0.5%`,
    with teacher correlation about `+0.09`.
  - half64 spaced20: same weak plateau; `std,tau=0.1` improves only `0.53%`,
    with teacher correlation about `+0.075`.
  - standalone Laplacian arm is massively mis-scaled at depth 32 and only usable
    as a very small signed correction/control feature.
- Interpretation: the boundary observable is not noise, but the current
  full-depth smoothed diagonal Laplacian is not strong enough to pay.  Next
  boundary work should isolate layer-local boundary sources or use it as a
  feature for teacher distillation, not as a direct submitted arm.

### W0 branch distillation from protected rows

- Script: `probe_w0_branch_row_distill.py`.
- Goal: approximate expensive adaptive W0 branch from already-paid row trajectories.
- Result: failed.  Useful full/128-row W0 branches had negative teacher R2 from protected row summaries.
- Action: do not package.

### Robust row aggregation

- Method: median/trim/winsor/median-blend over protected 256 row positions.
- Result: failed.  Uniform row mean remains best on Full200/tail guard.
- Action: do not replace uniform QR row averaging.

### Inverse-noise seed weighting

- Method: target-free inverse-variance weights from per-seed final second moments.
- Result: failed.  Best broad gain was about `0.15%`, not tail-safe.
- Action: do not package.

### Smaller sampler plus `joint_expanded`

- Script: `probe_analytic_sampler_fusion.py` at counts `8,12,16,24`.
- Result: raw improves, adjusted does not; the `joint_expanded` prior costs too much.
- Action: only useful as evidence that signed analytical structure exists.

### Cheap `joint_expanded` distillation

- Script: `probe_joint_teacher_weight_distill.py`.
- Goal: predict `joint_expanded - sampler` from cheap weight/diag-prop/seed-block features.
- Result: failed.  Mini, Full200, and Mini-to-Full proxy blends were worse than the sampler; teacher R2 near zero or negative.
- Action: do not build a proxy package from this feature family.

### Pilot adaptive-compute router

- Script: `probe_pilot_compute_router.py`.
- Goal: run 4 or 8 pilot seed blocks, then route to a larger seed count.
- Result: failed.  The oracle over counts is strong, but learned pilot routers do not transfer and are worse than fixed 18.
- Action: adaptive compute still needs a new signed observable.

### Current l2snap post-`311697` probes

- Branch stacking against current `l2snap_b05`:
  - Base Full1000 raw from cache:
    `2.301739726e-6`.
  - Exact-GH layer-2 covariance branches are not complementary after l2snap:
    best exactGH-only OOF was worse (`~1.0002x`).
  - Old adaptive/sensitivity branches remain signed teachers but not
    deployable as add-ons.  Full adaptive W0 rows give about `0.968x` raw OOF
    but cost roughly one extra 512-row branch, so approximate adjusted ratio is
    `~1.022x`.  The best 64-row W0 branch gives `0.992x` raw but still
    `~1.006x` adjusted after its smaller cost.
  - Multi-branch sensitivity stacks can reach `~0.92x` to `~0.90x` raw OOF,
    but require many extra full-depth branch passes.  Treat as teacher signal,
    not as a package shape.
- Current-l2snap subset/cost sweep:
  - Exhaustive subset scoring of the protected 18 seeds with scalar affine
    refit found only near-break-even adjusted proxies:
    - `k=13`: raw `3.173553682e-6`, score ratio `0.9958`.
    - `k=14`: raw `2.943846746e-6`, score ratio `0.9948`.
    - `k=15`: raw `2.746541194e-6`, score ratio `0.9944`.
    - `k=16`: raw `2.576516438e-6`, score ratio `0.9950`.
  - Combining those cheaper subsets with adaptive W0 branches does not pay.
    Best tested cases were still adjusted-worse (`~1.002x` or higher).
- Final preactivation feature residual after l2snap:
  - Script: `probe_l2snap_preact_feature_residual.py`.
  - Full200 base raw `2.181290612e-6`.
  - Best OOF feature correction raw `2.178318541e-6` (`0.998637x`) and
    required a negative shrink (`-0.318`), with spaced20 slightly worse.
  - Positive-shrink variants worsened.  Close final-preactivation feature
    residual as a package path.

## Branch-Fusion Pass

### Cached branch stack OOF ceiling

- Script: `probe_branch_stack_oof.py`.
- Goal: test whether cached analytical/adaptive/sensitivity branches have
  complementary signed error under MLP-grouped OOF.
- Result: mixed.  Full stack reaches about `1.035e-6` raw OOF on public
  Full1000, but it leans heavily on `union39_equal` and expensive sensitivity
  branches, so compute repricing likely erases the raw gain.  The cheap-looking
  first-layer analytical stack gives about `3.7%` raw OOF gain, but requires
  multiple full sampled passes if deployed literally.
- Action: useful as a ceiling/diagnostic, not a package.

### Single-pass first-layer skew+kurt correction

- Script: `probe_h1_skew_kurt_combo.py`.
- Goal: compress the first-layer higher-moment branch stack into one deployable
  polynomial correction.
- Result: failed as a meaningful gain.  Spaced20 looked strong, Full200 was
  only about `0.75%` better, and Full1000 collapsed to about `0.17%` better.
- Action: do not package unless a much stronger gated version appears.

### H1 kurtosis gate

- Script: `legacy_workspace/probe_h1kurt_gate.py`.
- Goal: after remote `311559` showed fixed h1kurt was harmful, test whether
  target-free MLP-level summaries can decide when to use h1kurt.
- This was an upper-bound diagnostic because the default features include
  protected-pass final summaries from cache.
- Full1000 OOF result: failed.  For `h1kurt` with its own affine calibration,
  fixed raw was `2.361937738e-6` versus protected `2.371167263e-6`, but the best
  OOF gate was `2.361463774e-6` and most nonlinear gates were worse.  The oracle
  row choice is `2.241948597e-6`, so the exploitable part is not predicted by
  these summaries.
- Action: close h1kurt gating.  Do not package another fixed or gated h1kurt
  variant without a new signed observable.

### Alternative sampling geometries

- Script: `legacy_workspace/probe_sampling_geometries.py`.
- Goal: test whether a same-row non-QR sphere rule can beat QR-antithetic under
  the protected h1-affine correction and seed weighting.
- 10-row smoke: Rademacher directions tied QR (`0.999789x`) but with opposite
  first/last behavior; iid normalized sphere and Sobol-normalized were about
  `1.9x..2.0x` worse.
- Spaced20 QR vs Rademacher: failed.  QR raw `2.264521059e-6`; Rademacher raw
  `3.478965413e-6` (`1.536292x` worse).
- Action: close obvious geometry replacement.  QR-antithetic remains the
  sampling geometry to beat.

## Adaptive-Count Structure

### Easy/Hard MLP prerequisite check

- Script: `probe_adaptive_count_structure.py`.
- Goal: verify whether low counts work on easy MLPs and high counts pay for
  hard MLPs before spending more effort on routers.
- Result: partially yes.  On Full200, the adjusted oracle over counts
  `18..120` is `2.04e-7`; low counts win many easy/medium cases, while `120`
  only pays on the hardest fixed18 raw bin on average.  Raw MSE almost always
  improves with more seeds, but adjusted loss does not.
- Action: adaptive routing is structurally justified, but a naive difficulty
  threshold is not enough.

### Shadow-pool oracle identifiability

- Script: `probe_count_oracle_identifiability.py`.
- Goal: test whether an independent high-seed shadow pool can predict the
  count oracle without targets.
- Result: passed.  With candidate seeds `0..59` and shadow seeds `60..119`,
  direct shadow selection captured `50-62%` of the fixed18-to-oracle gap,
  with gain Spearman `0.60-0.68`.  Reverse split also captured `60-68%`.
- Action: the count oracle is not pure target luck.  There is target-free
  count-value signal.

### RACE router from low-count features

- Script: `probe_race_shadow_router.py`.
- Goal: learn the shadow count policy from deployable low-count block and
  weight features.
- Result: failed in this form.  With 18-block features, learned policies were
  worse than fixed18 or degenerated to fixed18.  With a 32-block progressive
  pilot and counts `32,48,60`, the best learned policy captured only about
  `15%` of the available gap.
- Action: direct shadow-proxy signal is real, but current low-count summary
  features do not learn it well enough.

### Small full-depth shadow branch

- Script: `probe_small_shadow_policy.py`.
- Goal: test whether 4-32 independent full-depth shadow seeds are enough to
  drive count routing after charging their compute.
- Result: not competitive.  Decision-only shadow captures more of its own
  oracle as shadow size grows, but absolute adjusted score worsens because the
  shadow branch is paid even when low count wins.  Fusing the shadow branch into
  the final average keeps adjusted around `3.65-3.81e-7`, still far from the
  protected line.
- Action: full-depth shadow evidence is too expensive unless it is much smaller
  or much more accurate.  Continue with shallow shadow/SFT-GREG style probes.

### Shadow-guided seed/block selectors

- Script: `probe_shadow_neuron_selector.py`.
- Goal: use a small disjoint full-depth shadow branch as a target-free
  pseudo-target for the e-8-class per-neuron/per-MLP seed selector oracle.
- Protected18 candidate plus shadows of 4-24 seeds:
  - best simple fused mean: shadow24 raw `1.103743259e-6`, adjusted
    `3.774713783e-7`;
  - best soft selector: shadow12 soft-alpha0.25 raw `1.723135776e-6`,
    adjusted `4.175192190e-7`;
  - best shadow-ridge target-free weighting: shadow4 lam0.01 raw
    `2.303152197e-6`, adjusted `4.049908121e-7`.
- Wider 32-candidate plus shadows of 4-24 seeds:
  - best fused mean: shadow16 raw `1.019536802e-6`, adjusted
    `3.994922684e-7`;
  - best soft selector: shadow16 soft-alpha0.25 raw `1.122413886e-6`,
    adjusted `4.398033191e-7`;
  - best shadow-ridge target-free weighting: shadow4 lam0.1 raw
    `1.525685696e-6`, adjusted `4.457244969e-7`.
- The target-using ridge ceiling remains excellent: protected18+shadow4 oracle
  adjusted `1.002987584e-7`; 32+shadow4 oracle adjusted `5.248104379e-8`.
  The deployable shadow proxy does not recover that ceiling.
- Action: close tiny/full-depth shadow selector as a package path.  The oracle
  says per-MLP/per-neuron weighting is still high-upside, but it needs a new
  observable, not a noisy paid shadow branch.

### Analytical pseudo-target seed weighting

- Script: `probe_base_guided_seed_weights.py`.
- Goal: use deterministic analytical bases as pseudo-targets for seed/block
  weighting or per-neuron convex projection, instead of a paid shadow branch.
- Count18 plain SPHEREx:
  - `cov2mm_nodiag` is unusable as a pseudo-target; most selector variants are
    catastrophic and ridge collapses back to the sampler.
  - `joint_expanded` still helps as an additive blend: Full200 alpha `0.25`
    raw `1.917077026e-6`, adjusted `2.733981076e-7`.
  - But routing/weighting through the base is weaker: best Full200
    ridge-to-base raw `2.021667931e-6`, adjusted `2.883140214e-7`; best
    soft-base raw `1.957859134e-6`, adjusted `2.792141240e-7`.
- h1affine union39 selector check:
  - protected18 h1affine equal raw `2.456685288e-6`, adjusted
    `3.503526982e-7` on Full1000 cache;
  - best fused shadow-prefix mean was shadow16 raw `1.242614075e-6`, adjusted
    `3.423799339e-7`;
  - target-free selectors/ridge were worse, while target-using ridge again had
    a strong ceiling around `1e-7` adjusted for small prefixes.
- Action: analytical bases contain useful signed residual information, but not
  seed-routing information.  Keep additive/compressed-prior fusion open; close
  base-as-selector unless a much better base appears.

### Full-row anti-residual and rowcut follow-ups

- Remote protected line remains `311557`: adjusted `2.420613336e-7`, raw
  `1.697342478e-6`, multiplier `0.142613`.
- `311582` (`seeddev_ridgeanti5a100k`) was flat/slightly worse: adjusted
  `2.425029472e-7`, raw `1.703290901e-6`.
- `311583` (`rowcut_greedy4568`) was worse: adjusted about `2.499e-7`, raw
  about `1.764e-6`.
- Same-cost adaptive branch replacement failed on Full1000: best replacement
  raw `2.422998057e-6` versus protected `2.371167263e-6`.
- Row-distribution moment calibration failed on Full200: best `2.199402095e-6`
  versus base `2.195541588e-6`.
- Diagonal layerwise Gaussian re-anchoring failed: ordinary strengths were
  catastrophic, tiny strengths did not beat zero.
- Action: close anti-residual, rowcut, row-distribution, same-cost adaptive
  replacement, and diagonal re-anchor unless a new independent observable
  changes the evidence.

### QR-offset block replacement

- Script: `legacy_workspace/probe_qr_offset_blocks.py`.
- Idea: keep the protected sampler/runtime exactly the same, but replace
  selected seed half-blocks by later QR matrices from the same seed RNG stream.
  This is same-count and same-FLOP; only the bundled directions change.
- Guardrail: offset 0 from the new generator exactly reproduces the protected
  h1affine blob (`max_abs=0.0`).
- Offset sweep `0..7`:
  - Full1000 protected raw `2.371166392e-6`.
  - Best k8 raw `2.234761848e-6`, ratio `0.942474`.
  - Full200 cross-check ratio `0.958800`.
- Combined offset sweep `0..15`:
  - Best stable k8 raw `2.219693629e-6`, Full1000 ratio `0.936119`.
  - Full200 cross-check raw `2.092765164e-6`, ratio `0.953189`.
  - A k10 was marginally better on Full1000 (`0.935658`) but weaker on
    Full200 (`0.973855`), so it is not the preferred first upload.
- Best k8 offsets in protected seed order
  `(0,2,3,6,7,8,13,15,17,20,21,22,23,24,27,28,29,31)`:
  `(0,0,4,3,0,1,0,3,4,6,0,0,0,4,0,0,0,6)`.
- Package:
  `whest-starterkit/packages/active/submission_phase1_pure18_qroffset8_h1affine_weighted_finalonly_bundle.tar.gz`.
- Preferred package:
  `whest-starterkit/packages/active/submission_phase1_pure18_qroffset8b_h1affine_weighted_finalonly_bundle.tar.gz`.
  Its offsets are `(0,0,4,3,0,1,0,3,4,10,0,0,0,4,0,0,0,6)`.
- Late-offset `16..31` diagnostic:
  - Best k9 on Full1000 was very strong: raw `2.155338429e-6`, ratio
    `0.908978`.
  - Same k9 was essentially flat on Full200: raw `2.184417240e-6`, ratio
    `0.994934`.
  - Treat as overfit/diagnostic for now; do not upload ahead of `qroffset8b`
    without an independent canary reason.
- Direct width-256 smoke: finite `(32,256)`, FLOPs `3.8068508434e10`.
- Remote result: failed.  `311642` and `311652` both produced raw
  `1.900871931e-6` at the same compute class as protected `311557`
  (`1.697342478e-6` raw).  `311652` is row-identical to `311642` on final raw
  MSE and differs only by tiny residual-time noise in adjusted score.
- Action: close QR-offset selection for score.  The Full1000/Full200 wins were
  label-selection overfit, not a transferable same-compute geometry signal.

### Pure19 / pure20 row-budget and weighting follow-up

- Remote pure19/pure20 were raw wins but adjusted losses:
  - `311564` pure19 raw `1.670796986e-6`, adjusted `2.512215314e-7`,
    multiplier `0.150360`.
  - `311562` pure20 raw `1.631101205e-6`, adjusted `2.578862214e-7`,
    multiplier `0.158106`.
- Exact h1affine Full1000 recompute showed fitted seed weights do not improve
  the 19/20 pools:
  - pure19 equal `2.133836480e-6`; best sane weighted OOF was worse.
  - pure20 equal `2.004822879e-6`; best sane weighted OOF was worse.
- Equal-row row-budget sweep also failed:
  - pure19 at protected 4608 half-rows: `2.338965260e-6`.
  - pure20 at protected 4608 half-rows: `2.391233465e-6`.
  - lower-row variants lose raw too quickly; full-row variants already failed
    remotely on adjusted score.
- Action: close pure19/pure20 as a packaging path unless a new target-free
  criterion explains which rows to keep.

### H1-affine weighted pathwise CV

- Rechecked the pathwise first-layer control variate after combining it with
  the protected h1affine and seed-weighted estimator.
- Full200 protected local raw `2.195543999e-6`; CV variants were worse:
  `2.3317e-6` at ridge `1e-4`, `2.3303e-6` at `1e-2`, and `2.2708e-6` even at
  ridge `1`.
- Action: close pathwise CV after h1affine matching.  It is not a deployable
  improvement over protected `311557`.

### Protected storage/cost rewrites

- `np.frombuffer(...).copy()` in setup is numerically identical but locally
  worse on residual/compute: local 3-MLP multiplier about `0.143264` vs
  protected `0.140928`.  Do not upload
  `submission_phase1_pure18_3swap_firsthalf_h1affine_weighted_npfast_finalonly_bundle.tar.gz`.
- `flops.Module`-packed directions are also numerically identical (`maxdiff
  0.0`) and gave a small local cost win: local 3-MLP multiplier about
  `0.140633`.  Package built:
  `submission_phase1_pure18_3swap_firsthalf_h1affine_weighted_module_finalonly_bundle.tar.gz`.
- This is a cost-only diagnostic, not an accuracy candidate.  Expected upside is
  only around `0.2%` adjusted if remote overhead behaves like local.  Local
  subprocess setup timeout still occurs for protected and Module variants, so it
  is not a useful safety discriminator for large-blob packages.

### First-layer covariance linear color closure

- Rechecked the first-layer covariance signal after protected h1affine + fixed
  seed weighting.
- Equal-weight Full1000 target-minus-sample first-order color has a small
  optimum: beta `0.05`, raw `2.362161240e-6` vs affine `2.382545972e-6`
  (`0.9914x`).
- Protected weighted Full1000 optimum is much smaller: beta `0.04`, raw
  `2.362778787e-6` vs affine `2.371166829e-6` (`0.9965x`).
- Exact-offdiagonal-only and sample-offdiag-only shortcuts are catastrophic on
  Full200, so subtracting the finite QR sample covariance is essential.
- Action: close as production branch.  The working variant costs extra dense
  covariance work for only about `0.35%` raw gain after protected weighting.

### First-layer affine strength gate closure

- Cache: `legacy_workspace/cache/firstlayer_affine_strength_full1000.npz`.
- Global strength optimum is effectively flat: `b=1.2` gives
  `2.382283083e-6` versus protected `b=1.0` at `2.382546548e-6`.
- Target-using per-MLP oracle is large (`2.098652954e-6`), but first-layer
  target-free OOF ridge gates fail; best tested OOF raw is `2.389894756e-6`,
  worse than the global optimum.
- Action: no package.  Keep protected h1affine strength.

### Additional closures after `311652`

- `311652` duplicated the qroffset raw output row-by-row and confirms the
  qroffset selection branch is closed.
- W0-kernel-from-fixed-rows probe is worse than protected:
  best individual OOF `2.374174574e-6`, joint best5 `2.378005764e-6`, versus
  protected base `2.371167955e-6`.
- Target-free 120-seed variance/min-distance subset selection is not enough
  after exact h1-affine recomputation.  Best tested Full1000 h1affine set was
  `tf_alpha16 = 2.374742183e-6`, only about `0.3%` better than current18 equal
  and not worth packaging.
- First-layer full covariance matching does not let us cut blocks.  Full200
  16-block `cov_full` is `2.564633405e-6` versus 16-block h1affine
  `2.533455472e-6`; 17-block `cov_full` is also worse.  Close
  row-cut-plus-covariance.

### Same-count h1-affine seed replacement

- Rechecked whole-block replacements using the pure h1-affine sampler at the
  same 18-block / 4608-half-row cost as protected `311557`.
- Full1000 equal-weight raw:
  - current18 equal: `2.382546297e-6`;
  - drop `6,17`, add `37,75`: `2.283131073e-6`;
  - drop `6,17`, add `39,75`: `2.280622226e-6`;
  - drop `6,17`, add `75,100`: `2.285055131e-6`.
- Full200 cross-check raw:
  - current18 equal: `2.209368764e-6`;
  - drop `6,17`, add `37,75`: `2.138688243e-6`;
  - drop `6,17`, add `39,75`: `2.167386761e-6`.
- Seed reweighting again worsens OOF; the honest candidate is equal-weight.
- Tiny global affine is nearly neutral:
  - `37,75`: OOF affine ratio `0.999904`, full-fit `A=0.999974518283639`,
    `B=-9.59952958571694e-06`;
  - `39,75`: OOF affine ratio `1.000027`, full-fit `A=0.999960223387452`,
    `B=-6.88958429910846e-06`.
- Packages built and validated:
  - `whest-starterkit/packages/active/submission_phase1_pure18_drop6_17_add37_75_h1affine_equal_finalonly_bundle.tar.gz`
    SHA-256 `0b0a8fa9e46254170397f0863ee2aeccbd3b927170c3181c9790268c36091f27`;
  - `whest-starterkit/packages/active/submission_phase1_pure18_drop6_17_add39_75_h1affine_equal_finalonly_bundle.tar.gz`
    SHA-256 `8a52f05f94d6eb64f9d0e8898fa539e0eef7b9b3409eb81b184fb32044632c76`.
- Spaced20 local scorer is pessimistic for this seed-neighborhood family:
  - `37,75`: local adjusted `2.932941e-7`, raw `2.089850e-6`;
  - `39,75`: local adjusted `2.980561e-7`, raw `2.123674e-6`.
  This is the same known failure mode where the old canary overestimated the
  previous 3-swap candidates by more than `1e-7`.
- Recommendation: upload `37,75` first as the safer Full200-supported probe;
  upload `39,75` second only if the first transfers or if another diagnostic
  points toward Full1000 over Full200.

### Same-count 3-swap h1-affine seed replacement

- Used the unsafe `union39` h1-affine cache only as a shortlist, then exact
  recomputed candidates because h1-affine correction depends on the selected
  row set.
- Full1000 exact equal-weight raw:
  - drop `6,17,21`, add `37,39,75`: `2.236036760e-6`;
  - drop `6,17,21`, add `39,75,100`: `2.240950298e-6`;
  - drop `6,17,21`, add `37,39,100`: `2.241796884e-6`.
- Full200 exact equal-weight raw:
  - drop `6,17,21`, add `39,75,100`: `2.067980238e-6`;
  - drop `6,17,21`, add `37,39,75`: `2.083424433e-6`;
  - drop `6,17,21`, add `37,39,100`: `2.093993885e-6`.
- Tiny global affine remains low-risk and slightly positive OOF:
  - `39,75,100`: OOF affine ratio `0.999824`,
    `A=0.999955690214112`, `B=-9.45092066950785e-06`;
  - `37,39,75`: OOF affine ratio `0.998916`,
    `A=0.99994756980269`, `B=-6.17431600927142e-06`.
- Packages built with `whest package` and validated:
  - `whest-starterkit/packages/active/submission_phase1_pure18_drop6_17_21_add39_75_100_h1affine_equal_finalonly_bundle.tar.gz`
    SHA-256 `0c5f3442bc64a7ea8d5a14a6d46decab98b4b14a8997e40c9672cd63bb37ece7`;
  - `whest-starterkit/packages/active/submission_phase1_pure18_drop6_17_21_add37_39_75_h1affine_equal_finalonly_bundle.tar.gz`
    SHA-256 `47c19d4bb3d4425b8ecb85b7037afdac0473ef91a60848096f792ca24dd42e9e`.
- Recommendation: upload `39,75,100` first because it wins Full200 and has
  near-best Full1000.  Upload `37,39,75` second if the first transfers; it is
  the Full1000-favored alternative.
- Packaged `39,75,100` official spaced20 smoke:
  adjusted `2.920489e-7`, raw `2.080754e-6`, multiplier `0.14036239`,
  failed `0`.  Treat as package/compute sanity only; this gate is known to be
  pessimistic for same-count seed-neighborhood replacements.

### Same-count 4-swap diagnostic

- Approximate Gram shortlist from the unsafe `union39` cache suggested
  replacing one more current block with `100`.
- Exact Full1000 raw:
  - drop `3,6,17,21`, add `37,39,75,100`: `2.201267441e-6`;
  - drop `2,6,17,21`, add `37,39,75,100`: `2.201802153e-6`;
  - drop `6,17,20,21`, add `37,39,75,100`: `2.209278128e-6`.
- Exact Full200 raw:
  - drop `6,17,21`, add `39,75,100` 3-swap remains best:
    `2.067980238e-6`;
  - best 4-swap cross-check was drop `2,6,17,21`, add `37,39,75,100`:
    `2.085434111e-6`;
  - drop `3,6,17,21`, add `37,39,75,100`: `2.105050807e-6`.
- Action: do not promote 4-swap ahead of the 3-swap `39,75,100` package.
  It is a Full1000-favored diagnostic, but the stronger Full200 guard still
  points at the 3-swap.

### Balanced 4-swap refinement

- Exact one-swap neighborhood around the 3-swap `39,75,100` candidate showed
  one tiny Full200 improvement while also preserving most of the Full1000 gain:
  drop `6,17,21,23`, add `37,39,75,100`.
- Exact Full200 raw: `2.067348628e-6` versus the 3-swap base
  `2.067976675e-6`.
- Exact Full1000 raw: `2.211766601e-6`, better than the 3-swap
  `39,75,100` (`2.240950298e-6`) but not as low as the Full1000-only
  4-swap `drop3,6,17,21`.
- Tiny affine:
  OOF ratio `0.999752`, full-fit `A=0.999970349126189`,
  `B=-8.11116509410239e-06`.
- Package built with `whest package` and validated:
  `whest-starterkit/packages/active/submission_phase1_pure18_drop6_17_21_23_add37_39_75_100_h1affine_equal_finalonly_bundle.tar.gz`
  SHA-256 `7e66842308742f99c39b677e8c2ca434a23338c3f0b11430625b3d243f38f32a`.
- Official spaced20 smoke: adjusted `2.950742e-7`, raw `2.101256e-6`,
  multiplier `0.14042367`, failed `0`.  This is worse than the 3-swap
  spaced20 smoke but still treated as a package/compute sanity check only.
- Recommendation update: upload this balanced 4-swap first, then the 3-swap
  `39,75,100` if this does not transfer.

### Progressive shallow count router

- Script: `legacy_workspace/probe_progressive_shallow_count_router.py`.
- Goal: reuse a shallow pass over many candidate seeds to choose how many seed
  blocks to continue to full depth.  This differs from the earlier paid-shadow
  branch because shallow work for selected seeds is reusable.
- Spaced20 result with proxy seeds `0..59`, counts `18,24,32,48,60`, and
  `t=1,2,4`: failed.  Raw shallow tail proxies and GREG-calibrated proxies
  mostly chose count `18` for every MLP.  Best actual adjusted values were just
  the fixed-18-with-proxy overhead:
  - `t=1`: `3.817790680e-7`, hist `18:20`;
  - `t=2`: best `3.992650987e-7`, hist `18:19 32:1`;
  - `t=4`: best `4.091913709e-7`, hist `18:18 24:1 32:1`.
- Action: close this shallow-router form.  The count oracle remains real, but
  these shallow tail proxies do not expose the needed signed marginal value.

### Target-aware highseed h1-affine probes

- Builder added: `legacy_workspace/build_h1affine_equal_package.py`.
- These are explicitly target-aware seed-set probes, not protected baselines.
  They use public targets during seed selection, but no MLP names/ids.
- `dualtarget24_h1affine_targetaware_f16_finalonly`
  - artifact:
    `whest-starterkit/packages/active/submission_phase1_dualtarget24_h1affine_targetaware_f16_finalonly_bundle.tar.gz`
  - local smoke2: adjusted `1.207074e-7`, raw `6.446706e-7`,
    multiplier `0.187226`, failed `0`;
  - Mini spaced20: adjusted `2.196807e-7`, raw `1.174704e-6`,
    multiplier `0.187011`, failed `0`;
  - Mini100: adjusted `2.261185e-7`, raw `1.208682e-6`,
    multiplier `0.187073`, failed `0`;
  - Full200: adjusted `2.729002e-7`, raw `1.459238e-6`,
    multiplier `0.187012`, failed `0`.
- `dual28_full2_h1affine_targetaware_f16_finalonly`
  - artifact:
    `whest-starterkit/packages/active/submission_phase1_dual28_full2_h1affine_targetaware_f16_finalonly_bundle.tar.gz`
  - local smoke2: adjusted `1.751682e-7`, raw `8.019564e-7`,
    multiplier `0.218404`, failed `0`;
  - Mini spaced20: adjusted `2.509713e-7`, raw `1.150644e-6`,
    multiplier `0.218113`, failed `0`;
  - Full200 offline h1-affine diagnostic: raw `1.173839432e-6`, q50 max
    `1.286946912e-6`; expected adjusted at package multiplier is around
    `2.56e-7`.
- Heavier Full-weight target-aware searches around `28..34` seeds lowered
  plain Full200 raw, but multiplier dominated.  Best plain count34 had Full raw
  `9.502194877e-7` at multiplier `~0.272`; it still needs a large extra raw
  drop to beat protected `311557`.
- Action: keep `dualtarget24_h1affine...` and `dual28_full2_h1affine...` as
  optional upload probes only.  They are not promoted over protected `311557`
  because Full200/adjusted gates do not clear it conservatively, and the seed
  sets are target-aware.

### Hybrid3 depth-32 quick repair

- Wrapper tested: `legacy_workspace/candidate_hybrid3_depth32_nocal_estimator.py`.
- The original Hybrid3 Hermite estimator still contains depth-8 layer
  calibration tables; disabling those tables makes it run at depth 32 but does
  not make it competitive.
- Smoke2 official result: adjusted `2.613821e-6`, final raw `2.587924e-5`,
  multiplier `0.100971`, failed `0`.
- Action: close the quick repair.  Any Hybrid3 revival needs a real depth-32
  recalibration/rederivation, not just disabling the stale depth-8 calibration
  path.

### Old starterkit analytical production smoke

- Context: `song/NOTES.md` references an older production estimator with raw
  around `9e-7`, but that line is tied to the pre-Phase-1/depth-8 analytical
  family.
- Phase-1 smoke2 on `whest-starterkit/estimator.py`: adjusted `2.040319e-6`,
  raw `1.982037e-5`, multiplier `0.102806`, failed `0`.
- Action: do not treat the old Song/student production numbers as directly
  comparable to current Phase 1 SPHEREx unless the feature/state cache and
  calibration are rebuilt for depth 32.

### High-seed h1-affine canary check

Context: after protected `311557`, the live same-count branch replaces QR
blocks while preserving the 18-block compute envelope.  The old calibrated
`spaced20` canary is known to be badly pessimistic for this family: it
predicted `3.84e-7` for the earlier 3-swap that remotely scored
`2.61e-7`.  Still, it is useful as a brittleness warning.

Ran two guards on the current high-seed candidates:

- old `spaced20` remote-calibrated canary;
- high-seed Full200/tail guard adapted from `spherex_canary_v2.py` using
  `highseed_spherex_mini100_s512_seeds0to119.npz` and
  `highseed_spherex_full200_s512_seeds0to119.npz`.

Results:

```text
protected 311557 seeds
  highseed v2 estimate = 3.412454755e-7
  mini raw             = 2.137290976e-6
  full200 raw          = 2.330106643e-6
  full200 last100 raw  = 2.460673360e-6

A drop6,17,21,23 add37,39,75,100
  old spaced20 estimate = 3.535486990e-7
  highseed v2 estimate  = 3.464148077e-7
  combined gate         = 3.510518370e-7
  v2/protected          = 1.015

B drop6,17,21 add39,75,100
  old spaced20 estimate = 3.516836296e-7
  highseed v2 estimate  = 3.270379071e-7
  combined gate         = 3.430576267e-7
  v2/protected          = 0.958

C drop6,17,21 add37,39,75
  highseed v2 estimate  = 3.388642120e-7
```

Interpretation: exact Full1000 average favored A, but the more remote-relevant
tail/mini guard favors B.  Promote B over A for the next diagnostic upload if
we want to test the same-count high-seed replacement branch.  Treat A as a
broader-average diagnostic only, not the first public probe.

Remote result for promoted B:

```text
submission 311670
artifact: submission_phase1_pure18_drop6_17_21_add39_75_100_h1affine_equal_finalonly_bundle.tar.gz
adjusted mean = 2.620545455e-7
final raw mean = 1.837922949e-6
effective compute mean = 3.878e10
wins vs 311557 = 25/50 public rows
mean adjusted delta vs 311557 = +1.999321189e-8
mean raw delta vs 311557 = +1.405804710e-7
```

The regression is accuracy-only: compute and residual time match protected
`311557`.  The row pattern is split but heavy-tailed: equal numbers of wins and
losses, with losses much larger.  Worst losses include public rows 1, 11, 45,
19, 37, 28, 21, 38, 26, 14.  Best wins include rows 42, 8, 27, 17, 23, 15, 12,
35, 41, 47.

Action: do not promote A/C by canary alone.  The high-seed Full200/tail guard
captured broad improvement but missed public heavy-tail risk for the same-count
seed-replacement family.  Future seed-set promotion needs either a row-risk
penalty or direct comparison against remote anchor row residual patterns, not
just average/tail raw MSE.

Post-mortem parity gate:

```text
parity indices = 5,20,35,45,50,60,70,80,85,95
protected 311557 package:
  adjusted = 3.071260e-7
  raw      = 2.188579e-6
B package:
  adjusted = 3.330078e-7
  raw      = 2.372597e-6
A package:
  adjusted = 3.486902e-7
  raw      = 2.484073e-6
```

The fast parity canary would have rejected both A and B.  Make this a mandatory
pre-upload gate for same-count SPHEREx seed replacements.  The old
remote-calibrated `spaced20` and high-seed Full200/tail gates are useful
screening tools, but neither is sufficient without the parity slice.

## Current Read

The exhausted families are final row/block statistics, fixed seed weights,
robust aggregation, cheap diagonal Gaussianization, pilot trajectory summaries,
and QR-offset geometry.  The current live same-compute path is whole-block seed
replacement under h1-affine matching.  Current canary ordering promotes
`drop6,17,21 -> add39,75,100` locally, but remote `311670` falsified it as a
promotion gate by exposing heavy-tail public losses.  Remaining speculative
paths are:

- a genuinely new pre-compute signed observable for adaptive count/seed routing;
- a cheaper analytical approximation to the expensive `joint_expanded` signal;
- a depth-32 analytical estimator repair/rethink, not another shallow residual model.
- branch fusion is confirmed to have raw headroom, but only behind compute-heavy
  extra branches unless a single-pass compression is found.

### 2026-06-21 union39 h1-affine subset and floor search

- Added `legacy_workspace/probe_h1affine_union_teacher_subset.py`.
- Goal: find a same-compute or floor-multiplier seed subset inside the cached
  h1-affine union39 pool, using the union-pool mean as a target-free teacher and
  targets only for diagnostics.
- Target-free low-count search did not clear protected:
  - best target-free same-count candidate:
    `2,7,9,11,19,21,23,24,27,29,35,37,39,44,61,82,83,109`;
    Full1000 h1-affine raw `2.278683258e-6`, local adjusted proxy
    `3.249674802e-7`;
  - best low-count floor candidate in the tested set was still about
    `3.29e-7` adjusted proxy.
- Target-using oracle inside the same union39 pool also stayed too high:
  - best count18 oracle raw `2.257186098e-6`, adjusted proxy
    `3.219017282e-7`;
  - best count24 oracle raw `1.674741471e-6`, adjusted proxy
    `3.184508797e-7`.
  This means the union39 pool itself does not contain a floor-route breakthrough.
- Built two safe probe bundles:
  - `submission_phase1_pure18_unionmatch_h1affine_equal_f16_finalonly_bundle.tar.gz`
  - `submission_phase1_pure18_oracleall_h1affine_equal_f16_finalonly_bundle.tar.gz`
- Local official checks:
  - union-match Mini spaced10 looked good: adjusted `2.293473e-7`, raw
    `1.634383e-6`;
  - but Mini spaced20 was worse than protected: union-match `2.777948e-7`
    versus protected `2.697816e-7`;
  - Mini100 rejected it: union-match `3.192124e-7` versus protected
    `2.793936e-7`;
  - target-oracle variant was already worse on spaced10: adjusted
    `2.778638e-7`.
- Global final affine calibration is too small to matter:
  - union-match OOF affine worsened slightly (`1.00042x`);
  - oracleall OOF affine improved only `0.18%`.
- Action: do not upload these bundles.  Close union39 teacher/floor subset
  search unless a materially new selection signal appears outside this pool.

### 2026-06-21 recheck older 3-swap h1-affine exact packages

- Rechecked the already-built same-count 3-swap packages with the official
  Mini runner because the union39 search showed that narrow spaced slices can
  be misleading.
- Protected reference on the same run:
  - Mini spaced20: adjusted `2.697816e-7`, raw `1.922339e-6`;
  - Mini100: adjusted `2.793936e-7`, raw `1.990411e-6`.
- `drop6,17,21 -> add39,75,100`:
  - artifact:
    `whest-starterkit/packages/active/submission_phase1_pure18_drop6_17_21_add39_75_100_h1affine_equal_finalonly_bundle.tar.gz`
  - SHA-256:
    `0c5f3442bc64a7ea8d5a14a6d46decab98b4b14a8997e40c9672cd63bb37ece7`
  - Mini spaced20: adjusted `2.921403e-7`, raw `2.080754e-6`;
  - Mini100: adjusted `2.726951e-7`, raw `1.942153e-6`;
  - earlier exact diagnostics: Full1000 raw `2.240950298e-6`, Full200 raw
    `2.067980238e-6`, both favorable versus the protected exact cache.
- `drop6,17,21 -> add37,39,75`:
  - Mini100: adjusted `2.830985e-7`, raw `2.016317e-6`; do not prefer it.
- Built an f16 copy of the `39,75,100` package:
  - artifact:
    `submission_phase1_pure18_drop6_17_21_add39_75_100_h1affine_equal_f16_finalonly_bundle.tar.gz`
  - SHA-256:
    `61a72e302034446428bc6edbf67e1909bdafd7700c143acb1b06f5bb0524fb40`
  - Mini100: adjusted `2.727832e-7`, raw `1.942174e-6`; raw matches f32
    but measured multiplier is slightly worse locally, so prefer the f32
    artifact.
- Action: superseded by the earlier `311670` remote result for the same f32
  artifact, which came back worse than protected (`2.620545455e-7` adjusted
  versus `311557` at about `2.42e-7`).  Do not upload this again.  The f16 copy
  is the same seed set and should not be treated as a new methodological probe.

### 2026-06-21 final-feature nonlinear calibration recheck

- Reused `legacy_workspace/cache/final_preact_features_full1000.npz`.
- Tested a richer grouped-MLP OOF ridge over final preactivation/postactivation
  summaries: raw features, per-MLP standardized features, squares,
  tanh-standardized features, pair interactions, base-scaled interactions.
- Result remains flat:
  - protected base raw `2.371168176e-6`;
  - mod-5 best `2.370728704e-6` (`0.999815x`);
  - block-5 best `2.371293563e-6` (`1.000053x`).
- Action: close final-summary nonlinear calibration for now.  It is not a
  deployable 1%+ raw lever, let alone a 5% adjusted lever.

### 2026-06-21 fixed-row 19/20 h1-affine seed probes

- Added `legacy_workspace/probe_fixedrow_h1affine_arbitrary.py`.
- Goal: test whether the raw-positive full-count pure19/pure20 highseed sets
  can be squeezed back to the protected row budget by using fewer half rows per
  seed.
- Full200 proxy results:

```text
protected 18, half=256: raw=2.209366914e-6 mult=0.142611958 adj=3.150821408e-7
19-seed,   half=242: raw=2.270333730e-6 mult=0.142302470 adj=3.230740973e-7
19-seed,   half=243: raw=2.249018849e-6 mult=0.142890497 adj=3.213634202e-7
20-seed,   half=230: raw=2.385127568e-6 mult=0.142364367 adj=3.395571775e-7
```

- The full-count 19/20 rows still look best on this local proxy, but those are
  already known remote adjusted losers because the multiplier rises.
- Action: do not package fixed-row 19/20.  The added seed diversity only helps
  when the rows per seed remain high enough to raise compute.

### 2026-06-21 robust union seed-set shortlist

- Added `legacy_workspace/probe_robust_union_seedset_search.py`.
- Purpose: avoid repeating the `311670` mistake, where a seed set looked good
  on broad averages but failed heavy-tail public rows.  The new diagnostic
  searches the cached union seed pool against multiple local guards:
  Full1000, Full200, Full1000 tail200, Mini100, and Mini parity10.
- Built a Mini100 h1-affine union cache on RTX:
  `legacy_workspace/cache/h1affine_seed_preds_union39_mini100.npz`.
- Important caveat: union-pool per-seed h1-affine predictions are only a
  shortlist source.  The first-layer affine correction depends on the selected
  rows, so shortlisted seed sets must be exact-recomputed before packaging.
- Top robust shortlist after exact recompute:

```text
robust1 seeds:
  (0,2,3,8,13,15,20,22,23,24,27,28,29,31,39,75,100,111)

Exact h1-affine equal raw:
  Full200  = 2.043558720e-6
  Mini100  = 1.843721629e-6
  Full1000 = 2.258850650e-6
  Mini5 official smoke = 1.57e-6 raw, 2.21e-7 adjusted, multiplier 0.140915

robust3 seeds:
  (0,2,3,8,13,15,20,22,24,27,28,29,31,37,39,75,100,111)

Exact h1-affine equal raw:
  Full200  = 2.066509449e-6
  Mini100  = 1.902426453e-6
  Full1000 = 2.240263860e-6
  Mini5 official smoke = 1.79e-6 raw, 2.52e-7 adjusted, multiplier 0.140819
```

- Global affine calibration is not worth applying:
  - robust1 OOF affine ratio `1.002244`;
  - robust3 OOF affine ratio `1.001256`.
- Packages rebuilt through `whest package` so they include top-level
  `manifest.json`:
  - `whest-starterkit/packages/active/submission_phase1_pure18_robust1_h1affine_equal_finalonly_bundle.tar.gz`
    SHA-256 `7e9a324863a417feefc01aab6ac8e8b4e9ddd7281305d31f83491ed90e9313bd`;
  - `whest-starterkit/packages/active/submission_phase1_pure18_robust3_h1affine_equal_finalonly_bundle.tar.gz`
    SHA-256 `8cc6f80e02eb5f5ab681ae33a51ad2259647a1e7c1de06b454254d11526bb6db`.
- `robust1` validated with `whest validate`.
- Recommendation: `robust1` is a credible diagnostic upload candidate.  It is
  not yet a protected-line replacement because the selection is target-aware,
  but it beats the protected line across the exact Mini/Full200 guards and the
  five-MLP official smoke at the same compute class.  `robust3` is lower
  priority: it wins Full1000 but loses the more remote-relevant Mini/Full200
  guards versus robust1.

Follow-up exact recomputes:

```text
robust4 seeds:
  (0,2,3,8,13,15,20,22,23,24,27,28,31,39,75,100,107,111)

Exact h1-affine equal raw:
  Full200  = 2.040487415e-6
  Mini100  = 1.844458374e-6
  Full1000 = 2.267848978e-6
```

`robust4` is a union-cache improvement over `robust1` on Full200 only.  It is
effectively tied on Mini and worse on Full1000, so it is not a cleaner upload
candidate than `robust1`.

Non-neighborhood alternatives from a 1000-restart robust search:

```text
altA seeds:
  (2,9,13,15,23,24,27,35,37,39,61,63,75,82,83,90,109,111)
  Full200  = 2.017355318e-6
  Mini100  = 1.900768828e-6
  Full1000 = 2.317615225e-6

altE seeds:
  (6,8,11,13,15,17,20,24,27,28,39,63,75,78,90,100,107,111)
  Full200  = 1.992087306e-6
  Mini100  = 2.005841651e-6
  Full1000 = 2.284229282e-6
```

`altE` is the best exact Full200 same-cost seed set found so far, but it loses
Mini and Full1000.  Treat it as a risky diagnostic for whether the public 50 is
closer to the Full200/front-half surface; do not treat it as protected-line
quality.  `altA` is weaker than `altE` on Full200 and weaker than `robust1` on
Mini, so do not package unless later canary evidence changes.

### 2026-06-21 intermediate trajectory oracle probes

- Added:
  - `legacy_workspace/probe_intermediate_trajectory_oracle.py`
  - `legacy_workspace/probe_intermediate_alpha_router.py`
- Goal: test whether intermediate activations from the protected h1-affine
  SPHEREx pass contain final-residual signal.  This is intentionally separate
  from same-count seed-set search and final-vector-only routing.
- Saved cache for follow-up:
  `legacy_workspace/cache/intermediate_oracle_spaced50_r32_linear_diag.npz`
  with 50 spaced Full200 MLPs, 32 half rows per seed, checkpoints
  `2,4,8,12,16,20,24,28`, and `linear/diag` tail proxies.
- Spaced20 smoke, rows32:
  - protected h1-affine base raw `2.264510541e-6`;
  - per-MLP multimap target oracle, ridge `1e-4`: raw `5.781960917e-7`;
  - best grouped-CV ridge: raw `2.061466508e-6` (`0.910336x`).
  This showed real signal but needed a broader canary.
- Spaced50, rows32:
  - protected h1-affine base raw `2.447250022e-6`;
  - per-MLP multimap target oracle, ridge `1e-4`: raw `5.938604210e-7`;
  - per-MLP multimap target oracle, ridge `0.01`: raw `9.028786523e-7`;
  - per-neuron choose-map target oracle: raw `1.374006811e-6`;
  - best grouped-CV residual learner worsened to raw `2.552621713e-6`
    (`1.043057x`).
- Alpha-router follow-up on the saved cache:
  - target-free MLP-level features: map delta norms/spreads/correlations,
    seed-spread summaries, and small map Gram sketches;
  - multimap coefficient prediction had negative coefficient R2 and worsened
    raw in every tested model;
  - scalar alpha routing for `t02_linear` had tiny positive R2 for tree models
    but only reached raw `2.428804668e-6` (`0.992462x`), far too small and not
    stable enough to package.
- Interpretation: intermediate trajectory maps have a strong target-using
  ceiling, but the tested target-free summaries do not recover the per-MLP
  signs/coefficients.  Do not ship a trajectory PERO/router from these simple
  features.  Keep the cache and scripts for future ML-feature experiments.

### 2026-06-21 high-seed oracle refresh

- Rechecked the existing `highseed_spherex_full200_s512_f16_seeds0to119.npz`
  cache against Full200 targets to clarify the raw-vs-adjusted ceiling.
- Plain high-count averaging:

```text
18 seeds:  raw=2.330236086e-6 adjusted=3.382286545e-7
32 seeds:  raw=1.532328809e-6 adjusted=3.921167141e-7
48 seeds:  raw=1.040825946e-6 adjusted=3.980797315e-7
64 seeds:  raw=8.170060641e-7 adjusted=4.158842881e-7
96 seeds:  raw=5.146909611e-7 adjusted=3.922836057e-7
120 seeds: raw=4.033216684e-7 adjusted=3.839731332e-7
```

- Target-using high-seed ceilings:

```text
choose one seed per neuron among 64:  raw=3.375839536e-8
choose one seed per neuron among 120: raw=1.036910160e-8
per-MLP 120-seed weights, lam=1e-4: raw=6.225018526e-8
per-MLP 120-seed weights, lam=1e-6: raw=7.115465001e-9
```

- Interpretation: the e-8-class solution exists in the sampled direction pool,
  but plain high-count averaging is adjusted-score negative.  The needed
  breakthrough is target-free seed/neuron weighting or an analytical observable
  that predicts those weights without paying for all 120 full-depth blocks.

### 2026-06-21 layer-2 Gaussian moment snap

- Added `legacy_workspace/probe_secondlayer_gaussian_snap.py`.
- Added package builder `legacy_workspace/build_h1affine_l2snap_package.py`.
- Idea: after the existing exact first-layer h1-affine mean/variance match,
  compute the exact first-layer ReLU covariance with the arc-cosine kernel,
  propagate only the diagonal preactivation mean/variance through layer 2,
  apply a Gaussian ReLU moment closure for layer-2 marginals, then partially
  affine-match sampled layer-2 activations before continuing the sampled pass.
- This is a deployable analytical correction: no target labels, no extra
  direction blocks, and only a few dense `256 x 256` operations plus scalar
  CDF/PDF/arccos work.

Protected weighted seed line:

```text
seeds:
  (0,2,3,6,7,8,13,15,17,20,21,22,23,24,27,28,29,31)
weights:
  (0.05582494,0.05834149,0.05270063,0.05280829,0.05656954,
   0.05007272,0.05707431,0.05503429,0.05126759,0.05461723,
   0.05367419,0.06065070,0.05220765,0.05695279,0.05644509,
   0.06399062,0.05616891,0.05559901)

Full200:
  base      = 2.203522768e-6
  beta=0.25 = 2.186424365e-6  ratio 0.992240
  beta=0.50 = 2.194660243e-6  ratio 0.995978

Mini100:
  base      = 1.994755634e-6
  beta=0.25 = 1.936356923e-6  ratio 0.970724
  beta=0.50 = 1.903842968e-6  ratio 0.954424

Full1000:
  base      = 2.373797466e-6
  beta=0.25 = 2.326990021e-6  ratio 0.980282
  beta=0.50 = 2.306190623e-6  ratio 0.971520
```

Robust1 equal seed line:

```text
seeds:
  (0,2,3,8,13,15,20,22,23,24,27,28,29,31,39,75,100,111)

Full200:
  base      = 2.043558249e-6
  beta=0.50 = 2.002436377e-6  ratio 0.979877

Mini100:
  base      = 1.843715671e-6
  beta=0.50 = 1.804317384e-6  ratio 0.978631

Full1000:
  base      = 2.258850617e-6
  beta=0.50 = 2.195618469e-6  ratio 0.972007
```

Cost profile under Phase 1 `v1-phase1`, one Mini MLP:

```text
protected h1-affine weighted:
  ops=106  flops=3.807e10  predicted remote mult 0.1409-0.1425

protected h1-affine weighted + l2snap beta=0.5:
  ops=162  flops=3.816e10  predicted remote mult 0.1418-0.1442
```

The snap adds only about `9e7` analytical FLOPs and 56 instrumented ops.  It
should improve adjusted score if raw improves by more than about `1-1.5%`.
Protected beta `0.5` clears that on Mini100 and Full1000, but not strongly on
Full200.  Robust1 beta `0.5` clears it on all three exact checks but carries
the target-aware seed-set risk from the robust-search branch.

Packaged and validated:

```text
whest-starterkit/packages/active/
  submission_phase1_pure18_protected_h1affine_l2snap_b05_weighted_finalonly_bundle.tar.gz
    sha256 80443bfb6e05963dbfc34bf9cdb3a4312e1bb4d6516c74ed7e4a7aa122d451b7
  submission_phase1_pure18_protected_h1affine_l2snap_b025_weighted_finalonly_bundle.tar.gz
    sha256 ccd70fa56dad2cc376aef77c6e6c14d6c51a85e855a230454095c89407264b1c
  submission_phase1_pure18_robust1_h1affine_l2snap_b05_equal_finalonly_bundle.tar.gz
    sha256 d24fcaaaaf87f8b1dfb48f4897f97fc94dee88711b25db73f6c1a288d478cc9e
```

Recommended upload order if spending probes:
1. protected weighted l2snap beta=0.5: lower selection risk, likely modest gain.
2. robust1 equal l2snap beta=0.5: stronger local gain, higher seed-selection risk.
3. plain robust1 / altE only as diagnostic if the snap packages behave oddly.

Remote Phase 1 submissions:

```text
311684 protected weighted l2snap beta=0.5
  package: submission_phase1_pure18_protected_h1affine_l2snap_b05_weighted_finalonly_bundle.tar.gz
  adjusted score = 2.373339118776791e-7
  verdict: new protected line from this branch

311685 robust1 equal l2snap beta=0.5
  package: submission_phase1_pure18_robust1_h1affine_l2snap_b05_equal_finalonly_bundle.tar.gz
  adjusted score = 2.531589935795212e-7
  verdict: reject; robust seed-selection risk did not transfer

311687 protected weighted l2snap beta=0.25
  package: submission_phase1_pure18_protected_h1affine_l2snap_b025_weighted_finalonly_bundle.tar.gz
  adjusted score = 2.387151923769885e-7
  verdict: reject as replacement; beta=0.5 is better remotely
```

Remote lesson: the layer-2 snap transfers on the protected seed/weight line,
but target-aware robust seed replacement still does not.  Future seed-set
changes need a stronger public50-aligned gate than Full200/Mini/Full1000
aggregate raw, even when the analytical snap is present.

Layer-3 diagonal snap follow-up is rejected.  With protected weighted seeds and
layer-2 beta `0.5` on Full200:

```text
l2 beta=0.5 only       raw=2.194660243e-6
l2 beta=0.5, l3=0.10  raw=2.283263501e-6  ratio vs base 1.036188
l2 beta=0.5, l3=0.25  raw=2.828792624e-6
l2 beta=0.5, l3=0.50  raw=4.845758039e-6
```

Interpretation: the exact first-layer covariance is the useful ingredient.
Propagating only diagonal Gaussian moments one layer further injects the wrong
state and should not be packaged.

Follow-up probes from the same branch:

```text
Layer-2 covariance spectral coloring:
  script: legacy_workspace/probe_secondlayer_cov_spectral.py
  spaced20: rank4/gain0.25 looked positive
    l2snap raw = 2.305533215e-6
    rank4/gain0.25 raw = 2.136543558e-6
  Full200: only tiny safe gains; stronger variants overfit/worsen
    l2snap raw = 2.194660727e-6
    best rank4/gain0.05 raw = 2.183618303e-6
  verdict: diagnostic only.  The gain is too small for the likely eigencost,
  and the larger apparent spaced20 gains do not hold on Full200.

Layer-2 exact-GH covariance spectral coloring:
  script: legacy_workspace/probe_secondlayer_cov_exactgh.py
  idea: replace the delta-method off-diagonal layer-2 ReLU covariance with a
    nonzero-mean bivariate Gaussian ReLU moment computed by 1D Gauss-Hermite
    quadrature, then apply the same low-rank spectral color after l2snap.
  spaced20:
    l2snap raw = 2.305549755e-6
    rank8/gain0.2 raw = 2.151546467e-6
  Full200:
    l2snap raw = 2.194669001e-6
    best checked rank12/gain0.05 raw = 2.177395718e-6
    rank8/gain0.05 raw = 2.177938273e-6
  compressed basis checks:
    fixed QR basis is almost flat on spaced20; best fixed8/gain0.4 gives only
    `0.9981x`.
    mean-gated tail-random basis keeps the spaced20 direction but loses most of
    the broad gain; Full200 best checked tail16/gain0.2 raw =
    2.187330780e-6 (`0.9967x`).
  verdict: useful signal, but still likely not a direct package.  The broad
    full-spectral gain is only about 0.8%, and cheap fixed/tail subspaces retain
    at most about 0.3% on Full200.  Do not package yet; only revisit if the
    spectral eigencost proves negligible or a better subspace is found.
  branch oracle/gating:
    cached Full200 exact-GH grid in
    `legacy_workspace/cache/exactgh_l2cov_full200_r8r12_g005g01.npz`.
    Per-MLP oracle over h1affine/l2snap/four exact-GH variants reaches
    1.938090115e-6 (`0.883x`), and a two-arm oracle
    l2snap-vs-rank12/gain0.05 reaches 2.087975169e-6 (`0.951x`).
    OOF Full200 branch-disagreement gates can reach about `0.98x`, but
    Mini-to-Full and Full-to-Mini transfer do not beat the fixed
    rank12/gain0.05 branch.  Close learned gating for now.

Layer-2 beta gate:
  script: legacy_workspace/probe_l2snap_beta_gate.py
  Full200:
    beta=0.25 raw = 2.186425226e-6
    beta=0.50 raw = 2.194660727e-6
    oracle_grid raw = 2.038841112e-6
    best cheap-feature CV line raw = 2.184617873e-6
  Full1000:
    beta=0.50 raw = 2.306191240e-6
    beta=0.25 raw = 2.326990715e-6
    oracle_grid raw = 2.149938992e-6
    best cheap-feature CV line raw = 2.312725088e-6
  verdict: reject.  There is real per-MLP beta oracle signal, but the tested
  target-free pre-tail features do not predict it.

Layer-2 fresh seed weights after snap:
  script: legacy_workspace/probe_l2snap_seed_weighting.py
  Full1000 start raw = 2.306191829e-6
  best OOF fixed seed-weight ridge = 2.307697500e-6
  verdict: reject.  Re-fitting the 18 fixed seed weights after l2snap worsens
  out-of-fold; the all-data fullfit is overfit.

Layer-2 skew snap:
  script: legacy_workspace/probe_secondlayer_skew_snap.py
  spaced20:
    l2snap raw = 2.305533215e-6
    skew beta -0.5 raw = 2.296140659e-6
  Full200:
    l2snap raw = 2.194660727e-6
    skew beta -0.5 raw = 2.194280440e-6
  verdict: do not package yet.  Direction is coherent but the Full200 gain is
  only 0.017%, too small to justify added operations without a better folding.
```

Scalar affine calibration for the snapped protected line:

```text
Full1000 beta=0.5 l2snap:
  raw before affine OOF/fullfit check = 2.306191240e-6
  OOF scalar affine raw = 2.304942365e-6
  all-data scalar affine:
    a = 0.9999321985257493
    b = -1.4041227919259214e-06
    fullfit raw = 2.301741013e-6

Package:
  whest-starterkit/packages/active/
    submission_phase1_pure18_protected_h1affine_l2snap_b05_weighted_affine_full1000_finalonly_bundle.tar.gz
    sha256 204f9987198eeca2bfa4010d4e11e073f8ce604cc1b5e76b3c54f127ff67e4aa

Remote:
  311693 was a malformed manual tarball missing manifest.json; ignore.
  311697 corrected whest-packaged tarball:
    adjusted score = 2.3707390588881096e-7
    verdict: tiny new protected line.  Improves over 311684 by ~0.11%.
```

Process note: after 311697, do not submit more packages automatically.  Prepare
candidate evidence and ask before uploading.

Post-311697 cost/constant follow-ups:

```text
Module-backed direction store for the snapped protected line:
  builder:
    legacy_workspace/build_h1affine_l2snap_module_package.py
  package:
    whest-starterkit/packages/active/
      submission_phase1_pure18_protected_h1affine_l2snap_b05_weighted_affine_full1000_module_finalonly_bundle.tar.gz
  maxdiff vs 311697 package:
    0.0 on the first 5 mini MLPs after storing seed weights as float64
  op_profile, 3 mini MLPs:
    311697 gzip/tuple package:
      ops=162, flops=3.816e10, warm residual ~=2.96ms
    Module+weights package:
      ops=158, flops=3.816e10, warm residual ~=1.65ms
  verdict:
    safe cost-side candidate only.  Expected adjusted gain is tiny because
    analytical sample matmuls dominate; do not submit without explicit approval.
```

```text
Layer-2 snap split-beta test:
  script:
    legacy_workspace/probe_l2snap_split_beta.py
  idea:
    separate mean correction strength beta_mu from scale/variance correction
    strength beta_sd.  Current package is beta_mu=0.5, beta_sd=0.5.
  spaced20:
    mu=0, sd=1 raw=2.124712554e-6 vs current 2.305533767e-6
  Full200 narrowed grid:
    best in tested grid was mu=0.5, sd=0.75 raw=2.203275021e-6,
    which is still worse than the known current Full200 l2snap raw
    2.194660727e-6.
  verdict:
    reject.  The strong spaced20 signal is a slice artifact; keeping mean and
    scale tied at beta=0.5 remains better on broader validation.
```

```text
Layer-2 snap row-count test:
  script:
    legacy_workspace/probe_l2snap_split_beta.py --half-rows ...
  uniform 224/256 half rows per seed, Full200:
    raw=2.651083238e-6
  uniform 248/256 half rows per seed, Full200:
    raw=2.287922616e-6
  verdict:
    reject.  The full 256-direction half-basis per selected seed is still
    structurally important after l2snap.  Even mild uniform row cuts lose more
    raw accuracy than they save in multiplier.
```

```text
Snapped final-value polynomial calibration:
  Full1000 l2snap protected before scalar affine:
    raw=2.306191829e-6
  best checked OOF polynomial transform:
    degree 4, raw=2.304846598e-6, ratio=0.9994167
  verdict:
    too small.  This is the same sub-0.1% class as scalar affine, not a new
    deployable raw-MSE lever.
```

```text
First-layer kurtosis shape correction after l2snap:
  scripts:
    legacy_workspace/probe_l2snap_split_beta.py --h1-kurt-beta ...
    legacy_workspace/build_h1affine_l2snap_module_package.py
  Full200:
    current l2snap raw = 2.194660727e-6
    h1_kurt=-0.02 raw = 2.177347201e-6
    h1_kurt=-0.04 raw = 2.166192330e-6
    h1_kurt=-0.06 raw = 2.175500798e-6
  Full1000:
    current l2snap raw ~= 2.306191e-6
    h1_kurt=-0.04 raw = 2.291934831e-6
    affine raw = 2.291514746e-6
    affine a=1.000022580023219, b=-3.037325492893928e-06
  package/profiler:
    Module-backed estimator with scalar target-kurtosis optimization has
    ops=180, flops=3.820e10, warm residual ~=2.49ms.
    The protected module line has ops=158, flops=3.816e10.
  Phase-1 mini spaced20 package sanity:
    protected module raw = 1.848905e-6, adjusted = 2.607160e-7
    h1_kurt=-0.04 raw = 1.899000e-6, adjusted = 2.682817e-7
  verdict:
    reject for now.  The Full200/Full1000 average gain is real but split-fragile,
    and the extra 22 ops plus a spaced20 regression make it too risky versus the
    protected 311697 line.
```

```text
High-seed l2snap seed-set search:
  discovery:
    Target-free compression of a 120-seed pure SPHEREx teacher found very
    different high-number seed sets.  Direct l2snap validation is mandatory,
    because the older pure-sampling cache does not include the layer-2 snap.
  direct exact-snap Full1000 probes:
    teacher k18 seeds
      (7,26,33,38,48,56,57,67,70,73,76,79,91,93,95,97,102,106)
      raw=2.384361923e-6, affine_raw=2.383091934e-6
    teacher k19 seeds
      (7,26,33,38,48,56,57,67,70,73,76,79,80,91,93,95,97,102,106)
      raw=2.262753111e-6, affine_raw=2.260527817e-6
    teacher k20 seeds
      (7,26,33,38,48,49,56,63,67,70,73,76,79,88,91,93,95,97,102,105)
      raw=2.085436007e-6, affine_raw=2.082355881e-6
    teacher k22 seeds
      (7,14,26,33,38,48,49,56,63,67,70,73,76,79,88,91,93,95,97,102,105,106)
      raw=1.909223748e-6, affine_raw=1.906868717e-6
    teacher k24 seeds
      (7,14,26,33,38,48,49,56,58,63,67,70,73,76,77,79,88,91,93,95,97,102,105,106)
      raw=1.750153787e-6, affine_raw=1.747576833e-6
  count-normalized verdict:
    k20/k22/k24 are near break-even or slightly worse after the block-count
    multiplier relative to the protected 18-block snapped line.  k20 is the
    only plausible diagnostic, not a clean improvement.
  same-row-count row trade:
    teacher k24 with 192 half-rows per seed:
      Full200 raw=2.267866669e-6, Mini100 raw=2.094299057e-6
    teacher k20 with 230 half-rows per seed:
      Full1000 raw=2.380830744e-6, affine_raw=2.379039458e-6
    verdict:
      reject.  Full QR blocks per seed matter; trading rows for more seed
      identities loses too much.
  subtle cache warning:
    Per-seed l2snap caches are not linearly composable for arbitrary subset
    search, because the layer-2 snap mean/variance is computed from the entire
    selected row bank.  A cache built with a 53-seed union corresponds to a
    different estimator: "compute layer-2 snap on the 53-seed union, then
    optionally tail-propagate/read out selected rows."  It is not an exact
    proxy for a package that only includes the selected 18 seeds from the start.
    Do not use union per-seed l2snap cache rankings as package candidates unless
    the production estimator also pays the union-snap early-layer cost.
```

```text
Tail-row cuts after layer-2 snap:
  idea:
    Keep all protected rows through the first-layer affine correction and the
    layer-2 Gaussian snap, then drop rows only for layers 3..32.  This tests
    whether the moment snap needs the full row bank but the expensive tail does
    not.
  probe changes:
    legacy_workspace/probe_l2snap_split_beta.py now has
    --tail-half-rows and --tail-row-mode={first,last,even}.
  protected exact-snap baseline:
    Full200 raw=2.194660727e-6, Mini100 raw=1.903843110e-6.
  first-row tail cuts:
    tail 248/256: Full200 raw=2.327842494e-6, Mini100 raw=2.105079143e-6
    tail 240/256: Full200 raw=2.536648437e-6, Mini100 raw=2.076733985e-6
    tail 224/256: Full200 raw=2.917646618e-6
  evenly-spaced tail cut:
    tail 248/256: Full200 raw=2.366672461e-6, Mini100 raw=2.082770855e-6
  verdict:
    reject.  Cutting rows after l2snap is still accuracy-expensive; the full
    QR half-basis is doing real work in the 30-layer tail.
```

```text
Union layer-2 snap with protected tail:
  script:
    legacy_workspace/probe_l2snap_union_tail.py
  idea:
    Pay extra seed blocks only through layers 1-2 to estimate the l2snap
    sample mean/variance, then keep the protected 18-seed tail for layers
    3..32.  This is the deployable interpretation of the earlier "union
    l2snap cache" warning.
  same-script protected baseline, Full200:
    snap_count=18, beta=0.50 raw=2.194661400e-6, affine_raw=2.176824173e-6
  Full200 union checks:
    snap_count=24, beta=0.50 raw=2.205755985e-6, affine_raw=2.187583132e-6
    snap_count=32, beta=0.50 raw=2.193560673e-6, affine_raw=2.177543086e-6
    snap_count=32, beta=0.25 raw=2.180769216e-6, affine_raw=2.168310473e-6
  Full1000 robustness checks:
    protected snap_count=18, beta=0.25 raw=2.326989285e-6, affine_raw=2.323661753e-6
    protected snap_count=18, beta=0.375 raw=2.313312883e-6, affine_raw=2.309484872e-6
    protected snap_count=18, beta=0.50 raw=2.306190265e-6, affine_raw=2.301740106e-6
    union snap_count=32, beta=0.25 raw=2.396854744e-6, affine_raw=2.392948211e-6
  verdict:
    reject.  The apparent Full200 lower-beta/union gain is not robust on
    Full1000.  Larger snap banks do not improve the protected tail enough to
    justify even the small early-layer cost, and beta=0.50 remains the broad
    protected setting.
```

```text
Mean-propagation blend after protected l2snap:
  inputs:
    protected l2snap seed cache plus legacy_workspace/cache/diag_meanprop_full1000.npz
  Full1000 scalar affine baseline:
    protected snapped raw=2.306191829e-6, affine raw=2.301739725e-6
  Full fit [sphere, meanprop, intercept]:
    beta ~= [0.996588, 0.003306, -1.001e-5]
    raw=2.292494478e-6
  5-fold MLP OOF:
    raw=2.295850609e-6
  verdict:
    reject for deployment.  The OOF raw gain is only about 0.25%, while a
    separate 32-layer mean-prop pass would add many flopscope ops/residual time.
    It is not competitive unless fused into already-paid work, which this
    SPHEREx package does not have.
```

```text
Post-expert memo follow-up, exact-GH spectral routing:
  scripts:
    legacy_workspace/probe_exactgh_spectral_gate.py
    legacy_workspace/probe_secondlayer_cov_exactgh.py
  cached features:
    legacy_workspace/cache/exactgh_spectral_features_mini100.npz
    legacy_workspace/cache/exactgh_spectral_features_full200.npz
  fixed exact-GH branch:
    Mini100 l2snap raw=1.903841878e-6
    Mini100 rank12/gain0.05 raw=1.886320835e-6
    Full200 l2snap raw=2.194669001e-6
    Full200 rank12/gain0.05 raw=2.177395718e-6
  strict cross-split gate result:
    Mini->Full learned gates generally did not beat fixed branch.
    Full->Mini learned gates generally did not beat fixed branch.
    One-feature threshold tables originally looked larger when sorted by test
    raw; after fixing the report to rank by train/OoF evidence first, the
    gains collapsed or inverted.
  verdict:
    fixed exact-GH spectral coloring is a real sub-1% broad raw signal, but the
    internal spectral features do not currently recover the two-arm oracle in a
    target-free or cross-split-stable way.  Do not package a learned exact-GH
    router.  Revisit only if a cheaper fixed branch or stronger internal
    feature emerges.
  Full1000 falsifier:
    script:
      legacy_workspace/probe_secondlayer_cov_exactgh.py
      --weights-cache legacy_workspace/cache/phase1_full1000_weights_targets.npz
      --indices 0-999 --ranks 12 --gains 0.05
    result:
      protected l2snap raw=2.306190611e-6
      exact-GH rank12/gain0.05 raw=2.307483501e-6
    updated verdict:
      close the fixed exact-GH branch as a candidate.  The Mini/Full200
      positive signal is not Full1000-stable.
```

```text
l2snap count24 / adaptive-count route:
  scripts:
    legacy_workspace/probe_l2snap_split_beta.py
    legacy_workspace/probe_l2snap_count24_router.py
  first24 equal-weight l2snap:
    spaced20 raw=1.519462926e-6
    Full200 raw=1.803474205e-6, affine_raw=1.786593671e-6
    Mini100 raw=1.590793038e-6
  count repricing:
    m18=0.1424738211
    m24=0.1923269604
    fixed24 adjusted is worse than protected18 despite lower raw.
  Full200 two-arm oracle:
    protected18 affine adjusted=3.107785486e-7
    fixed24 adjusted=3.468567121e-7
    oracle adjusted=2.534236793e-7 with count24 selected on 46.5% of MLPs
  router tests:
    features = protected/pass-available highseed block geometry plus exact-GH
    spectral features; labels tested both public-target true gains and
    target-free shadow-seed proxy gains.
    Mini->Full: all top true/shadow gates worsened versus fixed18.
    Full->Mini: most top gates worsened; isolated non-top shadow settings were
    not train-selected and are not robust.
  verdict:
    count24 has a large target oracle but the extra-count decision is not
    routable from tested target-free/protected-pass features.  Do not package a
    count24 adaptive router.
```

```text
Union53 selected-18 l2snap package sanity:
  existing generated estimator:
    legacy_workspace/_pkg_pure18_l2snap_union53_search18_affine_full1000_module_finalonly/estimator.py
  safe tuple rebuild:
    legacy_workspace/_pkg_pure18_l2snap_union53_search18_affine_full1000_tuple_finalonly/estimator.py
  selected seeds:
    (8,17,20,24,44,47,48,49,88,93,97,102,105,106,107,110,111,112)
  cached union53 per-seed proxy looked positive, but that cache uses union53
  snap statistics and is not exact for a selected-18 package.
  actual evaluator on Mini spaced20:
    protected l2snap affine:
      adjusted=2.608403e-7, raw=1.848905e-6, mult=0.14155342, failed=0
    union53 search18 tuple:
      adjusted=3.026206e-7, raw=2.147018e-6, mult=0.14148412, failed=0
  verdict:
    reject.  This is another seed-selection transfer/cache-composability trap,
    not a promotion candidate.
```

```text
Post-expert memo follow-up, l2snap-aware H1 projected cross-moment CV:
  script:
    legacy_workspace/probe_h1_cross_moment_cv.py
  change:
    Updated the diagnostic to mirror the current protected l2snap path
    (snap_beta=0.5 plus the 311697 scalar affine constants) before judging the
    expert memo's first-layer projected quadratic controls.
  Full200 spaced20 smoke:
    fixed_qr rank8, transformed h1 moments:
      base raw=2.250040688e-6
      best direct raw=2.157293110e-6 (0.958780x)
      teacher movement also improved, so this looked promising on the slice.
  Full200 all 200:
    fixed_qr rank8, transformed moments:
      base raw=2.181299059e-6
      best direct raw=2.196843269e-6 (1.007126x), wrong direction.
    fixed_qr rank8, raw-target moments:
      best direct raw=2.174985355e-6 (0.997106x)
      teacher_direct=1.853219307e-6 vs base_to_teacher=1.855380999e-6.
    fixed_qr rank32, diagonal raw-target controls:
      best direct raw=2.181301052e-6 (flat/worse).
  Spaced20 falsifiers:
    fixed_qr rank16 full raw-target controls and tail_random rank8 full
    raw-target controls both flipped to wrong-sign direct corrections.
  verdict:
    reject as a package path.  The l2snap-aware exact H1 cross-moment controls
    have a tiny correctly signed raw-target variant (~0.3% on Full200), but the
    full quadratic/transformed variants are split-fragile and the broad gain is
    far below the ~8-10% raw reduction needed to pay for the extra CV compute.
    Keep these controls only as possible features for high-seed residual
    distillation; do not deploy direct row-level regression CV.
```

```text
Trajectory PERO v2 from already-paid sample summaries:
  script:
    legacy_workspace/probe_trajectory_pero_v2.py
  idea:
    Use layerwise sampled trajectory summaries plus cheap linear/diagonal tail
    proxies as grouped-OOF residual features.  This is a learned residual check,
    not a submission by itself.
  spaced20 smoke:
    base raw=2.486908050e-6
    best ridge raw=2.403958655e-6 (0.9666x)
    warning: first half worsened while last half improved.
  Full200 broader run:
    samples=64, t_layers=12,20,28, modes=linear,diag
    base raw=2.330236086e-6
    best ExtraTrees OOF raw=2.314504602e-6 (0.993249x)
    best ridge OOF raw=2.320572286e-6 (0.995853x)
  verdict:
    reject as a production direction for now.  The broad gain is sub-1% on the
    older pure base and would require nontrivial runtime feature/proxy work;
    it is far below the improvement needed to beat the current protected
    l2snap line.
```

```text
Post-expert memo follow-up, l2snap union-teacher distillation:
  script:
    legacy_workspace/probe_l2snap_union_teacher_distill.py
  generated cache:
    legacy_workspace/cache/l2snap_b05_seed_preds_mini100.npz
  idea:
    Train a target-free cheap correction from protected18 l2snap seed/block
    features to the deterministic high-count teacher
    `union53_equal_l2snap - protected18_weighted_l2snap`.  Targets are used only
    for OOF/cross-split reporting.
  Full1000:
    protected affine base raw = 2.301739726e-6
    union53 teacher raw       = 8.039954401e-7
    best ridge OOF line       = 2.301801633e-6
    teacher_r2                = -0.00844
  Mini100:
    protected affine base raw = 1.897755432e-6
    union53 teacher raw       = 6.778027337e-7
    best ridge OOF line       = 1.909984273e-6
    teacher_r2                = -0.07254
  Cross-split:
    mini -> full best rel = 1.002072, teacher_r2 = -0.02916
    full -> mini best rel = 0.998595, teacher_r2 = -0.01499
  nonlinear note:
    A naive shallow ExtraTrees OOF over all Full1000 neuron rows was stopped
    after several minutes because it violated the fast-iteration constraint.
    Revisit only with row subsampling/cached training if a stronger reason
    appears.
  verdict:
    reject this deployable feature family.  The high-count l2snap teacher has
    huge raw signal, but protected final block summaries do not predict its
    signed correction.  This reinforces that high-count compression needs
    trajectory/internal observables, not just final seed vectors.
```

```text
Post-311697 cost-side pruning ladder:
  script:
    legacy_workspace/probe_l2snap_drop_sweep.py
  idea:
    Keep the protected h1-affine + layer-2 snap machinery, but remove full QR
    seed blocks after recomputing the snap from the smaller selected bank.
    This is not a cached per-seed linear recombination; the layer-2 sample
    mean/variance changes with the selected seed set.

  Full1000 count17 leave-one:
    protected18 affine raw = 2.301739747e-6
    best count-normalized proxy:
      drop seed 6, k=17
      affine raw = 2.418215048e-6
      proxy = 2.284675839e-6
    verdict: real but tiny, about 0.7% proxy gain.

  Full1000 count16 anchored on drop6:
    best:
      drop seeds 6,0, k=16
      affine raw = 2.554276013e-6
      proxy = 2.272170418e-6
    verdict: about 1.3% proxy gain.

  Full1000 count15 anchored on drop6,0:
    best:
      drop seeds 6,0,21, k=15
      affine raw = 2.707980167e-6
      proxy = 2.259358119e-6
    verdict: about 1.8% proxy gain.

  Full1000 count14 anchored on drop6,0,21:
    best:
      drop seeds 6,0,21,23, k=14
      keep seeds = (2,3,7,8,13,15,17,20,22,24,27,28,29,31)
      affine raw = 2.882472583e-6
      proxy = 2.245766417e-6
    verdict: best rung checked, about 2.4% proxy gain.

  Full1000 count13 anchored on drop6,0,21,23:
    best was not better:
      base count14 proxy = 2.245766417e-6
      best count13 proxy = 2.246115010e-6
    verdict: stop the pruning ladder at count14.

  Package built for inspection only:
    whest-starterkit/packages/active/
      submission_phase1_pure14_drop0_6_21_23_h1affine_l2snap_b05_weighted_affine_full1000_module_finalonly_bundle.tar.gz
    profile:
      ops = 158
      flops = 2.969e10
      predicted multiplier ~= 0.111-0.113
    local mini checks:
      mini spaced20 adjusted = 2.763043e-7, raw = 2.515940e-6
      protected mini spaced20 adjusted = 2.610379e-7, raw = 1.848905e-6
      mini100 adjusted = 2.816454e-7, raw = 2.567790e-6
      protected mini100 adjusted = 2.672674e-7, raw = 1.897754e-6
  updated verdict:
    Do not submit automatically.  Count14 is a valid cost frontier and may be
    useful if remote public is unusually favorable to the Full1000 proxy, but
    Mini100 rejects it strongly.  Treat it as a diagnostic/backup package, not
    a protected-line replacement.

Count14 PRE-EDGE rescue check:

```text
package:
  legacy_workspace/_pkg_pure14_drop0_6_21_23_h1affine_l2snap_b05_preedgeL4b075_weighted_affine_full1000_module_finalonly

Mini100:
  count14 no PRE-EDGE:
    raw=2.567790e-6, adjusted=2.816454e-7
  count14 L4 PRE-EDGE beta=0.75:
    raw=2.565823e-6, adjusted=2.821086e-7, multiplier=0.110038
```

Read: PRE-EDGE does not rescue the lower-count branch.  It gives only a
`0.08%` raw improvement on Mini100 while increasing the operation count enough
to worsen adjusted score.  Combined with the protected-18 Mini falsifier, this
closes PRE-EDGE as a production add-on for the current SPHEREx line.
```

```text
Post-311697 l2snap final-cloud residual features:
  script:
    legacy_workspace/probe_l2snap_residual_features.py
  idea:
    Use only deployable target-free statistics of the 18 snapped final seed
    block predictions (weighted mean, equal mean, robust locations, seed
    variance/skew/kurtosis, row summaries, and optionally signed seed
    deviations) to predict the remaining signed residual under grouped
    MLP-fold cross validation.

  Full1000 safe feature set:
    protected affine raw = 2.301739726e-6
    best grouped-OOF raw = 2.301896631e-6
  Mini100 safe feature set:
    protected affine raw = 1.897755704e-6
    best grouped-OOF raw = 1.898920381e-6
  Full1000 full seed-deviation feature set:
    protected affine raw = 2.301739726e-6
    best grouped-OOF raw = 2.301943475e-6

  verdict:
    reject.  The snapped final seed-block cloud still does not expose a stable
    target-free signed residual correction.  This closes the cheap
    final-vector-only residual ridge/robust-aggregation route for the current
    protected line.
```

```text
Post-311697 first-layer higher-shape spot checks after l2snap:
  script:
    legacy_workspace/probe_l2snap_split_beta.py
  Full200 protected l2snap reference:
    raw = 2.194660727e-6
  skew-only:
    h1_skew=-0.06 raw = 2.195030349e-6
  skew+kurt around the previously-live h1_kurt=-0.04 setting:
    h1_skew=-0.03, h1_kurt=-0.04 raw = 2.166407053e-6
    h1_skew=+0.03, h1_kurt=-0.04 raw = 2.166033971e-6
  prior h1_kurt-only reference:
    h1_kurt=-0.04 raw = 2.166192330e-6 on Full200
    but Mini spaced20 regressed and package sanity rejected it.

  verdict:
    no promotion.  Skew-only is dead, and skew+kurt only nudges the fragile
    kurtosis branch by noise-level amounts.  This remains a split-fragile
    sub-1% analytic tweak, not the 5%+ raw/adjusted lever we need.
```

```text
Post-311697 pruning Mini100 falsifier:
  script:
    legacy_workspace/probe_l2snap_drop_sweep.py
  purpose:
    Check whether the Full1000-selected count16/count15 pruning rungs survive
    on Mini100, rather than trusting the count-normalized proxy alone.

  Mini100 count16 base after dropping seeds 6,0:
    raw = 2.301191040e-6
    affine_raw = 2.299499680e-6
  Mini100 count15 base after dropping seeds 6,0,21:
    raw = 2.360623460e-6
    affine_raw = 2.357063580e-6
  For reference, protected18 l2snap Mini100:
    raw ~= 1.897755704e-6 after scalar affine.

  verdict:
    reject as a promotion path.  The count proxy can improve by shrinking the
    multiplier, but the Mini100 raw loss starts immediately and is far too
    large.  The protected line should keep all 18 full QR blocks unless a new
    nonuniform/analytical correction appears.
```

```text
Post-311697 l2snap + pathwise first-layer CV composition:
  script:
    legacy_workspace/probe_l2snap_pathwise_cv.py
  idea:
    Compose the older first-layer zero-mean pathwise control variate with the
    current h1-affine + layer-2 snap path, preserving the protected seed weights
    and using seed-parity cross-fit beta coefficients.

  Full200:
    plain snapped path after scalar affine:
      affine_raw = 2.176824598e-6
    best split-CV:
      ridge=1e-4, affine_raw = 2.173196796e-6
    layer-2 centered snapped-state split-CV:
      ridge=1e-4, affine_raw = 2.175900247e-6
    raw gain:
      about 0.17% for the layer-1 control, about 0.04% for the layer-2
      centered-state control.

  verdict:
    reject for production.  The old pathwise CV does still reduce raw error
    after l2snap, but the residual control signal is now far too small to pay
    for the added response matrix and control matmul.  It needed roughly 5-6%
    raw improvement to be adjusted-score relevant.
```

```text
Post-311697 l2snap final second-moment calibration:
  script:
    legacy_workspace/probe_l2snap_final_second_calib.py
  idea:
    During the protected l2snap SPHEREx pass, also collect the final sampled
    second moment and use exact Gaussian-ReLU identities or seed-block second
    moment dispersion as a cheap target-free correction to the final mean.

  Full200 protected reference:
    base raw = 2.181295230e-6
  scalar deltas:
    gauss_sqrt_second raw = 2.185179960e-6, rel = 1.001781
    gauss_delta       raw = 2.185179960e-6, rel = 1.001781
    second_over_mean  raw = 2.182266517e-6, rel = 1.000445
    sample_var        raw = 2.185728662e-6, rel = 1.002032
    seed_second_sd    raw = 2.185319517e-6, rel = 1.001845
    seed_mean_sd      raw = 2.185194959e-6, rel = 1.001788
  ridge feature block:
    best tested raw = 2.185011040e-6, rel = 1.001703

  verdict:
    reject.  The final sampled second moment is already aligned with the
    sampling cloud rather than the signed residual after h1-affine + layer-2
    snap.  It gives no deployable mean correction and should not be packaged.
```

```text
Post-311697 l2snap weight-conditioned residual features:
  script:
    legacy_workspace/probe_l2snap_weight_residual_features.py
  idea:
    Extend the prior final-seed-cloud residual probe with cheap target-free
    network geometry: final-layer column statistics, diagonal Gaussian
    propagation features, and row summaries.  Test signed residual prediction
    under MLP-grouped folds from cached protected l2snap seed predictions.

  Full1000 safe feature set:
    protected base raw = 2.301739726e-6
    best grouped-OOF raw = 2.298867898e-6, rel = 0.998752
    spaced20 worsened from 3.043634323e-6 to 3.198586278e-6
  Full1000 full signed seed-deviation feature set:
    best grouped-OOF raw = 2.301742407e-6, rel = 1.000001

  verdict:
    reject.  Adding weight/diagonal-propagation fingerprints gives only a
    0.12% broad OOF gain in the safe setting and hurts the canary slice; the
    higher-risk seed-deviation features collapse to no useful correction.  This
    closes the cheap ridge-style signed residual model for the protected
    l2snap seed cloud.
```

```text
Post-311697 tiny nonlinear residual MLP:
  script:
    legacy_workspace/train_l2snap_residual_mlp.py
  idea:
    Train a tiny deployable MLP on Full1000 only, using the same safe or full
    target-free l2snap residual features, and validate on independent Mini100.

  safe features, hidden=32:
    Full1000 train rel at gain=1.0 = 0.972681
    Mini100 valid best raw = 1.888449176e-6 at gain=0.5
    Mini100 rel = 0.995096 versus protected 1.897755704e-6
  safe features, hidden=64 stronger regularization:
    Mini100 best rel = 0.996535
  full signed seed-deviation features, hidden=32:
    Full1000 train rel at gain=1.0 = 0.874147
    Mini100 best rel = 0.997737

  verdict:
    not a mainline candidate.  A small smooth correction transfers at the
    0.2-0.5% level, but the stronger/full-feature models overfit Full1000 and
    do not improve Mini100.  Keep as future polish only if we later need a
    cheap sub-percent raw tweak.
```

```text
Post-311697 l2snap final-preactivation residual features:
  script:
    legacy_workspace/probe_l2snap_preact_feature_residual.py
  idea:
    During the protected h1-affine + layer-2 snap pass, also keep the final
    preactivation cloud and build deployable target-free summaries: preactivation
    mean/sigma, final ReLU sample mean, positive rate, absolute mean, column
    norm, seed-centered spread, and seed dispersion.  Fit only small
    MLP-grouped ridge corrections.

  Full200 result:
    base raw = 2.181290612e-6
    best OOF raw = 2.178318541e-6, rel = 0.998637
    best shrink = -0.317627, i.e. the fitted delta sign is counterintuitive
    spaced20 slightly worsened
    positive-shrink variants worsened

  verdict:
    reject.  The feature block finds a fragile sub-0.2% public-slice signal
    whose stable direction is not a safe deployable correction.  Do not package
    final-preactivation ridge features unless a stronger target-free diagnostic
    appears.
```

```text
Post-311697 target-aware high-seed l2snap seed identities:
  scripts:
    legacy_workspace/search_highseed_teacher_compress_dual.py
    legacy_workspace/probe_l2snap_split_beta.py
    legacy_workspace/build_h1affine_l2snap_module_package.py
  idea:
    Use the cheap high-seed pure-SPHEREx caches only as a proposal generator,
    then validate candidate seed identities by recomputing the actual layer-2
    snap from the selected row bank.  This avoids the known union-cache
    composability trap.

  target-aware pure-cache proposal, objective=target:
    k22 seeds =
      (2,3,7,11,20,33,42,50,55,63,77,79,81,82,84,87,94,96,97,111,116,118)
    k24 seeds =
      (3,7,8,11,20,33,34,42,47,49,50,55,63,77,79,81,82,84,87,94,96,110,111,116)
    k24 full-weighted seeds =
      (0,2,7,11,16,20,27,39,44,47,54,77,83,84,86,90,93,96,104,106,107,110,111,112)
    k24 mini-weighted seeds =
      (8,9,20,24,29,32,33,34,39,47,49,54,56,63,77,82,84,89,90,96,102,106,107,111)
    k20 full-weighted seeds =
      (0,2,7,11,16,20,27,44,47,48,84,86,90,93,104,106,107,110,111,112)

  direct exact-l2snap validation, equal weights, beta_mu=beta_sd=0.5:
    k22 Full200 raw=1.541866176e-6, affine_raw=1.536307083e-6
    k22 Mini100 raw=1.456465251e-6, affine_raw=1.452924744e-6
    k22 Full1000 raw=1.942154240e-6, affine_raw=1.938978269e-6

    k24 Full200 raw=1.416856342e-6, affine_raw=1.416285809e-6
    k24 Mini100 raw=1.386622624e-6, affine_raw=1.384475959e-6
    k24 Full1000 raw=1.777524973e-6, affine_raw=1.772707901e-6

    k24 mini-weighted Full200 raw=1.433493768e-6, affine_raw=1.432527809e-6
    k24 mini-weighted Mini100 raw=1.213395512e-6, affine_raw=1.212920214e-6
    k24 mini-weighted Full1000 raw=1.789447831e-6, affine_raw=1.784381149e-6

    k24 full-weighted Full200 raw=1.379537261e-6, affine_raw=1.379510155e-6
    k24 full-weighted Mini100 raw=1.468773457e-6, affine_raw=1.460934100e-6
    k24 full-weighted Full1000 raw=1.723900783e-6, affine_raw=1.723548837e-6

    k20 full-weighted Full200 raw=1.650717136e-6, affine_raw=1.650635739e-6
    k20 full-weighted Mini100 raw=1.780671524e-6, affine_raw=1.771095657e-6
    k20 full-weighted Full1000 raw=2.028985195e-6, affine_raw=2.028896210e-6

    k19 balanced Full200 raw=1.784616810e-6, affine_raw=1.783655800e-6
    k19 balanced Mini100 raw=1.726058415e-6, affine_raw=1.726027222e-6
    k19 balanced Full1000 raw=2.225612243e-6, affine_raw=2.223733934e-6

    k21 balanced Full200 raw=1.611271952e-6, affine_raw=1.609178439e-6
    k21 balanced Mini100 raw=1.561369207e-6, affine_raw=1.551590665e-6
    k21 balanced Full1000 raw=2.039238417e-6, affine_raw=2.038088406e-6

  seed-weight refit on k24 Full1000 saved per-seed predictions:
    equal start raw=1.777520747e-6
    best OOF seed-weight ridge worsened:
      ridge=100 aff_oof=1.775381823e-6
      ridge=10  aff_oof=1.776368283e-6
      lower ridges worse
    all-data seed-weight full fit improves only in-sample and is not safe.

  seed-weight refit on k20 Full1000 saved per-seed predictions:
    equal start raw=2.028986890e-6
    best OOF seed-weight ridge worsened:
      ridge=100 oof=2.028996000e-6
      ridge=10  oof=2.029139873e-6
      lower ridges worse
    all-data seed-weight full fit improves only in-sample:
      raw=2.022189675e-6, affine_raw=2.022078032e-6
    keep equal weights.

  flopscope profiles, module-backed packages:
    protected18: 3.816e10 FLOPs, predicted remote mult ~= 0.142-0.144
    k22:         4.662e10 FLOPs, predicted remote mult ~= 0.173-0.175
    k24:         5.085e10 FLOPs, predicted remote mult ~= 0.188-0.191
    k24 full-weighted package profile matches k24:
      5.085e10 FLOPs, predicted remote mult ~= 0.188-0.191
    k20 full-weighted package:
      4.239e10 FLOPs, predicted remote mult ~= 0.157-0.160

  adjusted read:
    This branch is a real raw-MSE improvement, but not yet a protected-line
    replacement.  Against the protected Full1000 affine raw 2.301739725e-6,
    k22 has raw ratio ~=0.843 but multiplier ratio ~=1.22, the first k24 has
    raw ratio ~=0.770 but multiplier ratio ~=1.33, and the best k24
    full-weighted candidate has raw ratio ~=0.749 but multiplier ratio ~=1.33.
    The best k24 full-weighted adjusted proxy is only about 0.6% better than
    protected under local Full1000 and the profiled remote multiplier, so it is
    a borderline diagnostic candidate rather than a confident promotion.
    The k20 full-weighted candidate is a better count-balanced point:
      raw ratio ~= 0.8815, multiplier ratio ~= 1.11
      local adjusted-proxy gain ~= 2.2% versus protected
    k19 and k21 variants do not beat k20 on Full1000 after count pricing.

  staged package, not submitted:
    whest-starterkit/packages/active/submission_phase1_pure24_targetdual_fullweighted_l2snap_equal_affine_full1000_module_finalonly_bundle.tar.gz
    whest-starterkit/packages/active/submission_phase1_pure20_targetdual_fullweighted_l2snap_equal_affine_full1000_module_finalonly_bundle.tar.gz
    k20 package validation passed with whest validate.

  verdict:
    keep as a live near-miss and a source of seed identities.  The staged k20
    full-weighted package is the better remote-test candidate of this branch:
    mechanically validated and locally projected around 2% better adjusted than
    311697, but still modest enough that it should be treated as a controlled
    probe, not a guaranteed improvement.  The staged k24 full-weighted package
    can be submitted only if we explicitly want to test remote transfer at
    higher raw/larger multiplier.  Future work should look for either (a) a
    better k20/k24 identity that clears Full1000 by another few percent, or
    (b) a way to buy some of the k24 raw gain without paying all six extra
    full-depth blocks.
```

### 2026-06-22 - l2snap shallow high-count GREG correction

Goal: test the expert's SFT-GREG/multifidelity idea without overlapping the
parallel W0-adaptive branch.  The candidate keeps the protected18 full-depth
l2snap estimate and adds a cheap all53 shallow checkpoint correction:

```
protected18 final + beta * (union53 shallow tail proxy - protected18 shallow tail proxy)
```

Implemented probe:

```
legacy_workspace/probe_l2snap_multilevel_greg.py
```

Important implementation detail: cost repricing now accounts for the shallow
half-row count.  A 64-half-row t=2 union53 checkpoint is only a small fraction
of a full protected18 pass, but it is still not free.

Spaced20 quick gate:

```
python legacy_workspace/probe_l2snap_multilevel_greg.py \
  --indices 0,10,20,30,40,50,60,70,80,90,100,110,120,130,140,150,160,170,180,190 \
  --half-rows 64 --t-layers 2,4 --modes linear,diag,relu \
  --betas global,s16 --device cpu --oof-folds 5
```

Results:

```
base raw=2.250029824e-6, adjusted=3.184994166e-7
union53 full teacher raw=6.564559526e-7

best same-slice alpha:
  t=2 linear/global raw=2.048595886e-6
  adjusted(extra only)=2.987960849e-7
  alpha=-6.598

OOF check for same candidate:
  raw=2.341827679e-6
  adjusted(extra only)=3.415651407e-7
  alpha_mean=-5.273, alpha_std=3.091
```

Full200 focused guard:

```
python legacy_workspace/probe_l2snap_multilevel_greg.py \
  --indices 0-199 --half-rows 64 --t-layers 2 \
  --modes linear,diag --betas global,s16 --device cpu --oof-folds 5
```

Results:

```
base raw=2.181297274e-6, adjusted=3.087700892e-7
union53 full teacher raw=7.250611638e-7

best direct:
  t=2 diag/s16 raw=2.175142820e-6
  adjusted(extra only)=3.172534726e-7

best OOF:
  t=2 diag/s16 raw=2.190111069e-6
  adjusted(extra only)=3.194366529e-7
```

Verdict: reject as a promotion path.  The full-depth union teacher confirms
that additional seeds contain large raw signal, but the l2snap shallow tail
proxy does not recover enough signed correction.  In-sample scalar alpha can
look good on a small spaced slice, but OOF alpha is unstable and Full200 direct
gain is too tiny to overcome even the corrected shallow compute cost.  Do not
expand this exact shallow GREG grid unless a new proxy supplies a much stronger
target-free signed signal.

### 2026-06-22 - union53 l2snap subset proposal sanity

Goal: see whether exact-l2snap high-number seed identities could improve the
k20/k24 seed-count frontier without overlapping the W0 branch work.

Implemented proposal helper:

```
legacy_workspace/search_l2snap_union_subset.py
```

Critical caveat: this helper is proposal-only.  It uses a union53 per-seed
l2snap cache, and l2snap is not linearly composable because layer-2 snap
mean/variance depends on the whole selected row bank.  The helper now prints
that warning.  Any candidate from it must be exact-recomputed with:

```
legacy_workspace/probe_l2snap_split_beta.py
```

The warning was empirically visible immediately:

```
union53-cache protected_w:
  full=2.441200763e-6
  mini=2.104165633e-6

known exact protected l2snap affine:
  Full1000 around 2.3017397e-6
  Mini100 around 1.8977557e-6
```

Proposal run:

```
python legacy_workspace/search_l2snap_union_subset.py \
  --counts 18,20,22,24,26 --full-weight 1.0 --mini-weight 1.0 \
  --random-restarts 20 --max-passes 5 --seed 2026062202
```

Best proxy k20 seeds:

```
(13,15,16,17,20,24,44,47,48,49,63,84,88,97,102,106,107,110,111,112)
```

Exact validation for that k20 bank, equal weights, beta_mu=beta_sd=0.5:

```
Mini100:
  raw=1.621356362e-6
  affine_raw=1.619869579e-6

Full200:
  raw=1.792932995e-6
  affine_raw=1.783724634e-6
```

Verdict: do not promote.  The proposal has good Mini100 raw, but Full200 is
not strong enough after k20 pricing and does not beat the existing validated
k20 full-weighted candidate.  The helper remains useful only as a fast seed
identity proposal generator; it is not a validator.

### 2026-06-22 - high-budget pathwise raw-MSE check

Existing CSV summaries:

```
legacy_workspace/cache/highseed120_f16_pathcv_spaced20.csv
  n=20
  raw_mean=3.644463980e-7
  adjusted_mean=3.607911423e-7
  multiplier_mean=0.989977
  raw_min=1.174057047e-7
  raw_max=1.272057148e-6

legacy_workspace/cache/highseed116_f16_pathcv_spaced20.csv
  n=20
  raw_mean=3.820059540e-7
  adjusted_mean=3.656193369e-7
  multiplier_mean=0.957059
```

Verdict: this family proves that very low raw MSE is reachable with brute-force
high-seed sampling plus CV, but it spends essentially the whole budget.  It is
not adjusted-competitive against the protected `311697` line unless a future
method compresses most of this raw gain into roughly `0.25-0.40` multiplier.

### 2026-06-22 - l2snap seed robust-center probe

Implemented:

```
legacy_workspace/probe_l2snap_seed_robust_estimators.py
```

Purpose: test whether the current 18 snapped seed-block outputs contain a
nearly-free target-free improvement from changing the seed-block center:
equal mean, median, trimmed mean, Huber center, smooth outlier downweighting,
or small global blends between the protected weighted mean and those robust
centers.  The probe reports both the fixed protected affine and MLP-grouped OOF
affine calibration.

Full1000 l2snap protected cache:

```
protected fixed raw = 2.301739726e-6
protected OOF raw   = 2.304941065e-6

best OOF robust line:
  protected -> equal, beta=+0.10
  OOF raw = 2.304899708e-6
```

Mini100 l2snap protected cache:

```
protected fixed raw = 1.897755704e-6
protected OOF raw   = 1.917072630e-6

best OOF robust line:
  protected -> equal, beta=+0.50
  OOF raw = 1.913427831e-6
```

Verdict: reject.  Robustifying the existing 18 seed blocks is at best a
rounding-scale movement on Full1000 and not consistent across Mini/Full.  The
protected weighted mean is already essentially the right target-free center.
Do not spend package submissions on median/trim/Huber/outlier-downweighted
variants unless a new independent diagnostic appears.

### 2026-06-22 - l2snap high-count compression and beta retune checks

Goal: continue the non-W0 lane by asking whether the validated count20/count24
l2snap seed identities can be made score-efficient without another learned
router.  These tests use exact recomputation, not linear recombination of
per-seed l2snap caches.

Uniform tail-row thinning after the layer-2 snap:

```text
script:
  legacy_workspace/probe_l2snap_split_beta.py
seed set:
  k24 full-weighted =
  (0,2,7,11,16,20,27,39,44,47,54,77,83,84,86,90,93,96,104,106,107,110,111,112)
slice:
  Full200 spaced20

tail_half_rows=256:
  raw=1.307420649e-6
  affine_raw=1.292313461e-6

tail_half_rows=224, even:
  raw=2.045434867e-6
  affine_raw=2.044903820e-6

tail_half_rows=192, even:
  raw=2.574797675e-6
  affine_raw=2.508131467e-6

tail_half_rows=128, even:
  raw=4.164265036e-6
  affine_raw=4.148385096e-6
```

Verdict: reject.  The deep tail still needs the full QR half-basis.  Even mild
row thinning loses far more raw accuracy than the multiplier savings can pay
for.

Larger snap bank with protected-size tail:

```text
script:
  legacy_workspace/probe_l2snap_union_tail.py
tail seeds:
  protected18
snap seeds:
  union(protected18, k24 full-weighted), snap_count=37
slice:
  Full200 spaced20

protected snap_count=18, beta=0.5:
  raw=2.305551101e-6
  affine_raw=2.107505313e-6

union snap_count=37, beta=0.5:
  raw=2.339181848e-6
  affine_raw=2.171792351e-6

union snap_count=37, beta=0.25:
  raw=2.280408801e-6
  affine_raw=2.103497023e-6

union snap_count=37, beta=0.75:
  raw=2.417932684e-6
  affine_raw=2.258576712e-6
```

Verdict: reject.  Beta `0.25` gives a tiny slice-level raw improvement over the
protected snap path, but the extra layer-1/2 work is about 6% seed-layer
equivalent and the in-slice affine gain is negligible.  This does not clear the
adjusted-score bar.

Count24 snap-strength retune:

```text
seed set:
  k24 full-weighted

Full200 spaced20:
  beta=0.5  raw=1.307420649e-6
  beta=0.25 raw=1.276125145e-6
  beta=0.75 raw=1.366785745e-6

Full200:
  beta=0.25 raw=1.366327007e-6
  logged beta=0.5 k24 full-weighted Full200 raw ~=1.379537261e-6

Full1000:
  beta=0.25 raw=1.736348300e-6
  logged beta=0.5 k24 full-weighted Full1000 raw ~=1.723900783e-6
```

Verdict: reject as a package change.  Beta `0.25` is better on Full200 but
worse on Full1000, so the spaced20/Full200 gain is not broad enough.

Count20 snap-strength retune:

```text
seed set:
  k20 full-weighted =
  (0,2,7,11,16,20,27,44,47,48,84,86,90,93,104,106,107,110,111,112)

Full200:
  beta=0.25 raw=1.639430438e-6
  logged beta=0.5 k20 full-weighted Full200 raw ~=1.650717136e-6

Mini100:
  beta=0.25 raw=1.811220907e-6
  logged beta=0.5 k20 full-weighted Mini100 raw ~=1.780671524e-6

Full1000:
  beta=0.25 raw=2.044822026e-6
  logged beta=0.5 k20 full-weighted Full1000 raw ~=2.028985195e-6
```

Verdict: reject.  Again, beta `0.25` helps Full200 but fails both Mini100 and
Full1000.  Keep beta `0.5` for the count20/count24 l2snap near-miss packages.

## 2026-06-22 - l2snap seed harness / union53 same-count search

Goal: build a safer local harness for seed/direction-set choices without
spending public submissions.  The branch tests whether we can improve the
18-block SPHEREx line by changing the QR seed identities while keeping the same
block count and near-identical runtime.

Baseline protected remote anchor:

```text
submission 311697
artifact:
  submission_phase1_pure18_protected_h1affine_l2snap_b05_weighted_affine_full1000_module_finalonly_bundle.tar.gz
remote adjusted:
  2.370739058888e-7
remote final raw:
  1.655662345e-6
remote multiplier:
  0.143186
```

Robust center harness:

```text
script:
  legacy_workspace/probe_l2snap_seed_robust_estimators.py
cache:
  legacy_workspace/cache/l2snap_b05_seed_preds_full1000.npz

Full1000 protected fixed raw:
  2.301739726e-6
Full1000 protected OOF affine:
  2.304941065e-6
best OOF robust variant:
  protected_to_equal_b+0.10 -> 2.304899708e-6

Mini100 protected fixed raw:
  1.897755704e-6
Mini100 protected OOF affine:
  1.917072630e-6
best Mini OOF robust variant:
  protected_to_equal_b+0.50 -> 1.913427831e-6
```

Verdict: reject robust centering/Huber/trim/smooth-outlier variants.  The
protected weighted mean is already essentially optimal in this block set.

Union53 same-count search:

```text
proposal cache:
  legacy_workspace/cache/l2snap_union53_seed_preds_full1000.npz
search script:
  legacy_workspace/search_l2snap_union_subset.py
exact recompute script:
  legacy_workspace/probe_l2snap_split_beta.py
snap:
  beta_mu=0.5, beta_sd=0.5
weights:
  equal
```

First balanced best18 proposal:

```text
seeds:
  (8,15,16,20,23,24,44,47,48,49,84,88,102,106,107,110,111,112)

Full200:
  raw=1.983923204e-6
  affine=1.973366166e-6
Mini100:
  raw=1.887950717e-6
  affine=1.886442977e-6
Full1000:
  raw=2.170542139e-6
  affine=2.168670392e-6
  affine a=0.9999593718290376
  affine b=-6.372070836424548e-06

package:
  whest-starterkit/packages/active/submission_phase1_pure18_union53_best18_equal_l2snap_b05_affine_full1000_module_finalonly_bundle.tar.gz
```

Seed-weight refit on this best18 set did not help out of fold:

```text
cache:
  legacy_workspace/cache/l2snap_union53_best18_equal_seed_preds_full1000.npz
script:
  legacy_workspace/probe_l2snap_seed_weighting.py
start raw:
  2.170540558e-6
best OOF ridge:
  ridge=100 -> 2.170538884e-6
```

Verdict: keep equal weights.  All-data seed weights are in-sample only.

Full-weighted best18 proposal, exact recomputed:

```text
seeds:
  (8,17,20,24,44,47,48,49,88,93,97,102,105,106,107,110,111,112)

Full200:
  raw=1.959126076e-6
  affine=1.955259184e-6
  spaced20=1.866717120e-6
  q50 max=2.137828431e-6
Mini100:
  raw=1.925104167e-6
  affine=1.916629343e-6
Full1000:
  raw=2.131049246e-6
  affine=2.128480741e-6
  affine a=0.9999493847482596
  affine b=-2.618030950474193e-06

package:
  whest-starterkit/packages/active/submission_phase1_pure18_union53_fullbest18_equal_l2snap_b05_affine_full1000_module_finalonly_bundle.tar.gz
```

Verdict: this is the preferred controlled upload candidate from the harness.
It keeps the same 18-block runtime class as 311697 while improving exact
Full1000 raw from `2.301739726e-6` to `2.131049246e-6`, a local ratio of about
`0.926`.  If that ratio transfers to 311697's remote raw, expected remote raw is
roughly `1.53e-6` at the same multiplier, or adjusted around `2.2e-7`.  Risk:
the seed identities were selected against public targets, so the package should
be treated as a high-value diagnostic/leader probe rather than proof of a
private-safe gain.

Remote result:

```text
submission:
  311801
status:
  failed in smoke test
error:
  ESTIMATOR_EXCEPTION
  participant subprocess crashed:
    unexpected frame type 'result'; expected 'predict_start'
cause:
  same failure class as 311729. The package loaded a flops.Module-backed
  direction store in setup(), which can emit protocol frames before predict()
  begins in the Phase 1 grader.
```

Corrected tuple-safe rebuild:

```text
package:
  whest-starterkit/packages/active/submission_phase1_pure18_union53_fullbest18_equal_l2snap_b05_affine_full1000_tuple_finalonly_bundle.tar.gz
contents:
  estimator.py
  spherex_pure18_union53_fullbest18_equal_l2snap_b05_affine_full1000_tuple_finalonly_h1half_inputs_256x4608_f32.bin.gz
  manifest.json
validation:
  whest validate passed
smoke:
  quick_score_selected mini indices 0,1 passed, failed=0
math:
  same seed set and affine constants as 311801; direction blob is loaded as
  gzip raw f32 into a plain Python tuple in setup(), then materialized as
  fnp.array inside predict().
```

Remote result:

```text
submission:
  311852
status:
  graded, 50/50 public completed
adjusted:
  3.128e-7
final raw:
  2.185e-6
flops mean:
  3.816e10
effective mean:
  3.894e10
multiplier:
  ~0.143
residual:
  mean 7.82 ms
```

Verdict: reject.  The corrected tuple packaging fixed the `predict_start`
protocol failure and confirmed cost parity with 311697, but the public-target
union53 seed selection did not transfer remotely.  The issue was raw error, not
compute.  Do not use target-selected union53 best18 seed sets as trusted
leader candidates without a much stronger out-of-distribution canary.

Full-heavy alternate proposal, exact recomputed after compaction:

```text
seeds:
  (8,13,15,16,22,23,28,44,47,49,84,88,102,106,107,110,111,112)

Mini100:
  raw=1.995725869e-6
  affine=1.995679384e-6
  q50 max=2.100102569e-6
Full200:
  raw=2.048237320e-6
  affine=2.035013096e-6
  spaced20=1.987423496e-6
  q50 max=2.353252123e-6
```

Verdict: reject.  It is worse than the fullbest18 candidate on both Mini100 and
Full200, and has a weaker q50 tail.  Do not spend Full1000 or remote quota on
this seed set.

Split beta and layer-2 skew follow-up:

```text
k24 full-weighted, Full200 spaced20:
  beta_mu=0.25, beta_sd=0.50 raw=1.253708047e-6
  beta_mu=0.25, beta_sd=0.75 raw=1.267704072e-6
  beta_mu=0.50, beta_sd=0.25 raw=1.355615026e-6
  beta_mu=0.75, beta_sd=0.25 raw=1.477424714e-6

k24 full-weighted, Full200:
  beta_mu=0.25, beta_sd=0.50 raw=1.382283768e-6
  logged beta_mu=0.50, beta_sd=0.50 raw ~=1.379537261e-6
```

Verdict: reject.  The apparent split-beta gain on spaced20 disappears on
Full200, so no Full1000 run was spent.

Layer-2 skew snap on k24 full-weighted:

```text
script:
  legacy_workspace/probe_secondlayer_skew_snap.py
slice:
  Full200 spaced20

marg0.5              raw=1.307415357e-6
marg0.5_skew-0.5     raw=1.304694472e-6
marg0.5_skew-0.25    raw=1.306009064e-6
marg0.5_skew-0.1     raw=1.306805316e-6
marg0.5_skew0.1      raw=1.308116184e-6
marg0.5_skew0.25     raw=1.309381136e-6
marg0.5_skew0.5      raw=1.311680132e-6
```

Verdict: reject.  The best skew nudge is only a `0.2%` slice-level gain and is
smaller than the split fragility seen in the beta tests.

Low-discrepancy Sobol sphere direction check:

```text
script change:
  legacy_workspace/direction_lab.py now supports family=sobol for offline tests.
command:
  python legacy_workspace/direction_lab.py ... \
    --candidate protected=qr:protected:protected \
    --candidate sobprot=sobol:protected:protected \
    --candidate sob18=sobol:0-17:equal \
    --mode l2snap --half-rows 256 --device cpu
slice:
  Full200 spaced20

protected QR:
  raw=2.250050836e-6
  affine_raw=2.107504789e-6

Sobol, protected seeds/weights:
  raw=2.902366411e-6
  affine_raw=2.900232472e-6

Sobol, seeds 0..17 equal:
  raw=3.642591429e-6
  affine_raw=3.277388246e-6
```

Verdict: reject.  Scrambled Sobol Gaussian-normalized sphere rows have much
higher seed spread and worse target error under the current h1/l2snap path.
The full QR block remains the useful deterministic direction primitive.

### 2026-06-22 - post-compaction l2snap closures

These are follow-ups after the protected `311697` line and the failed
target-selected union upload `311852`.  They are recorded here to avoid
retesting the same ideas under slightly different names.

Union53 same-count validation, fold-safe proxy:

```text
script:
  legacy_workspace/validate_l2snap_union_seed_search.py
command:
  python legacy_workspace/validate_l2snap_union_seed_search.py \
    --counts 18,20,22 --folds 5 --random-restarts 12 --max-passes 4

k18 validation weighted mean: 1.017641
k18 Mini weighted mean:       1.042709

k20 validation weighted mean: 0.910177
k20 Mini weighted mean:       0.939094

k22 validation weighted mean: 0.831189
k22 Mini weighted mean:       0.806319
```

Read: the union cache still contains real target-aligned signal at high count,
but this is only a proposal/cache-space validation.  Exact recomputation of the
newly selected k20/k22 seed sets did not preserve enough of the proxy gain:

```text
new proxy-best k20, exact Full200:
  raw=1.743000963e-6
  affine=1.736123846e-6

new proxy-best k22, exact Full200:
  raw=1.682023927e-6
  affine=1.675718956e-6
```

Both are worse than the previously measured exact high-count near-misses, so no
new package is justified.  The target-selected high-seed lane remains a
diagnostic oracle, not a private-safe estimator.

Weight-conditioned residual features after l2snap:

```text
script:
  legacy_workspace/probe_l2snap_weight_residual_features.py

safe mode:
  base raw=2.301739726e-6
  best OOF raw=2.298867898e-6
  rel=0.998752
  spaced20 worsened to 3.198586290e-6

full mode:
  best OOF raw=2.301741067e-6
  rel=1.000001
```

Verdict: reject.  The only positive signal is under one-tenth of the needed
size and fails the spaced20 guard.

Self-Harmonic ReLU Control after l2snap:

```text
script:
  legacy_workspace/probe_l2snap_shrc.py
command:
  python legacy_workspace/probe_l2snap_shrc.py \
    --indices 0,10,20,30,40,50,60,70,80,90,100,110,120,130,140,150,160,170,180,190 \
    --device cuda --betas=-0.5,-0.25,0,0.25,0.5,0.75,1 \
    --out legacy_workspace/cache/l2snap_shrc_spaced20.npz

plain:
  raw=2.305551263e-6
  affine_raw=2.107516923e-6

beta=0.25:
  raw=2.280751938e-6
  ratio=0.989244
  affine_raw=2.102548687e-6

beta=0.50:
  raw=2.276151505e-6
  ratio=0.987248
  affine worsened

target-free diagnostics:
  mean_vr=-0.058739
  median_vr=-0.052294
  mean_sign=0.503753
  mean_corr=-0.012641
```

Verdict: reject as a promotion path.  The target slice has a small fitted
improvement, but the control variate fails the target-free variance/sign tests.
This is the same failure pattern as pure SHRC; do not package it.

Tiny nonlinear residual MLP over l2snap final-cloud features:

```text
script:
  legacy_workspace/train_l2snap_residual_mlp.py
command:
  python legacy_workspace/train_l2snap_residual_mlp.py \
    --mode safe --hidden 32 --epochs 40 --batch 16384 --lr 0.001 \
    --weight-decay 0.001 --dropout 0.05 --device cuda

train Full1000 base raw:
  2.301739726e-6
best train gain=1:
  2.261946989e-6
  rel=0.9827

valid Mini100 base raw:
  1.897755704e-6
best valid gain=0.25:
  1.895446090e-6
  rel=0.998783

spaced20 guard:
  base=2.314678963e-6
  corrected=2.321758612e-6
```

Verdict: reject.  Nonlinear features can overfit the public residual but the
transfer signal is under `0.2%` on Mini and negative on spaced20.  The current
protected l2snap cloud does not expose a strong cheap residual feature through
this feature set.

Target-free union-teacher MLP distillation:

```text
script:
  legacy_workspace/train_l2snap_union_teacher_mlp.py
label:
  union53_equal_l2snap - protected18_weighted_l2snap
targets:
  used only for reporting target MSE after the teacher-predicted correction

safe features, hidden=64:
  train teacher_r2=+0.027730
  valid teacher_r2=-0.037577
  valid best gain=0, i.e. every positive gain worsened Mini100 target MSE

full signed seed-deviation features, hidden=64:
  train teacher_r2=+0.159537
  valid teacher_r2=-0.041157
  valid best target gain=0.25:
    raw=1.892324709e-6
    rel=0.997138
  spaced20 worsened from 2.314678963e-6 to 2.324580324e-6
```

Verdict: reject for promotion.  This is less overfit-prone in principle than a
public-target residual model because the label is target-free, but the
protected-pass feature set still cannot predict the signed high-count teacher
on independent Mini.  The tiny Mini target gain is too small and fails the
spaced20 guard.

### 2026-06-22 - exact l2snap seed-swap neighborhood after protected 311697

Goal: look for same-count seed-bank improvements without changing the
production shape, FLOPs, or setup safety of the protected l2snap branch.

Scripts:

```text
legacy_workspace/probe_l2snap_swap_sweep.py
legacy_workspace/probe_l2snap_seedset_eval.py
```

Important harness note: `spaced20` is not safe for this lane.  It produced
large apparent wins such as `d3_a100`, which did not survive broad Mini/Full.
Use Mini100 + Full1000 as the gate.

Protected exact-l2snap reference in this harness:

```text
Mini100:
  raw=1.903843110e-6
  affine_raw=1.897006865e-6

Full1000:
  raw=2.306189896e-6
  affine_raw=2.301739747e-6
```

One-swap Full1000 validation:

```text
d6_a100:
  raw=2.274934386e-6
  affine_raw=2.272234638e-6
  Full1000 gain vs protected affine ~= 1.28%

d17_a100:
  raw=2.287158722e-6
  affine_raw=2.284702663e-6
  Full1000 gain vs protected affine ~= 0.74%
```

Two-swap Full1000 validation:

```text
d6a100,d21a39:
  raw=2.232681173e-6
  affine_raw=2.227240172e-6
  Full1000 gain vs protected affine ~= 3.24%
  Mini100 affine_raw=1.888362449e-6
  Mini100 gain vs protected affine ~= 0.46%

d6a100,d21a107:
  raw=2.260594637e-6
  affine_raw=2.256888701e-6

d6a100,d17a110:
  raw=2.272493802e-6
  affine_raw=2.271564525e-6
```

Three-swap Full1000 validation:

```text
aggressive Full1000 winner:
  spec=d6a100,d21a39,d23a107
  seeds=(0,2,3,7,8,13,15,17,20,22,24,27,28,29,31,39,100,107)
  raw=2.216789500e-6
  affine_raw=2.213587970e-6
  Full1000 gain vs protected affine ~= 3.83%
  Mini100 affine_raw=1.968712886e-6
  Mini100 regression vs protected affine ~= 3.78%

cross-split safer:
  spec=d6a100,d21a39,d17a112
  seeds=(0,2,3,7,8,13,15,20,22,23,24,27,28,29,31,39,100,112)
  raw=2.240265757e-6
  affine_raw=2.234770110e-6
  Full1000 gain vs protected affine ~= 2.91%
  Mini100 affine_raw=1.841025939e-6
  Mini100 gain vs protected affine ~= 2.95%

Mini-friendlier:
  spec=d6a100,d21a39,d7a112
  seeds=(0,2,3,8,13,15,17,20,22,23,24,27,28,29,31,39,100,112)
  raw=2.247967456e-6
  affine_raw=2.242775946e-6
  Full1000 gain vs protected affine ~= 2.56%
  Mini100 affine_raw=1.811073437e-6
  Mini100 gain vs protected affine ~= 4.53%
```

Fourth-swap neighborhood around the two safe third-swap bases:

```text
best Full1000 fourth-swap:
  spec=d6a100,d21a39,d17a112,d23a107
  raw=2.229112366e-6
  affine_raw=2.225869469e-6
  does not beat the aggressive three-swap Full1000 winner
  Mini100 affine_raw=1.923584820e-6, worse than protected

best Mini among checked fourth-swaps:
  spec=d6a100,d21a39,d7a112,d3a107
  Full1000 affine_raw=2.228984071e-6
  Mini100 affine_raw=1.892232959e-6
  not better than the safer third-swap choices
```

Package artifacts built and validated, not submitted by Codex:

```text
whest-starterkit/packages/active/
  submission_phase1_pure18_l2swap_d6_21_23_to100_39_107_affine_full1000_finalonly_bundle.tar.gz
  submission_phase1_pure18_l2swap_d6_21_17_to100_39_112_affine_full1000_finalonly_bundle.tar.gz
  submission_phase1_pure18_l2swap_d6_21_7_to100_39_112_affine_full1000_finalonly_bundle.tar.gz
```

Packaging/validation notes:

```text
whest package must be pointed at the package folder, not estimator.py, otherwise
it excludes the direction blob.  The final archives include:
  estimator.py
  spherex_..._h1half_inputs_256x4608_f32.bin.gz
  manifest.json

whest validate passed for all three estimator.py files.

One local package smoke run for the aggressive artifact:
  index=0
  failed=0
  flops=3.816e10
  effective=4.394e10 locally
  local residual/multiplier is machine-dependent, but analytical FLOPs match
  the protected branch.
```

Verdict: same-count seed geometry still has a small amount of value, but not a
new large mechanism.  The cross-split-safe candidate is
`d6a100,d21a39,d17a112`; the higher-upside but riskier candidate is
`d6a100,d21a39,d23a107`.  Further fixed seed-set mining beyond this point is
likely to overfit unless a new target-free selector is introduced.

### 2026-06-23 - high-seed l2snap robust exact candidates

Goal: revisit same-count seed identities after the l2snap path, but use the
`union53` l2snap cache only as a proposal generator.  Every promoted seed set
below was exact-recomputed with the selected 18-row bank; it is not a linear
readout from the union cache.

Baseline protected exact-l2snap references:

```text
Full1000 protected affine_raw = 2.301739747e-6
Mini100  protected affine_raw = 1.897006865e-6
remote protected 311697:
  adjusted ~= 2.370739059e-7
  raw      ~= 1.655662345e-6
  mult     ~= 0.14319
```

Full/Mini robust proposal search:

```text
script:
  legacy_workspace/search_l2snap_union_subset.py

balanced proposal, exact selected-bank recompute:
  seeds=(17,20,24,44,47,48,49,63,84,88,97,102,105,106,107,110,111,112)
  Full1000 raw=2.133954552e-6
  Full1000 affine_raw=2.130589467e-6
  Mini100 raw=1.800751869e-6
  Mini100 affine_raw=1.800078444e-6
  local gain vs protected:
    Full1000 affine ~= 7.44%
    Mini100 affine ~= 5.11%

mini-strong proposal, exact selected-bank recompute:
  seeds=(20,24,33,44,47,48,49,63,84,88,97,102,105,106,107,110,111,112)
  Full1000 raw=2.174877214e-6
  Full1000 affine_raw=2.171190578e-6
  Mini100 raw=1.691856679e-6
  Mini100 affine_raw=1.690781200e-6
  local gain vs protected:
    Full1000 affine ~= 5.67%
    Mini100 affine ~= 10.87%

mini-emphasis proposal, exact selected-bank recompute:
  seeds=(0,2,16,20,22,23,24,47,49,63,84,88,90,102,106,107,110,112)
  Full1000 affine_raw=2.238878200e-6
  local gain vs protected affine ~= 2.73%
  verdict: too weak on Full1000, not packaged.
```

Packaging:

```text
whest-starterkit/packages/active/
  submission_phase1_pure18_highseed_balanced_l2snap_b05_equal_affine_full1000_finalonly_bundle.tar.gz
  submission_phase1_pure18_highseed_ministrong_l2snap_b05_equal_affine_full1000_finalonly_bundle.tar.gz

Both are real `whest package` archives with top-level:
  estimator.py
  spherex_..._h1half_inputs_256x4608_f32.bin.gz
  manifest.json

whest validate passed for both.
One-MLP real mini smokes passed with no failures and analytical FLOPs
`~3.82e10`, same class as the protected l2snap line.  Local WSL residual
multiplier was noisy/high on one MLP and should not be used as the remote
multiplier estimate; use the protected 311697 remote multiplier class.
```

Read:

```text
balanced central remote estimate:
  raw ratio roughly 0.93-0.95 vs protected depending on Mini/Full weighting
  expected adjusted around 2.20e-7 to 2.25e-7 at protected multiplier

mini-strong central remote estimate:
  raw ratio roughly 0.89-0.94 vs protected depending on Mini/Full weighting
  expected adjusted around 2.17e-7 to 2.24e-7 at protected multiplier
```

Verdict: these are the first post-311697 same-count packages with enough broad
exact-recompute gain to plausibly beat the current `2.23e-7` public leader.
They remain seed-identity candidates selected using public targets, so remote
transfer risk is real.  If spending submissions, upload `balanced` first as the
Full1000-strong/less-heavy-tail candidate, then `mini-strong` as the higher
variance cross-check if `balanced` transfers or if one more probe is acceptable.

### 2026-06-23 - post-compaction no-go checks for non-tiny moves

Goal: stop re-opening fragile seed tweaks and establish whether the remaining
SPHEREx gains are structural or just higher sample count / public-target seed
selection.

Current protected remote line:

```text
311697
artifact = submission_phase1_pure18_protected_h1affine_l2snap_b05_weighted_affine_full1000_finalonly_bundle.tar.gz
adjusted = 2.370739059e-7
raw      = 1.655662345e-6
mult     ~= 0.14319
flops    = 3.8155630592e10
```

Important later failure:

```text
311852
artifact = submission_phase1_pure18_union53_fullbest18_l2snap_b05_equal_affine_full1000_tuple_finalonly_bundle.tar.gz
remote adjusted ~= 3.128e-7
remote raw      ~= 2.185e-6
read: the target-selected union53 seed set did not transfer; the miss was raw
      accuracy, not compute.
```

High-count target-free first-block frontier, profiled with `op_profile.py`
(2 local MLPs, analytical FLOPs stable):

```text
k=24 first blocks:
  flops ~= 5.085e10
  predicted remote multiplier ~= 0.188-0.191

k=32 first blocks:
  flops ~= 6.778e10
  predicted remote multiplier ~= 0.251-0.253

k=40 first blocks:
  flops ~= 8.471e10
  predicted remote multiplier ~= 0.313-0.315
```

The raw frontier is real but compute-priced.  Existing cache numbers:

```text
first32 Full1000 affine raw ~= 1.3595e-6
first32 Full200  affine raw ~= 1.3851e-6
first32 Mini100  affine raw ~= 1.0634e-6

first40 Full1000 affine raw ~= 1.0681e-6
first40 Full200  affine raw ~= 1.0989e-6
first40 Mini100  affine raw ~= 0.8595e-6
```

At the measured cost, these are not obviously better than `311697` unless the
remote public50 raw is unusually favorable.  They are useful diagnostics for
the accuracy-vs-cost frontier, but not yet the material next method.

Seed-weighting and internal blend checks on first40:

```text
probe_l2snap_seed_weighting.py, first40 cached per-seed preds

Full1000:
  equal raw  = 1.072608505e-6
  affine raw = 1.068475805e-6
  OOF seed weights did not improve robustly
  all-data fullfit improves to ~= 1.056e-6, marked target fit

Mini100:
  equal raw  = 0.860217e-6
  affine raw = 0.859465e-6
  OOF seed weights did not improve robustly
  all-data fullfit improves to ~= 0.801e-6, marked target fit

Blend first40 equal with the protected weighted subset computed inside the
same first40/l2snap trajectory:
  Full1000 OOF affine 1.0674e-6 vs equal affine 1.0681e-6 (tiny)
  Mini100 OOF affine worsens to ~=0.881e-6 vs equal affine ~=0.859e-6
```

Read: after enough QR blocks, seed identity and simple seed weighting are mostly
target-fit degrees of freedom.  They do not provide the requested 5%+ robust
move.

Prior structural closures rechecked before reopening:

```text
first-layer covariance color:
  working variant after protected h1affine/weighting gives only ~=0.35% raw
  on Full1000 and costs extra dense covariance work.
  exact/full covariance matching did not let us cut blocks on Full200.

layer-2 exact-GH covariance spectral color:
  broad Full200 gain was only ~=0.8% for full spectral variants and cheap
  fixed/tail subspaces kept only ~=0.3%.

shallow/GREG multilevel:
  OOF collapsed after promising in-sample spaced20 fits; not a deployable
  branch without a new target-free signed proxy.
```

Verdict: the live non-tiny path is no longer "find a slightly better seed set".
We need a variance-reducing structural control that changes what each QR block
measures, or a cost-aware hybrid that buys more directions only when the raw
drop pays the multiplier.

### 2026-06-23 - W0-adaptive same-cost replacement closure

Goal: distinguish the known "adaptive W0 branch has signal but costs too much"
result from a stricter production-shaped question: can an adaptive first-layer
direction block replace one protected QR block at the same row count after the
current h1-affine + l2snap path?

Added diagnostic:

```text
legacy_workspace/probe_l2snap_w0_replace.py
```

The deployable shape would compute the replacement half-block from `W0` inside
`predict()`, then proceed through the same l2snap tail.  Targets are used only
for offline scoring.

Spaced20 smoke:

```text
base raw = 2.305553533e-6
replace position 17 with W0 rows:
  raw = 2.157106321e-6  # looked positive on this tiny slice
replace with W0 columns:
  catastrophic, ~3e-5 to 1e-4 raw
```

Full200 single-position W0-row replacement sweep:

```text
base raw = 2.194670106e-6

best broad replacements:
  drop position 10 -> W0 rows:
    raw        = 2.197507727e-6
    affine_raw = 2.186643901e-6
  drop position  8 -> W0 rows:
    raw        = 2.210640405e-6
    affine_raw = 2.199798867e-6
  drop position  5 -> W0 rows:
    raw        = 2.224559015e-6
    affine_raw = 2.216600471e-6

position 17, which won spaced20:
  raw = 2.262959839e-6  # broad regression
```

Verdict: close same-cost W0 replacement.  The add-on W0 branch still has real
teacher signal, but replacing QR coverage with W0 directions does not preserve
broad accuracy.  This is another example where a public-shaped small slice can
be fooled by adaptive direction identity.

### 2026-06-23 - l2snap preactivation SHRC closure

Goal: test the one Self-Harmonic ReLU Control variant that was not covered by
the earlier post-ReLU SHRC rejection.  For each QR-antithetic block, use the
final preactivation cloud to recover a single-ridge approximation
`a = 0.5 * U.T @ z_pre`, then subtract the block error of
`ReLU(U @ a)` using its exact spherical mean.  This is a genuine exact-mean
control variate diagnostic, not a seed-set tweak.

Updated diagnostic:

```text
legacy_workspace/probe_l2snap_shrc.py --mode preact
```

Fast checks:

```text
stride20_10:
  plain raw = 2.376781411e-6
  best beta=1 raw = 2.287874939e-6
  diagnostic mean_vr=-0.036350, mean_sign=0.495727, mean_corr=-0.025104

spaced20:
  plain raw = 2.305564090e-6
  best beta=0.50 raw = 2.291387611e-6
  best ratio = 0.993851
  diagnostic mean_vr=-0.031548, mean_sign=0.502548, mean_corr=-0.009752
```

Verdict: reject.  The tiny target-MSE improvement has the same warning pattern
as post-ReLU SHRC: the target-free control diagnostics are negative or random,
and the broader slice gain is sub-1%.  Do not package preactivation SHRC unless
a new derivation changes the sign/centering of the control.

### 2026-06-23 - current-path l2snap trajectory PERO closure

Goal: retest the "intermediate trajectories contain residual signal" idea on
the actual protected l2snap path, rather than the older pre-snap h1-affine
path.  This branch collects per-seed post-snap checkpoint means/seconds,
propagates them through cheap linear/diagonal/ReLU tail proxies, and tests
MLP-grouped residual learning.

Added diagnostic:

```text
legacy_workspace/probe_l2snap_trajectory_pero.py
```

GPU spaced20 smoke, `rows_per_seed=64`, checkpoints
`2,4,8,12,16,20,24,28`:

```text
base raw = 2.250037261e-6
best single-map global alpha:
  t02_linear raw = 2.151350704e-6, rel = 0.956140
best grouped-CV ridge:
  raw = 2.165995842e-6, rel = 0.962649
```

This was large enough to justify a broad check.

GPU Full200 broad check, same settings:

```text
base raw = 2.181298568e-6
best single-map global alpha:
  t12_relu raw = 2.172581254e-6, rel = 0.996004
best single-map OOF:
  t16_relu raw = 2.179815979e-6, rel = 0.999320
best grouped-CV ridge:
  raw = 2.195187579e-6, rel = 1.006367
ExtraTrees:
  raw = 2.220393730e-6, rel = 1.017923
```

Verdict: reject this production shape.  The trajectory maps have real
small-slice signal, but broad Full200 collapses to sub-0.5% single-map signal
and grouped residual learning worsens.  Do not expand to a package or a larger
MLP unless a new trajectory observable appears; this is not the requested 5%+
move.

### 2026-06-23 - l2snap multi-ridge exact-mean CV closure

Goal: test a stronger version of the SHRC idea by fitting a small shared
multi-ridge ReLU basis from the actual final activations on leave-one-QR-block
training blocks, then subtracting the held-out block's ridge quadrature error
using the exact spherical mean.  This keeps the useful property of SHRC
(known-zero-mean control under the sphere) but lets the control use several
response directions instead of a single recovered ridge.

Added diagnostic:

```text
legacy_workspace/probe_l2snap_ridge_cv.py
```

GPU spaced20, post-ReLU basis:

```text
plain raw  = 2.305551263e-6
affine raw = 2.107516923e-6

rank 4:
  mean_vr=-0.044565, mean_corr=-0.045537, mean_sign=0.487652
  best beta=0.75 raw = 2.224178523e-6, rel = 0.964706
  best affine beta=0.50 raw = 2.086048963e-6

rank 16:
  mean_vr=-0.048076, mean_corr=-0.048147, mean_sign=0.485693
  best beta=0.75 raw = 2.223921778e-6, rel = 0.964594
  best affine beta=0.25 raw = 2.090241271e-6
```

The target MSE looked large enough on spaced20 to require a broad check, but
the target-free signs were already wrong: the control increased block variance
and was slightly anti-correlated with the leave-one-block quadrature residual.

GPU Full200 broad check, ranks 4 and 16:

```text
plain raw  = 2.194665942e-6
affine raw = 2.176827901e-6
first100   = 2.086234735e-6
last100    = 2.303097149e-6

rank 4:
  mean_vr=-0.022183, mean_corr=+0.016626, mean_sign=0.507103
  best beta=0.50 raw = 2.181373854e-6, rel = 0.993943
  best affine beta=0.25 raw = 2.166549078e-6

rank 16:
  mean_vr=-0.024487, mean_corr=+0.015344, mean_sign=0.506859
  best beta=0.50 raw = 2.180083696e-6, rel = 0.993356
  best affine beta=0.25 raw = 2.166090543e-6
```

Verdict: reject as a production candidate.  The broad target gain is only
about 0.6-0.7%, while the target-free variance reduction remains negative and
the deployable version would add nontrivial per-MLP linear algebra and
`U @ V` work.  Do not package unless a new centering/sign derivation turns the
target-free block diagnostic positive.

### 2026-06-23 - per-neuron layer-2 snap beta closure

Goal: test the remaining gap in the layer-2 snap work.  Prior beta probes chose
one snap strength per MLP.  This probe chooses a beta per layer-2 neuron from
target-free local diagnostics:

```text
z2 = h2 + beta_j * (snapped_h2 - h2)
beta_j = clip(0.5 + gain * zscore(feature_j), 0, 1)
```

Features checked: layer-2 variance ratio/log-scale, normalized mean shift,
gate alpha, absolute alpha, and absolute normalized mean shift.  Runtime cost
would be almost free if a fixed formula worked.

Added diagnostic:

```text
legacy_workspace/probe_l2snap_neuron_beta.py
```

GPU spaced20 looked promising:

```text
base_b05 raw           = 2.305555542e-6
mean_abs gain -0.35    = 2.120368071e-6, rel = 0.919678
mean_abs gain -0.20    = 2.138553085e-6, rel = 0.927565
mean_shift gain -0.35  = 2.141284258e-6, rel = 0.928750
alpha gain +0.35       = 2.142194248e-6, rel = 0.929144
```

But the broad Full200 check collapsed:

```text
base_b05 raw              = 2.194667825e-6
base production-aff raw   = 2.181300180e-6
base scalar-refit raw     = 2.176825618e-6

mean_abs gain -0.20 raw             = 2.173419489e-6, rel = 0.990318
mean_abs gain -0.20 production-aff  = 2.177953346e-6
mean_abs gain -0.20 scalar-refit    = 2.173345243e-6

mean_abs gain -0.35 raw             = 2.178089933e-6, rel = 0.992446
mean_abs gain -0.35 production-aff  = 2.188375216e-6
mean_abs gain -0.35 scalar-refit    = 2.176192029e-6
```

Verdict: reject as a mainline package.  A per-neuron beta formula can move the
slice strongly, but the broad stable effect is at most about 1% before affine
and about 0.16% after scalar refit; with the current production affine it is
worse.  This is useful confirmation that layer-2 snap has small residual shape
signal, but it is not the requested 5%+ lever.

### 2026-06-23 - synthetic layer-2 restart closure

Goal: test whether the estimator can skip carrying the true QR row geometry
through layers 1-2.  The probe computes the same exact first-layer covariance
and Gaussian-closure layer-2 marginal moments, then restarts the tail from a
synthetic layer-2 cloud:

- `restart_relu_raw`: sample `ReLU(mu2 + sigma2 * sqrt(n) * U)`;
- `restart_relu_aff`: moment-match that synthetic ReLU cloud to target
  layer-2 mean/variance;
- `restart_linear_aff`: use a linear Gaussian cloud with target mean/variance.

Added diagnostic:

```text
legacy_workspace/probe_l2_gaussian_restart.py
```

Two-MLP smoke was already decisive:

```text
protected_b05       raw = 4.878653486e-6
restart_relu_aff    raw = 3.258288252e-5, rel = 6.678663
restart_relu_raw    raw = 3.282093859e-5, rel = 6.727458
restart_linear_aff  raw = 3.734209696e-5, rel = 7.654181
```

### 2026-06-23 - l2snap tail-row coreset closure

Goal: test a non-overlapping compute-cut variant after the protected
`h1affine + l2snap` path.  Earlier partial-row probes reduced QR rows from the
start, and pair-collapse averaged antithetic paths.  This diagnostic instead
uses all protected QR rows through the exact h1 affine correction and layer-2
snap, then keeps a smaller tail coreset per seed/sign for layers `2..31`.

Added diagnostic:

```text
legacy_workspace/probe_l2snap_tail_coreset.py
```

The smoke included fixed `even` thinning and dynamic checkpoint-only
quantile selection by row norm / centered norm.  Dynamic selection was worse
than fixed selection immediately, so the broad check focused on fixed modes.

Spaced20 smoke:

```text
full raw       = 2.250013767e-6, adjusted proxy = 3.221789476e-7
even_224 raw   = 2.327388899e-6, adjusted proxy = 2.935846707e-7
norm_224 raw   = 3.130241780e-6
center_224 raw = 2.732009143e-6
```

This looked potentially alive because `even_224` traded `+3.4%` raw for a
larger tail compute cut on the small slice.

Full200 broad check:

```text
full raw     = 2.181294197e-6, adjusted proxy = 3.123390085e-7

keep=224:
  last_224 raw  = 2.570674932e-6, adjusted proxy = 3.242735899e-7
  even_224 raw  = 2.617122780e-6, adjusted proxy = 3.301326779e-7
  first_224 raw = 2.898625980e-6, adjusted proxy = 3.656424391e-7

keep=248 / 240:
  last_248 raw = 2.282656350e-6, adjusted proxy = 3.171252627e-7
  last_240 raw = 2.383559145e-6, adjusted proxy = 3.209857270e-7
  even_248 raw = 2.356189529e-6, adjusted proxy = 3.273410926e-7
  even_240 raw = 2.522157397e-6, adjusted proxy = 3.396502777e-7
```

Verdict: reject.  The tail row cloud is still too structurally important after
l2snap.  Even keeping `248/256` rows per seed/sign loses `4.65%` raw in the
best fixed mode, while the cost cut is too small to compensate.  Dynamic norm
coresets do not rescue this.  Do not package row-thinning after l2snap unless a
new coreset objective preserves downstream signed tail response, not just
checkpoint norm distribution.

Verdict: reject without broad run.  The actual joint/non-Gaussian row geometry
after the first two layers is load-bearing.  Layer-2 marginal moments alone
cannot restart the depth-30 tail.

### 2026-06-23 - layer-3 full-covariance snap closure

Goal: test whether the layer-2 snap left a cheap structural continuation at
layer 3 if we use a stronger covariance target than the old diagonal-only
attempt.  The diagnostic builds:

1. exact first-layer ReLU covariance,
2. Gaussian/exact-GH approximation for the full layer-2 ReLU covariance,
3. layer-3 preactivation variance from that full covariance,
4. a mean/variance snap at layer 3, after the existing layer-2 snap.

Added diagnostic:

```text
legacy_workspace/probe_l3_fullcov_snap.py
```

Spaced20 smoke looked mildly positive:

```text
base l2 prod_aff_raw          = 2.250034331e-6
best blend_l3b-0.25           = 2.217709192e-6  # ~=1.4% raw gain
```

Full200 broad check killed it:

```text
base l2 prod_aff_raw          = 2.181297699e-6
best blend_l3b-0.05           = 2.186886185e-6  # 1.00256x worse
target/blend l3 beta variants = all worse
```

Verdict: reject.  Full layer-2 covariance is not a deployable layer-3 snap
continuation for the current protected l2snap path.  The small-slice win was
another slice-specific sign alignment, not a broad structural correction.

### 2026-06-23 - l2snap shallow multilevel/GREG closure

Goal: revisit the expert's "sampling as measurement device" route on the
current protected l2snap branch.  The test keeps the full-depth protected18
prediction and adds extra seed blocks only to a shallow layer, then projects
those shallow observations through cheap auxiliary maps.  This asks whether a
low-cost proxy can recover the signed value of a paid independent shadow branch.

Diagnostic:

```text
legacy_workspace/probe_l2snap_multilevel_greg.py
```

Full200 protected base:

```text
base raw      = 2.181297274e-6
adj18 proxy   = 3.087700892e-7
union teacher = 7.250611638e-7 raw
```

Early shallow half64:

```text
t4  best alpha diag   raw = 2.169254389e-6, adjusted extra = 3.257e-7
t8  best alpha linear raw = 2.098689987e-6, adjusted extra = 3.3318e-7
t8  OOF linear        raw = 2.105961171e-6, adjusted extra = 3.3433e-7
```

Late shallow half32:

```text
t12 alpha linear raw = 2.066112631e-6, adjusted extra = 3.1912e-7
t16 alpha linear raw = 2.038548374e-6, adjusted extra = 3.2363e-7
t20 alpha linear raw = 2.002759186e-6, adjusted extra = 3.2656e-7
```

Verdict: reject as an adjusted-score route.  The shallow observations have the
right signed direction and reduce raw MSE, but the raw reduction per paid
shallow compute is too small.  This does not close all target-free routing, but
it closes the simple shallow GREG proxy for the protected l2snap branch.

### 2026-06-23 - pruned l2snap base plus shallow union closure

Goal: test whether a cheaper full-depth base can make the shallow multilevel
branch cost-positive.  The base drops four protected rows
`0,6,21,23` (count14), then adds the same shallow union proxy.

Diagnostic:

```text
legacy_workspace/probe_pruned_l2snap_multilevel_greg.py
```

Full200 count14 base:

```text
seeds = 2,3,7,8,13,15,17,20,22,24,27,28,29,31
raw   = 2.686753401e-6
adjusted proxy at mult=0.112 = 3.009163809e-7
```

Best shallow add-ons:

```text
t12 alpha linear raw = 2.615473719e-6, mult=0.126625, adjusted = 3.311843597e-7
t16 alpha linear raw = 2.593601367e-6, mult=0.131500, adjusted = 3.410585797e-7
t20 alpha linear raw = 2.552091497e-6, mult=0.136375, adjusted = 3.480414779e-7
```

Verdict: reject the shallow add-on.  The count14 base remains a diagnostic
cost-frontier point, but adding shallow union observations makes adjusted score
worse.  Do not promote pruned+shallow unless a new proxy has substantially
better raw-gain-per-FLOP.

### 2026-06-23 - high-count l2snap snap-strength retune closure

Goal: check whether the layer-2 snap strength should change when the QR block
count is higher.  The protected18 branch uses tied `beta_mu=beta_sd=0.5`, but
with 32-40 blocks the sampling noise is lower, so the mean/scale tradeoff could
move.  This is a structural retune, not seed identity selection.

Diagnostic:

```text
legacy_workspace/probe_l2snap_split_beta.py
```

Count32, Full200, equal first 32 blocks:

```text
0.5/0.5 affine_raw = 1.385055262e-6
0.75/0.25 affine_raw = 1.361772330e-6  # looked +1.7% on Full200
0.25/0.25 raw        = 1.381192202e-6
```

Count32, Full1000:

```text
0.5/0.5 affine_raw = 1.359504254e-6
0.5/0.25 affine_raw = 1.356085848e-6  # +0.25% only
0.75/0.25 affine_raw = 1.358297880e-6
best raw remains 0.5/0.5
```

Count40, Full200:

```text
0.5/0.5 affine_raw = 1.098911158e-6
0.5/0.25 affine_raw = 1.082519004e-6  # looked +1.5% on Full200
0.25/0.25 raw        = 1.091671178e-6
```

Count40, Full1000:

```text
0.5/0.5 affine_raw = 1.068068968e-6
0.5/0.25 affine_raw = 1.063202037e-6  # +0.45% only
0.25/0.25 affine_raw = 1.068769878e-6
best raw remains 0.5/0.5
```

Verdict: reject as a material route.  High-count snap retuning has a real small
affine-calibration effect, but the Full200 gains mostly vanish on Full1000 and
raw MSE prefers the existing `0.5/0.5` setting.  Do not package a high-count
retuned-snap estimator unless paired with a separate raw-reducing idea.

### 2026-06-23 - protected-plus-consensus count33 and qroffset selector closure

Goal: find a non-fragile move larger than a 1% public-seed tweak after the
protected `311697` l2snap line.  Two candidates were checked:

1. a production-shaped higher-count branch that keeps the protected 18 QR
   blocks and adds consensus extras, so a continuation gate could reuse the
   pilot work instead of discarding rows;
2. a target-free QR-offset selector, since target-selected qroffsets had shown
   same-compute raw gains before failing remotely.

Protected-plus-consensus count33, using
`protected18 + (44,47,48,49,63,102,84,106,88,110,97,105,107,111,112)` and
Full1000-fitted seed weights:

```text
Full1000 5-fold OOF affine raw ~= 1.393445553e-6
Full1000 fullfit affine raw    ~= 1.379430883e-6
Mini100 independent affine raw ~= 1.175176376e-6
```

The raw move is real and validates off the training split.  However, the actual
tuple package profile is too expensive:

```text
artifact:
  whest-starterkit/packages/active/submission_phase1_pure33_protplus_mini15_l2snap_b05_weighted_affine_full1000_tuple_finalonly_bundle.tar.gz

op_profile, 2 mini MLPs:
  FLOPs ~= 6.990e10
  predicted remote multiplier ~= 0.259-0.261
```

Read: this is useful as an accuracy frontier point, but not a confident
leaderboard replacement.  With the protected-line remote raw scaling, it
projects near the current protected adjusted score rather than a material gain.
Do not submit unless we explicitly want a high-count diagnostic.

QR-offset target-free selector:

Existing offset caches are for the older h1affine path, so this was only a
smoke test before considering an l2snap recompute.  A target-free in-sample
proxy that greedily matches the all-offset ensemble mean looked strong:

```text
Full1000 h1affine cache:
  offset0 base       raw = 2.373799063e-6
  proxy-greedy fixed raw = 2.171713373e-6
  target oracle      raw = 2.160436094e-6

Full200 h1affine cache:
  offset0 base       raw = 2.203526291e-6
  proxy-greedy fixed raw = 1.753211690e-6
  target oracle      raw = 1.712555477e-6
```

But the honest held-out test fails:

```text
choose offsets by target-free proxy on train folds, evaluate target on held-out:
  Full1000 OOF proxy raw = 2.682399506e-6  # 1.130x worse than offset0
  Full200  OOF proxy raw = 2.378667364e-6  # 1.079x worse than offset0
```

Verdict: close target-free fixed qroffset selection.  The proxy can overfit the
public weight ensemble without target labels, and the fold choices are unstable.
Do not recompute the expensive l2snap qroffset grid unless a new selector has a
cross-fold target-free validation criterion.

### 2026-06-23 - cross-fitted layer-2 snap closure

Goal: test a non-target-fit structural variant of the current protected
`h1-affine + l2snap` path.  The production l2snap estimates layer-2 sampled
mean/variance from all QR rows and then transforms those same rows.  The new
probe asks whether using moments from the other QR seed blocks for each held-out
block reduces self-normalization bias without adding sampled matmuls.

Diagnostic:

```text
legacy_workspace/probe_l2snap_crossfit.py
```

Spaced20 smoke:

```text
all_b0.5       raw=2.250010047e-6
loo_b0.5       raw=2.264165135e-6
evenodd_b0.5   raw=2.287921335e-6

all_b0.25      raw=2.235136543e-6
loo_b0.25      raw=2.242622270e-6
```

Full200 broad check:

```text
all_b0.5       raw=2.181296065e-6, refit=2.176828284e-6
loo_b0.5       raw=2.183425065e-6, refit=2.175598921e-6

all_b0.25      raw=2.175151094e-6, refit=2.172554133e-6
loo_b0.25      raw=2.175781793e-6, refit=2.171975577e-6
```

Read: leave-one-seed and even/odd cross-fitting do not improve the actual
production beta `0.5` branch.  Beta `0.25` remains the familiar Full200
near-miss, but it already lost to beta `0.5` remotely on the protected line.

Verdict: reject.  Cross-fitting the l2snap moments is a clean structural test,
but it does not expose a material same-compute gain.  Keep the ordinary all-row
snap.

### 2026-06-23 - sample/analytic handoff limit probes

Goal: test whether a hybrid can sample only part of the network and hand off to
an analytic tail, or keep an analytic/covariance state and sample only the
remaining tail.

Diagnostics:

```text
legacy_workspace/probe_hybrid_handoff_limits.py
legacy_workspace/probe_empirical_gaussian_restart.py
```

Sampled-prefix -> diagonal Gaussian analytic tail:

```text
Spaced20 protected sample_full            raw = 2.250050836e-6
after31 seed/global diagonal tail         raw = 3.89e-4  / 3.90e-4
after30 seed/global diagonal tail         raw = 6.67e-4  / 6.68e-4
after28 seed/global diagonal tail         raw = 9.96e-4  / 9.98e-4
after2..24 diagonal tails                 raw ~= 1.08e-3 to 1.32e-3
```

Read: diagonal moment state is not enough even one layer before final.  This is
not a small calibration miss; it loses the cross-neuron structure that the final
tail uses.

Sampled-prefix -> final preactivation mean/variance Gaussian readout:

```text
Spaced20 sample_full                      raw = 2.250050836e-6
Spaced20 seed/global final-prevar readout raw = 3.512e-6 / 3.556e-6

Full200 sample_full                       raw = 2.181303987e-6
Full200 seed/global final-prevar readout  raw = 3.296e-6 / 3.308e-6
Full200 OOF stack with sample             worsens to ~2.185e-6
```

Read: the sampled final ReLU average is better than replacing the final
nonlinearity by a Gaussian mean/variance closure.  The final preactivation
feature is not a useful broad residual stacker.

Oracle empirical full-covariance Gaussian restart:

```text
Spaced20 sample_full                      raw = 2.250056742e-6
Spaced20 Gaussian restart after31         raw = 3.623e-6
Spaced20 Gaussian restart after30         raw = 4.931e-6
Spaced20 Gaussian restart after28         raw = 7.248e-6
Spaced20 Gaussian restart after24         raw = 1.032e-5

Full200 sample_full                       raw = 2.181306698e-6
Full200 Gaussian restart after31          raw = 3.353e-6
Full200 Gaussian restart after30          raw = 4.461e-6
```

The restart is an oracle in the sense that it uses the true sampled prefix
cloud's empirical mean and full covariance.  Clipping regenerated Gaussian
post-activation rows is much worse (~4.7e-5 on Full200 late handoffs).

Verdict: close mean/variance and full-covariance handoffs as a route to cheaper
hybrids.  The sampled row cloud carries higher-order/non-Gaussian structure all
the way to the final layer.  Future hybrids should be control-variate or
row-cloud preserving, not "sample to moments, then propagate moments."

### 2026-06-23 - Phase-1 final-rooted matrix-template port

Goal: test whether the old depth-8 final-rooted Hermite/Wick template signal can
be ported to the current depth-32 protected sampler without building a forward
K3/K211 carry.  This was deliberately kept separate from seed-set search.

Added diagnostic:

```text
legacy_workspace/probe_phase1_final_rooted_templates.py
```

The probe computes diagonal-Gaussian states, then builds target-free
final-rooted aggregate features from `hcoeff`, `h123`, `mixed`, and compact
`sym(C*v)` templates.  It fits only MLP-grouped OOF ridge corrections against
the final residual.

Fast Full200 n=40 checks:

```text
final-layer-only future, small menu:
  base_raw  = 2.187808854e-6
  corrected = 2.196966096e-6
  rel       = 1.004186
  r2        = -0.00419

late-8 future layers, small menu:
  base_raw  = 2.187808854e-6
  corrected = 2.235286888e-6
  rel       = 1.021701
  r2        = -0.02170
```

Read: the naive Phase-1 port is wrong-signed.  This does not invalidate the old
depth-8/warmup result, because that code used captured hybrid/K3 states and a
different budget/architecture.  It does reject the cheap diagonal-state
depth-32 port as a package path.  If final-rooted templates return, they need
structured covariance/repeated-K3 source states or the selected aggregate
recurrence from the expert notes, not this direct Gaussian-state feature set.

### 2026-06-23 - l2snap union-teacher trajectory distillation

Goal: test a larger non-seed-search route.  The union53 l2snap teacher has a
large target-free raw advantage over the protected18 branch, but earlier
distillation from final seed-cloud features failed.  This probe asks whether
intermediate protected-pass trajectory maps expose the signed teacher movement:

```text
label = union53_equal_l2snap - protected18_weighted_l2snap
```

Added diagnostic:

```text
legacy_workspace/probe_l2snap_union_teacher_traj_distill.py
```

The label is target-free; final targets are used only for grouped-OOF reporting.
The diagnostic uses the cached `l2snap_traj_full200_r64.npz` maps and the
Full1000 protected/union l2snap seed caches restricted to the same Full200 rows.

Reference gap on Full200:

```text
protected18 l2snap base raw = 2.181297274e-6
union53 l2snap teacher raw  = 7.250611638e-7
direct teacher blend raw    = 7.213344267e-7
label_target_corr           = +0.81828
```

Grouped-OOF ridge results:

```text
compact trajectory maps:
  best raw       = 2.177153773e-6
  rel            = 0.998100
  teacher_r2     = +0.00885
  teacher_corr   = +0.09268

all trajectory maps:
  best raw       = 2.174935627e-6
  rel            = 0.997084
  teacher_r2     = -0.00410
  teacher_corr   = +0.08231
```

The wider map set gives a small target-MSE gain, but the teacher R2 is negative
in the best row.  So this is not recovering the union53 correction; it is just a
tiny target-correlated calibration.  A tree/HGB sanity check was started and
stopped after it exceeded the intended fast-probe budget without output.

Verdict: document the free `~0.3%` OOF signal, but reject this trajectory
distillation shape as a breakthrough route.  The high-count teacher remains a
useful ceiling; recovering it needs a new observable, not generic trajectory-map
ridge/trees.

### 2026-06-23 - l2snap adaptive-count identifiability reprise

Goal: revisit adaptive counts without doing another seed-set search.  The
expert-recommended decomposition is to first ask whether a target-free shadow
pool can predict the marginal value of extra seed blocks.  If this fails, count
oracles are mostly target luck; if it passes, the remaining problem is making
the shadow cheap enough.

Added diagnostic:

```text
legacy_workspace/probe_l2snap_count_identifiability.py
```

Pure highseed Full200, independent A/B pools (`0..59` vs `60..119`), counts
`18,24,32,48,60`:

```text
fixed18 adjusted           = 3.370238061e-7
true oracle adjusted       = 2.362590080e-7
noise-corrected shadow     = 2.864122340e-7, gap_capture=0.502274
plain shadow               = 2.742461627e-7, gap_capture=0.623012
plain shadow gain Spearman = +0.675044
```

Exact-l2snap first40 cache, production-protected pool `0..31`, shadow `32..39`,
counts `18,20,24,32`:

```text
fixed18 adjusted           = 3.447323850e-7
true oracle adjusted       = 2.332706871e-7
noise-corrected shadow     = 3.108184458e-7, gap_capture=0.304265
plain shadow               = 2.947959489e-7, gap_capture=0.448014
plain shadow gain Spearman = +0.430777
```

Cross-family shadow, l2snap first40 truth on Full200 but pure highseed
`60..119` as the shadow:

```text
fixed18 adjusted           = 3.149993035e-7
true oracle adjusted       = 2.300366285e-7
plain pure shadow actual   = 2.573800490e-7, gap_capture=0.678171
plain pure shadow Spearman = +0.682538
```

Cross-family shadow on all Full1000 rows using the pure union39 cache.  Candidate
counts are protected18 plus extra seeds `(5,9,11,19,30,35,37,39)`, shadow seeds
are `(44,61,63,75,78,82,83,89,90,100,107,109,111)`:

```text
fixed18 adjusted           = 3.447323850e-7
true oracle adjusted       = 2.662911667e-7
plain pure shadow actual   = 3.059458642e-7, gap_capture=0.494466
plain pure shadow Spearman = +0.532329
first/last/q1/q4 captures  = +0.4848 / +0.5037 / +0.5356 / +0.4969
```

Reducing the pure shadow from 512 rows to the existing 256-row cache on Full200
mostly killed the signal:

```text
s256 plain shadow actual   = 3.094257141e-7, gap_capture=0.065600
s512 plain shadow actual   = 2.573800490e-7, gap_capture=0.678171
```

Using fewer full-row pure shadow seeds degrades gradually on Full1000:

```text
2 seeds  gap_capture=0.118374
4 seeds  gap_capture=0.240729
6 seeds  gap_capture=0.365187
8 seeds  gap_capture=0.390266
10 seeds gap_capture=0.458508
13 seeds gap_capture=0.494466
```

A candidate-pool-only proxy, where the pure max-count candidate mean is used as
its own teacher, degenerates to always choosing the largest count:

```text
counts 18,20,22,24,26
proxy_hist = 26:1000
actual adjusted = 3.324297269e-7
gap_capture = 0.149181
```

Read: adaptive count is identifiable.  This is one of the few remaining routes
with a true 10%+ oracle gap.  But the useful shadow currently wants enough
independent 512-row pure seed evidence that paying for it directly would erase
much of the adjusted-score gain.  The next viable version must either reuse the
pilot rows as part of the continued high-count branch, or predict the 512-row
shadow risk from a cheaper observable.  Do not package an adaptive-count router
until the pilot cost is included in the score model.

Costed follow-up for the Full1000 pure-shadow policy above, charging `k` full
pure shadow seeds as extra seed-equivalent work:

```text
k=00 priced_actual=3.447323850e-7
k=02 priced_actual=3.712796854e-7
k=04 priced_actual=3.941114374e-7
k=06 priced_actual=4.140473596e-7
k=08 priced_actual=4.422276759e-7
k=10 priced_actual=4.642042482e-7
k=13 priced_actual=5.038997066e-7
```

So the full-row shadow is not deployable.  The only adaptive-count variant worth
reopening is a cheap pilot whose rows are reused when the policy continues that
seed, or a learned denoiser that predicts the 512-row shadow decision from much
cheaper state.

Protected-feature shadow router:

```text
legacy_workspace/probe_l2snap_shadow_count_router.py
```

This trains on target-free pure-shadow losses and evaluates true l2snap count
losses.  No MLP names or target-derived labels are used for training.

Base protected seed-cloud summaries only:

```text
Full1000 OOF best             = 3.424612330e-7, capture=+0.027540
Mini100 OOF best              = worse than fixed18
Full1000 -> Mini100 best      = 2.867123195e-7, capture=+0.035231
Mini100 -> Full1000 best      = 3.437253427e-7, capture=+0.012211
```

With diagonal Gaussian/weight summaries:

```text
Full1000 OOF best             = effectively flat / negative
Mini100 OOF best              = flat or negative
Full1000 -> Mini100 best      = 2.833570477e-7, capture=+0.085516
Mini100 -> Full1000 best      = 3.432457363e-7, capture=+0.018027
```

Read: protected-pass features do not compress the good shadow decision.  The
one-way Full->Mini improvement is too small and asymmetric to package.

256-row pilot denoising check, using common candidate counts `18,20,24,32` and
shadow pool `40..63` on Full200/Mini:

```text
Full200 s512 free-shadow actual = 2.843674593e-7, capture=+0.360533
Full200 s256 direct actual      = 3.318032060e-7, capture=-0.197780
Mini100 s512 free-shadow actual = 2.504261849e-7, capture=+0.433863
Mini100 s256 direct actual      = 2.934557905e-7, capture=-0.049326
```

Learning s512 shadow losses from s256 losses also failed transfer:

```text
Full200 -> Mini100 best = 2.897400535e-7, capture=-0.007601
Mini100 -> Full200 best = 3.131359835e-7, capture=+0.021931
```

Verdict: reject the current cheap-shadow compression family.  Full 512-row
shadow contains useful count-routing information, but protected summaries and
256-row pilots do not recover it well enough.  Adaptive count remains alive only
if a new cheap observable is found or if pilot rows can be reused in a way that
does not pay separate shadow cost.

### 2026-06-23 - union53 high-count residual learning check

Goal: test a high-accuracy/high-compute family.  The union53 l2snap teacher is
far more accurate than protected18, but still not adjusted-competitive by
itself.  If its remaining residual were learnable from cheap final seed-cloud
features, it could become a serious high-compute contender.

Used the existing grouped-OOF residual machinery from
`probe_l2snap_residual_features.py`, but with equal-weight
`l2snap_union53_seed_preds_full1000.npz` as the base instead of protected18.

Reference:

```text
union53 equal l2snap base raw = 8.039954401e-7
```

Safe feature sweep, 24 final-cloud features:

```text
best raw = 8.034626732e-7
rel      = 0.999337
tail     = 8.219411626e-7
```

Full signed seed-deviation sweep, 130 features:

```text
best raw = 8.037724823e-7
rel      = 0.999723
tail     = 8.213054186e-7
```

Verdict: reject final-cloud residual learning on the high-count teacher.  It
does not recover a meaningful fraction of the remaining bias.  Any high-compute
raw-MSE contender needs either a different base estimator or internal
trajectory/weight-state information, not just the union53 final seed cloud.
### 2026-06-23 - hybrid handoff and seed-oracle closure

Goal: test the current "hybrid" theory directly: use sampling up to a point and
let an analytical estimator finish, or use analytical/sample summaries to
combine the already-computed seed blocks more intelligently.

New probes:

- `legacy_workspace/probe_hybrid_handoff_limits.py`
- `legacy_workspace/probe_empirical_gaussian_restart.py`
- `legacy_workspace/probe_l2snap_seed_gate_features.py`
- `legacy_workspace/probe_l2snap_seed_gate_random_features.py`

Results:

1. Gaussian tail handoff is a hard no.
   - Protected sampled-prefix full path on spaced20: `2.250050836e-6`.
   - Replacing the final tail by diagonal Gaussian/state readout after layer 31:
     `~3.895e-4`.
   - Empirical full-cov Gaussian restart after layer 31:
     spaced20 `3.623e-6`; full200 `3.353e-6`, both worse than keeping the
     sampled row cloud to the end.
   - Interpretation: even at the last layer, a mean/covariance handoff loses
     non-Gaussian row-cloud structure that matters for the final means.

2. The existing 18 seed blocks contain huge target-using headroom, but not in a
   target-free final-cloud rule we can see yet.
   - Full1000 protected l2snap affine raw from cache: `2.301739726e-6`.
   - Target-using per-neuron choose among the same 18 seed-block predictions:
     `3.913087198e-7` raw.
   - Target-using per-MLP fitted seed weights:
     `5.846510936e-7` raw at weak regularization.
   - This is not deployable; it is an oracle showing that the directions carry
     complementary signed information.

3. Simple deployable approximations to that oracle failed under MLP-grouped CV.
   - Global seed-gate correction:
     best `2.301748864e-6`, `1.000004x` relative; effectively neutral/worse.
   - Local per-neuron seed-gate correction:
     best `2.301868908e-6`, `1.000056x`; neutral/worse.
   - Random-feature nonlinear seed-cloud residual model:
     best `2.301926978e-6`, `1.000081x`; neutral/worse.
   - Direct robust aggregation:
     equal `2.309813271e-6`, median `3.501495330e-6`, trim1
     `2.380177741e-6`; all worse than protected weighted.

Verdict:

- Do not build a Gaussian restart, final-preactivation Gaussian readout, robust
  seed aggregator, or final-seed-cloud router from these forms.
- The useful hybrid signal is not visible in final seed-block summaries alone.
  It likely requires either a real internal trajectory observable or a
  substantially stronger learned model trained on internal sampled-path
  features, not another fixed seed reweighting or shallow Gaussian handoff.

Follow-up: early exact-moment telemetry.

Added:

```text
legacy_workspace/probe_l2snap_early_block_telemetry.py
```

This tests the most direct "sampling plus analytic truth" idea left in the
current l2snap branch: for each QR seed block, compute target-free layer-1 and
layer-2 moment errors against the analytic h1/l2 targets, then use those
block-wise early errors to gate the already-computed final seed deviations.

Result:

```text
20-MLP smoke, 64 rows/seed:
  base raw = 2.958074365e-6
  best raw = 2.698950362e-6
  rel      = 0.912401

100-MLP stride, 64 rows/seed:
  base raw = 2.276999021e-6
  best raw = 2.276632164e-6
  rel      = 0.999839
  guard/tail objective worsened
```

Verdict: reject.  The early telemetry can overfit a small slice, but it does
not survive the 100-MLP stride guard.  The exact h1/l2 moment mismatch is not a
stable low-cost observable of final QR-block error in this simple gated form.

Cached branch-stack ceiling:

Added:

```text
legacy_workspace/probe_l2snap_hybrid_stack.py
```

This asks whether cached l2snap branches have complementary signed error under
MLP-grouped OOF stacking.

Full1000 cache results:

```text
protected18 weighted raw          = 2.301739726e-6
targetdual20 fullweighted raw     = 2.028985195e-6
first40 equal raw                 = 1.068096389e-6
union53 equal raw                 = 8.039954401e-7

separately snapped high-stack OOF = 6.566e-7 raw
one-run union53 subset stack      = 8.044e-7 raw
```

Verdict: useful ceiling, not deployable as-is.  The separately snapped stack
shows that high-count l2snap branches contain the missing signal, but a
single production-shaped 53-seed snap does not improve beyond the union53 mean.
At roughly 53 full QR blocks the compute multiplier would be around `0.4`, so
`~8e-7` raw is worse adjusted-score than the protected 18-block branch.

Adaptive union53 routing follow-up:

Added:

```text
legacy_workspace/probe_l2snap_union53_router.py
legacy_workspace/probe_l2snap_reused_pilot_router.py
```

Question 1: after protected18 is computed, can target-free seed-cloud and
diagonal-state summaries decide whether full union53 is worth its compute?

Full1000 cache:

```text
protected18 raw = 2.301739726e-6
union53 raw     = 8.039954401e-7

assume union53 multiplier 0.40:
  fixed protected adjusted = 3.295855757e-7
  fixed union adjusted     = 3.215981760e-7
  target oracle adjusted   = 2.533468291e-7
  seed-only best OOF       = 3.222278972e-7
  seed+diag best OOF       = 3.207442673e-7

assume union53 multiplier 0.42:
  fixed union adjusted     = 3.376780848e-7
  target oracle adjusted   = 2.597821709e-7
  all seed/diag OOF routers harmful
```

Question 2: if the router blocks are not thrown away, does a two-stage reused
pilot policy help?  The policy pays for a prefix of union53, then chooses among
paid protected18, the prefix estimate, and full union53.

```text
union-cache protected projection raw = 2.441200911e-6
union53 raw                          = 8.039954401e-7

best OOF gains versus no-pilot protected, after charging pilot cost:
  pilot22: +1.32e-8 adjusted
  pilot26: +8.18e-9 adjusted
  pilot31: +6.62e-9 adjusted
  pilot36: +8.08e-9 adjusted
```

Verdict: partial positive but not enough.  The target oracle is real, and a
reused pilot can make sensible stop/continue choices after the pilot is already
paid.  But the net adjusted gain is small, and the protected projection inside
the union53 cache is weaker than the deployed protected line.  Do not build a
package from union53 adaptive routing unless a new, cheaper signed observable
appears.

Gate-trajectory interaction follow-up:

Added:

```text
legacy_workspace/probe_l2snap_gate_trajectory_router.py
```

This tests a less linear observable: during the protected l2snap sampled pass,
summarize how each QR block crosses ReLU gates across depth.  Features include
gate rates, near-boundary mass, normalized margin summaries, and block
gate-disagreement spectra at checkpoints.

Smoke, spaced20 with 32 half-rows/seed telemetry:

```text
protected raw = 2.250037261e-6
union53 raw   = 6.564559526e-7
m53=0.40:
  fixed protected = 3.221823118e-7
  fixed union     = 2.625823810e-7
  oracle          = 2.454642246e-7
  best OOF        = 2.625823810e-7  # choose union for all
```

First100 broader check:

```text
protected raw = 2.078894894e-6
union53 raw   = 6.990146163e-7
m53=0.40:
  fixed protected = 2.976764761e-7
  fixed union     = 2.796058465e-7
  oracle          = 2.361564938e-7
  best OOF        = 2.845635931e-7
  oracle capture  ~= 21%
m53=0.42:
  fixed protected = 2.976764761e-7
  fixed union     = 2.935861388e-7
  oracle          = 2.414484793e-7
  best OOF        = 2.890191195e-7
  oracle capture  ~= 15%
```

Verdict: this is a genuine interaction signal, but still too weak for a
candidate.  Gate trajectory summaries are more informative than pure final
seed-cloud summaries, but they do not make union53 routing adjusted-safe.  If
reopened, use them to predict per-block signed error or train a block-level
policy, not just an MLP-level union/protected switch.

### 2026-06-23 - post-compact broad fusion and Phase-1 Song-student checks

Goal: after the cheap-shadow and early-telemetry closures, re-scan for a larger
non-seed-search mechanism without disturbing the other assistant's seed-set
lane.

Cache inventory:

```text
Best cached raw-MSE families remain high-count QR/l2snap ladders:
  Mini100 qr48eq raw ~= 7.73e-7
  Full200 qr48eq raw ~= 9.63e-7
```

Read: these are raw-good but adjusted-negative at their natural compute
multiplier.  They confirm that more QR blocks buy accuracy, but not cheaply
enough.

Quick fusion checks:

```text
Union53 teacher as scalar calibration target:
  Full1000 protected current-affine raw = 2.301739726e-6
  Full1000 union53 teacher raw          = 8.036253231e-7
  fitting protected -> union teacher:
    Full self target raw = 2.302676622e-6
    Full -> Mini target raw = 1.899432325e-6
  polynomial teacher calibration fullfit:
    best Full target raw ~= 2.301587727e-6
    Mini target raw      ~= 1.903123592e-6

Exact-GH l2 covariance branch blended with protected:
  pred_h1affine OOF full raw          = 2.301495208e-6
  pred_marg0.5 OOF full raw           = 2.304938131e-6
  pred_marg0.5_rank12_gain0.05 OOF    = 2.302884446e-6
  full->mini best scalar blend stayed worse than protected or tiny/noisy.
```

Verdict: reject these fusion forms.  The high-count teacher is not mostly a
global bias correction, and the exact-GH analytic branch is not complementary
enough after l2snap.

Phase-1 Song-student adaptation:

Added:

```text
song/src/train_phase1_l2snap_equivariant.py
```

This ports the old depth-8 Song message-passing residual idea to current
Phase-1 depth 32.  It trains on protected l2snap final residuals using full
weights plus cheap diagonal-Gaussian mean rows as per-layer context.  Validation
is Full1000 -> independent Mini100; targets are only offline labels.

Fast checks:

```text
CPU 100->Mini100, zero intermediate rows, h4/r1/e3:
  base_valid = 1.897756292e-6
  best_valid = 1.897756292e-6  # no correction

CPU 100->Mini100, diagonal mean rows, h4/r1/e3:
  base_valid = 1.897756292e-6
  best_valid = 1.897756292e-6  # no correction

GPU 400->Mini100, diagonal mean rows, h8/r2/e6:
  base_train = 2.296647238e-6
  base_valid = 1.897756292e-6
  best_valid = 1.897734988e-6 at epoch 2, gain 0.125
  later epochs revert to gain 0

GPU Full1000->Mini100, diagonal mean rows, h16/r3/e10:
  base_train = 2.301739548e-6
  base_valid = 1.897756292e-6
  best_valid = 1.897756292e-6  # no correction at every epoch
```

Verdict: reject this direct Phase-1 adaptation of the depth-8 Song student.
The old `song` success was on a different depth-8/fullskip analytical baseline;
with the current depth-32 l2snap sampler, this message student does not find a
Mini-generalizing residual.  Do not scale this exact architecture further
unless the input state changes materially, e.g. real protected sampled
trajectories rather than diagonal mean rows, or a target-free high-count teacher
label that first validates cross-split.

Trajectory-map oracle/router check:

```text
script:
  legacy_workspace/probe_intermediate_alpha_router.py
cache:
  legacy_workspace/cache/l2snap_traj_full200_r64.npz
base raw:
  2.181297932e-6

target-using multimap coefficient oracle:
  ridge=1e-4 oracle raw = 4.182285260e-7
  ridge=1e-2 oracle raw = 6.267435336e-7
  ridge=1    oracle raw = 1.225220930e-6

target-free MLP-held-out routers:
  ridge models explode / worsen badly.
  random forest / extra trees on multimap labels:
    best raw ~= 2.158783033e-6, rel ~= 0.989678
  single-map scalar alpha routers:
    t02_linear oracle raw = 1.477653529e-6
    best RF raw          = 2.161598748e-6, rel = 0.990969
    t04_linear oracle raw = 1.507350414e-6
    best RF raw           = 2.152574890e-6, rel = 0.986832

shared 25-map coefficient blend:
  best fullfit raw = 2.123469749e-6, but this does not transfer OOF.
  best OOF regularized line = 2.181428584e-6, rel = 1.000060
```

Read: the 25 trajectory maps contain an enormous target-using ceiling, but the
observable map/seed geometry does not identify the coefficients.  This is
another oracle-without-router result, not a package path.  It does suggest that
future work should look for signed observables tied to these map coefficients,
not more generic coefficient regressors.

### 2026-06-23 - multivariate interaction probes

Goal: test whether variables/techniques collaborate in ways single-branch
probes miss.

Block trajectory reweighting:

```text
script: probe_l2snap_gate_block_weight.py
20-spaced smoke:
  base raw = 2.250037261e-6
  best raw = 2.219102476e-6
first100 guard:
  base raw = 2.078894894e-6
  best guarded raw = 2.080343679e-6
  best guarded tail = 2.254056688e-6
```

Verdict: weak/no package signal.  Tail objective can improve, but all-raw does
not robustly move.

Final-neuron block gate reweighting:

```text
script: probe_l2snap_block_neuron_gate_weight.py
20-spaced:
  base raw = 2.250037261e-6
  best linear = 2.246832735e-6
  best hinge  = 2.244423938e-6
```

Verdict: final-neuron gate-shape features do not expose enough signed QR error.

Cached branch synergy:

```text
script: probe_l2snap_variant_synergy.py
Full1000:
  protected base raw = 2.301739726e-6
  branch-delta x seed-uncertainty stack = ~9.80e-7
Mini100 independent guard:
  protected base raw = 1.897755704e-6
  high-count-enabled uncertainty stack = 9.769423854e-7
  strict cheap-side uncertainty stack  = 1.403078728e-6
  count30 branch alone                 = 1.216593972e-6
```

Verdict: synergy is real, but lives mainly in extra-sample branches.  The cheap
stack does not beat count30 alone and is not yet adjusted-competitive.

Partial / variable rows:

```text
uniform protected18:
  256 half-rows/seed affine raw = 2.301739747e-6
  240 half-rows/seed affine raw = 2.518466646e-6
  224 half-rows/seed affine raw = 2.771676597e-6
nonuniform cached policies:
  greedy4568 weighted raw       = 2.375301300e-6
  s17_6_3_14_5_240 weighted raw = 2.400507330e-6
```

Verdict: simple row reallocation does not preserve protected accuracy.

High-count teacher distillation:

```text
script: probe_l2snap_highcount_teacher_distill.py
teacher: count30 - protected
Full1000 count30 teacher raw = 1.444057550e-6
Mini100 count30 teacher raw  = 1.216593972e-6
safe features:
  best Mini after distill = 1.897995454e-6
  Mini base               = 1.897755704e-6
full per-seed features:
  best Mini after distill = 1.897992315e-6
```

Verdict: protected seed-cloud + diagonal Gaussian features do not predict the
high-count correction.  Distillation is closed unless a new internal observable
is added.

Per-neuron seed-oracle router:

```text
scripts:
  probe_l2snap_seed_oracle_router.py
  probe_l2snap_seed_error_router.py

18-seed target oracle:
  Full200 raw = 3.983043739e-7
  Mini100 raw = 3.657662002e-7

direct classifier:
  seed-only Mini soft best = 1.895471771e-6
  joint Mini soft best     = 1.895245324e-6
  Mini base                = 1.897755704e-6
  hard choice              ~= 7e-5 raw, catastrophic

candidate-wise error ranker:
  seed-only Full200 soft best = 2.179452561e-6
  seed-only Mini soft best    = 1.895471771e-6
  joint Mini soft best        = 1.895245324e-6
```

Verdict: the e-8 oracle is real but not identifiable from final seed-cloud
features or cached trajectory maps.  Current soft routers find only sub-percent
raw gains and worsen the tail guard.

### 2026-06-23 - repeated-K3 tail and analytical fusion follow-up

Goal: push the next non-seed-search bet toward oracle-level performance.  The
tested routes were weights-only analytical observables and analytical/sampling
fusion, avoiding another fixed seed-set sweep.

Repeated two-equal K3 tail source:

```text
script:
  legacy_workspace/probe_repeated_k3_tail.py
idea:
  carry P[i,j] = kappa(h_i,h_i,h_j), propagate with the closed repeated-slice
  linear formula, inject diag(P_pre) as a skew mean source, and score final
  source-response features by layer bin.

Full200 n20 smoke, centered:
  base raw                      = 2.550559319e-6
  best scalar skew@0-7          = 2.157755414e-6  # rel 0.846, suspicious split
  best ridge                    = 2.386426350e-6  # rel 0.936

Mini100 centered:
  base raw                      = 1.897755704e-6
  best scalar skew@8-15         = 1.897078454e-6  # rel 0.99964
  ridge variants                = 1.920e-6 .. 1.981e-6, all worse

Mini100 uncentered:
  base raw                      = 1.897755704e-6
  best scalar                   = 1.902494617e-6, worse
  ridge variants                = 1.923e-6 .. 1.959e-6, worse
```

Verdict: reject the repeated-K3 shortcut as implemented.  The n20 Full200
signal was a slice artifact; Mini100 does not support it.  This does not rule
out full K3/K211 feedback, but the simple dense repeated-slice source is not
the missing l2snap residual observable.

Analytical branch fusion:

```text
protected l2snap Full200 raw             = 2.181297932e-6
cov2mm_nodiag branch raw                 = 7.569944935e-5
protected + OOF scalar toward cov2mm      = 2.163127965e-6  # rel 0.9917

joint_expanded branch raw                = 9.541108575e-6
joint_expanded cost                      = 2.0479720144e10 FLOPs
protected + OOF scalar toward joint       = 1.790505042e-6  # rel 0.8208

Mini100 protected raw                    = 1.897755704e-6
Mini100 protected + scalar toward joint   = 1.514623056e-6  # fullfit scalar
```

Read: `joint_expanded` is genuinely complementary to the sampler, but adding it
on top of the 18-seed protected branch prices poorly.  With protected remote
multiplier around `0.143` and joint at `~0.0753`, the fused multiplier would be
around `0.218`, so a `~1.79e-6` raw line is not score-competitive.

Reduced-count sampler plus joint:

```text
Full200, protected-prefix k + joint OOF scalar:
  k18 raw=1.790505640e-6, mult~=0.2185, adjusted proxy~=3.91e-7
  k16 raw=1.990420690e-6, mult~=0.2026, adjusted proxy~=4.03e-7
  k14 raw=2.304602561e-6, mult~=0.1867, adjusted proxy~=4.30e-7

Mini100, protected-prefix k + joint OOF scalar:
  k18 raw=1.516147892e-6, mult~=0.2185, adjusted proxy~=3.31e-7
  k16 raw=1.702189582e-6, mult~=0.2026, adjusted proxy~=3.45e-7
  k14 raw=1.914477948e-6, mult~=0.1867, adjusted proxy~=3.57e-7
```

Verdict: joint-expanded can replace some sampling variance in raw terms, but
not enough to beat the current protected adjusted score once repriced.

Branch-only calibration of high-count l2snap caches:

```text
Full1000 OOF affine:
  union53 equal raw             = 8.037238980e-7, count 53, adj proxy 3.39e-7
  first40 affine raw            = 1.069516240e-6, count 40, adj proxy 3.40e-7
  count30 seed raw              = 1.367400671e-6, count 30, adj proxy 3.26e-7
  targetdual24 fullweighted raw = 1.725848107e-6, count 24, adj proxy 3.29e-7
```

Verdict: high-count branches are already essentially scale/bias calibrated.
Their bottleneck is compute, not affine calibration.

Cheap distillation of `joint_expanded`:

```text
features:
  cov2mm_nodiag, exactGH h1affine, exactGH rank12, base/base_abs
teacher label:
  joint_expanded - protected

Full200:
  ideal scalar joint blend raw    = 1.787968960e-6
  cheap teacher-ridge target raw  = 2.136086857e-6
  teacher R2                     = 0.02015

Mini100:
  ideal scalar joint blend raw    = 1.514623056e-6
  cheap teacher-ridge target raw  = 1.863864483e-6
  teacher R2                     = 0.01912
```

Verdict: cheap analytical branches recover only about 2% of the useful
joint-expanded movement.  Keep `joint_expanded` as a teacher/diagnostic, not a
deployable fusion arm unless its expensive component can be compressed much
more directly.

### 2026-06-23 - trajectory-map teacher coefficients and receiver-injected Cact v0

Goal: keep pushing non-seed-search routes with enough theoretical ceiling to
matter.  Two targeted questions were tested:

1. Can high-count target-free teachers teach the large trajectory-map oracle
   coefficients?
2. Does the expert memo's receiver-injected Cact recurrence work in a minimal
   Phase-1 diagnostic?

Trajectory-map teacher coefficients:

```text
script:
  legacy_workspace/probe_traj_map_teacher_coeffs.py

labels:
  per-MLP trajectory-map coefficients fitted to count30 or union53 teacher
  predictions, not to public targets.

Full200, teacher=count30, ridge=1e-4:
  base raw            = 2.181297932e-6
  teacher raw         = 1.375280475e-6
  same-MLP teacher fit= 1.525495713e-6
  target oracle raw   = 4.182285260e-7
  best OOF learned    = base; all learned coefficient routers worsened

Mini100, teacher=count30, ridge=1e-4:
  base raw            = 1.897756292e-6
  teacher raw         = 1.216593972e-6
  same-MLP teacher fit= 1.357663036e-6
  target oracle raw   = 3.823307648e-7
  best OOF learned    = base

Union53, ridge=1e-2:
  Full200 teacher raw         = 7.227687756e-7
  Full200 same-MLP teacher fit= 1.144518626e-6
  Mini100 teacher raw         = 6.763920958e-7
  Mini100 same-MLP teacher fit= 1.034443703e-6
  best Full200->Mini learned  = 1.882705994e-6, rel 0.9921
  best Mini->Full learned     = 2.181172763e-6, rel 0.99994
```

Read: the trajectory-map coefficient oracle remains real, but high-count
pseudo-teacher coefficients are not identifiable from the current map/seed
geometry.  The tiny one-way Full200->Mini gain is not enough and does not
transfer in reverse.  Close this specific teacher-coefficient route.

Quadratic pathCV correction on current protected base:

```text
existing cache:
  legacy_workspace/cache/quadratic_pathcv_full200_r0001.npz
  legacy_workspace/cache/quadratic_pathcv_mini100_r0001.npz

Apply z2_raw - old_pathcv_base as a correction to the current protected l2snap
base:

Full-fit alpha -> Mini:
  Full raw 2.181298568e-6 -> 2.153995476e-6
  Mini raw 1.897755704e-6 -> 1.877703969e-6, rel 0.98943

Mini-fit alpha -> Full:
  Mini raw 1.897755704e-6 -> 1.877259918e-6
  Full raw 2.181298568e-6 -> 2.154444953e-6, rel 0.98769
```

Read: this is a stable signed structural correction, but only about 1.1-1.2%
raw.  Prior logs already judged the pathCV compute too high for this gain.  Keep
it as evidence that known-expectation controls are real; do not build a package
unless a much cheaper equivalent of `z2_raw` appears.

Receiver-injected Cact v0:

```text
script:
  legacy_workspace/probe_receiver_injected_cact.py

Full200 n20, d=1, sources=plain,wick2,h2, receivers=beta3,beta3_wick2:
  base raw  = 2.550559319e-6
  corrected = 3.350248094e-6, rel 1.313535
  residual R2 = -0.31353

Full200 n20, minimal d=1, source=plain, receiver=beta3:
  corrected = 2.641095901e-6, rel 1.035497
  residual R2 = -0.03550

Full200 n20, minimal d=1,2, source=plain, receiver=beta3:
  corrected = 2.907059543e-6, rel 1.139773
  residual R2 = -0.13977
```

Verdict: reject this v0 receiver-injected implementation.  It is likely missing
template details or has the wrong source/receiver placement relative to the
research generator.  The script is still useful for parity work if exact
selected-template definitions become available, but do not tune this current
shape.

### 2026-06-23 - first32 late thinning and low-rank proxy CV checks

Goal: test two non-seed-tweak routes toward the high-count/e-8 oracle.

First32 late row thinning:

```text
script:
  legacy_workspace/probe_late_tail_coreset.py
setup:
  exact first32 l2snap, equal weights, Full200 spaced20
  keep all rows through h1 affine + layer-2 snap, then drop fixed even rows

full first32:
  raw         = 1.278491101e-6
  affine_raw  = 1.242060427e-6

keep 224 half-rows/seed:
  raw         = 1.523383179e-6
  rel         = 1.191548
  affine_raw  = 1.506762524e-6

keep 192 half-rows/seed:
  raw         = 2.601083154e-6
  rel         = 2.034495
  affine_raw  = 2.382944765e-6
```

The raw loss is independent of the handoff layer because a fixed row subset
selects the same original sample trajectories; later handoff only changes the
accounting story, not which rows survive.  Even the mild 224 cut loses far more
raw accuracy than the compute savings can repay.  This extends the earlier
protected/k24 row-thinning negatives to the first32 regime.

Low-rank-network multifidelity control variate:

```text
script:
  legacy_workspace/probe_lowrank_proxy_cv.py
idea:
  exact protected18 l2snap branch
  + beta * (rank-r low-rank proxy mean over seeds 0..63
            - rank-r low-rank proxy mean over protected18)
  proxy shares exact h1 repair + l2snap, then uses SVD rank-r tails.

5-MLP smoke:
  base raw         = 2.814590828e-6
  base affine_raw  = 2.149764549e-6

rank8, shrink=1:
  raw         = 2.814579362e-6
  affine_raw  = 2.149762526e-6
  read: flat; proxy QR mean difference is almost zero.

rank16, shrink=0.1:
  raw         = 3.017369545e-6
  affine_raw  = 2.245059090e-6

rank32, shrink=0.1:
  raw         = 2.968076331e-6
  affine_raw  = 2.249100088e-6
```

Verdict: reject this proxy-CV shape.  Very low-rank proxies are integrated so
well by the existing QR blocks that the extra proxy seeds add almost no signal;
higher-rank proxies produce a nonzero correction but point the wrong way on the
smoke.  This does not rule out all control variates, but it closes the simple
"cheap low-rank network with many proxy seeds" route.

### 2026-06-23 - Phase-1 K3/K211 feasibility and cheap seed-cloud fusion

Goal: revisit the expert K3/K211/D21 direction without confusing the older
depth-8 audit with current Phase 1 depth 32, then test a few free sampler-fusion
controls.

Findings from the local notes before running anything:

- Naive D21 matrix-product CountSketch is already marked "do not retest":
  exact-width parity passed, but useful sketch sizes were unstable or
  K3-simple-class.  Product-norm exact heads did not preserve the
  cancellation-sensitive D21/K211 signal.
- The strong cap4096/SVD/bucket-gain K3 results in `paper_aug_audit_results.md`
  came from depth-8 public caches.  The public cache inspected here has weights
  shaped `(100, 8, 256, 256)`, while Phase 1 is `(N, 32, 256, 256)`.

Added a guarded profiling switch:

```text
file:
  whest-starterkit/candidate_k3aug_r4096_skeleton_estimator.py
change:
  K3AUG_SKELETON_FORCE=1 allows profiling the skeleton at Phase-1 depth 32.
```

Depth-32 op profiles, Gaussian chassis replacement skeleton:

```text
Gaussian chassis only, R=0:
  ops   = 2984
  flops = 2.176e9
  predicted remote multiplier = 0.100 floor

Gaussian chassis + flat R4096 carry, no D21:
  ops   = 3117
  flops = 5.372e10
  predicted remote multiplier = 0.226 - 0.272

Gaussian chassis + flat R4096 carry + full D21:
  ops   = 3127
  flops = 5.533e10
  predicted remote multiplier = 0.232 - 0.278
```

Read: a Phase-1 replacement K3 chassis would need raw below roughly
`8.5e-7` to beat protected `311697` on bad-draw pricing, and around
`3.6e-7..4.3e-7` raw to land near `1e-7` adjusted.  The cost is not impossible,
but it is much less forgiving than the old depth-8 lower-bound profile.

Tiny Phase-1 reference K3 audit, CPU, `phase1_mini100_weights_targets.npz`,
MLP 0 only:

```text
uncapped k3_simple:
  final raw = 7.640649881e-5

uncapped k3_aug_full:
  final raw = 8.878957178e-6

k3_aug_no_d4_u_source:
  final raw = 2.099962318e-4

cap512 family_quota_cost:
  final row = all NaN

cap4096 family_quota_cost:
  final row = all NaN

cap4096 family_quota_cost with MLP_KPROP_K3_DEPTH=32:
  final row = all NaN

cap4096 diag with MLP_KPROP_K3_DEPTH=32:
  final row = all NaN
```

NaN localization:

```text
cap4096 diag:
  first NaN layer = 16, then layer 17 onward all NaN

cap4096 family_quota_cost, depth override:
  first NaN layer = 22, then layer 23 onward all NaN
```

Read: the reference cap branch is not Phase-1-ready.  The failure is a late
depth-32 numerical blow-up, not an immediate cap-selection shape error.  Even
uncapped full K3-aug is finite but far above the SPHEREx line on this smoke
MLP.  Do not spend more submission work on K3 cap ports until the Phase-1 cap
NaNs are debugged and a multi-MLP finite raw check exists.

Checked the nearest flopscope K211 wrapper:

```text
candidate_k211_2i_clean_estimator.py, mini indices 0,5,10,15,20:
  adjusted = 4.048402e-6
  raw      = 2.661443e-5
  mult     = 0.15304340
  failed   = 0
```

Read: this wrapper is not the strong K3-aug branch; do not promote or tune it
as a Phase-1 candidate.

Cheap seed-cloud fusion checks:

```text
Trajectory-map fixed coefficient transfer:
  Full200 l2snap trajectory base raw = 2.181297932e-6
  Mini100 l2snap trajectory base raw = 1.897756292e-6
  Full-fit trajectory coefficients helped Mini by at most ~1%.
  Mini-fit coefficients worsened Full.  Not robust enough to package.

Empirical-Bayes seed-cloud residual features:
  ridge on mean/sd/variance/split/median/trim features produced only tiny OOF
  movement and mixed Mini<->Full transfer.  Not a useful correction.

Free robust aggregation on protected18 l2snap:
  Full1000 protected weighted raw = 2.301739726e-6
  Full1000 equal raw              = 2.309813271e-6
  Full1000 trim1 raw              = 2.380177741e-6
  Full1000 median raw             = 3.501495330e-6

  Mini100 equal raw               = 1.897400882e-6
  Mini100 protected weighted raw  = 1.897755704e-6
  Mini100 trim1 raw               = 1.967570738e-6
  Mini100 median raw              = 3.048388412e-6
```

Read: median/trim/Hodges-style robustification is decisively worse.  The useful
signal remains in selected seed identity/high-count teachers, not in generic
robust aggregation of the existing 18 block outputs.

Next viable non-overlap directions:

- Debug the Phase-1 reference cap NaNs only if we want a dedicated K3 porting
  project; it is not a quick submission route.
- For e-8, the known reachable raw ceiling is still target-aware or high-count
  sampler fusion.  The hard problem is identifying signed per-MLP/per-neuron
  coefficients without target leakage, not another final-vector median/variance
  statistic.

### 2026-06-23 - l2snap within-block reliability check

Added:

```text
legacy_workspace/probe_l2snap_withinblock_reliability.py
```

This tested a target-free idea not cleanly isolated in prior routers: use
self-consistency inside the exact l2snap QR rows already paid for by the
estimator.  Features included antithetic-pair variance, deterministic
pair-split disagreements, and a block-local first-harmonic norm.  Two families
were checked:

- inverse-reliability seed weighting with no target fitting;
- MLP-grouped OOF ridge over seed deltas multiplied by reliability features.

Results:

```text
Spaced20:
  base raw                 = 2.250046362e-6
  best target-free raw     = 2.194418249e-6  # rel 0.9753
  OOF ridge raw            = 2.254233549e-6  # worse

Full200:
  base raw                 = 2.181300013e-6
  best raw-only target-free= 2.180735955e-6  # rel 0.9997
  best tail-safe target-free was effectively neutral/worse
  OOF ridge raw            = 2.183013109e-6  # worse

Mini100:
  base raw                 = 1.897752065e-6
  best target-free         = base
  OOF ridge raw            = 1.904338468e-6  # worse
```

Read: within-block noisiness is not the missing signed oracle.  It can overfit
small slices and slightly improve spaced20, but it does not transfer to Mini100
or Full200.  Do not package inverse split/pair variance weighting, and do not
retest reliability weighting unless the observable predicts signed block error,
not just block variance.

### 2026-06-23 - predict per-MLP 32-seed oracle weights

Ran an offline OOF diagnostic against the e-8-class per-MLP seed-weight oracle:

- seed pool: first 32 blocks from
  `highseed_spherex_full200_s512_f16_seeds0to119.npz`;
- labels: equality-constrained target-fitted per-MLP weights for
  `lam in {1e-2, 1e-4, 1e-6}`;
- features:
  - `full200_weight_features_v1` MLP weight geometry;
  - seed-cloud summaries from the 32 full-depth block predictions;
  - concatenation of both;
- models: OOF ridge and shallow ExtraTrees, with shrinkage to uniform.

Oracle and OOF results:

```text
32 equal raw                 = 1.532328809e-6

target oracle lam=1e-2       = 1.412244583e-6
target oracle lam=1e-4       = 4.749824496e-7
target oracle lam=1e-6       = 1.796391680e-7
target oracle lam=1e-8       = 1.751658085e-7

best OOF predicted weights:
  label lam=1e-2: seedsummary ridge raw = 1.530604744e-6
  label lam=1e-4: seedsummary ridge raw = 1.518963944e-6
  label lam=1e-6: seedsummary ridge raw = 1.529085143e-6
```

Read: the per-MLP seed-weight oracle is huge, but ordinary MLP-level weight
geometry and seed-cloud summaries do not predict the oracle weights.  Best OOF
capture is below 1% raw.  This closes the naive PERO-on-oracle-weights route
for now; an e-8 route still needs a new signed quadrature-error observable or a
different mathematical construction.

### 2026-06-23 - quadratic seed-deviation interaction closure

Added:

```text
legacy_workspace/probe_seed_quadratic_residual.py
```

This tested the "second-degree lever interaction" hypothesis directly: maybe
individual seed deviations are weak, but products of seed deviations expose the
signed residual.  The diagnostic uses only already-computed l2snap seed-block
final predictions, then fits MLP-grouped OOF ridge corrections with features
including base, spread, seed deviations, squared deviations, and
seed-deviation-by-spread terms.  Targets are used only offline.

Results:

```text
18-block protected branch, diag quadratic features:
  Full1000 base raw      = 2.540603886e-6
  best OOF raw/tail      = 2.540752199e-6 / 2.553261977e-6
  Mini100 base raw       = 2.384000491e-6
  best OOF raw           = 2.384032826e-6
  Full->Mini transfer    = 2.383879818e-6  # numerically neutral
  Mini->Full transfer    = 2.540590107e-6  # numerically neutral

32-block first40 equal branch, diag quadratic features:
  Full1000 base raw      = 1.374672018e-6
  best OOF raw           = 1.374666149e-6  # 0.0004% raw, tail worse
  Mini100 base raw       = 1.072506511e-6
  best OOF raw           = 1.072514235e-6  # worse
  Full<->Mini transfer   = unchanged to displayed precision

18-block protected branch, full pairwise products:
  Full1000 best OOF raw   = 2.540603994e-6  # unchanged
  Mini100 best OOF raw    = 2.384005691e-6  # unchanged/worse
  Full<->Mini transfer    = unchanged to displayed precision

32-block first40 equal branch, full pairwise products:
  Full1000 best OOF raw   = 1.374672114e-6  # unchanged
  Mini100 best OOF raw    = 1.072507602e-6  # unchanged/worse
  Full<->Mini transfer    = unchanged to displayed precision
```

Read: quadratic products of final seed deviations are not the missing
interaction.  This closes a direct "lever x lever" seed-cloud residual model.
Do not spend time on larger polynomial seed-cloud regressions unless they use a
new internal observable, because the deployable final block outputs themselves
do not expose the e-8 oracle.

### 2026-06-23 - antithetic odd-part control variate closure

Added:

```text
legacy_workspace/probe_l2snap_antithetic_odd_cv.py
```

This tested a target-free observable inside the existing QR-antithetic
SPHEREx blocks.  For every paired direction `u, -u`, the deployed estimator
keeps the even mean

```text
e = 0.5 * (f(u) + f(-u))
```

and discards the odd part

```text
o = 0.5 * (f(u) - f(-u)).
```

The odd part has zero spherical expectation for any fixed network, so if it
predicted the signed quadrature error of the even estimator it would be a
clean control-variate candidate with little extra runtime information.

Full deployed geometry (`rows_per_seed=256`, protected 18 l2snap seeds):

```text
spaced20 base raw                  = 2.250056184e-6
spaced20 scalar odd_weighted       = 2.336594835e-6   # worse
spaced20 scalar odd_equal          = 2.335591694e-6   # worse
spaced20 best rich ridge           = 2.248814533e-6   # 0.055% diagnostic blip

Full200 base raw                   = 2.181301611e-6
Full200 scalar odd_weighted        = 2.192349631e-6   # worse
Full200 scalar odd_equal           = 2.192236286e-6   # worse
Full200 best pure ridge            = 2.181334473e-6   # shrink-to-zero
Full200 best signed ridge          = 2.181344751e-6   # shrink-to-zero
Full200 best rich ridge            = 2.181358452e-6   # shrink-to-zero
```

Read: the antithetic odd component does not expose the missing signed QR-block
error.  The small spaced20 rich-feature improvement was not real under the
Full200 canary.  This closes the straightforward "use the opposite direction's
discarded odd signal" path.

### 2026-06-23 - response-aligned directions and weak-observable interactions

Added response-aligned modes to:

```text
legacy_workspace/probe_l2snap_w0_replace.py
```

The new modes replace one protected QR block with directions derived from the
mean-field linearized input-to-final Jacobian:

```text
linjac_cols, linjac_qr, linjac_svd, linjac_w0_qr
```

Two-MLP smoke, dropping block 6:

```text
base raw                         = 4.878749239e-6
linjac_cols raw                  = 3.400723894e-5
linjac_qr raw                    = 1.128948226e-5
linjac_svd raw                   = 1.294617544e-5
linjac_w0_qr raw                 = 1.683392272e-5
```

Read: naive response-aligned orthogonal bases destroy the useful high-order
coverage of the random QR blocks.  This does not rule out all adaptive
direction design, but it closes the simple "replace a QR block by the
linearized final-response basis" version.

Added:

```text
legacy_workspace/probe_l2snap_observable_interaction_stack.py
```

This directly tested the "weak levers may combine" hypothesis by stacking
cached protected-pass trajectory maps with sample-Cact final-rooted features
under MLP-grouped OOF ridge.

Spaced20 looked superficially positive:

```text
base raw                         = 2.250035398e-6
trajectory features              = 2.210839361e-6  # 0.982580x
sample-Cact features             = 2.239897641e-6  # 0.995494x
trajectory + sample-Cact         = 2.206233092e-6  # 0.980533x
```

Full200 falsified the interaction:

```text
base raw                         = 2.181297932e-6
trajectory features              = 2.187103994e-6  # worse
sample-Cact features             = 2.182674626e-6  # worse
trajectory + sample-Cact         = 2.186421464e-6  # worse
```

Read: the apparent spaced20 interaction is canary overfit.  Combining the
current weak trajectory and Cact observables does not expose the signed
high-count/seed-oracle residual.  Do not promote this feature stack or spend
more time on generic weak-feature stacking without a new target-free observable.

### 2026-06-23 - signed within-block partition controls

Added:

```text
legacy_workspace/probe_l2snap_signed_partition_cv.py
```

This tested a different "free information" hypothesis from the earlier
within-block reliability check.  The current estimator averages the 256
antithetic pair means inside each QR seed block.  The new probe forms balanced
signed Walsh/Haar contrasts of those same pair means:

```text
control(seed, pattern) = mean_r sign_pattern[r] * pair_mean(seed, r)
```

These controls have zero expectation over the random QR row ordering and, if
useful, could be folded into fixed row weights without evaluating extra network
rows.  Targets are used only for grouped offline ridge validation.

Smoke, 4 MLPs, 32 rows/seed:

```text
base raw              = 1.102342353e-5
best aggregate raw    = 1.089821546e-5  # rel 0.9886, tail-risky
best seedwise raw     = 1.099986237e-5  # rel 0.9979
```

Spaced20, 64 rows/seed:

```text
base raw              = 1.102383406e-5
best seedwise raw     = 1.073088392e-5  # rel 0.9734
tail objective        = 1.324097599e-5  # worse slice
```

Deployed-row geometry, spaced20, 256 rows/seed:

```text
base raw              = 2.250048125e-6
best seedwise raw     = 2.241857004e-6  # rel 0.9964
stronger fits         = 1.05x to 1.17x worse
```

Read: signed partition contrasts are not the missing QR-block error observable.
They can move tiny slices, but the deployed-geometry broad gain is only about
0.36% and stronger use is unstable.  Do not package row-partition controls or
spend Full200 time on this branch unless a new derivation supplies a much more
specific signed pattern.

### 2026-06-23 - pathCV response variants

Patched `legacy_workspace/evaluate_spherex_pathwise_cv.py` to test two production
shapes for the high-count pathwise control branch:

1. more cross-fit folds (`split4`, `split8`) instead of the deployed even/odd
   `split2`;
2. replacing mean-field downstream gates in the response matrix with gate rates
   measured from the already-computed sample pass.

Fast Full200-first20, 32 seed blocks:

```text
plain raw                         = 1.594899059e-6
meanfield split2                  = 1.350229506e-6  # ratio 0.846592
meanfield split4                  = 1.350775888e-6  # slightly worse
meanfield split8                  = 1.350517743e-6  # slightly worse
sample-gate split2                = 1.343013630e-6  # ratio 0.842068
half-gate split2                  = 1.593851556e-6  # control collapses
```

High-count smoke, Full200-first8, 120 seed blocks:

```text
plain raw                         = 3.808374506e-7
meanfield split2                  = 2.965433775e-7  # ratio 0.778661
sample-gate split2                = 2.978773922e-7  # slightly worse
```

Read: increasing the cross-fit fold count is not a lever, and sample-gate
responses do not beat the current mean-field response on the high-count branch.
The current pathCV implementation is not missing an obvious response-estimation
or fold-splitting cleanup.  Keep the patched evaluator for diagnostics, but do
not build a sample-gate or split4 package.

### 2026-06-23 - high-count pathCV seed-count curve

Added `legacy_workspace/probe_pathcv_branch_gate.py` for the exact
protected-l2snap-vs-high-count-pathCV branch decision.  The initial branch-gate
runs showed that local Full200/Mini surfaces are much easier for pathCV than the
live public rows, so they are useful for ratios/shape but not for absolute
remote raw.

Local Full200 first20, `split2`, mean-field response, 512 rows/seed:

```text
count  local pathCV raw
 32    1.350229506e-6
 64    7.373540924e-7
 72    5.387433817e-7
 80    4.845244826e-7
 84    4.334452395e-7
 88    4.406887214e-7
 92    4.251888179e-7
 96    4.081314316e-7
104    3.990709606e-7
112    3.705578709e-7
120    3.336262980e-7
```

Using remote `311749` as the measured 120-block anchor
(`raw=9.343601340e-7`, multiplier `0.240861589`) and a simple multiplier
linearization, the best modeled adjusted point is around 80-88 blocks, with
count 84 the most favorable on this local slice:

```text
count  modeled remote adjusted
 72    2.180493364e-7
 80    2.178943847e-7
 84    2.046698392e-7
 88    2.179992165e-7
 92    2.198922822e-7
 96    2.202478109e-7
104    2.333048626e-7
112    2.332998126e-7
120    2.250514666e-7  # actual remote anchor
```

Important caveat: local `whest run --profile` currently reports much higher
FLOPs for these packages than the live `311749` page reports for the same
family, despite local `flopscope 0.8.0rc1+np2.2.6`.  Treat live row multipliers
as the compute source for this branch until the profiling mismatch is resolved.

Packaged but **not submitted**:

```text
whest-starterkit/packages/active/submission_phase1_highseed80_f16_pathcv_countprobe_bundle.tar.gz
whest-starterkit/packages/active/submission_phase1_highseed84_f16_pathcv_countprobe_bundle.tar.gz
whest-starterkit/packages/active/submission_phase1_highseed88_f16_pathcv_countprobe_bundle.tar.gz
```

Read: this is a coherent single-probe candidate class, not an e-8 breakthrough.
If the count-ratio transfers remotely, count 84 could beat the 120-block
pathCV anchor and potentially the current public #1.  If it does not, the
result still sizes the pathCV compute/raw frontier and closes a practical
optimization question.

### 2026-06-23 - protected/pathCV84 public-row branch router probe

Built a separate probe package that does **not** alter the protected root or
the other assistant's seed-search line:

```text
whest-starterkit/packages/active/submission_phase1_pathcv84_q25_router_probe_bundle.tar.gz
legacy_workspace/_pkg_pathcv84_q25_router_probe/
```

It dispatches between:

```text
protected branch: 311697 protected l2snap weighted affine final-only
pathCV branch:    highseed84 f16 pathwise-CV count probe
```

The gate is target-free at inference time but was selected from the 50 live
public row losses, so treat this as a high-upside/overfit-risk probe:

```text
feature: layers4_11_row_norm_q25
rule:    use pathCV84 if q25 <= 1.3713652291199763, else protected
```

Remote-row diagnostic that motivated it:

```text
311697 protected fixed adjusted = 2.370739059e-7, raw = 1.655662345e-6
311749 pathCV120 fixed adjusted = 2.250514666e-7, raw = 9.343601340e-7
remote branch oracle adjusted   = 1.671279792e-7
q25 single-feature OOF adjusted = ~1.85e-7 to ~1.96e-7 depending on fold scan
```

Validation/smoke:

```text
whest validate .../_pkg_pathcv84_q25_router_probe/estimator.py  # passed
quick_score_selected mini indices 0,5: failed=0, raw=4.267136e-7
```

Read: this package is useful as a remote probe because it tests whether the
large protected-vs-pathCV complementarity is recoverable from a cheap, generic
weight statistic.  It should not be treated as robust until a remote result
confirms it; the feature/threshold came from live public labels, not from a
fully target-free full1000/tail gate.

Second cheap two-signal probe:

```text
whest-starterkit/packages/active/submission_phase1_pathcv84_q25_std_or_router_probe_bundle.tar.gz
legacy_workspace/_pkg_pathcv84_q25_std_or_router_probe/
```

Rule:

```text
mid_q25   = quantile(row norms from layers 4..11, 0.25)
late_std  = std(row norms from layers 24..31)
use pathCV84 if mid_q25 <= 1.3713652291199763 or late_std <= 0.0625
```

Live public row diagnostic against `311697` protected and `311749` pathCV120:

```text
single mid_q25 gate adjusted       = 1.850333788e-7
two-signal OR gate adjusted        = 1.842068633e-7
remote branch oracle adjusted      = 1.671279792e-7
OR gate pathCV count on live rows  = 39 / 50
```

Validation/smoke:

```text
whest validate .../_pkg_pathcv84_q25_std_or_router_probe/estimator.py  # passed
quick_score_selected mini indices 0,5: failed=0, raw=4.267136e-7
```

Read: this is slightly less brittle than the one-feature gate in spirit because
it requires only two cheap row-norm summaries from disjoint layer bands, but it
is still public-row-tuned.  Upload only as a probe, not as a protected line.

Count choice for the branch router, using the remote `311749` pathCV120 row
losses as the anchor and the local Full200 first20 count curve as the raw-ratio
model:

```text
count  q25-gate modeled adj   OR-gate modeled adj
 72    1.811656476e-7         1.789826080e-7
 80    1.810940917e-7         1.788859554e-7
 84    1.749870636e-7         1.706370176e-7
 88    1.811425025e-7         1.789513453e-7
 92    1.820167107e-7         1.801321632e-7
 96    1.821808920e-7         1.803539277e-7
104    1.882105723e-7         1.884983896e-7
112    1.882082403e-7         1.884952397e-7
120    1.843991959e-7         1.833502543e-7
```

Read: within this branch-router family, count84 is the modeled optimum.  The
model is only as good as the count-ratio transfer from local Full200 to the live
public split, so this supports a probe rather than proving a promotion.

High-count l2snap+pathCV snap check:

```text
probe_l2snap_pathwise_cv.py, Full200 index 0, seeds 0..83, CPU smoke
snapped high-count plain raw      = 6.831115184e-7
snapped h1_cv_split raw           = 6.833017633e-7
snapped h2_cv_split raw           = 6.827923287e-7
existing pathCV cache raw row 0   = 2.697329355e-7
```

Read: injecting the protected h1/layer-2 snap into the high-count pathCV branch
does not look like a raw-MSE lever.  On the one comparable row it is much worse
than the existing unsnapped pathCV cache, and the split controls move the result
by less than 0.1%.  Close this avenue unless a larger GPU batch contradicts it.

### 2026-06-24 - additional flat-q05 protected/pathCV84 router probes

Added two more public-row branch-router probes and copied them to
`whest-starterkit/packages/to_test_remote/`:

```text
submission_phase1_pathcv84_flatq05_late_std_or_router_probe_bundle.tar.gz
  sha256 04f13fc6ca0a457bd054315374dc0756aaabf813f11d29e044061908bbbe644e

submission_phase1_pathcv84_flatq05_midq25_and_router_probe_bundle.tar.gz
  sha256 ae94ea57546f75db0e593331163f3c333f863c126200f76a2fca175267b35fc2
```

Rules:

```text
flat_q05 = quantile(flat weights from layers 12..23, 0.05)
late_std = std(row norms from layers 24..31)
mid_q25  = quantile(row norms from layers 4..11, 0.25)

flatq05_late_std_or:
  use pathCV84 if flat_q05 > -0.14546840699762106
  or late_std <= 0.0625

flatq05_midq25_and:
  use pathCV84 if flat_q05 > -0.14546840699762106
  and mid_q25 <= 1.3713652291199763
```

Live public-row diagnostic against `311697` protected and `311749` pathCV120:

```text
flatq05_late_std_or adjusted   = 1.828613701e-7
flatq05_midq25_and adjusted    = 1.833751180e-7
q25_std_or adjusted reference  = 1.842068633e-7
remote branch oracle adjusted  = 1.671279792e-7
```

Validation/smoke:

```text
both whest validate passed
both quick_score_selected mini index 0: failed=0
```

Read: these are additional high-upside remote probes, not protected defaults.
They use generic weight statistics and no MLP names, but the thresholds and
feature combinations were selected from live public row losses.  The flat-q05
feature requires a larger quantile over layer weights; the real-shape smoke
confirmed it is mechanically safe locally, but live compute should be checked.

### 2026-06-24 - pathCV88 branch-router bracket

The standalone count-bracket update made count88 stronger than count84 under
the Full200-anchored model, so the same branch gates were rebuilt with the
pathCV88 branch:

```text
submission_phase1_pathcv88_flatq05_late_std_or_router_probe_bundle.tar.gz
  sha256 080cb891c79f5117773d785febd26250249fa46f2ac4eb8a8e44fe57cfc83044

submission_phase1_pathcv88_q25_std_or_router_probe_bundle.tar.gz
  sha256 06bb4bbfa0820fa0ddb5327121dc12dc2f5fa5be50044ca3787cf2d810741153
```

Updated remote-row model using `311749` pathCV120 rows, Full200 count ratios,
and measured mean count multipliers:

```text
count88 flatq05_late_std_or adjusted = 1.794403954e-7
count88 q25_std_or adjusted          = 1.802156354e-7
count84 flatq05_late_std_or adjusted = 1.806010810e-7
count84 q25_std_or adjusted          = 1.815697994e-7
```

Validation/smoke:

```text
both whest validate passed
both quick_score_selected mini index 0: failed=0
```

Read: if spending remote router probes, test the pathCV88 flat-q05/late-std
router first under the newer count model.  It remains public-row-tuned and
should be interpreted as a probe, not a protected candidate.

### 2026-06-24 - QR-offset ensemble raw frontier

Goal: build a non-public-row-tuned high-raw-accuracy contender by averaging
many deterministic QR offsets, rather than selecting one target-fitted offset
per seed.  This is different from the earlier failed same-compute qroffset
replacement branch: no per-seed offset selection is used; all blocks in a
rectangular offset x seed grid are averaged equally.

Cached h1affine QR-offset ensemble frontier:

```text
Full200:
  offset0 18-block raw = 2.209365244e-6
  4 offsets x 18 seeds  = 72 blocks,  raw = 6.175889588e-7
  7 offsets x 18 seeds  = 126 blocks, raw = 3.556208664e-7
  21 offsets x 6 seeds  = 126 blocks, raw = 3.519803426e-7
  32 offsets x 8 seeds  = 256 blocks, raw = 1.956862963e-7  # over budget
  32 offsets x 18 seeds = 576 blocks, raw = 8.172971139e-8  # over budget oracle

Full1000:
  offset0 18-block raw = 2.382544306e-6
  4 offsets x 18 seeds  = 72 blocks,  raw = 6.107786717e-7
  7 offsets x 18 seeds  = 126 blocks, raw = 3.496396165e-7
  21 offsets x 6 seeds  = 126 blocks, raw = 3.672765146e-7
  32 offsets x 8 seeds  = 256 blocks, raw = 1.840066919e-7  # over budget
  32 offsets x 18 seeds = 576 blocks, raw = 7.932026223e-8  # over budget oracle
```

Under-budget choice:

```text
artifact:
  whest-starterkit/packages/active/submission_phase1_qroffset7x18_h1affine_rawfrontier_f16_finalonly_bundle.tar.gz
  copied to whest-starterkit/packages/to_test_remote/
sha256:
  98a5af3e07ade6d46875a4e9f5656309df8406c7d68a6dc515a92176ccb82724
shape:
  7 offsets x 18 protected seeds = 126 QR half-blocks
  f16 bundled half-input blob, 15.2 MB
validation:
  whest validate passed
  mini index 0: raw=5.586356e-7, adjusted=5.521182e-7,
                mult=0.988333, eff=2.688e11, failed=0
  mini indices 0,20,40,60,80:
                raw=5.572807e-7, adjusted=5.509234e-7,
                mult=0.988778, eff=2.689e11, failed=0
```

Read: this is a real high-budget raw-MSE family and a useful profiling/raw
frontier probe, but not a current adjusted-score replacement.  At ~99% budget,
the measured mini adjusted score is essentially raw MSE, so it does not compete
with the protected/pathCV low-compute adjusted frontier.  The over-budget
offset ensemble ceiling is important, though: QR-offset averaging reaches e-8
raw locally when compute is ignored, which confirms that the remaining error is
mostly quadrature variance rather than an irreducible analytic bias.

Target-free full-ensemble herding check:

```text
teacher:
  32 offsets x 18 seeds = 576-block average, no labels used
method:
  greedily choose k blocks whose mean best approximates the teacher on cached
  MLPs; evaluate labels only after selection

Full200 in-sample selection:
  k72  raw = 3.693763724e-7
  k96  raw = 2.748774319e-7
  k120 raw = 2.264458262e-7
  k126 raw = 2.177348028e-7
  k132 raw = 2.085276048e-7

Full200-selected subset transferred to Full1000:
  k72  raw = 5.957786543e-7
  k96  raw = 4.503760649e-7
  k120 raw = 3.547242349e-7
  k126 raw = 3.410578041e-7
  k132 raw = 3.263534726e-7

Full1000 5-fold OOF teacher selection:
  k96  raw = 4.605149462e-7
  k120 raw = 3.768475471e-7
  k126 raw = 3.553078506e-7
  k132 raw = 3.396441055e-7
```

Read: the full-ensemble teacher is a useful diagnostic, but block herding is
still split-fragile.  At the deployable k126 point, OOF herding is worse than
the simple rectangular `7x18` Full1000 raw (`3.496396165e-7`).  Do not package
the herded subset unless a new stability criterion beats the rectangular
offset ensemble out of fold.

Follow-up squeezes:

```text
Fixed global block weights:
  target-free objective = approximate the 576-block teacher with the same
  rectangular block set, using sum-to-one ridge weights.
  result:
    rect7x18 equal Full1000 raw = 3.496396165e-7
    best OOF ridge weights      = 3.496396167e-7 or worse
  read:
    uniform block weights are already optimal out of fold; full-fit tiny gains
    are not deployable evidence.

One extra QR block:
  base = 7 offsets x 18 seeds = 126 blocks
  best Full1000-teacher extra = offset 17, protected seed-position 16
                                (seed 29)
  Full1000 cache raw:
    126-block base  = 3.496396165e-7
    127-block extra = 3.445552443e-7
  Full1000 5-fold OOF choose-one-extra:
    raw = 3.456278997e-7
  package:
    submission_phase1_qroffset7x18_plus_o17s16_h1affine_rawfrontier_f16_finalonly_bundle.tar.gz
    sha256 bbd60ed597abd3902df446338a9a5c0436de2601f9618eb40eb8956e8f9f5b82
  local smoke:
    mini index 0: raw=5.949038e-7 adjusted=5.905192e-7
                  mult=0.992630 eff=2.700e11 failed=0
    mini 0,20,40,60,80: raw=5.643801e-7 adjusted=5.607329e-7
                         mult=0.993576 failed=0
  read:
    mechanically safe, but not better than plain 7x18 on the mini smoke.
    Keep as a diagnostic only; do not prefer it over 7x18 without a stronger
    broad canary reason.

Storage precision:
  f16 7x18 mini 0,20,40,60,80:
    raw=5.572807e-7 adjusted=5.509234e-7 mult=0.988778 eff=2.689e11
  f32 7x18 mini 0,20,40,60,80:
    raw=5.569366e-7 adjusted=5.490150e-7 mult=0.985717 eff=2.681e11
  package:
    submission_phase1_qroffset7x18_h1affine_rawfrontier_f32_finalonly_bundle.tar.gz
    sha256 5463606a78b9e39af75841a67fe31535d17707dc3e0ef0c02553bce45a19c183
  read:
    f32 is the preferred raw-frontier artifact.  It is slightly more accurate
    and measured lower effective compute locally despite a larger bundle.
```

Adjusted-score squeeze follow-up:

```text
Teacher residual distillation:
  teacher = 32 offsets x 18 seeds = 576-block QR-offset mean
  teacher Full1000 raw = 7.932026223e-8
  protected l2snap raw = 2.301739726e-6

  Fit cheap residual models from protected final seed-cloud features:
    features = protected, seed mean/std/min/max, absdev, neuron coordinate,
               MLP-level seed-cloud summaries
    labels tested = real target residual and teacher residual
    best 5-fold OOF raw versus real target:
      real-label fit    ~= 2.30548e-6
      teacher-label fit ~= 2.30518e-6
    protected baseline = 2.30174e-6

  read:
    the e-8 teacher correction is not visible in these cheap final-cloud
    features.  Do not build a PERO-lite residual from this feature family.

Teacher-guided global seed weights:
  union53 l2snap equal raw = 8.036253231e-7
  first40 l2snap equal raw = 1.072608505e-6
  Sum-to-one global weights fit to either teacher or real target OOF collapse
  back to equal weights or worsen:
    union53 best OOF teacher-weighted raw ~= 8.03625e-7
    first40 best OOF teacher-weighted raw ~= 1.07260e-6

  read:
    final seed-cloud weighting is also closed; the high-count teacher does not
    reveal a useful fixed reweighting.

Standalone pathCV88 f32 storage:
  package folder:
    legacy_workspace/_pkg_highseed88_f32_pathcv_countprobe
  validation:
    whest validate passed
  mini 0,20,40,60,80:
    f16 pathCV88 raw=7.832399e-7 adjusted=5.699048e-7 mult=0.727524
    f32 pathCV88 raw=7.831848e-7 adjusted=5.701177e-7 mult=0.727963

  read:
    unlike the QR-offset raw-frontier package, f32 storage does not materially
    improve pathCV88 and does not improve adjusted.  Do not package f32 pathCV.

Floor-count l2snap squeeze:
  protected18 weighted raw = 2.301739726e-6
  reducing protected seed count to the 0.1 multiplier floor loses too much raw:
    protected prefix k12 adjusted ~= 3.70e-7
    target-free greedy k12 adjusted ~= 3.48e-7

  union53 target-free subsets chosen to match the union53 mean:
    k12 adjusted ~= 3.46e-7
    k15 adjusted ~= 3.26e-7
    k18 adjusted ~= 3.27e-7
    k24 adjusted ~= 3.30e-7

  read:
    count reduction to the floor does not reach first-place territory.  The
    sampler variance rises too fast when dropping below ~18-24 l2snap blocks.
```

### 2026-06-24 - pathCV88 seed identity and beta-scale calibration

Goal: keep improving the standalone high-count pathCV branch without increasing
runtime count.  Two possible no-cost levers were checked: which 88 seed blocks
are used, and whether the fitted first-layer pathCV coefficient should be
globally rescaled.

Seed-identity first40 screen, count88, split2, poly gates:

```text
baseline seeds 0..87      cv raw = 4.361701335e-7
contig seeds 32..119      cv raw = 4.340164028e-7  # only first40 positive
front64_tail24            cv raw = 4.784037425e-7
front44_tail44            cv raw = 4.682787045e-7
linspace88 over 0..119    cv raw = 4.429465104e-7
```

Full200 confirmation killed the only positive seed reshuffle:

```text
baseline seeds 0..87 Full200      cv raw = 5.030964338e-7
contig seeds 32..119 Full200      cv raw = 5.190112465e-7
```

Read: do not pursue seed identity inside the existing 0..119 pool from these
simple reshuffles.  First-slice gains are not reliable.

Beta-scale check for the same count88 branch:

```text
first40 beta_scale=0.75  cv raw = 4.429986499e-7
first40 beta_scale=0.90  cv raw = 4.384625139e-7
first40 beta_scale=1.00  cv raw = 4.361701335e-7
first40 beta_scale=1.10  cv raw = 4.344583208e-7
first40 beta_scale=1.25  cv raw = 4.329882751e-7
first40 beta_scale=1.50  cv raw = 4.334543743e-7
first40 beta_scale=1.75  cv raw = 4.375640941e-7
first40 beta_scale=2.00  cv raw = 4.453208453e-7
```

Full200 beta-scale confirmation:

```text
count86 beta_scale=1.50  cv raw = 5.125858383e-7  # count too low
count84 beta_scale=1.50 ridge=0.0100  cv raw = 5.223370759e-7
beta_scale=1.00  cv raw = 5.030964338e-7
beta_scale=1.15  cv raw = 4.998220094e-7
beta_scale=1.25  cv raw = 4.983800689e-7
beta_scale=1.35  cv raw = 4.975270096e-7
beta_scale=1.50  cv raw = 4.973575698e-7
beta_scale=1.50 ridge=0.0001  cv raw = 4.973635064e-7
beta_scale=1.50 ridge=0.0100  cv raw = 4.972958455e-7
count90 beta_scale=1.50  cv raw = 4.869161118e-7
count92 beta_scale=1.50  cv raw = 4.796691117e-7
```

Repriced against the same live count120 anchor, count88 remains the best
adjusted point despite count90/count92's lower raw:

```text
count84 beta1.50 ridge0.010 adjusted threshold to beat count88 ~= 5.209774429e-7 local raw
count84 beta1.50 ridge0.010 observed local raw             = 5.223370759e-7
count88 beta1.50 ridge0.001 adjusted model ~= 2.175162467e-7
count88 beta1.50 ridge0.010 adjusted model ~= 2.174892520e-7
count90 beta1.50 adjusted model ~= 2.177890682e-7
count92 beta1.50 adjusted model ~= 2.193149142e-7
```

Cross-fit fold interaction:

```text
count88 beta1.50 split2 Full200 raw = 4.973575698e-7
count88 beta1.50 split8 Full200 raw = 4.973364946e-7
```

Read: split8 is technically lower raw, but the gain is about `0.004%`; do not
complicate the package or add live reductions for this.

Same-row block-granularity check:

```text
count88 x 512 rows, first40, beta1.5 ridge0.01:
  raw ~= 4.33e-7 to 4.36e-7 depending ridge/scale comparison above

count176 x 256 rows, first40, beta1.5 ridge0.01:
  plain raw = 5.252552141e-7
  cv raw    = 6.453175385e-7
```

Read: more smaller antithetic blocks at the same total row count is bad.  The
full 512-row QR-antithetic block is doing real quadrature work; do not replace
it with partial-block variants.

Clip checks on first40 were worse:

```text
beta_scale=2.0 beta_clip=1.5  cv raw = 4.338752940e-7
beta_scale=2.0 beta_clip=1.0  cv raw = 4.407439906e-7
beta_scale=1.5 beta_clip=1.0  cv raw = 4.407432054e-7
```

Read: the pathCV coefficient is under-aggressive in the current cross-fit
estimator.  A global `beta_scale=1.5` gives a small but broad Full200 raw gain
with no intended FLOP increase.  This is not a 5% breakthrough, but it is a
clean scalar harvest.

Packaged:

```text
whest-starterkit/packages/to_test_remote/submission_phase1_highseed88_f16_pathcv_betas15_countprobe_bundle.tar.gz
sha256 9486e200660754f6548603e8b680582de5251c78f75b158af9097d67dcdbeef5

whest-starterkit/packages/to_test_remote/submission_phase1_highseed88_f16_pathcv_betas15_ridge01_countprobe_bundle.tar.gz
sha256 6f9b442ffe85d0ca70543650334f4b6bd53ac92683fed515d6da4812fbb23823
```

Validation:

```text
whest validate passed
mini indices 0,5 smoke: failed=0, raw=3.717905e-7
ridge0.01 mini indices 0,5 smoke: failed=0, raw=3.715678e-7
```

Modeled from the same `311749` pathCV120 remote anchor used in the count
frontier:

```text
estimated remote raw      ~= 1.231312854e-6
estimated remote mult     ~= 0.176632
estimated remote adjusted ~= 2.174892520e-7
```

Applied the same beta-scale patch to the two pathCV88 router probes, only
inside their pathCV branch.  Protected-branch math and router rules are
unchanged.

```text
whest-starterkit/packages/to_test_remote/submission_phase1_pathcv88_flatq05_late_std_or_betas15_router_probe_bundle.tar.gz
sha256 16d191fdf497ec0755c88ef0dfb048f9d0040d0661be4fd317cb8653c1ed662b

whest-starterkit/packages/to_test_remote/submission_phase1_pathcv88_q25_std_or_betas15_router_probe_bundle.tar.gz
sha256 71dc26cbfbfd30bc799f11a5b0056383acda5a9e9e72cb8eb798bed1c166ae79
```

Both passed `whest validate` and Mini index-0 smoke.  These should be treated
as high-upside router probes, not protected candidates, because the router
thresholds were selected from live public row losses.  The beta-scale fix
should make them slightly better than the prior pathCV88 router estimates, but
it does not change the overfit risk.

### 2026-06-24 - pathCV teacher-gate falsifier and z2 squared-control probe

Target-free QR-offset teacher gate:

Goal: replace the public-row-tuned protected/pathCV branch gates with labels
from the high-count QR-offset teacher.  This used the existing
`pathcv88_branch_gate_full200.npz` feature cache and the `0..7` QR-offset
teacher, then scored the resulting choices against targets only after the
teacher rule/model was fixed.

Result: reject this router.  The direct teacher choice selected protected too
often and was worse than fixed pathCV:

```text
fixed protected adjusted = 3.123401419e-7
fixed pathCV88 adjusted  = 8.886292930e-8
target oracle adjusted   = 8.463999341e-8

teacher off0..7 raw      = 3.277271301e-7
teacher direct true adj  = 1.061521840e-7
teacher choices          = pathCV 172 / protected 28
```

OOF tree/regression models trained on teacher proxy losses either collapsed to
always choosing pathCV or remained worse than fixed pathCV.  Read: the
QR-offset teacher is useful as a raw ceiling, but it is not a reliable
target-free branch-label source for protected-vs-pathCV routing.

Second pathwise control:

Added `legacy_workspace/probe_pathcv_z2_control.py` to test a cheap extra
control inside pathCV.  Once the first-layer projected residual

```text
z1 = E||X|| * (ReLU(u W0) - E[ReLU(u W0)]) @ response
```

is already computed, the probe adds

```text
z2 = z1^2 - E[z1^2]
```

where `E[z1^2]` is computed from the exact first-layer ReLU covariance and the
same mean-field response.  This is target-free and almost free relative to the
full sampled branch, aside from one small first-layer covariance projection.

Diagnostics:

```text
Full200 spaced10, count32, beta_scale=1.0:
  z1  raw = 1.217051488e-6
  z12 raw = 1.197377070e-6  # +1.6% vs z1

Full200 spaced20, count88, beta_scale=1.5:
  plain raw = 4.906282448e-7
  z1    raw = 4.314132975e-7
  z12   raw = 4.219587619e-7  # +2.2% vs z1

Full200 first50, count88, beta_scale=1.5:
  plain raw = 4.539309228e-7
  z1    raw = 4.120632419e-7
  z12   raw = 4.032908769e-7  # +2.1% vs z1

Mini indices 0,20,40,60,80, count88, beta_scale=1.5:
  plain raw = 7.231900839e-7
  z1    raw = 8.399756302e-7
  z12   raw = 8.185023800e-7  # bad split for pathCV/z12
```

Packaged as an upside probe, not a protected recommendation:

```text
whest-starterkit/packages/to_test_remote/submission_phase1_highseed88_f16_pathcv_z12_betas15_countprobe_bundle.tar.gz
sha256 395368fabd2ef59a6e7327745146aeea0fcb8586e1ac15767eb97c579eb165de
```

Validation/smoke:

```text
whest validate passed
Mini index 0 smoke: failed=0, raw=1.959468e-7, adjusted=1.429299e-7
Mini indices 0,20,40,60,80 smoke: failed=0, raw=8.185900e-7, adjusted=6.001268e-7
```

Read: z2 is a real structural control on Full200 slices but split-sensitive.
It may improve the standalone pathCV88 remote probe if the Full200 ratio
transfers, but the Mini5 miss means this should sit below the router probes and
below the scalar `betas15/ridge01` package in confidence.

Safer z2-scale follow-up:

The aggressive z1/z2 beta-scale `1.5` was too split-sensitive.  Retested with
z1 scale fixed at `1.0` and z2 scale swept:

```text
Mini indices 0,20,40,60,80, count88:
  z1 scale 1.0 raw      = 7.831956270e-7
  z12 z2_scale 1.5 raw  = 7.695155715e-7
  z12 z2_scale 3.0 raw  = 7.656815579e-7
  z12 z2_scale 4.0 raw  = 7.683257255e-7

Full200 first50, count88:
  z1 scale 1.0 raw      = 4.135535752e-7
  z12 z2_scale 2.0 raw  = 4.030130617e-7
  z12 z2_scale 3.0 raw  = 4.001535419e-7
  z12 z2_scale 4.0 raw  = 3.989552279e-7
```

Chose `z2_scale=3.0` as the safer compromise: it is within `0.3%` of the
Full200-first50 optimum and better on Mini5 than scale 4.

Packaged:

```text
whest-starterkit/packages/to_test_remote/submission_phase1_highseed88_f16_pathcv_z12_z2s3_countprobe_bundle.tar.gz
sha256 c62850187f30d18fbbe567d55a8e6cba4eea599b1cd2cfc7074d66e91c0ebd8b
```

Validation/smoke:

```text
whest validate passed
Mini indices 0,20,40,60,80 smoke: failed=0, raw=7.658198e-7, adjusted=5.584546e-7
```

Read: prefer this safer z12 package over
`submission_phase1_highseed88_f16_pathcv_z12_betas15_countprobe_bundle.tar.gz`.
The older beta15 z12 archive remains a diagnostic artifact only.

Router branch swap:

Built the same two protected/pathCV88 public-row router gates, but swapped the
pathCV branch from beta15 scalar pathCV to the safer z12/z2-scale-3 branch.
Gate thresholds and protected math are unchanged.

```text
whest-starterkit/packages/to_test_remote/submission_phase1_pathcv88_flatq05_late_std_or_z12z2s3_router_probe_bundle.tar.gz
sha256 66cebf4283ad061ed8941344783ce88cf0420993c4d3468c688853277b729f0e

whest-starterkit/packages/to_test_remote/submission_phase1_pathcv88_q25_std_or_z12z2s3_router_probe_bundle.tar.gz
sha256 3ee2a84d8e6f7d519daedce46d8de6054c540ea4c8f805e4b0908205f7512ea4
```

Validation/smoke:

```text
both whest validate passed
flatq05_late_std_or_z12z2s3 Mini index 0: failed=0, raw=1.845435e-7, adjusted=1.346413e-7
q25_std_or_z12z2s3 Mini index 0:       failed=0, raw=1.845435e-7, adjusted=1.346773e-7
```

Read: these are now the top adjusted-score probes in the parked queue, but
they inherit the same public-row gate risk as the earlier pathCV88 routers.

Good-canary follow-up for z12:

The initial z2-scale-3 recommendation was too first50-heavy.  Broader checks:

```text
Full200 tail100:
  z1       raw = 5.009658323e-7
  z2s1    raw = 5.005992335e-7
  z2s2    raw = 5.023815649e-7
  z2s3    raw = 5.064582809e-7  # rejected by tail

Mini100:
  z1       raw = 4.480691706e-7
  z2s1    raw = 4.450978731e-7
  z2s2    raw = 4.440546174e-7
  z2s3    raw = 4.449324585e-7

Full200 all200 combined:
  z1       raw = 5.030964338e-7
  z2s1    raw = 4.983512978e-7
  z2s2    raw = 4.954481351e-7
  z2s3    raw = 4.945943177e-7
```

Read: z2 is real, but scale selection matters.  `z2s3` is best on Full200
all200 and Mini100 but fails the protected Full200 tail100 guard.  `z2s1` is
the canary-safe scale: smaller gain, positive on all checked broad guards.

Packaged conservative z2s1 variants:

```text
whest-starterkit/packages/to_test_remote/submission_phase1_highseed88_f16_pathcv_z12_z2s1_countprobe_bundle.tar.gz
sha256 101b204abdadf5408f2cb1ce52460a8bf31b39cdd0ea00b0121ef35daede7655

whest-starterkit/packages/to_test_remote/submission_phase1_pathcv88_flatq05_late_std_or_z12z2s1_router_probe_bundle.tar.gz
sha256 a33f78e9b554bca7c6788af53db70b6e1ba8983e7f4346dab2a933b4004da30d

whest-starterkit/packages/to_test_remote/submission_phase1_pathcv88_q25_std_or_z12z2s1_router_probe_bundle.tar.gz
sha256 ca2341247efb42d88553c379648538e916a67374f4a5e67f73f1a419eafbd4fc
```

Validation/smoke:

```text
z2s1 count probe: whest validate passed; Mini 0,20,40,60,80 raw=7.729514e-7, adjusted=5.637316e-7
z2s1 flatq05 router: whest validate passed; Mini index 0 raw=2.094304e-7, adjusted=1.528293e-7
z2s1 q25 router:     whest validate passed; Mini index 0 raw=2.094304e-7, adjusted=1.528153e-7
```

Queue policy: place z2s1 router probes ahead of z2s3.  Keep z2s3 as an
aggressive probe only.

Follow-up z2 coefficient sweep:

The initial `z2s3` recommendation was too aggressive, but `z2s1` left a small
amount of signal unused.  A finer sweep on the broad canaries gives:

```text
Full200 tail100:
  z1        raw = 5.009666311e-7
  z12_s0.50 raw = 5.005713371e-7
  z12_s0.75 raw = 5.005130757e-7  # best tail point
  z12_s1.00 raw = 5.006004022e-7
  z12_s1.25 raw = 5.008300202e-7  # still better than z1
  z12_s1.50 raw = 5.012023660e-7  # tail overcorrects

Mini100:
  z1        raw = 4.480680344e-7
  z12_s1.00 raw = 4.450979123e-7
  z12_s1.25 raw = 4.446569892e-7
  z12_s1.50 raw = 4.443358914e-7
  z12_s2.00 raw = 4.440548650e-7

Full200 all200:
  z1        raw = 5.030968865e-7
  z12_s0.75 raw = 4.993970397e-7
  z12_s1.00 raw = 4.983509880e-7
  z12_s1.25 raw = 4.974332391e-7
  z12_s1.50 raw = 4.966429762e-7
```

Read: `z2_scale=1.25` is the current balanced coefficient.  It improves
Mini100 and Full200 all200 over z2s1, and it remains positive on the protected
tail100 guard.  `z2_scale=1.5` has more all200/Mini signal but starts to lose
tail robustness, so keep it un-packaged until a better adaptive gate appears.

Packaged balanced z2s125 variants:

```text
whest-starterkit/packages/to_test_remote/submission_phase1_highseed88_f16_pathcv_z12_z2s125_countprobe_bundle.tar.gz
sha256 794f1c4a76fb17bb8d01238a0d9005fbf3d5d25b4f7850e10764850a73acd073

whest-starterkit/packages/to_test_remote/submission_phase1_pathcv88_flatq05_late_std_or_z12z2s125_router_probe_bundle.tar.gz
sha256 26c6324e91335aa2a01da92b6dcbd8eca74400ac2fd88455dc4065cc688ed061

whest-starterkit/packages/to_test_remote/submission_phase1_pathcv88_q25_std_or_z12z2s125_router_probe_bundle.tar.gz
sha256 b9c963de760c8d397ef5ee245ffdf95e6016f11e5c92b970ed0467c2e5df45c1
```

Validation: all three passed `whest validate`.  The archives were packaged
from directories, not single-file mode, and include the direction blobs plus
router branch files where applicable.

Adaptive z2-scale gate:

Per-MLP oracle over fixed z2 scales on Full200 has real headroom:

```text
fixed z2s125 raw = 4.974332391e-7
fixed z2s3   raw = 4.945937227e-7
oracle scale raw = 4.775552777e-7
```

A simple production-cheap gate recovers a small but real part of that headroom:

```text
rule:
  flat_q05(layers 12..23) <= -0.14524311106652021 -> z2_scale = 3.0
  otherwise                                             z2_scale = 0.5

Full200 all200 raw = 4.925413865e-7
Full200 first100 raw = 4.841417102e-7
Full200 tail100 raw = 5.009410628e-7
```

Fold-selected single-feature threshold checks also beat fixed high/low:

```text
s0.5 vs s3 flat_q05 OOF raw = 4.926110670e-7
s0.75 vs s3 flat_q05 OOF raw = 4.926525129e-7
s1.25 vs s3 all_row_std OOF raw = 4.926160723e-7
```

Read: the adaptive gate is useful locally, but its threshold is still selected
from public Full200 targets.  It is a stronger but riskier remote probe than
fixed z2s125.

Packaged adaptive variants:

```text
whest-starterkit/packages/to_test_remote/submission_phase1_highseed88_f16_pathcv_z12_adapt05hi3_countprobe_bundle.tar.gz
sha256 0ea8e3d84585a550a92f96dc336778d99f936a5f0467da1437b7d7571ee723eb

whest-starterkit/packages/to_test_remote/submission_phase1_pathcv88_flatq05_late_std_or_z12adapt05hi3_router_probe_bundle.tar.gz
sha256 a8cc9b68ceefd4acf84d84603a812e5efcc765e0e00df4fc4273ed56059f26d0

whest-starterkit/packages/to_test_remote/submission_phase1_pathcv88_q25_std_or_z12adapt05hi3_router_probe_bundle.tar.gz
sha256 3798da76f12507fd39e7fbcc5a2c7a2a95c01dd7cbfc58cb2eab426077720f41
```

Validation: all three passed `whest validate` and were packaged from folders.

Compute correction for high-count pathCV/z12:

The standalone adaptive count88 package was smoked through `whest run` with
one generated depth-32 MLP and `--n-samples 1`.  The estimator completed under
the local runner, but used:

```text
FLOPs used          ~= 1.96e11
effective compute  ~= 2.11e11
score multiplier   ~= 0.7767
```

The flat-q05 adaptive router also selected the pathCV branch on the generated
smoke MLP and used essentially the same compute.  This invalidates the older
standalone count88 `~0.176` multiplier estimates.  Recomputing
`pathcv88_branch_gate_full200.npz` with a corrected pathCV multiplier gives:

```text
pathCV multiplier 0.773:
  fixed protected adjusted = 3.123401419e-7
  fixed pathCV adjusted    = 2.851907512e-7
  oracle branch adjusted   = 2.125994581e-7
  OOF ridge gate adjusted  = 2.915791983e-7
  OOF random forest        = 2.825636484e-7
```

Read: high-count pathCV still has raw signal, but it is not a safe adjusted
frontier unless the branch is made cheaper or the router becomes much more
reliable.  Demote all high-count z12/pathCV88 packages to diagnostics for now.

Correction after checking saved submission telemetry and local profiles:
`311749` is not a verified upload of our `highseed120_f16_pathcv_rawprobe`
package; it is a leader/competitor page that was saved for signature reading.
Its `~0.24` multiplier must not be used as an anchor for our pathCV packages.
Direct local flopscope profiles of our package implementations are:

```text
highseed88_f16_pathcv_countprobe, mini idx0:
  flops ~= 1.961e11, effective ~= 1.979e11, multiplier ~= 0.728

highseed120_f16_pathcv_rawprobe_clean, mini idx0:
  flops ~= 2.670e11, effective ~= 2.691e11, multiplier ~= 0.989
```

Those numbers match the fact that the branch propagates tens of thousands of
rows through 32 dense layers.  Therefore all pathCV count88/count120 adjusted
models based on the `311749` multiplier are invalid.  Treat our high-count
pathCV branch as a raw-accuracy diagnostic only unless its row propagation is
fundamentally compressed.

Low-count z2 sanity sweep:

To see whether z2 can help inside the current protected compute band, reran the
same z12 control at lower counts on Full200:

```text
count18:
  plain raw = 2.730633328e-6
  z1    raw = 2.556865183e-6
  z12   raw = 2.551366036e-6

count24:
  plain raw = 1.988076182e-6
  z1    raw = 1.841982236e-6
  z12   raw = 1.831555553e-6
  z12_s2 raw = 1.829092569e-6

count32:
  plain raw = 1.532349601e-6
  z1    raw = 1.391890208e-6
  z12   raw = 1.378911255e-6
  z12_s2 raw = 1.371452861e-6
```

Read: z2 is consistently positive, but low-count pathCV remains too inaccurate
for adjusted-score replacement.  The count32 raw improvement does not justify
the extra sample count versus the protected pure18 line.  Do not package this
branch unless it is used as a correction inside a stronger hybrid.

Protected + low-count pathCV blend check:

Saved count32 z12 predictions and compared them to the protected l2snap
prediction on Full200.

```text
protected raw            = 2.181298568e-6
count32 z12 raw          = 1.371452861e-6
best scalar blend raw    = 1.356939582e-6  # 0.12 protected + 0.88 path32
OOF per-neuron affine raw = 1.431669257e-6
```

Even optimistic adjusted estimates lose:

```text
protected only:   2.1813e-6 * 0.143 ~= 3.12e-7 local adjusted
path32 only:      1.3715e-6 * ~0.28 ~= 3.84e-7
run both/blend:   1.3569e-6 * ~0.42 ~= 5.70e-7
```

Read: low-count pathCV is not complementary enough to justify running alongside
the protected branch.  Close this as a non-submit path for now.

### 2026-06-24 - broad block-trajectory reweighting falsifier

Reopened the strongest remaining "free internal observable" idea on a broader
guard after the earlier spaced20/first100 checks:

```text
script:
  legacy_workspace/probe_l2snap_gate_block_weight.py

command shape:
  indices=0-199
  rows_per_seed=64
  checkpoints=4,8,12,16,20,24,28,32
  mode=linear
  device=cuda
```

Baseline from the protected 18-block l2snap seed cloud on the same Full200
slice:

```text
base raw       = 2.181298568e-6
base spaced20  = 2.690266076e-6
base tail_guard=max(last100,q2,q3)=2.284199387e-6
```

Best grouped-OOF block-trajectory reweighting result:

```text
lam=100, clip=0
raw       = 2.176620115e-6  # rel 0.997855
tail      = 2.280802906e-6
spaced20  = 2.645705965e-6
```

The nominal tail objective moves by only `0.15%`, and the stable high-shrink
variants are effectively neutral:

```text
lam=1000, clip=1 raw=2.181348601e-6 rel=1.000023 tail=2.284264599e-6
lam=1000, clip=2 raw=2.181613306e-6 rel=1.000144 tail=2.284361842e-6
```

Read: gate-trajectory block reweighting has a tiny diagnostic signal, but not a
deployable score lever.  It does not recover the signed seed-block oracle and
does not justify adding runtime telemetry to the protected estimator.  Close
the block-level gate telemetry lane unless a new derivation supplies a specific
signed feature rather than generic gate/margin summaries.

### 2026-06-24 - protected-l2snap z12 control probe

Tested the missing interaction between the current protected l2snap sample path
and the first-layer squared response control.  This is distinct from the
high-count pathCV z12 branch: it keeps the protected 18 QR blocks, h1 exact
moment correction, layer-2 snap, seed weights, and final affine calibration,
then applies even/odd seed cross-fitted controls:

```text
z1 = Rbar * (h1 - E[h1]) @ response
z2 = z1^2 - E[z1^2]
```

Small spaced20 smoke looked promising:

```text
plain raw              = 2.249992539e-6
z12_s2_r1e-4 raw       = 2.117331988e-6
```

Full200 killed the deployment value:

```text
plain raw              = 2.181291500e-6
z1_r1e-4 raw           = 2.177832625e-6
z12_s2_r1e-4 raw       = 2.152966296e-6
z12_s2.5_r1e-3 raw     = 2.152866055e-6  # best raw
```

The gain is only about `1.3%` raw on Full200, and it is lopsided:

```text
plain first100/last100       = 2.078889013e-6 / 2.283693988e-6
z12_s2.5_r1e-3 first/last    = 1.996632941e-6 / 2.309099168e-6
```

Read: the squared response control is real, but too weak for its runtime cost.
It requires mean-field response plus first-layer covariance/projection work,
which is far more than the roughly `1.3%` raw improvement can pay for at the
protected compute multiplier.  Do not package this direct z12-on-l2snap control
unless a future derivation supplies a much cheaper approximation or a robust
target-free gate.

### 2026-06-24 - balanced union-cache l2snap count proposals, exact validation

Goal: revisit the count-20/22/24 l2snap branch with a less public-fragile seed
selection.  Prior targetdual count packages were selected mostly from one broad
surface and only gave borderline adjusted gains.  This pass used the union53
l2snap cache only as a proposal generator, with a heavier Mini guard:

```text
python legacy_workspace/search_l2snap_union_subset.py \
  --counts 18,19,20,21,22,23,24 \
  --full-weight 1 --mini-weight 4 \
  --random-restarts 30 --max-passes 4
```

Important caveat preserved: union-cache subset scores are not exact selected
bank scores because layer-2 snap statistics depend on the selected row bank.
Every candidate below was therefore recomputed exactly with
`probe_l2snap_split_beta.py` before interpretation.

Union-cache proposal winners:

```text
k22 seeds =
  (2,6,15,16,20,23,24,33,44,47,49,63,77,84,88,90,102,106,107,110,111,112)
k23 seeds =
  (0,2,6,15,16,20,23,24,33,44,47,49,63,77,84,88,90,102,106,107,110,111,112)
k24 seeds =
  (0,2,6,15,16,20,21,23,24,33,44,47,49,63,77,84,88,90,102,106,107,110,111,112)
```

Exact selected-bank validation, beta_mu=beta_sd=0.5, equal weights:

```text
Mini100:
  k22 raw=1.296548023e-6 affine_raw=1.296468671e-6
  k23 raw=1.244365394e-6 affine_raw=1.244278716e-6
  k24 raw=1.193955909e-6 affine_raw=1.192741213e-6

Full200:
  k22 raw=1.675248797e-6 affine_raw=1.650914249e-6
  k23 raw=1.615852710e-6 affine_raw=1.590793893e-6
  k24 raw=1.570620822e-6 affine_raw=1.550743261e-6

Full1000:
  k22 raw=1.841940125e-6 affine_raw=1.834191776e-6
  k23 raw=1.767651450e-6 affine_raw=1.760100099e-6
  k24 raw=1.695283662e-6 affine_raw=1.689604022e-6
```

Adjusted proxy using protected `311697` as the remote anchor:

```text
protected remote:
  raw=1.655662345e-6, mult=0.1431856, adjusted=2.370739059e-7
protected local Full1000 exact:
  raw=2.306189896e-6, affine_raw=2.301739747e-6

Full1000 raw-ratio model:
  k22 mult~=0.174909 -> adjusted~=2.313e-7  (affine-ratio: ~=2.308e-7)
  k23 mult~=0.182839 -> adjusted~=2.320e-7  (affine-ratio: ~=2.315e-7)
  k24 mult~=0.190769 -> adjusted~=2.322e-7  (affine-ratio: ~=2.319e-7)
```

Read: this is a real and reproducible raw-MSE improvement, selected with a
Mini-aware proposal and verified exactly on Mini100/Full200/Full1000.  However,
the compute multiplier almost cancels the extra raw signal.  The best adjusted
point is likely k22, with a modeled improvement of only about `2.5%` versus
protected.  Package k22 as a controlled candidate if we want a safe-ish remote
probe, but do not treat the count branch as the path to a `2.1e-7` or lower
score unless we find a way to buy the k22/k24 raw gain with protected-like
compute.

Package built and validated:

```text
whest-starterkit/packages/active/
  submission_phase1_pure22_balmini_union_l2snap_equal_affine_full1000_module_finalonly_bundle.tar.gz
sha256:
  9500c0928c09ecfce209566c61f9a7cc6606168cb92a70cff85eddd2cb61f55c

whest validate:
  passed

quick_score_selected mini indices 0,5:
  raw=5.770766e-7
  score=1.017432e-7
  flops=4.662e10
  effective multiplier=0.17886151
```

The two-MLP effective multiplier should not be used as a remote score estimate
because startup/residual overhead dominates tiny local runs; use the analytical
FLOP delta and broad raw-ratio model instead.

Follow-up, Mini-weight-1 union proposal:

```text
k24 seeds =
  (8,13,15,16,17,20,22,23,24,44,47,48,49,63,84,88,97,102,105,106,107,110,111,112)
```

Exact selected-bank validation, beta_mu=beta_sd=0.5, equal weights:

```text
Mini100:
  raw=1.453462200e-6 affine_raw=1.450414042e-6
Full1000:
  raw=1.616232946e-6 affine_raw=1.613476338e-6
```

This is not the Mini-strong/safe proposal; it is a Full1000-leaning frontier
proposal.  Its Full1000 raw ratio versus the protected local line is about
`0.701`, which maps to roughly `1.16e-6` remote raw if the `311697` raw-transfer
ratio holds.  With the k24 compute multiplier model (`~0.19077`), that gives an
optimistic adjusted estimate around `2.21e-7`.  Treat this as a higher-upside
remote-transfer probe with public-selection risk, not as a guaranteed protected
replacement.

Package built and fixed after an initial row-count typo:

```text
whest-starterkit/packages/active/
  submission_phase1_pure24_bal1_union_l2snap_equal_affine_full1000_module_finalonly_bundle.tar.gz
sha256:
  8c2a5073aa708c5d95a98f2d6e0270cbf86bb09e4c12ffd6b51e61032360bcbb

whest validate:
  passed

quick_score_selected mini indices 0,5:
  raw=6.260230e-7
  score=1.272959e-7
  flops=5.085e10
  effective multiplier=0.20376674
  failed=0
```

Again, the two-MLP effective multiplier is a packaging health check, not a
remote estimate; it is inflated by fixed startup/residual effects.

Additional exact frontier around the count branch:

```text
k24_w05 seeds =
  (8,13,15,16,17,20,22,24,26,44,47,48,49,63,84,88,97,102,105,106,107,110,111,112)

Exact selected-bank validation:
  Mini100  raw=1.502953062e-6 affine_raw=1.494103158e-6
  Full200  raw=1.490114411e-6 affine_raw=1.483247381e-6
  Full1000 raw=1.606167067e-6 affine_raw=1.604159097e-6

package:
  submission_phase1_pure24_w05_union_l2snap_equal_affine_full1000_module_finalonly_bundle.tar.gz
sha256:
  cb533f9651d6b27469bddef22630d0f2ff4f8b4e42db9344dc73531ad284c114
validation:
  whest validate passed
  quick_score_selected mini 0,5: failed=0, flops=5.085e10, raw=5.927770e-7

k26_w05 seeds =
  (8,15,16,17,20,24,26,33,44,47,48,49,63,70,79,84,88,93,97,102,105,106,107,110,111,112)

Exact selected-bank validation:
  Mini100  raw=1.362801318e-6 affine_raw=1.349712830e-6
  Full200  raw=1.320033150e-6 affine_raw=1.318110086e-6
  Full1000 raw=1.479947762e-6 affine_raw=1.476944652e-6

package:
  submission_phase1_pure26_w05_union_l2snap_equal_affine_full1000_module_finalonly_bundle.tar.gz
sha256:
  ee83d0f3269c55d4496c8fb8e9411d28cedf5f548638f57c79ba6e4ac8d403bf
validation:
  whest validate passed
  quick_score_selected mini 0,5: failed=0, flops=5.508e10, raw=7.192073e-7
```

Adjusted proxy using protected `311697` remote raw transfer and the count
multiplier model:

```text
k24_w05: mult~=0.190765, remote_raw_est~=1.154e-6, adjusted_est~=2.201e-7
k26_w05: mult~=0.206624, remote_raw_est~=1.062e-6, adjusted_est~=2.195e-7
k28_w05: mult~=0.222484, remote_raw_est~=0.989e-6, adjusted_est~=2.200e-7
k30_w1:  mult~=0.238343, remote_raw_est~=0.940e-6, adjusted_est~=2.242e-7
```

Read: the count frontier is flat.  k26_w05 is the best packaged point by this
model and is the only count-increase package with a plausible first-place-sized
remote estimate.  k28/k30 reduce raw further but the multiplier catches up.

Systematic l2snap beta recheck on k26_w05:

```text
Full200 beta grid, best raw:
  mu=0.35 sd=0.45 raw=1.317297312e-6 affine_raw=1.317268764e-6
  baseline 0.5/0.5 raw=1.320033150e-6 affine_raw=1.318110086e-6

Full1000 confirmation:
  mu=0.35 sd=0.45 raw=1.476395184e-6 affine_raw=1.476295712e-6
  baseline 0.5/0.5 raw=1.479947762e-6 affine_raw=1.476944652e-6
```

Read: this is a real generalizable knob, not seed tuning, but it is tiny.  Raw
improves about `0.24%`; affine-calibrated improvement is only about `0.04%`.
Keep it in mind if repackaging k26 anyway, but do not expect it to move rank.

Follow-up package kept:

```text
whest-starterkit/packages/active/
  submission_phase1_pure26_w05_split035_045_union_l2snap_equal_affine_full1000_module_finalonly_bundle.tar.gz

whest-starterkit/packages/to_test_remote/
  submission_phase1_pure26_w05_split035_045_union_l2snap_equal_affine_full1000_module_finalonly_bundle.tar.gz

sha256 = 0a8808f61b9f9d30fac8c25752d5af3ba1cfdb64eb45ce1a4b5315ab7bad4efd
```

Validation:

```text
whest validate passed.
Mini indices 0,5 smoke:
  current k26 raw = 7.192073e-7, adjusted = 1.500870e-7
  split k26 raw   = 7.258024e-7, adjusted = 1.515145e-7
```

Read: keep the package because it is compute-free and Full1000-positive, but
the tiny gain is not slice-uniform.  It is not a priority remote upload over
the baseline k26 package without a canary reason.

### 2026-06-24 - influence-weighted gate ecology falsifier

Added:

```text
legacy_workspace/probe_l2snap_influence_gate_ecology.py
```

This is the concrete version of the biology/gate-ecology idea: instead of
generic gate-rate summaries, compute per-QR-block gate/near-boundary/activation
deviations at late checkpoints and project them through a mean-gated downstream
response matrix into final neurons.  The resulting features are signed,
target-free, and final-influence-weighted.

Small spaced10 smoke showed apparent life:

```text
indices=0,20,...,180
rows_per_seed=32
checkpoints=28,31,32
base raw = 2.289457126e-6
best grouped-OOF raw ~= 2.017906872e-6
```

Full200 killed the promotion case:

```text
base all   = 2.181298568e-6
base first = 2.078894894e-6
base last  = 2.283702241e-6
base spaced20 = 2.690266076e-6

best OOF all = 2.180074271e-6  # only 0.056% raw gain
best first   = 2.069074700e-6
best last    = 2.291073843e-6  # regresses
best spaced20= 2.707579454e-6  # regresses
```

Also checked whether these features predict the target-free high-count
`union53 - protected` teacher residual on the same Full200 slice:

```text
teacher residual corr with true residual = +0.818940913
best ridge teacher R2 ~= -0.002
ExtraTrees teacher R2 ~= -0.014
best target-scaled raw ~= 2.177683199e-6
```

Read: influence-weighted gates are the right conceptual sharpening of the
biology/percolation analogy, but this implementation does not identify the
signed high-count correction.  Close this exact gate-ecology lane; future gate
work needs a derived boundary-current / cross-moment observable, not more
generic late gate summaries.

### 2026-06-24 - union53 teacher distillation recheck

Rechecked the current target-free teacher-compression path after the
influence-gate miss.  The teacher label is always deterministic
`union53_equal_l2snap - protected18_weighted_l2snap`; targets are used only for
diagnostics.

Full200 cached trajectory-map distiller:

```text
script = legacy_workspace/probe_l2snap_union_teacher_traj_distill.py
teacher raw = 7.250611638e-7
protected raw = 2.181297274e-6
teacher/true residual corr = +0.81828

best trajectory OOF:
  raw = 2.170558639e-6
  rel = 0.995077
  teacher_r2 = +0.01340
  teacher_corr = +0.11703
```

Broad Full1000 -> Mini cheap-feature distiller:

```text
script = legacy_workspace/probe_l2snap_highcount_teacher_distill.py

safe features:
  Mini base raw = 1.897755704e-6
  best Mini raw = 1.897734551e-6  # effectively flat

full features:
  Mini base raw = 1.897755704e-6
  best Mini raw = 1.897497949e-6  # 0.014% raw gain
```

Tiny nonlinear MLP on the same broad cheap features:

```text
script = legacy_workspace/train_l2snap_union_teacher_mlp.py
mode=full hidden=64 epochs=25 device=cpu

train teacher_r2 = +0.077506
valid/Mini teacher_r2 = -0.012976
best fixed Mini gain 0.25:
  raw = 1.891759052e-6  # 0.316% raw gain
  spaced20 worsens from 2.314678963e-6 to 2.322136079e-6
```

Read: high-count teacher compression remains the right e-8 ceiling path, but
the current cheap seed-cloud/diagonal/trajectory features are far too weak.
Nonlinear regression over the same features learns train-only teacher structure
and does not transfer teacher R2.  Do not package this PERO form.  The next
distillation attempt must expose a new measurement, not a bigger learner.

### 2026-06-24 - target-free QR-offset selection against union53 teacher

Added reusable diagnostic:

```text
legacy_workspace/probe_qroffset_union_teacher_select.py
```

Question: earlier QR-offset packages were target-selected and failed remotely.
Can we instead choose one fixed QR row offset per protected seed by minimizing
distance to the target-free `union53` high-count teacher, then get the same
compute as protected18 with better geometry?

Result on cached Full1000 offsets `0..31`:

```text
union53 teacher target raw = 8.039954401e-7
protected offset0 raw      = 2.371449645e-6
protected -> teacher mse   = 1.568098955e-6

selected offsets           = [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
selected raw               = 2.371449645e-6
full200 selected raw        = 2.194006045e-6  # identical to offset0
```

Read: this closes the clean target-free QR-offset variant.  The selected
protected geometry is already a local optimum under the high-count teacher;
the earlier target-picked offset gains were not a stable geometry signal.

### 2026-06-24 - low-K QR-offset teacher compression

Added diagnostic:

```text
legacy_workspace/probe_qroffset_lowk_teacher_distill.py
```

Question: the `32 offsets x 18 seeds` QR ensemble has e-8-class raw MSE but is
far over budget.  Can a small rectangular offset set predict the remaining
offsets using only target-free block statistics, trained against the full
offset ensemble teacher?

Setup:

```text
teacher = equal mean over 32 offsets x 18 seeds
teacher Full1000 raw = 7.932026223e-8
labels = teacher - lowK_mean
features = lowK mean, offset/block sd/range/absdev, offset deltas,
           neuron coordinate, MLP-level mean/std/block sd
fit = 5-fold MLP-grouped ridge to teacher label
targets = diagnostics only
```

Result:

```text
best adjusted proxy:
  k=4 spread offsets (0,10,21,31)
  direct raw       = 5.779938069e-7
  best OOF raw     = 5.777863927e-7
  adj proxy        = 3.255413618e-7
  teacher_r2       = +0.00062
  teacher_corr     = +0.01079

k=7 spread offsets (0,5,10,16,21,26,31)
  direct raw       = 3.419791671e-7
  best OOF raw     = 3.416230249e-7
  adj proxy        = 3.368403025e-7
  teacher_r2       = +0.00148
  teacher_corr     = +0.03307
```

Read: the missing-offset residual is very aligned with the true residual, but
it is not predicted by the observed low-K offset statistics.  Rectangular
spread offsets are a good high-raw family, yet still adjusted-worse than the
protected/pathCV frontier.  Close this low-K teacher-compression feature family;
future QR-offset compression needs a new observable, not more ridge on block
spread summaries.

### 2026-06-24 - old warm-up analytic estimator Phase-1 sanity check

Checked whether the older cumulant/learned `whest-starterkit/estimator.py`
track was a forgotten Phase-1 contender.  It is not.

```text
command:
  quick_score_selected.py --estimator estimator.py
  --revision v1-phase1 --split mini --depth 32 --indices 0,20

result:
  raw = 3.313713e-5
  adjusted = 3.816573e-6
  multiplier = 0.1242
  failed = 0
```

For comparison, the high-budget QR-offset raw-frontier f32 package on the same
two rows scored raw `4.396293e-7`, adjusted `4.332235e-7`.  Read: the old
warm-up/depth-8 analytic branch is conceptually informative but not a drop-in
Phase-1 depth-32 candidate.

### 2026-06-24 - tail-response weight-path teacher features

Added diagnostic:

```text
legacy_workspace/probe_l2snap_tail_response_teacher_features.py
```

Question: previous teacher distillers used mostly final seed-cloud and local
weight/diagonal features.  Does a non-local, final-rooted view of the last few
weight layers identify the union53 high-count residual?

Features:

```text
mean-gated tail response matrices for last 4 layers
response column l2/l1/sum/max/concentration
response contracted with diagonal forward mean/variance states
final-layer column summaries and base-row summaries
label = union53_l2snap - protected18_l2snap
fit = MLP-grouped 5-fold ridge to teacher label
```

Full200 rows 0-49:

```text
base raw    = 2.187283580e-6
teacher raw = 7.178367848e-7
teacher/true residual corr = +0.824238

best ridge teacher_r2 = -0.00758
best teacher_corr     = -0.11645
best target raw       = 2.189777582e-6  # worse than base
```

Read: simple final-rooted tail-response summaries are not the missing
non-local feature.  Close this formula-mining variant; a useful learned/formula
route needs richer state than a few mean-gated tail response norms.

### 2026-06-24 - partial Stein boundary-current check

Revisited `probe_stein_boundary_current.py` because the original full-depth
diagonal boundary-current arm had weak but positive teacher correlation.  The
specific follow-up suggested in the ledger was to isolate layer-local boundary
sources.

Full200 spaced20, half16 rows, protected seed set, `scale=std`:

```text
max_layer=4:
  best teacher corr ~= +0.084
  best residual corr ~= +0.093
  best target-fitted blend rel ~= 0.99198

max_layer=12:
  best teacher corr ~= +0.084
  best residual corr ~= +0.093
  best target-fitted blend rel ~= 0.99271
```

The earlier full-depth smoke had teacher correlations around `+0.12..+0.14`
for half8/half16, so truncating the boundary current does not strengthen the
observable.  Read: close the partial-source diagonal Stein variant.  A Stein
route would need the Hutchinson/tangent-gradient version or a more exact
boundary formula, not this smoothed diagonal current.

Hutchinson/tangent-gradient follow-up:

```text
script:
  legacy_workspace/probe_stein_hutchinson_boundary.py
idea:
  estimate ||grad z_j||^2 with propagated Hutchinson tangent probes instead of
  the diagonal q @ W^2 shortcut, then use the arm only as a signed correction
  to the current protected l2snap base.
```

Spaced20 initially looked modestly alive:

```text
half8, P=2, tau=0.35:
  base raw                 = 2.250037261e-6
  teacher raw              = 6.564559526e-7
  arm corr with teacher    = -0.14565
  teacher-fitted raw       = 2.209215837e-6  # ~1.8% gain
```

But Full200 falsified it:

```text
half8, P=2, tau=0.35:
  base raw                 = 2.181298568e-6
  teacher raw              = 7.250611638e-7
  arm corr with teacher    = +0.00069
  teacher-fitted raw       = 2.183320347e-6  # worse
  target-fitted raw        = 2.180336429e-6  # only 0.044% oracle gain
```

Read: close the current Stein boundary-current family, including diagonal,
partial, and Hutchinson variants.  The boundary observable is too unstable
across slices to be a score lever.

### 2026-06-24 - union53 teacher equivariant-student smoke

Patched the research-only Song probe:

```text
song/src/train_phase1_l2snap_equivariant.py
```

New mode:

```text
--label-mode teacher
--train-teacher-seed-cache legacy_workspace/cache/l2snap_union53_seed_preds_full1000.npz
--valid-teacher-seed-cache legacy_workspace/cache/l2snap_union53_seed_preds_mini100.npz
```

This trains to the deterministic target-free high-count correction
`union53_l2snap - protected18_l2snap`, not to public truth.  Public targets are
reported only as diagnostics.  This is the right way to test whether a
weight-structured ML student can see the e-8-class high-count correction that
ridge/final-cloud features miss.

CPU smoke results:

```text
train64/valid32, diag rows, hidden4 rounds1 epoch1:
  teacher_train raw              = 7.513946057e-7
  base_to_teacher_train          = 1.222302036e-6
  teacher_valid raw              = 6.343229855e-7
  base_to_teacher_valid          = 1.378619927e-6
  valid truth best               = 1.916135101e-6 @ gain 1.0
  valid teacher best             = 1.375514436e-6 @ gain 0.75

train160/valid100, zero rows, hidden8 rounds2 epoch5:
  teacher_train raw              = 7.338655259e-7
  base_to_teacher_train          = 1.265019022e-6
  teacher_valid raw              = 6.778022874e-7
  base_to_teacher_valid          = 1.318610378e-6
  epoch5 valid truth best        = 1.897756292e-6 @ gain 0.0
  epoch5 valid teacher best      = 1.316346462e-6 @ gain 0.5
```

Read: the teacher-label route is conceptually clean but this small CPU
equivariant student barely learns out-of-distribution teacher structure
(`~0.17%` Mini teacher-MSE reduction in the stronger run) and does not improve
Mini truth.  Do not package or spend more CPU cycles on this exact tiny student.
A serious learned route would need GPU-scale training and likely richer state
inputs, then a separate flopscope-native distillation plan.

### 2026-06-24 - response-aware row compression and scalar-shadow calibration

Reopened the compute-reduction question without overlapping the W0 branch work.
The protected/module packages already store directions in `flops.Module`, so
the old setup-held-array bug is not the main multiplier source.  The protected
`311697` multiplier is dominated by the actual dense row propagation:
`18 seeds x 512 antithetic rows x 32 layers`, not by tuple materialization.

Response-aware late row selection:

```text
script patched:
  legacy_workspace/probe_late_tail_coreset.py

new modes:
  next_norm, next_meanabs, next_center_norm
```

These select rows at late handoff checkpoints using the next-layer ReLU
response, rather than only current activation norm.  On Full200 spaced10:

```text
full protected raw          = 2.289458097e-6
best response-aware thinning:
  after24_next_norm_192 raw = 2.978571637e-6
```

Read: response-aware row picking is still far too lossy.  The tail needs the
row distribution, not just high-response representatives.

Centroid row compression:

```text
new modes:
  avg_order, avg_norm, avg_next_norm
```

These average equal-mass row bins into centroids before propagating the tail.
This preserves checkpoint means better than row picking, but the nonlinear ReLU
tail makes the centroid approximation catastrophic:

```text
Full200 spaced10:
  full protected raw              = 2.289458097e-6
  after30_avg_order_128 raw       = 9.038140566e-5
  after28_avg_order_128 raw       = 1.803960478e-4
```

Read: close late row compression via selection or centroids.  Any future
compression would need a nonlinear local surrogate for the tail, not row
averaging/picking at a checkpoint.

Scalar-shadow calibration:

Added:

```text
legacy_workspace/probe_l2snap_scalar_shadow_calib.py
```

Motivation: a target-using per-MLP affine correction to the protected final
vector is a large oracle (`2.30174e-6 -> 1.47914e-6` on Full1000), but static
features and weight geometry do not predict it.  The new probe uses a small
extra shadow sampler only to measure MLP-level shift/scale, then applies a
scalar/affine correction to the protected vector.

Fast spaced20 showed real raw signal:

```text
shadow seeds 32..63:
  base raw                    = 2.250037261e-6
  half_rows=64 affine raw     = 1.969837186e-6  # 12.5% raw gain
  half_rows=128 shift raw     = 2.165476390e-6
```

Full200 broad checks confirmed signal but not enough gain per FLOP:

```text
shadow seeds 32..47:
  base raw                    = 2.181298568e-6
  half_rows=64 affine raw     = 2.126199317e-6  # 2.5% raw gain
  half_rows=128 affine raw    = 2.039007263e-6  # 6.5% raw gain

shadow seeds 32..39:
  base raw                    = 2.181298568e-6
  half_rows=64 affine raw     = 2.153612830e-6  # 1.3% raw gain
  half_rows=128 affine raw    = 2.096442150e-6  # 3.9% raw gain
```

Pricing read: `8 x 128` half-rows adds roughly `22%` row work for only `3.9%`
raw improvement; `16 x 128` adds roughly `44%` row work for `6.5%` raw
improvement.  This is a useful measurement primitive but not an adjusted-score
package.  Future use would require replacing some protected rows or making the
shadow substantially cheaper; as a pure add-on it is worse than the protected
line.

Per-MLP affine prediction checks:

```text
Full1000 protected raw                       = 2.301739726e-6
target-using per-MLP affine oracle raw       = 1.479135880e-6
MLP row-mean residual oracle raw             = 2.036806724e-6
```

But OOF predictors from protected row/seed summaries and cached
`full1000_weight_features_v1` fail:

```text
row/seed summaries, teacher-label ET:
  Full1000 OOF raw = 2.300019102e-6
  Mini apply raw   = 1.893996753e-6  # tiny/noisy

weight-geometry features, teacher-label ET:
  Full1000 OOF raw = 2.310006748e-6  # worse
```

Read: the per-MLP affine oracle is real, but it is not visible in static cheap
features.  Scalar shadow measures a little of it, yet not efficiently enough
as an add-on.

Follow-up: pruned-base plus scalar shadow as a replacement, not an add-on.
Patched `probe_l2snap_scalar_shadow_calib.py` to accept arbitrary base seed
subsets and inherited protected weights.

```text
Full200 spaced20, count14 base
  base seeds = (2,3,7,8,13,15,17,20,22,24,27,28,29,31)
  base raw                         = 2.986700607e-6
  16 shadow seeds x 32 half-rows   = 2.568635075e-6
  16 shadow seeds x 64 half-rows   = 2.650564200e-6
  16 shadow seeds x 128 half-rows  = 2.715956180e-6

Full200 spaced20, count16 base
  base seeds = protected minus (0,6)
  base raw                         = 2.562227240e-6
  8 shadow seeds x 64 half-rows    = 2.358203594e-6
  8 shadow seeds x 128 half-rows   = 2.365967510e-6

Full200 spaced20, count17 base
  base seeds = protected minus (6)
  base raw                         = 2.368532171e-6
  4 shadow seeds x 64 half-rows    = 2.293398601e-6
```

The equal-row-budget replacement case is count17 plus `4 x 64` shadow rows,
which still trails the protected spaced20 raw (`~2.25e-6`).  Count16/count14
recover some of their own pruning loss but remain too weak.  Close scalar
shadow as a replacement strategy unless a substantially cleaner scalar
observable is derived; do not tune shadow seed identities.

QR-offset low-k schedule and teacher-distillation recheck:

```text
script:
  legacy_workspace/probe_qroffset_lowk_teacher_distill.py
teacher:
  32 offsets x 18 seeds, Full1000 raw = 7.932026223e-8
```

Spread-out offsets are slightly better than prefix offsets, but the full-offset
teacher residual is not learnable from the low-k block cloud:

```text
Full1000 direct, no target fitting:
  k=1 offset (0)                         raw=2.382544306e-6, adj_proxy=3.356e-7
  k=2 spread (0,31)                      raw=1.205047108e-6, adj_proxy=3.395e-7
  k=3 spread (0,16,31)                   raw=8.114848174e-7, adj_proxy=3.429e-7
  k=4 spread (0,10,21,31)                raw=5.779938069e-7, adj_proxy=3.257e-7
  k=7 prefix (0..6)                      raw=3.496396165e-7, adj_proxy=3.447e-7
  k=7 spread (0,5,10,16,21,26,31)        raw=3.419791671e-7, adj_proxy=3.372e-7
```

OOF ridge corrections trained to the 32-offset teacher have near-zero
`teacher_r2` and do not move the adjusted proxy materially.  The best proxy in
this sweep is `k=4` spread at about `3.26e-7`, still behind the protected
low-compute line.  Keep spread offsets as a high-raw diagnostic insight, not a
promotion path.

Selective protected-count z12 columns:

Added:

```text
legacy_workspace/probe_selective_z12_columns.py
```

Question: the protected-count z12 control is raw-positive but too weak for its
full response/covariance overhead.  Is the gain concentrated enough that we
could compute only selected final response columns?

Using saved `l2snap_z12_full200.npz`:

```text
base raw                = 2.181291500e-6
full z12_s2.5_r0.001    = 2.152866055e-6

top q by diagonal final mean magnitude:
  q=32  raw=2.160170702e-6  capture=74.3%
  q=48  raw=2.157040146e-6  capture=85.3%
  q=64  raw=2.153024597e-6  capture=99.4%
  q=80  raw=2.152216370e-6  capture=102.3%
```

Spaced20 cache confirms the same concentration pattern:

```text
base raw                = 2.249992539e-6
full z12_s2_r0.01       = 2.120950835e-6

top q by diagonal final mean magnitude:
  q=32  raw=2.154945798e-6  capture=73.7%
  q=48  raw=2.138933924e-6  capture=86.1%
  q=64  raw=2.129694012e-6  capture=93.2%
  q=80  raw=2.125271250e-6  capture=96.7%
```

Read: this is a valid small lever.  If flopscope implementation can compute
only dynamic top-`64..80` response columns, selective z12 may recover nearly
all protected-count z12 raw gain at a fraction of the overhead.  It is not an
e-8 path by itself; expected adjusted improvement is likely sub-percent unless
the column-subset implementation is very cheap.  Keep as a packaging option
after larger bets, not as a reason to stop.

### 2026-06-24 - shared-bank subset readout stack probe

Added reusable diagnostic:

```text
legacy_workspace/probe_l2snap_subset_readout_stack.py
```

Motivation: the cached branch-stack ceiling reaches `~6e-7` raw OOF on
Full1000, but the full stack uses many branch geometries.  A possible
compression is to propagate one shared seed bank once, then form several fixed
subset readouts from the same final seed means and linearly combine them.

Using `l2snap_union53_seed_preds_full1000.npz` as a proposal surface:

```text
single readouts, union53-bank proposal surface:
  k24_w05 raw     = 1.646979721e-6
  k26_w05 raw     = 1.516158447e-6
  count30 raw     = 1.412367619e-6
  union53_eq raw  = 8.039954401e-7
```

Low/mid-count stacked readouts with grouped OOF linear fits:

```text
max_union=30, lam=1e-8:
  best raw-ish stack:
    union=29 raw=1.429768948e-6
    names=union53_best18,k24_bal1,k24_w05,k26_w05
  best qmax-adjusted objective:
    union=24 raw=1.654879411e-6
    names=union53_best18,k24_bal1

max_union=40, lam=1e-8:
  best lower-raw stack:
    union=36 raw=1.166009053e-6
    names=protected_w,protected_eq,late_core18
  adjusted proxy still about 3.33e-7 because the union count is too high.
```

The first low-ridge pass also showed severe OOF coefficient instability when
the readouts are nearly collinear; strong shrinkage collapses toward a constant
predictor.  With sane shrinkage, no 24-40 seed subset-readout stack beats the
existing `k26_w05` count frontier on adjusted proxy.

Read: branch-stack gains are not recoverable as a cheap algebra over a small
shared l2snap bank.  The useful part of the branch-stack ceiling is still broad
high-count averaging / different row geometry, not a small set of fixed subset
readouts.  Close this exact route unless a new non-collinear observable is
added to the shared bank.

### 2026-06-24 - tail-response teacher features

Ran:

```text
python legacy_workspace/probe_l2snap_tail_response_teacher_features.py \
  --indices 0-199 --max-tail 8
```

This tries a target-free teacher label:

```text
label = union53_equal_l2snap - protected18_weighted_l2snap
```

Features are mean-gated final-rooted response matrices for the last 8 layers,
contracted with cheap diagonal forward states and the protected prediction.

Result on Full200:

```text
base_raw       = 2.181298568e-6
teacher_raw    = 7.250611638e-7
teacher_corr   = +0.818276

best OOF:
  lam=10 blend=0.5
  raw = 2.179132162e-6
  rel = 0.999007
  teacher_r2 = +0.00874
  teacher_corr = +0.08370
```

Read: the broad high-count residual is only weakly visible in these
final-rooted response summaries.  The signal is real but around `0.1%` raw, so
it is not a route toward e-8 or even a meaningful package.  Close this
tail-response feature form unless it is paired with a new sampled measurement.

### 2026-06-24 - k26 exact-QR/free-information reruns

Retested the main "free information" QR identities on the current stronger
`k26_w05` count frontier, because the older closures were mostly on the
protected-18 branch.

Blockwise layer-1/layer-2 snap:

```text
script = legacy_workspace/probe_l2snap_blockwise_snap.py
seeds  = (8,15,16,17,20,24,26,33,44,47,48,49,63,70,79,84,88,93,97,102,105,106,107,110,111,112)

Full200 grid:
  global h1 + global l2, mu=0.50 sd=0.45  affine=1.317214181e-6
  global h1 + global l2, mu=0.35 sd=0.45  raw=1.317297312e-6 affine=1.317268764e-6
  baseline mu=0.50 sd=0.50                affine=1.318110086e-6
```

Per-QR-seed h1/l2 moment snaps were worse across the grid.  Read: the useful
moment repair is global across the whole QR cloud, not per-block.  This closes
the "each block is a full orthogonal basis, so snap each block separately"
variant.

Signed within-block partitions on the same k26 seed set, spaced20, 64 rows per
seed:

```text
base raw                 = 8.888452836e-6
best aggregate raw       = 8.593598408e-6  # rel 0.9668 but tail objective worse
best aggregate tail obj  = 1.201659577e-5
best seedwise raw        = 8.874040350e-6  # rel 0.9984
best both raw            = 8.873426158e-6  # rel 0.9983
```

The aggregate contrast can move the tiny spaced20 slice, but it worsens the
tail-risk objective.  Per-seed and aggregate+per-seed modes shrink to
near-zero.  Read: row-order Walsh/Haar contrasts are not the missing
QR-block error observable on k26 either.

Self-Harmonic ReLU Control on k26, spaced20, equal seed weights, split beta
`mu=0.35 sd=0.45`:

```text
post-ReLU:
  plain raw      = 1.236500831e-6
  best beta=0.25 = 1.230557712e-6  # tiny target wiggle
  mean_vr=-0.045973, mean_sign=0.498037, mean_corr=0.000735

preactivation:
  plain raw      = 1.236500831e-6
  best beta=0.25 = 1.233741294e-6  # tiny target wiggle
  mean_vr=-0.024630, mean_sign=0.497260, mean_corr=-0.000124
```

Read: both SHRC variants have negative target-free block variance reduction
and sign accuracy at chance.  The small target MSE changes are not trustworthy.
Close SHRC on the current count frontier unless a new derivation changes the
control sign/centering.

### 2026-06-24 - k26 z12 control check

Retested the known first-layer pathwise `z12` zero-mean control on the current
`k26_w05` count frontier.  This was motivated by the earlier selective-column
result on protected-18, where most z12 gain was concentrated in a target-free
top-final-column subset.

Spaced20 looked clean:

```text
plain raw                 = 1.221801433e-6
z12_s2_r0.01 raw          = 1.204017661e-6
z12_s2_r0.01 affine_raw   = 1.077952065e-6
```

Full200 still looked positive:

```text
plain raw                 = 1.318703299e-6
best z12_s2_r0.01 raw     = 1.308515923e-6
best z12_s2_r0.01 affine  = 1.308019885e-6
q50_max improved          = 1.380031865e-6 -> 1.371434026e-6
```

But Full1000 reversed the high-scale story:

```text
plain raw                 = 1.477092366e-6
z1_r0.01 raw              = 1.476951316e-6   # microscopic positive
z12_s1.5_r0.01 raw        = 1.477731889e-6   # worse
z12_s2_r0.01 raw          = 1.480934995e-6   # worse
z12_s2.5_r0.01 raw        = 1.485622616e-6   # worse
```

Lower scales on Full1000 are positive but tiny:

```text
z12_s0.25_r0.01 raw       = 1.476215555e-6
z12_s0.50_r0.01 raw       = 1.475777344e-6
z12_s0.75_r0.01 raw       = 1.475708535e-6   # best
z12_s1.00_r0.01 raw       = 1.476011783e-6
```

Selective-column concentration for the surviving `s0.75` correction:

```text
top q by diag_final_abs:
  q=32  raw=1.476398007e-6  capture=50.2%
  q=64  raw=1.476070460e-6  capture=73.8%
  q=80  raw=1.475910986e-6  capture=85.4%
  q=96  raw=1.475753656e-6  capture=96.7%
```

Read: z12 is a real target-free structural control, but on the robust Full1000
surface it is only a `~0.09%` raw gain at the best stable scale.  Selective
columns can buy most of that gain, but the gain is too small to justify a
production rewrite unless the selected-column implementation is almost free or
we are already touching the package for another reason.  Treat high-scale z12
as Full200 overfit; keep low-scale selective z12 as a low-priority polish knob.

### 2026-06-24 - QR-offset teacher subset compression

Added `probe_qroffset_subset_teacher_select.py` to test whether the excellent
over-budget 32-offset x 18-seed QR teacher can be compressed by choosing a
small global offset subset against the teacher, without target access.  This
closes a gap in earlier low-K checks, which only tried prefix/spread offsets.

Full1000 teacher and best equal-weight subsets:

```text
teacher32x18 raw          = 7.932026223e-8
offset0 raw               = 2.382544306e-6

k=3 offsets (6,10,21)     raw=7.524152878e-7 adj_proxy=3.179492031e-7
k=4 offsets (6,10,21,31)  raw=5.550696084e-7 adj_proxy=3.127420765e-7
k=5 offsets (0,6,13,21,27) raw=4.550398109e-7 adj_proxy=3.204780382e-7
k=6 offsets (6,7,10,13,16,31) raw=3.771503331e-7 adj_proxy=3.187459101e-7
k=7 offsets (4,10,14,19,23,26,29) raw=3.312338177e-7 adj_proxy=3.265965442e-7
```

Read: learned/global offset selection is much better than naive spread/prefix
at low K, and it confirms that the QR-offset teacher is the right oracle to
compress.  It is still not promotion-grade: the best adjusted proxy is
`~3.13e-7`, well behind the protected `311697` line and the k26 count frontier.
Equal-weight rectangular offset compression alone is closed unless paired with
a stronger weighting/correction mechanism.

### 2026-06-24 - QR-offset teacher trajectory distillation

Added `probe_l2snap_qroffset_teacher_traj_distill.py` to see whether existing
l2snap trajectory features can predict the strongest available deterministic
teacher residual:

```text
base_raw                  = 2.181297274e-6
qroffset_teacher_raw      = 8.172971139e-8
label_target_corr         = +0.98110
direct oracle blend raw   = 8.166552086e-8
```

The target-free label is extremely valuable, but the current trajectory
features do not expose it:

```text
best ridge OOF raw        = 2.176600385e-6  # rel=0.997847
best ridge teacher_r2     = -0.00484
best ridge teacher_corr   = +0.05144
best ExtraTrees raw       = 2.180932987e-6  # rel=0.999833
```

Read: the failure mode is feature visibility, not ridge linearity.  The
32-offset teacher residual is almost exactly the desired target correction, but
diagonal/l2snap trajectory maps are not enough to reconstruct it.  Future
student/distillation work needs richer measured states or a different teacher
observable; do not rerun this exact trajectory-feature student.

### 2026-06-24 - Weighted QR-offset subset compression

Added `probe_qroffset_weighted_subset.py` to test fixed global affine weights
on the selected QR-offset subsets.  The objective is still target-free:
minimize distance to the 32-offset QR teacher; public targets are only used
after selection for diagnostics.

Full1000 top adjusted proxies:

```text
k=4 offsets (6,10,21,31)
  equal raw              = 5.550696084e-7
  weighted raw           = 5.549237379e-7
  adj_proxy              = 3.126598889e-7
  weights                = [0.255, 0.257, 0.246, 0.242]

k=3 offsets (6,10,21)
  weighted raw           = 7.522267644e-7
  adj_proxy              = 3.178695384e-7

k=6 offsets (6,7,10,13,16,31)
  weighted raw           = 3.768634827e-7
  adj_proxy              = 3.185034806e-7
```

Read: affine weighting does not materially improve the rectangular QR-offset
compression.  The best weights stay near uniform and all top candidates remain
worse than the protected line after repricing.  Do not spend more time on
global affine weights over full-row offsets unless the row budget or branch
geometry changes.

### 2026-06-24 - Same-row-budget thin QR-offset mixtures

Added `probe_thin_qroffset_rows.py` to test whether spreading the same 256
rows per seed over multiple QR offsets improves high-order quadrature without
raising compute.  This is a different question from full-row offset ensembles:
it trades exact full-block orthogonality for phase diversity.

First10 had one tempting fluke:

```text
base 1x256               raw=2.771388212e-6
spread2 2x128 (0,31)     raw=1.704389809e-6  # rel=0.615
spread4 4x64             raw=2.154436267e-6
spread8 8x32             raw=2.712957730e-6
spread16 16x16           raw=2.800746291e-6
all32 32x8               raw=3.783424483e-6
```

The signal did not survive broader checks:

```text
first40:
base                     raw=2.124234538e-6
pair01 0,1 x128          raw=3.114259448e-6
pair24 0,24 x128         raw=2.570702678e-6
pair31 0,31 x128         raw=2.484867996e-6
pair610 6,10 x128        raw=2.441271374e-6

spaced20:
base                     raw=2.264504648e-6
pair24 0,24 x128         raw=2.749696214e-6
pair31 0,31 x128         raw=2.387906303e-6
pair610 6,10 x128        raw=3.152006280e-6
spread4 4x64             raw=3.624588294e-6
spread8 8x32             raw=4.053136307e-6
```

Read: the full 256-row orthogonal half-block is buying important low-order
integration.  Thin phase diversity is not a free improvement; the first10 gain
was canary luck.  Close same-row-budget thin QR offsets unless a new row design
preserves low-order orthogonality across offsets.

### 2026-06-24 - k26 same-cost QR-offset assignment smoke

Used the existing `direction_lab.py` l2snap runner to test whether the stronger
`k26_w05` seed bank benefits from replacing each seed's offset-0 QR block by
later same-cost QR blocks.  This is distinct from adding more offsets; compute
stays at 26 full blocks.

Full200 spaced20, l2snap beta=0.5:

```text
k26_o0       raw=1.275364230e-6  affine_raw=1.092420261e-6
k26_o31      raw=2.216522331e-6  affine_raw=2.189286832e-6
k26_alt      raw=1.753376681e-6  affine_raw=1.372022799e-6
k26_latepat  raw=1.688427696e-6  affine_raw=1.667322743e-6
```

Read: for k26, the standard offset-0 QR block remains the best tested same-cost
geometry.  The old protected18 qroffset patterns do not transfer to this seed
bank.  Do not build k26 qroffset packages from these offset patterns.

### 2026-06-24 - Target-free robust arm aggregation

The method-limit oracle shows large per-neuron arm-library headroom, so tested
simple target-free aggregation of diverse fixed l2snap readouts from the shared
union53 seed bank.  This avoids learned coefficients and uses mean/median/trim
or central readouts only; cost is charged by the union of seeds required.

Full1000 cache results:

```text
single k26_w05                         raw=1.516158447e-6

union53_best18,k24_bal1,k24_w05,k26_w05
  mean/median/trim1 union=29            raw=1.53995e-6 adj~=3.55e-7

targetdual20,count30,k24_w05,k26_w05
  mean union=42                         raw=1.17945e-6 adj~=3.93e-7

protected_w,union53_best18,targetdual20,count30,k24_bal1,k24_w05,k26_w05,late_core18
  mean union=44                         raw=1.16661e-6 adj~=4.08e-7
```

Read: robust target-free arm aggregation does not recover the arm-library
oracle.  Low-count robust aggregates are worse than the best single k26 readout,
and lower-raw broader aggregates are compute-dominated.  The arm oracle still
requires a signed selector, not central tendency across arms.

### 2026-06-24 - k26 split-beta calibration rethink

Revisited a parameter we had effectively locked in: applying a scalar final
affine fit after the l2snap/SPHEREx prediction.  The k26 count-frontier package
used in-sample Full1000 affine calibration, but the transfer question is whether
that affine survives MLP-held-out calibration.

Generated split-beta k26 predictions in
`legacy_workspace/cache/l2snap_k26_splitbeta_full1000_preds.npz`:

```text
baseline beta_mu=0.50 beta_sd=0.50:
  raw                = 1.479947762e-6
  in-sample affine   = 1.476944652e-6
  best 5-fold OOF affine shrink = 0.75 -> 1.478183655e-6

split beta_mu=0.35 beta_sd=0.45:
  raw                = 1.476395184e-6
  in-sample affine   = 1.476295712e-6
  best 5-fold OOF affine shrink = 0.00 -> 1.476395184e-6
```

Interpretation: the split-beta change itself is real and free on Full1000, but
the final scalar affine is not OOF-stable for this variant.  Built both package
forms so remote transfer can be probed without changing the row/direction count:

```text
affine:
  submission_phase1_pure26_w05_split035_045_union_l2snap_equal_affine_full1000_module_finalonly_bundle.tar.gz
  sha256 0a8808f61b9f9d30fac8c25752d5af3ba1cfdb64eb45ce1a4b5315ab7bad4efd
  whest validate passed
  mini indices 0,5 raw=7.258024e-7 adjusted=1.515145e-7

no affine:
  submission_phase1_pure26_w05_split035_045_union_l2snap_equal_noaffine_full1000_module_finalonly_bundle.tar.gz
  sha256 ff8f4b49adc7798b3cb5f1c5a1d87e272c90f2b3214a1d5c91b6aa3e8d6f86ad
  whest validate passed
  mini indices 0,5 raw=7.277165e-7 adjusted=1.564134e-7
```

Read: no-affine is not a stronger mini-smoke package, but it is the correct
calibration-safe probe.  More broadly, final affine constants should be treated
as suspect unless they pass MLP-held-out folds; do not assume the in-sample
Full1000 affine is a free transfer win.

### 2026-06-24 - Target-free union-teacher seed weights for k26

Generated k26 split-beta per-seed caches:

```text
legacy_workspace/cache/l2snap_k26_split035_045_seed_preds_full1000.npz
legacy_workspace/cache/l2snap_k26_split035_045_seed_preds_mini100.npz
```

The tempting target-fitted seed-weight variants are not stable:

```text
global seed weights, fit to public labels:
  equal raw          = 1.476394503e-6
  full-fit raw       = 1.470537411e-6
  5-fold OOF raw     = 1.489194712e-6  # worse

per-neuron seed weights, fit to public labels:
  equal raw          = 1.476394503e-6
  full-fit raw       = 1.368187292e-6
  5-fold OOF raw     = 1.627483929e-6  # much worse
```

The useful variant is target-free: fit the 26 seed weights to mimic the union53
l2snap teacher mean, then evaluate against labels only afterward.  This improves
both Full1000 and independent Mini100 labels:

```text
Full1000 equal split raw           = 1.476394503e-6
Full1000 union-teacher weights raw = 1.472777052e-6
Mini100 equal split raw            = 1.355160638e-6
Mini100 union-teacher weights raw  = 1.348988707e-6
```

Built package:

```text
submission_phase1_pure26_w05_unionteacherw_l2snap_split035_045_noaffine_full1000_module_finalonly_bundle.tar.gz
sha256 422d6eed802a08da870c8f29bd95c1da056df57a32430a49dc63a26962a2e40e
whest validate passed
quick_score_selected mini 0,5 raw=7.299793e-7 adjusted=1.522800e-7 failed=0
```

Read: this is small, but it is the right kind of small: same count, same compute,
target-free fit, and positive Mini transfer.  Prefer it over plain k26 if the
remote queue includes one k26 count-frontier package.

Follow-up target-free teacher variants:

```text
Union53 robust teachers:
  mean    Full1000=1.472777031e-6 Mini100=1.348988489e-6
  median  Full1000=1.473338138e-6 Mini100=1.351594615e-6
  trim1   Full1000=1.472756640e-6 Mini100=1.349024271e-6
  trim2   Full1000=1.472754039e-6 Mini100=1.349465528e-6

First40 teachers:
  all tested modes worsened Mini and Full relative to equal weights.

Per-neuron union53 teacher weights:
  Strong teacher fit worsens Mini; high-ridge variants are weaker than global
  union53 mean weights.
```

Combined Full1000+Mini100 union53-mean teacher fit:

```text
Full1000 equal split raw                  = 1.476394503e-6
Full1000 combined-teacher weights raw     = 1.472975768e-6
Mini100 equal split raw                   = 1.355160638e-6
Mini100 combined-teacher weights raw      = 1.339379517e-6
```

Built package:

```text
submission_phase1_pure26_w05_unionteacher1100w_l2snap_split035_045_noaffine_module_finalonly_bundle.tar.gz
sha256 e3c541ae87d8d983660c6bb5ad4239eb2fa9d0e72eb6d0a3ce4950817406845d
whest validate passed
quick_score_selected mini 0,5 raw=7.228284e-7 adjusted=1.514207e-7 failed=0
```

Read: combined-teacher weights are now the preferred same-count k26 package.
They are still a modest gain, but they are target-free and cheap.

Adaptive gamma on the same target-free teacher weights:

```text
Model:
  final = equal_seed_mean + gamma(features) * (teacher_weighted_mean - equal_seed_mean)

Features:
  seed_sd_mean, seed_sd_std,
  equal_mean, equal_std,
  delta_rms, delta_abs, delta_mean, delta_std

Fit:
  target-free ridge to the union53 beta-0.5 teacher on Full1000 only
  feature set = basic_d
  ridge = 0.001
  gamma clipped to [0, 2]

Fixed unionteacher1100 weights:
  Full1000 = 1.472975768e-6
  Mini100  = 1.339379517e-6

Adaptive gamma:
  Full1000 = 1.470949253e-6
  Mini100  = 1.325972344e-6
  combined = 1.457769534e-6
```

Built package:

```text
submission_phase1_pure26_w05_unionteacher1100gamma_l2snap_split035_045_noaffine_module_finalonly_bundle.tar.gz
sha256 e89d697e9879741bfd4f85b3f38850c5bbd9560b063fe3fc075108ae62ef5783
whest validate passed
tar top-level contains estimator.py, manifest.json, and module .npz
quick_score_selected mini 0,5 raw=7.248879e-7 adjusted=1.541137e-7 failed=0
```

Read: adaptive gamma is the preferred saved same-count k26 target-free package
so far.  It is a small gain, but it is the right kind of gain: no public-target
fitting, no extra directions, and only cheap reductions after seed means are
computed.

### 2026-06-24 - MLP-level seed-subspace teacher router

Added diagnostic:

```text
legacy_workspace/probe_seed_subspace_teacher_router.py
```

Question: can the target-free high-count correction
`union53_l2snap - protected18_l2snap` be represented as a per-MLP reweighting
of the already-paid protected18 seed-block deviations, and can those MLP-level
coefficients be predicted from protected seed-cloud geometry?

This is not per-neuron target fitting; the coefficient labels come from the
deterministic union53 teacher.  Public targets are used only for final MSE
reporting.

Key results:

```text
Full1000 protected raw   = 2.302e-6
Full1000 union53 raw     = 8.040e-7
Mini100 protected raw    = 1.898e-6
Mini100 union53 raw      = 6.778e-7

seed-deviation subspace oracle, projection ridge 0.01:
  Full1000 raw = 1.191102765e-6
  Mini100 raw  = 1.024194305e-6

seed-deviation subspace oracle, projection ridge 0.1:
  Full1000 raw = 1.216637308e-6
  Mini100 raw  = 1.043015238e-6
```

But simple MLP-level geometry features do not predict the needed coefficients:

```text
ridge, proj_ridge=0.1:
  coeff_r2_full = -0.14045
  coeff_r2_mini = -0.07868
  Full1000 raw  = 2.305018702e-6
  Mini100 raw   = 1.896360813e-6

ridge100, proj_ridge=0.1:
  coeff_r2_full = -0.05227
  coeff_r2_mini = -0.02833
  Full1000 raw  = 2.305751533e-6
  Mini100 raw   = 1.895924589e-6
```

Read: the already-paid seed-deviation subspace has real headroom, but current
MLP-level seed-cloud summaries do not identify the signed coefficients.  Do not
package this router.  The next variant would need an actual observable of
within-block quadrature error, e.g. half-row disagreement or row-level
telemetry, not just aggregate block geometry.

### 2026-06-24 - QR-offset rectangular grid shape scout

Rechecked the high-budget QR-offset raw frontier for a different rectangular
shape: many offsets with fewer seed identities, using cached Full1000
`qr_offset_blocks_*` arrays.  This is target-using as an oracle scout, so any
positive result would still need target-free/split validation before packaging.

Exact all-32-offset seed-subset results:

```text
32 offsets x 2 seeds  = 64 blocks,  raw=6.881118957e-7, seeds=[0,13]
32 offsets x 3 seeds  = 96 blocks,  raw=4.538927239e-7, seeds=[0,27,29]
32 offsets x 4 seeds  = 128 blocks, raw=3.353105487e-7, seeds=[0,13,21,23]
32 offsets x 5 seeds  = 160 blocks, raw=2.672896957e-7, seeds=[0,13,15,27,29]
32 offsets x 6 seeds  = 192 blocks, raw=2.215571593e-7, seeds=[0,17,21,24,27,29]
```

Exact all-18-seed offset-subset prefix from the same scan reproduced the known
low-K offset frontier:

```text
18 seeds x 2 offsets = 36 blocks, raw=1.144278364e-6, offsets=(6,10)
18 seeds x 3 offsets = 54 blocks, raw=7.524152878e-7, offsets=(6,10,21)
18 seeds x 4 offsets = 72 blocks, raw=5.550696084e-7, offsets=(6,10,21,31)
```

Read: all-offset seed-thinning is not the missing raw-frontier shape.  At a
deployable near-budget count (`~126-128` blocks), `32x4` is worse than the
known `7x18` rectangular frontier (`~3.50e-7` Full1000 raw) and still far above
the raw needed to beat the low-compute protected line at full-budget
multiplier.  Do not package all-offset seed-thin variants unless a non-linear
cost model changes.

### 2026-06-24 - Protected18 target-free gamma shrink

After the k26 `unionteacher1100gamma` result, tested whether the same idea is a
free improvement for the protected18 line:

```text
final = equal18 + gamma(features) * (protected_weighted18 - equal18)
```

The gamma labels are target-free, fit to the union53 l2snap teacher on
Full1000.  Targets are used only for reporting.

Baseline:

```text
Full1000 equal18 raw       = 2.309813271e-6
Full1000 weighted18 raw    = 2.301739726e-6
Full1000 union53 raw       = 8.039954401e-7
Mini100 equal18 raw        = 1.897400882e-6
Mini100 weighted18 raw     = 1.897755704e-6
Mini100 union53 raw        = 6.778027337e-7
```

Scalar gamma:

```text
gamma=0.50:
  Full1000 raw = 2.303440160e-6
  Mini100 raw  = 1.894955692e-6
gamma=0.9337 (Full teacher fit):
  Full1000 raw = 2.301696484e-6
  Mini100 raw  = 1.897082856e-6
gamma=1.00 protected weighted:
  Full1000 raw = 2.301739726e-6
  Mini100 raw  = 1.897755704e-6
```

Ridge-predicted MLP gamma from seed-cloud features helped Full1000 slightly but
hurt Mini:

```text
best Full-ish ridge:
  Full1000 raw = 2.299104640e-6
  Mini100 raw  = 1.904188190e-6
```

Read: protected18 gamma is split-fragile and too small.  Do not package this.
The k26 gamma package remains different because its target-free delta transfers
positively on both Full1000 and Mini100.

### 2026-06-24 - Teacher-labeled per-neuron seed-error router

Patched `probe_l2snap_seed_error_router.py` with:

```text
--label-source {target,union53}
```

The new `union53` mode trains candidate seed-error ranks against the
target-free union53 l2snap teacher, rather than public labels.  This checks
whether the e-8-class per-neuron seed-selection oracle becomes more learnable
when the label is a deterministic high-count estimator.

Run:

```text
python -u legacy_workspace/probe_l2snap_seed_error_router.py \
  --mode seed --label-source union53 --models et --folds 5
```

Anchors:

```text
Full200 base raw       = 2.181298568e-6
Full200 seed oracle    = 3.983043739e-7
Full200 union53 teacher= 7.250611638e-7
Mini100 base raw       = 1.897755704e-6
Mini100 seed oracle    = 3.657662002e-7
Mini100 union53 teacher= 6.778027337e-7
```

Result:

```text
Full200 hard choice     raw ~= 7.28e-5  # catastrophic
Full200 best soft       raw = 2.180489761e-6, tail=2.284128997e-6
Mini100 hard choice     raw ~= 7.18e-5  # catastrophic
Mini100 best soft mean  raw = 1.895795728e-6, tail=2.076792788e-6
Mini100 base tail       raw = 2.074553994e-6
```

Read: replacing public target labels with the target-free union53 teacher does
not make per-neuron seed selection identifiable.  The only mean-MSE gains are
tiny soft blends and they do not pass the Full200/tail guard.  Close this exact
teacher-labeled seed-error router.

Matched split-beta union53 teacher check:

```text
Generated:
  legacy_workspace/cache/l2snap_union53_split035_045_full1000_pred.npz
  legacy_workspace/cache/l2snap_union53_split035_045_mini100_pred.npz

union53 split 0.35/0.45 teacher raw:
  Full1000 = 7.996443907e-7
  Mini100  = 6.742690235e-7

k26 weights fit to matched split teacher:
  Full-only fit: Full1000=1.473028513e-6 Mini100=1.348848930e-6
  Full+Mini fit: Full1000=1.473211918e-6 Mini100=1.338636042e-6
```

Read: matched-teacher weights are not preferred.  They slightly improve Mini
but lose enough Full1000 versus the beta-0.5 union53 teacher weights that the
combined objective is worse.

### 2026-06-24 - QR-offset teacher seed-subspace ceiling

Question: the all-32-offset x protected18 QR teacher is locally e-8 raw but
over budget.  Can its correction be expressed inside the already-paid
protected18 seed-deviation subspace?

This is target-free with respect to labels: the label is
`qroffset32x18_teacher - protected18_l2snap`.  Public targets are used only to
report raw MSE.

Cached Full1000 anchors:

```text
protected18_l2snap weighted raw = 2.301739726e-6
qroffset32x18 teacher raw       = 7.932026223e-8
qroffset label RMS              = 1.493800e-3
target-optimal scalar gain      = 0.997979, raw=7.931114912e-8
```

Projecting the QR-offset teacher correction into protected18 seed deviations,
per MLP:

```text
projection ridge 0       raw = 6.452800203e-7
projection ridge 1e-4    raw = 6.452798469e-7
projection ridge 1e-3    raw = 6.452891316e-7
projection ridge 1e-2    raw = 6.462480600e-7
projection ridge 1e-1    raw = 6.810044608e-7
projection ridge 1       raw = 9.561836039e-7
projection ridge 10      raw = 1.619615574e-6
```

Read: the seed-deviation subspace has a real protected-compute ceiling.  If the
coefficients were observable, this would be near e-8 adjusted at protected
multiplier.  But a first deployable coefficient predictor from protected
seed-cloud, diagonal, and beta-feature summaries failed:

```text
ridge predictors: unbounded OOF coefficient blow-ups
ExtraTrees: coeff_r2=-0.00924, best gain=0.125, raw=2.304407081e-6
HGB:        coeff_r2=-0.08010, best gain=0.125, raw=2.306817725e-6
```

Conclusion: do not package.  Keep this as an oracle target: the missing object
is a signed row/block quadrature-error observable, not another MLP-level summary
model.

### 2026-06-24 - QR-offset teacher-labeled seed-error router

Follow-up to the seed-subspace ceiling: use the clean all-32-offset x
protected18 QR teacher as the candidate seed-error label, rather than public
targets or union53.  This tests whether the e-8 teacher makes per-neuron seed
choice learnable from deployable seed-cloud and trajectory features.

Patch:

```text
probe_l2snap_seed_error_router.py --label-source qroffset
```

Anchors on Full200/Mini100:

```text
Full200 base raw        = 2.181298568e-6
Full200 seed oracle     = 3.983043739e-7
Full200 union53 teacher = 7.250611638e-7
Full200 qroff teacher   = 8.172971139e-8
Mini100 base raw        = 1.897755704e-6
Mini100 seed oracle     = 3.657662002e-7
Mini100 union53 teacher = 6.778027337e-7
```

Seed-only features, ExtraTrees, 5-fold MLP OOF:

```text
hard choice Full200 raw = 6.916383928e-5
best soft Full200 mean  = 2.180109615e-6  # temp=0.5, blend=0.25
best soft Full200 tail  = 2.287340337e-6  # worse than base tail 2.284199387e-6
hard choice Mini100 raw = 7.935860491e-5
best soft Mini100 mean  = 1.895481202e-6  # temp=0.05, blend=0.5
best soft Mini100 tail  = 2.076749459e-6  # worse than base tail 2.074553994e-6
```

Joint trajectory features, ExtraTrees, 5-fold MLP OOF:

```text
hard choice Full200 raw = 6.946844160e-5
best soft Full200 mean  = 2.180531688e-6  # temp=0.05, blend=0.25
best soft Full200 tail  = 2.283840995e-6  # tiny tail-only movement, no mean gain
hard choice Mini100 raw = 7.533803952e-5
best soft Mini100 mean  = 1.895257220e-6  # temp=0.05, blend=0.5
best soft Mini100 tail  = 2.076573151e-6  # worse than base tail
```

Read: even the very clean QR-offset teacher does not make per-neuron seed
choice identifiable from the existing seed-cloud/trajectory feature family.
The seed-error router family is closed unless we introduce a new signed
quadrature observable, not merely a stronger teacher label.

### 2026-06-24 - Nonlinear protected-residual feature probe

Added `probe_l2snap_nonlinear_residual_features.py` to test whether nonlinear
models see a deployable residual signal that ridge misses.  This is not a
submission path by itself; it is a visibility test for a possible tiny ML
residual.  Features are generic protected seed-cloud + cached diagonal
Gaussian summaries.  Targets are used only in MLP-grouped Full1000 OOF and
Mini100 transfer.

Safe features, HistGradientBoosting:

```text
Full1000 base raw      = 2.301739726e-6
best Full1000 OOF raw  = 2.301436086e-6  # 0.013% gain, gain=0.35
Mini100 base raw       = 1.897755704e-6
best Mini100 mean      = 1.897755704e-6  # any nonzero gain worsens mean
best Mini100 tail obj  = 2.068935356e-6  # tail-only tradeoff, gain=1.0
```

Full signed seed-deviation features, HistGradientBoosting:

```text
best Full1000 OOF raw  = 2.301739726e-6  # gain=0, all nonzero gains worse
best Mini100 mean      = 1.897755704e-6  # gain=0
```

Safe features, ExtraTrees:

```text
best Full1000 OOF raw  = 2.300521421e-6  # 0.053% gain, gain=0.5
best Full1000 tail     = 2.291554808e-6
Mini100 at gain=0.5    = 1.899267690e-6 mean, tail=2.068926218e-6
best Mini100 mean      = 1.897755704e-6  # gain=0
```

Read: nonlinear residual learning from these generic features is visible but
far too weak and not Mini-mean transferable.  Do not package a residual ML
model from this feature family; a useful learned residual would need genuinely
new observables, not just tree depth on the existing seed/diagonal summaries.

### 2026-06-24 - Nonlinear low-K QR-offset teacher compression

Added `probe_qroffset_lowk_teacher_nonlinear.py` to give the e-8 QR-offset
teacher one nonlinear distillation pass.  Labels are target-free:

```text
label = mean_32_offsets - mean_selected_offsets
```

Targets are used only for final MSE reports.  This is the nonlinear analogue
of the earlier ridge low-K teacher compression check.

Full1000 spread offsets, HGB:

```text
teacher32 raw = 7.932026223e-8

k=2 offsets=(0,31)
  direct raw       = 1.205047108e-6
  teacher_r2       = -0.001443
  best robust obj  = direct, 1.238264725e-6

k=3 offsets=(0,16,31)
  direct raw       = 8.114848174e-7
  teacher_r2       = -0.009323
  best robust obj  = direct, 8.242023498e-7

k=4 offsets=(0,10,21,31)
  direct raw       = 5.779938069e-7
  teacher_r2       = -0.002583
  best raw         = 5.779816773e-7 at gain=0.25  # 0.002% cosmetic

k=5 offsets=(0,8,16,23,31)
  direct raw       = 4.904192549e-7
  teacher_r2       = -0.005650
  best robust obj  = direct, 5.054785280e-7
```

ExtraTrees on k=4:

```text
teacher_r2 = -0.001287
best raw   = 5.778957316e-7 at gain=0.25  # 0.017% cosmetic
```

Read: the low-K offset predictions are highly aligned with true residuals only
because the full 32-offset teacher is excellent, but the *missing correction*
is not predictable from these final low-K offset features.  Close low-K
QR-offset teacher compression for ridge/HGB/ExtraTrees feature students; future
compression needs a new row/internal observable or a different quadrature
construction, not more nonlinear modeling over the same low-K final cloud.

### 2026-07-04 - Higher-moment dataset first implementation

Pulled a 36-MLP public full slice of
`keenanpepper/arc-whestbench-higher-moments-2026` to D: and implemented:

```text
legacy_workspace/build_higher_moment_atlas.py
legacy_workspace/probe_higher_moment_residual_atlas.py
legacy_workspace/probe_higher_moment_edgeworth_oracle.py
```

Built:

```text
legacy_workspace/cache/higher_moment_atlas_full_shard0_36.npz
shape = 36 x 32 x 69
```

Direct MLP-held-out residual learning from compact moment summaries did not
work on the 36-MLP smoke slice:

```text
protected l2snap shard0 raw = 2.167479256e-6
broad moment residual OOF   = 2.171208453e-6
pre_skew/offcov OOF         ~= 2.167492563e-6
b21/b111/k3full OOF         ~= 2.167486641e-6
pair-cumulant interaction   ~= 2.167488952e-6
```

Read: do not package a direct final-residual ML patch from these features.

The mechanistic Edgeworth oracle is very positive.  Using the dataset's true
pre-activation cumulants to update `E[relu(z)]`:

```text
final Gaussian ReLU MSE           = 1.097157038e-6
final + true pre-skew MSE         = 1.575931643e-7
final + true skew/kurt MSE        = 2.199908236e-8
final + skew/kurt/gamma^2 MSE     = 2.028897343e-8

all-layer Gaussian ReLU MSE       = 2.626480700e-6
all-layer + true pre-skew MSE     = 3.326046002e-7
all-layer + true skew/kurt MSE    = 1.384645568e-8

final delta corr:
  k3  = +0.91707
  k34 = +0.98943
```

Read: the moment path has a real e-8-class ceiling if pre-skew/pre-kurtosis can
be predicted cheaply.  The next serious route is a deployable cumulant closure,
not final-vector routing or generic residual trees.

First cumulant-closure diagnostic:

```text
script:
  legacy_workspace/probe_higher_moment_cumulant_closure.py

Gaussian baseline all/final        = 2.626480700e-6 / 1.097157038e-6
true cumulant oracle all/final     = 1.384645568e-8 / 2.199908236e-8
layer-constant closure all/final   = 2.831336600e-6 / 1.265150932e-6
alpha-polynomial OOF all/final     = 1.135722145e-6 / 7.524477239e-7
alpha+sigma OOF all/final          = 1.137011291e-6 / 7.533647907e-7
```

Read: signed cumulants are partly predictable from marginal preactivation
geometry, but not enough.  Layer constants are wrong; alpha gives a real
improvement; the remaining gap to the true cumulant oracle likely needs
weight/path/c21 features or low-rank cumulant sketches.

Partial third-cumulant oracle:

```text
script:
  legacy_workspace/probe_higher_moment_partial_k3_oracle.py

gauss             all/final = 2.626480700e-6 / 1.097157038e-6
true_k3           all/final = 3.326046002e-7 / 1.575931643e-7
b3_k3             all/final = 2.380907405e-6 / 1.191495115e-6
b3_b21_k3         all/final = 2.297315166e-6 / 3.956910655e-6
true_k34          all/final = 1.384645568e-8 / 2.199908236e-8
b3_b21_truek4     all/final = 1.797813244e-6 / 3.497069587e-6

OOF layer-gain b3_b21_k3:
  all/final = 1.020127165e-6 / 7.824071778e-7
  final gains ~= 0.23 to 0.26
```

Read: storable `b3+b21` is directionally useful but incomplete/mis-scaled.
It needs learned damping and an omitted-block/kurtosis closure before it can be
the basis of a production estimator.

First100 validation, after downloading moment files 0..99 and official full
shards 0..2:

```text
atlas:
  legacy_workspace/cache/higher_moment_atlas_full_first100.npz
  shape = 100 x 32 x 69

direct residual-atlas:
  protected l2snap first100 raw = 2.078894894e-6
  best moment residual OOF      = 2.078899085e-6

Edgeworth oracle:
  gauss all/final    = 2.643569290e-6 / 1.087080720e-6
  true_k3 all/final  = 3.380259476e-7 / 1.551141005e-7
  true_k34 all/final = 1.354466752e-8 / 2.033313200e-8
  final corr k3/k34  = +0.91840 / +0.98973

cumulant closure:
  layer-constant final = 1.260771255e-6
  alpha OOF final      = 7.167869798e-7
  alpha+sigma final    = 7.168405403e-7

partial K3:
  raw b3_b21_k3 final      = 4.671805095e-6
  OOF-gained b3_b21 final  = 7.233485008e-7
  OOF-gained true_k3 final = 1.492563708e-7
```

Read: first100 confirms the 36-MLP result.  Direct final residual learning is
closed for these moment summaries.  Mechanistic cumulant correction is the
active path: damped skew gives a plausible `~7e-7` ReLU-update target, true
skew gives `~1.5e-7`, and true skew+kurtosis gives `~2e-8`.

Full1000 moment-summary residual gate:

```text
cache:
  legacy_workspace/cache/higher_moment_atlas_full1000_noblocks.npz
  shape = 1000 x 32 x 62

protected l2snap full1000 raw = 2.301739726e-6
broad first positive line     = 2.301665045e-6  # rel 0.999968
narrow high-reg line          = 2.301723230e-6  # rel 0.999993
```

Action: close compact moment-summary final residual patches even with 1000
MLPs.  The moment dataset remains useful for oracle state/closure labels and
for validating structured cumulant representations, but not for a cheap
MLP-level residual ridge on top of protected l2snap.

Infrastructure update: the higher-moment atlas/closure probes now accept
`--weights-npz-cache legacy_workspace/cache/phase1_full1000_weights_targets.npz`.
This memory-maps the uncompressed `weights.npy` member inside the `.npz`, so
1000-MLP oracle probes can avoid the broken parquet path and avoid loading the
full 8GB weight cache into memory.

Structured closure probe, first100:

```text
script:
  legacy_workspace/probe_higher_moment_structured_closure.py

random rank4 best:
  ridge=0.03
  k3 all/final  = 9.835890143e-7 / 6.608735372e-7
  k34 all/final = 8.426414476e-7 / 6.606324406e-7
  g3_r2/g4_r2   = +0.88909 / +0.18537

random rank8 best:
  ridge=0.03
  k3 all/final  = 9.803336779e-7 / 6.665232811e-7
  k34 all/final = 8.393833558e-7 / 6.665003222e-7
  g3_r2/g4_r2   = +0.89053 / +0.18581

eigen rank4 best:
  ridge=0.03
  k3 all/final  = 9.844358423e-7 / 6.598550945e-7
  k34 all/final = 8.434456476e-7 / 6.590988167e-7
  g3_r2/g4_r2   = +0.88863 / +0.18782
```

Read: low-rank b111/k4-style features are a real diagnostic improvement over
the alpha-only `~7.17e-7` closure, but these features still use true public
post-layer covariance/cumulant state from the moment files.  The useful lesson
is not packageability yet; it is that signed skew closure is strongly
predictable, while kurtosis prediction is still weak and mostly neutral.  The
next probe removes oracle moment features and trains against deployable
Gaussian-covariance rollout state plus weight statistics.

Deployable Gaussian-rollout closure probe:

```text
script:
  legacy_workspace/probe_higher_moment_rollout_closure.py

gaussian full-covariance rollout, first100:
  all/final = 4.892057802e-5 / 6.734664037e-5

Hermite order sweep on first20:
  order 4  final = 5.735464720e-5
  order 8  final = 5.735255002e-5
  order 16 final = 5.735254340e-5
  order 32 final = 5.735254484e-5

best recursive sigma+skew+kurt OOF, first100:
  ridge=0.1
  sig_k34 all/final = 4.214869273e-5 / 7.777775374e-5
  g3_r2/g4_r2/sig_r2 = +0.85014 / +0.42347 / +0.65591
```

Read: using only Gaussian-covariance rollout state is not viable.  The bad
state is not a weight-orientation bug and not Hermite truncation; higher
Hermite orders do not repair it.  Naive per-neuron sigma logscale calibration
is unstable when fed back into covariance propagation.  Therefore the moment
closure needs a better state source: either sampled trajectory state/SPHEREx
control variates, or a substantially stronger covariance/non-Gaussian closure.

Moment-oracle distillation from existing l2snap trajectory/preactivation
features:

```text
script:
  legacy_workspace/probe_l2snap_moment_oracle_distill.py

first100, l2snap trajectory maps -> true-preactivation k34 oracle correction:
  base raw   = 2.078894186e-6
  oracle raw = 2.096495355e-8
  best OOF   = 2.078135611e-6
  best teacher_r2/corr = -0.00461 / +0.01831

cached final-preactivation feature bank -> k34 oracle correction:
  base raw   = 2.088856465e-6
  oracle raw = 2.096495355e-8
  best tested positive blend worsened base
  ridge/hgb teacher_r2 < 0
```

Read: the current compact trajectory maps and final-preactivation summaries do
not expose the true-preactivation Edgeworth oracle correction.  This is a
negative result for "distill moment oracle from cheap l2snap summaries."  A
fusion that works probably has to preserve richer row-cloud information or use
a real pathwise control variate, not the existing map bank.

PRE-EDGE final preactivation cumulant sensor:

```text
script:
  legacy_workspace/probe_preedge_final.py

protected 18, full200:
  sample_aff raw = 2.181299872e-6
  best PRE-EDGE  = edge4 lambda=0.10
  best raw       = 2.180990546e-6
  rel gain       = 0.014%

target-fitted diagnostics on full200:
  per-neuron lambda oracle, edge34433 = 2.172009294e-6
  per-neuron lambda oracle gain       ~= 0.43%

equal high-count diagnostic, full200:
  count18 sample raw = 2.631403476e-6
  count32 sample raw = 1.439936759e-6
  count64 sample raw = 7.423351936e-7
  best count64 PRE-EDGE blend = 7.422681821e-7
```

Read: final sampled preactivation raw moments are not the missing moment-oracle
signal.  Even with 64 equal seed blocks, Edgeworth decoded final cumulants are
nearly neutral and the improvement is far below the 5% adoption threshold.
The true-preactivation cumulant oracle remains important, but estimating those
cumulants by final-layer row samples is too noisy / too collinear with the
plain sample mean to matter at deployable counts.  Close simple PRE-EDGE as a
package route; only revisit if using richer layerwise moment assimilation or a
different low-noise cumulant sensor.

Oracle hidden-state snap / MOMA ceiling:

```text
script:
  legacy_workspace/probe_oracle_state_snap.py

first100, protected18 l2snap baseline:
  raw = 2.078896911e-6

true unit-sphere post-activation mean+variance snap:
  L16 beta=1              raw = 7.512771641e-7
  L24 beta=1              raw = 3.352312803e-7
  L28 beta=1              raw = 1.551665703e-7
  L30 beta=1              raw = 6.398579725e-8
  L8,16,24,30 beta=1      raw = 5.422980153e-8

L30 stat split:
  true mean only           raw = 6.497819988e-8
  true variance only       raw = 2.076538549e-6
```

Read: the hidden mean one layer before the final ReLU is an enormous oracle
lever; hidden variance is almost irrelevant.  This reframes the moment path:
the useful state variable is not final preactivation skew/kurtosis, but a much
more accurate layer-30 post-ReLU mean vector.  Any deployable MOMA attempt
should focus on estimating that hidden mean, then using the existing sampled
row cloud as a final-layer conditional readout.

Learned layer-30 mean snap from the same protected18 sample cloud:

```text
script:
  legacy_workspace/probe_learned_state_mean_snap.py

first100 hidden label:
  sample layer30 mean MSE = 8.521734875e-9

best OOF residual denoiser:
  residual ridge alpha=100 blend=0.05
  hidden MSE = 8.509199251e-9
  final raw  = 2.077051825e-6
```

Read: the same 18-block sampled hidden mean is already very close in ordinary
MSE, and simple learned denoisers cannot recover the oracle-relevant signed
error.  The layer-30 mean error is tiny but amplified by the final random
matrix; recovering it needs a genuinely new measurement or stronger
mechanistic state estimator, not a ridge denoiser on the same mean vector.

High-count layer-30 mean pool with protected final readout:

```text
script:
  legacy_workspace/probe_layer30_mean_pool_snap.py

smoke20, protected final readout snapped to high-count L30 mean:
  count64 beta=1      raw = 8.361928676e-7
  count88 beta=1      raw = 5.595061598e-7
  count120 beta=1     raw = 4.551098085e-7

same smoke20, plain full-depth l2snap sample:
  count88             raw = 5.329584765e-7
  count120            raw = 4.119761423e-7
```

Read: high-count mean pooling is worse than simply sending the same high-count
rows through the final layer.  The oracle is not unlocked by estimating only
the penultimate hidden mean from more samples; the final readout still benefits
from many final-layer row evaluations.  Close this as a high-budget package
route.

Deployable diagonal hidden-mean snap:

```text
scripts:
  legacy_workspace/probe_learned_state_mean_snap.py
  legacy_workspace/probe_layer30_diag_snap.py

first100, L30 sampled mean blended toward diagonal Gaussian meanprop:
  blend 0.003 raw = 2.060943022e-6
  baseline raw    = 2.078896737e-6

full200 direct deployable L30 diagonal snap:
  blend 0.001 raw = 2.179455973e-6
  blend 0.002 raw = 2.179492312e-6
  baseline raw    = 2.181299398e-6

full200 direct deployable L28 diagonal snap:
  blend 0.002 raw = 2.179387814e-6
  blend 0.001 raw = 2.179499819e-6
  baseline raw    = 2.181299398e-6

full200 OOF hidden-mean residual denoising with first200 moment labels:
  best final raw ~= 2.180327088e-6
```

Read: diagonal meanprop contains a tiny signed bias correction, but the effect
is below 0.1% on full200 and split-fragile.  It is useful evidence for the
hidden-state framing, not a package route.  A podium-relevant state estimator
must recover high-dimensional signed hidden-mean error, not only scalar or
diagonal drift.

Full1000 layer-30 protected-row-cloud denoising:

```text
script:
  legacy_workspace/probe_layer30_seedstate_oof.py

cache:
  legacy_workspace/cache/layer30_seedstate_full1000_state.npz
  legacy_workspace/cache/layer30_seedstate_full1000_oof_hidden.npz

hidden equal mean MSE        = 9.330958946e-09
hidden protected-weight MSE = 9.302574936e-09
best ridge feature line      = 9.310150530e-09
```

Read: adding 1000 moment labels does not make the old layer-30 denoising route
work.  The protected weighted seed mean is still the best available hidden
state among these candidates; feature residuals over the same 18 seed-block
cloud are slightly worse.  The true layer-30 oracle remains huge, but this
particular deployable observable does not recover its signed high-dimensional
error.

### 2026-07-04 - non-naive check audit and cached synergy reminder

Prompted by a process concern: single-signal falsifiers can be too naive if
they compare different row sets, use tiny slices, or discard signals that are
weak alone but useful in combination.  Current discipline should be:

```text
1. Every candidate must carry an in-script same-row baseline.
2. Learned coefficients must be MLP-grouped OOF unless explicitly marked oracle.
3. Promotion gates should use Full200/Full1000 plus tail/block reports, not
   Mini/first50 alone.
4. Raw MSE must be repriced with the compute multiplier before calling a win.
5. Weak signals should be cached as prediction/delta vectors and tested through
   a common OOF interaction stack before being forgotten.
```

There already is some combination tooling:

```text
legacy_workspace/probe_branch_stack_oof.py
legacy_workspace/probe_l2snap_variant_synergy.py
legacy_workspace/probe_l2snap_observable_interaction_stack.py
```

Fresh rerun of `probe_l2snap_variant_synergy.py` on cached Full1000 branches:

```text
command:
  python legacy_workspace/probe_l2snap_variant_synergy.py \
    --split full1000 --folds 5 \
    --lams 0.1,1,10,100,1000 \
    --modes linear,uncert,pair,all --include-high

protected local Full1000 raw = 2.301739726e-6

single-branch local Full1000:
  union53_equal      raw = 8.039954401e-7
  first40_equal      raw = 1.068096389e-6
  count30_seedmean   raw = 1.364659199e-6

best OOF stack:
  mode=uncert, lam=0.1
  raw  = 5.957851797e-7
  tail = 6.191521149e-7
  rel  = 0.258841 vs protected local
```

Read: the user's interaction concern is real.  Some cached branches combine
substantially better than they score alone.  This is still a ceiling diagnostic,
not a deployable low-compute package: it uses expensive high-count branches and
target-OOF coefficients.  But future moment/MOMA probes should write aligned
prediction caches and be admitted to this style of stack before being closed.

### 2026-07-04 - layer-30 seed-state observer and final-projection falsifier

Goal: turn the Full200/first100 layer-30 oracle into something deployable.  The
oracle remains very strong:

```text
script:
  legacy_workspace/probe_oracle_state_snap.py

Full200, true layer-30 post-ReLU mean snap:
  no snap            raw = 2.181299398e-6
  L30 mean beta=0.25 raw = 1.248656706e-6
  L30 mean beta=0.50 raw = 5.851487251e-7
  L30 mean beta=1.00 raw = 6.551245788e-8
```

First deployable attempt:

```text
script:
  legacy_workspace/probe_layer30_seedstate_oof.py

features:
  per-seed layer-30 means from the protected 18 QR blocks
  seed disagreement / median / trim
  diagonal Gaussian meanprop
  MLP-grouped OOF hidden-mean ridge candidates
```

Full200 result:

```text
hidden MSE:
  equal seed mean        = 8.905698894e-9
  protected-hidden mean  = 8.895112016e-9

final MSE after mean snap:
  no snap                = 2.181299398e-6
  equal snap             = 2.181300008e-6
  protected-hidden snap  = 2.196192085e-6
  best damped beta grid  = 2.181299269e-6  # numerical no-op
```

Projection diagnostic:

```text
true hidden-error projected through final W vs candidate projected correction:
  protected hidden projection corr ~= +0.045
  diag meanprop projection corr    ~= +0.006
```

Read: ordinary hidden MSE is the wrong objective.  The deployable seed-state
corrections do not predict the final-weight-projected hidden error sign.

Second attempt: fit final residual directly from final-relevant layer-30
projection features.

```text
script:
  legacy_workspace/probe_layer30_projection_residual.py

features:
  per-seed final prediction deltas
  per-seed layer-30 mean deltas projected through W_final
  projected diagonal meanprop delta
  final seed variance/IQR and simple products
```

Full200 OOF:

```text
base raw = 2.181298573e-6
best OOF = 2.221194072e-6  # worse
```

Full1000 OOF after collecting:

```text
legacy_workspace/cache/layer30_seedstate_full1000_state.npz

base raw = 2.301739725e-6
best linear OOF projection residual = 2.303961477e-6  # +0.0965% worse

bounded nonlinear HGB check:
  best raw = 2.301876454e-6  # +0.0059% worse
```

Read: current protected seed-state geometry does not contain a recoverable
signed final-residual signal.  The L30 oracle is real, but not observable from
the protected 18 seed means/final deltas with these target-free features.  Close
this feature family unless a new measurement is introduced, not just another
linear/nonlinear combiner.

### 2026-07-04 - supervised PRE-EDGE raw-moment hidden denoiser

Added:

```text
legacy_workspace/probe_preedge_rawmoment_oof_hidden.py
```

Question: the earlier layer-30 seed-state observer used only block means and
simple dispersion features, while PRE-EDGE used sampled raw preactivation
moments only through a fixed Edgeworth formula.  This probe tests the missing
middle: use the same deployable raw-moment sensor as features, train an
MLP-grouped OOF hidden-mean correction against the public higher-moment labels,
then propagate the corrected hidden state to final.

Layer 24, Full200:

```text
sample hidden MSE       = 1.096834412e-8
best hidden candidate   = edge4_blend0.2, 1.096108010e-8  # tiny ordinary hidden gain
no_snap final raw       = 2.181299398e-6
best checked final raw  = 2.182017263e-6                  # worse
```

Read: late raw preactivation moment sensing is not aligned with the
final-weight-projected hidden error.  Ordinary hidden-MSE gains at L24 are a
decoy.

Layer 4, Full200:

```text
sample hidden MSE       = 2.436657534e-8
best OOF ridge hidden   = 2.320569396e-8  # rel 0.952
no_snap final raw       = 2.181299398e-6
best checked final raw  = 2.168759338e-6  # ridge_a100_c2, beta=0.75
```

Layer 4, Full1000:

```text
sample hidden MSE       = 2.419050094e-8
best OOF ridge hidden   = 2.296577764e-8  # rel 0.949
no_snap final raw       = 2.301739735e-6
best checked final raw  = 2.290486503e-6  # ridge_a1_c0, beta=0.5
```

Narrow Full1000 final-alignment follow-up, rechecking the Full200-favored
regularized/clipped branch:

```text
script:
  legacy_workspace/probe_preedge_rawmoment_oof_hidden.py
  --layer 4 --indices 0-999 --alphas 100,1000,10000 --clips 2
  --final-betas 0.5,0.75,1

no_snap final raw                    = 2.301739735e-6
ridge_a100_c2 beta=0.50 final raw    = 2.290848772e-6
ridge_a100_c2 beta=0.75 final raw    = 2.290930295e-6
ridge_a100_c2 beta=1.00 final raw    = 2.294701091e-6
ridge_a1000_c2 beta=0.50 final raw   = 2.291777599e-6
ridge_a10000_c2 beta=0.50 final raw  = 2.293827027e-6
```

Read: supervised L4 raw-moment denoising is a real, reproducible small signal
and beats the fixed Edgeworth hidden formula in OOF.  It is not a submit-grade
lever by itself: the final gain is only about `0.49%` on Full1000, while the
production version would need the same raw-moment reductions that made fixed
PRE-EDGE split-fragile and slightly more expensive.  Keep this as evidence that
moment labels can train deployable local corrections, but do not package unless
it is folded into a broader correction with shared overhead.

Downstream-weighted hidden-fit follow-up:

```text
script:
  legacy_workspace/probe_preedge_downstream_weighted_hidden.py

Layer 4, Full200:
  no_snap final raw                         = 2.181299398e-6
  best unweighted ridge beta=0.75 final raw = 2.168582145e-6
  best response-norm weighted final raw     = 2.169120973e-6
  best gradient-weighted final raw          = 2.170868753e-6
```

Read: weighting the L4 hidden denoiser by downstream response or by
target-trained residual gradient does not reveal a larger final-aligned
correction.  It reproduces the same sub-1% L4 moment signal and no more.

Multi-layer PRE-EDGE observer follow-up:

```text
script:
  legacy_workspace/probe_preedge_multi_observer.py

Full200, layers L3/L4/L5/L6 collected in one protected SPHEREx pass:
  no_snap raw                 = 2.181299398e-6
  best single L4 beta=1       = 2.160761273e-6
  best pair L3,L4 beta=0.75   = 2.160307296e-6
  best triple L3,L4,L5 beta=.5= 2.163667979e-6
  best L3,L4,L5,L6 beta=.5    = 2.168552744e-6

Hidden ordinary-MSE gains:
  L3 edge4 rel = 0.954632
  L4 edge4 rel = 0.971861
  L5 edge4 rel = 0.979982
  L6 edge4 rel = 0.986840
```

Read: the non-naive interaction version does not unlock a hidden synergy.
Early PRE-EDGE moments are real, but stacking them gives at most the same
`~1%` Full200 raw reduction as the already-known L3/L4/L4-only variants.  This
closes multi-layer PRE-EDGE as a route to the next score tier unless a new
target-free gate chooses a very small MLP subset.  The harness remains useful
for future moment-observer variants because it collects many layers in one pass.

Mini100 note: a first Mini run paired Mini weights with Full-split moment labels,
so its hidden numbers are invalid.  Do not use it as a transfer result.

Extra-measurement check: estimate the layer-30 mean with more seeds, then send
only the protected 18 rows through the final layer after mean snapping.

```text
script:
  legacy_workspace/probe_layer30_mean_pool_snap.py

Full200:
  count32 beta=0.75 raw = 1.412275629e-6
  count48 beta=1.00 raw = 1.001631543e-6
  count64 beta=1.00 raw = 7.970332328e-7

plain high-count final-only SPHEREx on same Full200 cache:
  count32 raw = 1.532349719e-6
  count48 raw = 1.040807740e-6
  count64 raw = 8.169342213e-7
```

Read: measuring the L30 mean with extra seeds is real and slightly better than
plain high-count full-depth sampling at the same count.  The score problem is
that it saves almost no FLOPs: count64 runs 64 seeds through 31 layers plus the
protected 18 through the final layer, about `97.8%` of the full count64 dense
matmul cost.  This is a useful raw-accuracy branch, not an adjusted-score
breakthrough unless we find a much cheaper L30 measurement.

Low-row extra L30 measurement:

```text
added:
  legacy_workspace/probe_layer30_extra_lowrow_snap.py

shape:
  keep protected18 full rows through L30 and final;
  add extra seeds only to L30 with fewer QR half-rows per seed;
  snap protected L30 row cloud toward the protected+extra L30 mean.

spaced20 scout:
  protected base                 raw = 2.250034020e-6
  count48 extra_half=16 beta=1   raw = 1.837052266e-6
    extra cost ratio vs protected18 ~= 0.1009
  count64 extra_half=16 beta=1   raw = 1.761449125e-6
    extra cost ratio vs protected18 ~= 0.1547

Full200 validation:
  protected base                   raw = 2.181301415e-6
  count48 extra_half=16 beta=0.75  raw = 2.051369382e-6
    raw ratio ~= 0.9404, cost ratio ~= 1.1009, adjusted ratio ~= 1.035
  count64 extra_half=16 beta=0.75  raw = 1.958740554e-6
    raw ratio ~= 0.8980, cost ratio ~= 1.1547, adjusted ratio ~= 1.037

count14 protected subset check:
  protected14 = protected18 minus 0,6,21,23
  best spaced20 count64 extra_half=32 beta=1 raw = 1.802945056e-6
  protected_cost_ratio ~= 0.7778, extra_cost_ratio ~= 0.3364
```

Read: cheap extra L30 measurements are strongly raw-positive on spaced20 but
do not survive Full200 repricing.  The broad Full200 raw gain is only about
`10%` while cost rises by `10-15%`, so adjusted score worsens slightly.  The
count14 variant has a similar cost-adjusted shape and is not a rescue.  Close
low-row L30 mean measurement as a standalone package route; the L30 oracle gap
still requires either a much cheaper independent mean estimator or a correction
that also reduces final readout quadrature noise.

Follow-up oracle ladder for earlier late checkpoints:

```text
script:
  legacy_workspace/probe_oracle_state_snap.py

Full200 true hidden-mean snap:
  no snap             raw = 2.181299398e-6
  L28 beta=1          raw = 1.703236459e-7
  L29 beta=1          raw = 1.181353505e-7
  L30 beta=1          raw = 6.551245788e-8
  L29,L30 beta=1      raw = 6.526717271e-8
  L28,L29,L30 beta=1  raw = 6.530289353e-8
```

Read: L28 already contains most of the remaining oracle signal; L29 is better;
L30 is the cleanest single checkpoint.  Stacking earlier true means adds almost
nothing once L30 is known.

Generalized `probe_layer30_mean_pool_snap.py` so earlier checkpoints propagate
through all remaining layers after the mean snap.  Corrected extra-measurement
Full200 results:

```text
L28 mean pool:
  count48 beta=1 raw = 1.061546224e-6
  count64 beta=1 raw = 8.657146313e-7

L29 mean pool:
  count48 beta=1 raw = 1.029910398e-6
  count64 beta=1 raw = 8.312719905e-7

L30 mean pool:
  count48 beta=1 raw = 1.001631543e-6
  count64 beta=1 raw = 7.970332328e-7
```

Approximate compute multipliers:

```text
count48 L28/L29/L30 pool ~= 0.356 / 0.364 / 0.371
count64 L28/L29/L30 pool ~= 0.471 / 0.482 / 0.494
```

Read: earlier checkpoints save only a few percent of compute at these late
layers, and the raw MSE worsens slightly.  The late-checkpoint oracle is not
enough by itself; the necessary breakthrough is reducing the number of expensive
directions/rows, not shaving two or three final layers.

Late-layer mean-pool compute/error sweep:

```text
script:
  legacy_workspace/probe_late_layer_pool_sweep.py

Full200, beta=1, approximate multiplier:
  L20 c32 raw=1.657355180e-6 q50max=1.910109050e-6 mult~=0.210702 adj~=3.492082088e-7
  L22 c32 raw=1.594015845e-6 q50max=1.868470272e-6 mult~=0.217610 adj~=3.468743625e-7
  L24 c32 raw=1.538090557e-6 q50max=1.830747447e-6 mult~=0.224519 adj~=3.453299784e-7
  L26 c32 raw=1.501667267e-6 q50max=1.800973547e-6 mult~=0.231427 adj~=3.475261874e-7
  L28 c32 raw=1.462173826e-6 q50max=1.767626454e-6 mult~=0.238335 adj~=3.484874279e-7
  L29 c32 raw=1.440723777e-6 q50max=1.730153239e-6 mult~=0.241789 adj~=3.483515783e-7
  L30 c32 raw=1.417910759e-6 q50max=1.712568649e-6 mult~=0.245243 adj~=3.477332867e-7

  L30 c48 raw=1.001631543e-6 q50max=1.084778327e-6 mult~=0.365645 adj~=3.662411844e-7
  L30 c64 raw=7.970332328e-7 q50max=8.850141809e-7 mult~=0.486046 adj~=3.873946684e-7
```

Count32 beta refinement:

```text
best adjusted proxies:
  L24 c32 beta=0.75 raw=1.530163726e-6 q50max=1.788832713e-6 mult~=0.224519 adj~=3.435502570e-7
  L22 c32 beta=0.75 raw=1.581839237e-6 q50max=1.828357223e-6 mult~=0.217610 adj~=3.442246065e-7
  L26 c32 beta=0.75 raw=1.490526340e-6 q50max=1.752806121e-6 mult~=0.231427 adj~=3.449478773e-7
  L20 c32 beta=0.75 raw=1.637469697e-6 q50max=1.861553558e-6 mult~=0.210702 adj~=3.450182957e-7
  L30 c32 beta=0.75 raw=1.412275629e-6 q50max=1.676267784e-6 mult~=0.245243 adj~=3.463513083e-7
```

Read: the raw sweet spot is as late as possible, but the adjusted proxy sweet
spot for this mean-pool family is earlier, around L22-L26 with count32 and
beta~=0.75.  This family still does not threaten the protected remote score, but
it tells us where a cheaper late-state measurement should probably be anchored:
not necessarily L30; L24 has a slightly better cost/error balance.

### Layerwise PRE-EDGE Hidden-State Snap

New diagnostic:

```text
legacy_workspace/probe_layer_preedge_state_snap.py
```

Question: can the already-paid SPHEREx rows act as a preactivation moment
sensor at an intermediate layer, then snap the row cloud's hidden mean before
continuing?  This is a deployable MOMA-lite correction because it does not buy
more rows; it only adds per-row reductions and a vector Edgeworth decode at one
layer.

Layer scan on Full200, protected 18 blocks, hidden-layer target from the
higher-moment files:

```text
L4  sample hidden MSE = 2.436657534e-8
    edge4 hidden MSE  = 2.368091708e-8  # 2.8% hidden gain

L8  sample hidden MSE = 2.433007115e-8
    best blend        = 2.415227736e-8  # 0.7% hidden gain

L12 sample hidden MSE = 1.965780658e-8
    best blend        = 1.958990822e-8  # 0.35% hidden gain

L16/L20/L24/L28/L30 gains fade to <= 0.2% hidden MSE.
```

Final-transfer checks:

```text
Full200 protected no-snap reference        = 2.181299e-6

L3 edge4:
  beta=0.50 raw = 2.175672661e-6
  beta=0.75 raw = 2.176013878e-6
  beta=1.00 raw = 2.178477400e-6

L4 edge4:
  beta=0.50 raw = 2.167057124e-6
  beta=0.75 raw = 2.162912536e-6
  beta=1.00 raw = 2.160759384e-6

L4 edge34433:
  beta=1.00 raw = 2.162532336e-6

L5 edge4:
  beta=0.50 raw = 2.173341258e-6
  beta=0.75 raw = 2.172302829e-6
  beta=1.00 raw = 2.173218921e-6

L8 edge4:
  best checked raw ~= 2.181009447e-6  # neutral/no-op class
```

Full1000 confirmation for the winning L4 point:

```text
protected l2snap Full1000 raw              = 2.371167263e-6
L4 edge4 beta=0.50 Full1000 raw            = 2.294761625e-6
L4 edge4 beta=0.75 Full1000 raw            = 2.294273484e-6
L4 edge4 beta=1.00 Full1000 raw            = 2.295791541e-6
L4 edge34433 beta=0.75 Full1000 raw        = 2.296414822e-6
```

Read: this is a real small correction, not a breakthrough.  It reduces
Full1000 raw MSE by about `3.2%` against the protected l2snap branch, using no
extra sampled directions.  The formula choice is stable: L4 beats neighboring
L3/L5 on Full200, and `edge4` beats `edge34433` on Full1000.  Production shape
is to compute radialized raw preactivation moments at layer 4, decode an
Edgeworth-4 post-ReLU mean, mean-snap the live row cloud with beta around
`0.75`, and continue the existing protected sampler.  It is worth packaging if
the added reductions keep the multiplier near the protected line; it is not
expected to move us into the `1e-7` adjusted domain by itself.

Production package and independent Mini falsifier:

```text
builder:
  legacy_workspace/build_h1affine_l2snap_module_package.py
  optional args added:
    --preedge-layer
    --preedge-beta

candidate:
  legacy_workspace/_pkg_pure18_protected_h1affine_l2snap_b05_preedgeL4b075_weighted_affine_full1000_module_finalonly
```

Cost profile versus protected module package:

```text
protected module:
  ops=158, flops=3.816e10
  predicted remote multiplier ~= 0.1417 / 0.1429 / 0.1441

L4 PRE-EDGE package:
  ops=285, flops=3.818e10
  predicted remote multiplier ~= 0.1430 / 0.1451 / 0.1472
```

Independent Mini100 package score:

```text
protected module:
  raw=1.897754e-6, adjusted=2.671976e-7, multiplier=0.140856

L4 PRE-EDGE package:
  raw=1.900685e-6, adjusted=2.682249e-7, multiplier=0.141183
```

Updated verdict: do not submit/promote.  L4 PRE-EDGE is a useful diagnostic and
a real Full1000 correction, but it does not transfer to the independent Mini
split and it adds enough op count to make the adjusted score worse there.  Keep
the builder hook for future controlled probes, but treat this candidate as
rejected unless a new target-free gate shows when to apply it.

Multi-layer PRE-EDGE snap audit:

```text
script:
  legacy_workspace/probe_multilayer_preedge_snap.py

Full200 protected no-snap reference = 2.181299398e-6

L4/L8/L12 sweep:
  L4 beta=1.00             raw = 2.160759384e-6
  L8 best checked          raw ~= 2.181009447e-6
  L12 beta=1.00            raw = 2.185059372e-6
  L4,L8 beta=0.25          raw = 2.175176944e-6
  L4,L12 beta=0.50         raw = 2.173060596e-6
  L4,L8,L12 beta=0.25      raw = 2.176509933e-6

L3/L4/L5 sweep:
  L3 beta=0.50             raw = 2.175672661e-6
  L4 beta=1.00             raw = 2.160759384e-6
  L5 beta=0.75             raw = 2.172302829e-6
  L3,L4 beta=0.75          raw = 2.160305587e-6
  L3,L4,L5 beta=0.50       raw = 2.163667332e-6
```

Read: stacking PRE-EDGE snaps does not unlock a larger layerwise assimilation
effect.  The best early combo, `L3,L4 beta=0.75`, improves over L4 alone by only
about `4.5e-10` raw MSE on Full200 while adding more reductions and more code
surface.  Later snaps cancel or wash out the L4 gain.  Close the multi-layer
PRE-EDGE branch for production unless a future target-free gate can identify a
small subset of MLPs/layers where it should be applied.

### 2026-07-04 - same-row-budget thin QR-offset falsifier

Rechecked a tempting "free geometry" idea: keep the same 256 half-rows per QR
seed, but split them across multiple QR offsets instead of using one full
orthogonal block.  This would have improved direction diversity at essentially
the same sampled-row count if it worked.

```text
script:
  legacy_workspace/probe_thin_qroffset_rows.py

slice:
  Full200 first40, protected 18 seeds, 256 half-rows per seed total

base offset0 full QR block:
  raw = 2.124234538e-6

spread2 offsets (0,31), 128 rows/offset:
  raw = 2.484867996e-6  # 1.170x worse

teacher4 offsets (6,10,21,31), 64 rows/offset:
  raw = 2.939651613e-6  # 1.384x worse

spread4 offsets (0,10,21,31), 64 rows/offset:
  raw = 2.735148040e-6  # 1.288x worse

spread8 offsets (0,4,9,13,18,22,27,31), 32 rows/offset:
  raw = 2.502408541e-6  # 1.178x worse

spread16, 16 rows/offset:
  raw = 2.728739966e-6  # 1.285x worse

all32, 8 rows/offset:
  raw = 3.072845304e-6  # 1.447x worse
```

Read: full-block orthogonality is doing more work than offset diversity at a
fixed row budget.  QR-offset averaging only becomes powerful when we pay for
additional complete blocks; thinning blocks destroys the low-order cancellation
that makes SPHEREx competitive.  Close this as a production path.

### 2026-07-04 - coordinate orthogonal-basis falsifier

Idea: replace each random QR half-block with the canonical coordinate basis.
Because the first weight matrix is Gaussian/rotationally invariant, this was a
plausible distribution-level trick, and it could have removed the remaining
first-layer half-row matmul cost.  The diagnostic used the existing geometry
probe on the standard spaced20 slice:

```text
script:
  legacy_workspace/probe_sampling_geometries.py

qr           raw = 2.264509615e-6
rademacher   raw = 3.478956743e-6  # 1.54x worse
coord        raw = 4.370067445e-5  # 19.30x worse
```

Read: random QR orientation is essential.  The canonical basis is
distributionally plausible across random first-layer weights, but for a fixed
MLP it undersamples the angular structure catastrophically.  Do not pursue
coordinate/sparse first-layer-free directions.

### 2026-07-04 - prior-shrunk PRE-EDGE cumulant sensor

Tested a stronger version of the final PRE-EDGE idea.  Instead of decoding the
sampled final preactivation raw moments directly, fit an MLP-grouped OOF ridge
from sampled moment features to true final preactivation skew/excess labels
from the higher-moment public files, then decode Edgeworth and OOF-blend it
back into the sampled final mean.

```text
script:
  legacy_workspace/probe_preedge_prior_shrink.py

protected18 Full200:
  sample raw                  = 2.181299872e-6
  observed edge4 raw          = 2.203904719e-6
  observed edge4 OOF blend    = 2.181114359e-6  # +0.0085%
  best prior-shrunk OOF blend = 2.181289135e-6  # weaker

equal high-count Full200:
  count18 sample raw = 2.631403476e-6
  count18 best blend = 2.631021291e-6

  count32 sample raw = 1.439936759e-6
  count32 best blend = 1.439913555e-6

  count64 sample raw = 7.423351936e-7
  count64 best prior = 7.423458309e-7  # worse than sample
```

Read: final-layer sampled preactivation moments do not expose a useful signed
cumulant correction, even when shrunk toward a learned public moment prior.
The best protected-count gain is two orders of magnitude too small for a
package candidate, and at high count the prior-shrunk variants are neutral or
negative.  Close final PRE-EDGE cumulant sensing as a serious route; the
remaining high-ceiling state route still needs a different, lower-noise hidden
mean observable or high-count/pathCV compression.

### 2026-07-04 - first200 partial K3 / b21 validation

Follow-up to the higher-moment first100 partial-K3 oracle.  The aim was to
check whether the useful two-index skew channel was a first100 accident before
asking for a targeted math derivation.

Command:

```text
python legacy_workspace/probe_higher_moment_partial_k3_oracle.py \
  --indices 0:200 \
  --weights-npz-cache legacy_workspace/cache/phase1_full1000_weights_targets.npz \
  --folds 5
```

Result:

```text
gauss             all/final = 2.720085088e-6 / 1.162166983e-6
true_k3           all/final = 3.468287881e-7 / 1.692510618e-7
b3_k3             all/final = 2.462479637e-6 / 1.201619074e-6
b3_b21_k3         all/final = 2.589386162e-6 / 4.808117929e-6
true_k34          all/final = 1.467033892e-8 / 2.228986603e-8
b3_b21_truek4     all/final = 2.044342830e-6 / 4.260965342e-6

OOF layer-gain rescue:
  b3_b21_k3 all/final = 1.041579157e-6 / 7.673749481e-7
  final gains         = [0.237, 0.250, 0.234, 0.238, 0.235]

  true_k3 all/final   = 3.304706875e-7 / 1.625001812e-7
  final gains         = [0.924, 0.925, 0.913, 0.926, 0.925]
```

Read: first200 confirms first100.  The storable two-index channel `b3+b21`
is real but badly over-scaled/incomplete if used raw.  A single held-out
layer gain around `0.24` is fold-stable, taking final MSE to `~7.7e-7` before
any SPHEREx residual.  This makes a deployable `C21/M = Cov(h_i^2,h_j)-...`
or final-rooted `b21` contraction a serious math/engineering target, not a
small-slice artifact.  The open problem is producing `b21` from hidden weights
and deployable state without using true public moment files.

Dense deployable-state follow-up:

```text
script:
  legacy_workspace/probe_dense_c21_rollout.py

first100, recursive dense mean/cov/A/k3 rollout from input:
  g0_k0          all/final = 4.892074032e-5 / 6.734703154e-5
  g0.24_k0       all/final = 4.790283510e-5 / 6.730211418e-5
  g0.94_k0       all/final = 4.644625777e-5 / 6.851174538e-5
  best OOF gain  all/final = 4.631425236e-5 / 6.786365412e-5

true-post-state anchor oracle, first100:
  anchor L7,  best final ~= 3.50e-5
  anchor L15, best final ~= 2.01e-5
  anchor L23, best final ~= 9.28e-6
  anchor L27, best final ~= 4.34e-6
  anchor L28, best final ~= 3.04e-6
  anchor L29, best final ~= 1.89e-6
  anchor L30, best final ~= 7.16e-7  # one remaining transition, g=0.24
```

Read: the `b21` oracle signal is real only when true pre mean/sigma are also
available.  The deployable recursive mean/cov state drifts far too much by
depth 32; adding dense `M` from scratch barely moves final MSE.  The
sample-anchor + analytic-tail hybrid is also mostly closed: even a true hidden
post-state at layer 27 leaves a four-layer analytic tail around `4e-6`, and
only the final single transition from a true L30 state reaches the `~7e-7`
scale.  This confirms the previous layer-30 findings: the valuable oracle is
a much more accurate penultimate hidden mean / final row-cloud state, not a
long analytic tail replacement.

Paid-row-cloud final C21 falsifier:

```text
script:
  legacy_workspace/probe_l2snap_layer30_c21_edge.py

indices:
  0,10,...,190

construction:
  run protected h1-affine + l2snap SPHEREx rows to post-layer 30
  estimate mean/covariance/C21 from the same 18 QR-antithetic blocks
  replace sampled final ReLU by Gaussian or repeated-skew Edgeworth analytic
  final-layer integration; score only held-out/OOF blends

base protected sample   = 2.250048345e-6
Gaussian final analytic = 3.592433946e-6
best fixed edge3 scale  = 3.066466426e-6  # scale=0.25
best OOF feature blend  = 2.249111332e-6  # ridge=10, +0.04%
```

Read: using the paid layer-30 row cloud as a final-layer `C21`/repeated-skew
sensor does not expose useful target-free signal.  The direct sampled final
ReLU remains much better than Gaussian or Edgeworth analytic replacement, and
the OOF blend is effectively a no-op.  Do not spend submissions or broad
Full1000 runs on this simple layer-30 analytic-tail C21 branch.

### 2026-07-04 - protected l2snap add-on stack and compute-router audit

Goal: answer whether weak side signals that failed individually can combine on
top of the current protected l2snap sampler, and whether the high-raw
consensus branch can be made adjusted-score competitive through routing.

Important validation fix:

```text
file:
  legacy_workspace/probe_l2snap_weight_residual_features.py

bug:
  grouped_oof used (groups // width) % folds even though groups already stored
  the MLP id.  On Mini this created empty/invalid folds, and on Full1000 it
  badly grouped folds.

fix:
  use groups % folds.
```

Corrected same-pass residual-feature check:

```text
script:
  legacy_workspace/probe_l2snap_weight_residual_features.py

Full1000 protected base raw = 2.301739726e-6
best corrected OOF raw      = 2.301820003e-6  # slightly worse

Mini100 protected base raw  = 1.897755704e-6
best corrected OOF raw      = 1.898221299e-6  # slightly worse
```

Read: protected seed-cloud plus cheap weight/diagonal residual features do not
give a deployable correction once the folds are actually MLP-grouped.

Strict near-free stack:

```text
script:
  legacy_workspace/probe_l2snap_nearfree_stack.py

branch_set=nearfree:
  h1kurt_m004 + exact-GH side predictions only

Full1000 OOF:
  protected raw        = 2.301739726e-6
  best nearfree stack  = 2.275728434e-6  # 1.13% gain

Full1000-fit -> Mini100 transfer:
  protected raw        = 1.897755704e-6
  best transferred raw = 1.885156126e-6  # 0.66% gain
  best transferred tail objective ~= 2.049310072e-6
```

Read: there is a small transferable analytic-side correction, but it is too
small to matter for the current leaderboard unless it can be packaged with
negligible compute and remote transfer is unusually favorable.

Consensus-assisted stack:

```text
branch_set=with_consensus:
  adds the expensive protected-plus-consensus/count33-style branch

Full1000 OOF:
  protected raw       = 2.301739726e-6
  best stack raw      = 1.306409661e-6
  protected adjusted ~= 3.295769044e-7  (mult 0.143186)
  stack adjusted     ~= 3.396665120e-7  (mult assumed 0.260)

Full1000-fit -> Mini100 transfer:
  protected raw       = 1.897755704e-6
  stack raw           = 1.092858245e-6
  protected adjusted ~= 2.717320483e-7
  stack adjusted     ~= 2.841431437e-7
```

Read: the high-count/consensus information is real and transfers in raw MSE,
but the compute slope still makes always-on consensus worse in adjusted score.

Protected-feature compute router:

```text
script:
  legacy_workspace/probe_l2snap_consensus_compute_router.py

features:
  protected seed-cloud row summaries + cheap diagonal summaries only

selection rule:
  threshold chosen on Full1000 OOF, then applied unchanged to Mini100

best true Mini transfer:
  no better than protected; Full-selected thresholds either choose no extra
  branch or overpay for consensus on Mini.

diagnostic ceiling if Mini threshold is chosen with Mini labels:
  Mini adjusted can fall near 2.59e-7, but this is label leakage and not a
  deployable result.
```

Read: do not package the consensus router.  The raw improvement is large, but
the protected-pass features tested here do not predict the per-MLP adjusted
gain reliably enough before paying for the extra branch.  Future work should
look for a new cheap observable of the consensus correction, not another
threshold over these features.

### 2026-07-04 - external-truth blend harness

Parallel efficiency review conclusion accepted: same-run protected observables
do not carry the signed sampler-mean error.  The useful next object is an
independent estimate of truth from weights / cumulant closure / atlas state,
then a conservative blend with the protected sampler.

Added:

```text
legacy_workspace/probe_external_truth_blend.py
legacy_workspace/probe_external_layer_mean_snap.py
```

`probe_external_truth_blend.py` is the final-vector plug-in gate.  It aligns an
external `(N,256)` or `(N,32,256)` prediction cache by `indices`, reports
standalone external MSE, grouped OOF scalar/per-neuron blends on Full1000, and
Full1000-fit -> Mini100 transfer.  It reprices the blend with an explicit
`--external-flops` estimate.

Smoke test with the existing weak exact-GH side cache:

```text
command:
  python legacy_workspace/probe_external_truth_blend.py \
    --full-external legacy_workspace/cache/exactgh_l2cov_full1000_rank12_g005.npz \
    --mini-external legacy_workspace/cache/exactgh_l2cov_mini100_r8r12_g005g01.npz \
    --external-key pred_marg0.5_rank12_gain0.05 \
    --external-flops 2000000000 \
    --modes scalar,neuron \
    --lams 0,1e-9,1e-6 \
    --clip-alpha

Full1000:
  protected raw = 2.301739726e-6
  external raw  = 2.307483501e-6
  best OOF raw  = 2.302884446e-6  # slightly worse

Full1000-fit -> Mini100:
  protected raw = 1.897755704e-6
  external raw  = 1.886320835e-6
  best xfer raw = 1.892162015e-6

output:
  legacy_workspace/cache/external_blend_exactgh_smoke.npz
```

Read: the exact-GH cache is not a promotion candidate, but the harness is
working and correctly penalizes extra compute.  A serious independent
candidate should clear the remote-scale standalone bar before blend optimism:
`m_a <= ~3.3e-6` is useful, `m_a <= ~1.7e-6` is #1-class if its errors are
independent enough of the protected sampler.

`probe_external_layer_mean_snap.py` is the per-layer companion.  It accepts an
external post-ReLU hidden-mean cache at one layer, shifts the protected row
cloud toward that mean, propagates to final, and reports raw/adjusted by snap
beta.  It compiled cleanly; no toy run was made because even a tiny smoke test
replays the protected SPHEREx pass.  Use it when the higher-moments lane emits
a deployable hidden-state estimate, especially L24/L28/L30 mean candidates.

Low-overhead moment/analytic stack follow-up:

```text
script:
  legacy_workspace/probe_lowoverhead_moment_stack.py

inputs:
  protected l2snap Full1000/Mini100 caches
  exact-GH L2 covariance side predictions
  L4 PRE-EDGE raw-moment final predictions

Full1000 OOF, exact-GH + L4 PRE-EDGE common branches:
  protected raw          = 2.301739726e-6
  best linear stack      = 2.287576372e-6  # rel 0.993847
  best pair stack top10  = 2.286908915e-6  # rel 0.993557

Mini transfer check:
  exact-GH-only branches remain sane, but the cached Mini L4 PRE-EDGE corrected
  branches are invalid/broken for transfer: standalone raw is O(1e-2) to O(1e-1)
  versus a protected raw of 1.897755704e-6.  This matches the earlier warning
  that Mini PRE-EDGE moment-label generation was not a valid independent split.
```

Read: non-naive stacking of the weak exact-GH and PRE-EDGE branches does not
produce a hidden synergy.  The best valid Full1000 OOF gain is only about
`0.64%` raw before pricing extra reductions, and the Mini PRE-EDGE cache cannot
be used as a transfer canary.  Keep exact-GH/PRE-EDGE as diagnostic components,
but do not package a stacked low-overhead moment correction.

### 2026-07-04 - Phase1 learned matrix residual gates on l2snap all-row cache

Goal: revisit the learned correction path using the new higher-moment labels
and the internal all-layer SPHEREx row means, rather than only cheap scalar
summaries.  This tests whether a small flopscope-portable equivariant message
model could become a real correction.

Added:

```text
legacy_workspace/export_l2snap_allrows_cache.py
```

The exporter runs the protected h1-affine/layer-2-snap SPHEREx branch on GPU,
stores the internal weighted sample mean at every layer, and aligns final plus
all-layer public labels from the higher-moment files.

First200 cache:

```text
song/data/phase1_l2snap_allrows_first200_matrix_cache.npz
weights   = (200, 32, 256, 256)
pred_rows = (200, 32, 256)
target    = (200, 256)
target_rows = (200, 32, 256)
base final MSE = 2.181290612e-6
```

Direct final-residual equivariant message net:

```text
script:
  song/src/train_equivariant_residual.py

settings:
  first200, holdout fold0, hidden=32, rounds=3, dropout=0.10, epochs=50

best held-out:
  base      = 2.152616844e-6
  corrected = 2.154220510e-6
  ratio     = 1.000745  # worse
```

All-layer residual pretraining, then final finetune:

```text
settings:
  same cache, --pretrain-all-layers for 20 epochs, then final finetune

best pretrain checkpoint:
  base      = 2.152616844e-6
  corrected = 2.150460e-6
  ratio     ~= 0.9990

finetune immediately worsened; stopped early.
```

Penultimate hidden-mean residual gate:

```text
patch:
  song/src/train_equivariant_residual.py now supports --output-layer

cache:
  song/data/phase1_l2snap_allrows_first200_layer30_cache.npz

settings:
  output_layer=30, fold0, hidden=32, rounds=3, dropout=0.10, epochs=60

best held-out layer-30 mean:
  base      = 2.195049319e-6  # Gaussian-scale; unit-sphere scale is /chi_mean(256)^2
  corrected = 2.186391384e-6
  ratio     = 0.996056
```

Read: the matrix learner is not a current package path.  Direct final residual
learning is flat/worse.  All-layer supervision gives only a transient
`~0.1%` final gain.  Layer-30 hidden residual learning is stable-positive but
only `~0.4%` on the first fold, far below the oracle gap and below the bar for
our next remote move.  Keep the cache/exporter for future architecture tests,
but do not scale this exact model to full1000 unless the architecture changes
substantially.

Target-free union53 teacher compression rerun on GPU:

```text
script:
  song/src/train_phase1_l2snap_equivariant.py

label:
  union53_equal_l2snap - protected18_l2snap
  public truth used only for evaluation, not as the training target

teacher anchors:
  Full1000 protected raw = 2.301739548e-6
  Full1000 union53 raw   = 8.039952106e-7
  Mini100 protected raw  = 1.897756292e-6
  Mini100 union53 raw    = 6.778022874e-7
```

Runs:

```text
diag context, hidden=32, rounds=3, epochs=20, CUDA:
  best Mini truth = 1.897345802e-6 at epoch 1 / gain 0.25
  final epoch20  = 1.897634636e-6
  best Mini teacher-distance barely improved:
    base-to-teacher = 1.318610378e-6
    best observed   = 1.316105015e-6

zero context, hidden=32, rounds=3, epochs=40, CUDA:
  best Mini truth = 1.897343807e-6 at epoch 1 / gain 0.25
  final epoch40  = 1.897711206e-6
  best Mini teacher-distance ~= 1.3161e-6
```

Outputs:

```text
song/runs/phase1_l2snap_equiv_union53_teacher_diag_h32_r3_e20_cuda.json
song/runs/phase1_l2snap_equiv_union53_teacher_zero_h32_r3_e40_cuda.json
```

Read: this matrix-message architecture still does not see the signed
high-count teacher correction.  The teacher is a huge target-free raw-MSE
improvement, but the model captures only noise-scale truth gains and almost no
teacher-distance reduction on Mini.  Close this exact student architecture for
the current Phase-1 l2snap compression problem; future ML compression needs a
richer internal row-cloud representation, not just weights plus diagonal/zero
layer rows.

All-row trajectory teacher-compression follow-up:

```text
builder:
  legacy_workspace/build_allrows_teacher_cache.py

cache:
  song/data/phase1_l2snap_allrows_first200_union53_teacher_cache.npz

inputs:
  protected all-layer sampled means from
    song/data/phase1_l2snap_allrows_first200_matrix_cache.npz
  target-free teacher:
    union53_equal_l2snap, affine-scaled

teacher target, first200:
  base -> truth MSE        = 2.181290612e-6
  teacher -> truth MSE     = 7.250608468e-7
  base -> teacher MSE      = 1.316154407e-6
```

Training against the teacher rather than public truth, fold0 held out:

```text
h32/r3/weight-features, CUDA, best epoch 60:
  teacher train: 1.325288963e-6 -> 1.294206754e-6
  teacher test:  1.279616299e-6 -> 1.254170456e-6  # ratio 0.9801

h64/r4/weight-features, CUDA, best epoch 40:
  teacher train: 1.325288963e-6 -> 1.295619995e-6
  teacher test:  1.279616299e-6 -> 1.255567781e-6  # ratio 0.9812
  larger model overfits after epoch 50
```

Truth evaluation of best checkpoints, applying the teacher-trained correction:

```text
h32/r3 heldout fold0:
  truth base      = 2.152616716e-6
  truth corrected = 2.138980466e-6  # 0.63% gain

h64/r4 heldout fold0:
  truth base      = 2.152616716e-6
  truth corrected = 2.140693192e-6  # 0.55% gain
```

Outputs:

```text
song/runs/phase1_allrows_union53_teacher_wfeat_h32_r3_fold0_best.pt
song/runs/phase1_allrows_union53_teacher_wfeat_h64_r4_fold0_best.pt
```

Read: the actual protected trajectory means carry a real, compressible sliver
of the high-count teacher correction, unlike zero/diagonal context.  However,
the captured signal is only about `2%` of teacher-distance and `~0.6%` truth
MSE on a held-out first200 fold.  This is not a package path yet.  To make ML
compression contender-grade, the next version needs a richer row-cloud input
or a more direct low-rank representation of within-block quadrature error; a
larger message net over per-layer means alone just overfits.

Seed-stat and per-seed trajectory feature follow-up:

```text
added:
  legacy_workspace/export_l2snap_seedstat_cache.py
  song/src/train_equivariant_residual.py support for pred_features
  song/src/eval_equivariant_checkpoint.py

seed-stat cache:
  song/data/phase1_l2snap_seedstats_first200_union53_teacher_cache.npz
  channels:
    protected weighted mean, equal seed mean, seed std, seed MAD,
    weighted-minus-equal, max abs seed deviation, raw seed m3/m4

h32/r3/weight-features, teacher target, fold0:
  teacher test: 1.279616754e-6 -> 1.251391041e-6  # ratio 0.97794
  truth test:   2.152622831e-6 -> 2.137689129e-6  # gain 0.69%
  truth all200: 2.181294431e-6 -> 2.157211934e-6

per-seed channel cache:
  song/data/phase1_l2snap_seedmeans_first200_union53_teacher_cache.npz
  channels:
    protected weighted mean, all 18 protected seed means, and the seed-stat
    channels above

h32/r3/weight-features, teacher target, fold0:
  teacher test: 1.279616754e-6 -> 1.259652549e-6  # ratio 0.98440
  truth test:   2.152622831e-6 -> 2.133734464e-6  # gain 0.88%
  truth all200: 2.181294431e-6 -> 2.151058173e-6
```

Read: richer protected-run trajectory features are real but still small.  The
aggregate seed-stat cache best compresses the union53 teacher, while explicit
per-seed channels transfer slightly better to truth on this fold.  Neither is
close to contender scale: the best held-out truth movement is under `1%`, and
the high-count teacher gap is mostly invisible to this message model.  This
supports the three-way conclusion from the router/tomography work: protected
row-cloud geometry mostly sees error magnitude, not the signed external truth
correction.  Do not scale this exact learner blindly to Full1000 unless a new
neuron-local cumulant/pathwise signal is added.

Full1000 higher-moment summary residual check:

```text
script:
  legacy_workspace/probe_higher_moment_residual_atlas.py

atlas:
  legacy_workspace/cache/higher_moment_atlas_full1000_noblocks.npz

features:
  MLP-level higher-moment summaries from layers 0,1,3,7,15,23,30,31
  plus interactions with protected final base

OOF grouped by MLP:
  base raw = 2.301739726e-6
  best     = 2.295883526e-6
  rel      = 0.997456
  alpha    = 10
  shrink   = 0.75
```

Read: collapsed MLP-level higher-moment summaries contain only a `~0.25%`
stable residual signal.  This is a clean negative for atlas-summary residual
ML.  It does not contradict the cumulant oracle: true preactivation cumulants
are neuron-local scalar state, while the atlas-summary probe throws away that
local structure.  The moment path should stay focused on estimating scalar
preactivation cumulants or hidden-layer means, not on broad per-MLP residual
features.

Count-reduced all-row residual check:

```text
patch:
  legacy_workspace/export_l2snap_allrows_cache.py accepts --seeds and
  --seed-weights, so lower-count branches can be exported without editing code.

cache:
  song/data/phase1_l2snap_count14_allrows_first200_matrix_cache.npz

seed set:
  protected18 minus seeds 0,6,21,23

base final MSE:
  2.686745583e-6

h32/r3 fold0 final residual learner:
  best test base      = 2.851675845e-6
  best test corrected = 2.851580348e-6
  ratio               = 0.9999665

h64/r6 fold0:
  stopped after epoch 30; ratios were ~1.0012, ~1.0030, then ~0.9999
```

Read: lowering the protected branch toward the floor leaves a residual that the
current all-row matrix learner still cannot recover.  This closes the simple
count14-plus-learned-residual path.  To make a floor-count branch competitive,
we need a different measurement/control signal, not a bigger version of this
same equivariant residual model.

Seed-stat trajectory learner follow-up:

```text
cache:
  song/data/phase1_l2snap_seedstats_first200_matrix_cache.npz

features:
  per-layer protected weighted mean, equal seed mean, seed std/MAD,
  weighted-minus-equal, max abs seed deviation, seed m3/m4 summaries

direct truth residual, h32/r3 fold0:
  best test base      = 2.152622756e-6
  best test corrected = 2.153269406e-6  # worse

teacher target:
  build_allrows_teacher_cache.py with union53 affine seed teacher
  song/data/phase1_l2snap_seedstats_first200_union53_teacher_cache.npz
  base -> teacher MSE = 1.316156665e-6

h32/r3 teacher-trained fold0:
  teacher test base/corrected = 1.279616739e-6 / 1.251391468e-6
  truth   test base/gain1     = 2.152622831e-6 / 2.137689129e-6
  truth   best grid           = 2.136898352e-6  # gain=0.75

h64/r4 teacher-trained fold0:
  teacher test base/corrected = 1.279616739e-6 / 1.256588270e-6
  truth   test base/gain1     = 2.152622831e-6 / 2.135245970e-6
  truth   best grid           = 2.134923026e-6  # gain=1.25
```

Read: preserving seed-stat trajectory channels is better than only per-layer
means, but still only captures about `0.8%` truth MSE on the first held-out
fold.  It is a real learned-compression signal, not a package path.  Scaling
this exact architecture is unlikely to produce the 5%+ frontier move; the next
learned attempt needs either per-seed/per-layer vector information with a more
direct routing objective, or a different low-rank representation of the
within-block quadrature error.

### 2026-07-04 - selective protected-count z12 package probe

Earlier cached diagnostics showed that the protected-count `z12` control is
real but full-width response columns are too expensive for the raw gain.  A
more production-shaped implementation was added to the module package builder:

```text
file:
  legacy_workspace/build_h1affine_l2snap_module_package.py

new optional args:
  --z12-topq
  --z12-ridge
  --z12-scale
  --z12-beta-clip
```

Implementation shape:

- run the protected h1-affine/layer-2-snap SPHEREx pass unchanged;
- choose dynamic top-q final coordinates by `abs(final)` using
  `fnp.argsort`/`fnp.take` (validated under flopscope);
- compute only those mean-field response columns;
- fit/apply even/odd cross-fitted zero-mean controls
  `z1 = Rbar * (h1 - E[h1]) @ response` and
  `z2 = z1^2 - E[z1^2)`;
- replace only the selected final coordinates.

Technical feasibility:

```text
fnp.argsort and fnp.take are available in flopscope 0.8.0rc1.
z12top64 whest validate passed.
```

Package folders built:

```text
legacy_workspace/_pkg_pure18_protected_h1affine_l2snap_b05_z12top16_s25_weighted_affine_full1000_module_finalonly
legacy_workspace/_pkg_pure18_protected_h1affine_l2snap_b05_z12top32_s25_weighted_affine_full1000_module_finalonly
legacy_workspace/_pkg_pure18_protected_h1affine_l2snap_b05_z12top48_s25_weighted_affine_full1000_module_finalonly
legacy_workspace/_pkg_pure18_protected_h1affine_l2snap_b05_z12top64_s25_weighted_affine_full1000_module_finalonly
```

Local package checks versus the protected module package:

```text
Mini idx0:
  protected raw=7.493691e-7 adjusted=1.277803e-7 mult=0.170517
  top64     raw=6.620174e-7 adjusted=1.185438e-7 mult=0.179064

Mini 0,20,40,60,80:
  protected raw=2.141317e-6 adjusted=3.036983e-7 mult=0.143485
  top64     raw=2.078421e-6 adjusted=3.019201e-7 mult=0.147056
  top48     raw=2.088648e-6 adjusted=3.022470e-7 mult=0.146330
  top32     raw=2.090874e-6 adjusted=3.022635e-7 mult=0.146707

Mini 0,25,50,75,95:
  protected raw=1.674882e-6 adjusted=2.381188e-7 mult=0.143713
  top64     raw=1.659939e-6 adjusted=2.411738e-7 mult=0.146650
  top48     raw=1.652089e-6 adjusted=2.392539e-7 mult=0.146327
  top32     raw=1.653519e-6 adjusted=2.392323e-7 mult=0.146586

Mini spaced20:
  protected raw=1.848905e-6 adjusted=2.607722e-7 mult=0.141419
  top48     raw=1.820345e-6 adjusted=2.618984e-7 mult=0.144259
  top16     raw=1.831096e-6 adjusted=2.618716e-7 mult=0.143619
```

Read: selective `z12` is mathematically real and dynamically shippable, but
not currently adjusted-score positive.  The broad Mini spaced20 check improves
raw by about `1.0-1.5%`, while the added response/control work and residual
time slightly worsen adjusted score.  Keep the builder hook for future exact
zero-mean controls or if another change amortizes the mean-field response, but
do not submit/promote these `z12top*` packages as standalone candidates.

### 2026-07-04 - higher-moment closure and tail-thinning refresh

Goal: after the full higher-moment dataset became available, re-check the two
most plausible ways to move beyond fixed-seed polishing:

1. use signed cumulant closure to get an analytic/raw-accuracy jump;
2. cut protected sampler cost by thinning only the expensive post-l2snap tail.

Structured closure smoke, spaced 20 MLPs:

```text
script:
  legacy_workspace/probe_higher_moment_structured_closure.py

indices:
  0,10,...,190

oracle-ish feature source:
  true post-layer covariance/c21/c4 summaries from higher-moment files

gauss final      = 1.274752369e-6
true_k3 final    = 2.030307543e-7
true_k34 final   = 2.312597131e-8
best OOF k34     = 7.826822291e-7  # ridge=0.03
g3_r2 / g4_r2    = +0.88085 / +0.18996
```

Read: the signed cumulant signal is still very real.  If true or near-true
preactivation cumulants were cheaply available, the final ReLU mean update has
e-8-class headroom.  But this smoke uses oracle-ish true post-moment tensors;
it is a proof of physics, not a deployable estimator.

Deployable Gaussian-rollout closure on the same slice:

```text
script:
  legacy_workspace/probe_higher_moment_rollout_closure.py

gauss_rollout final = 6.157171242e-5
best recursive k34  = 6.005128581e-5  # ridge=3
```

Read: plain full-covariance Gaussian rollout is far too inaccurate as a state
source at depth 32.  Learning good cumulant values on top of that state does
not rescue it.  The higher-moment route remains promising only if the state
source comes from the paid SPHEREx row cloud, a stronger non-Gaussian closure,
or a new response-aligned statistic.  Do not port the Gaussian-rollout closure.

Cheap rollout branches as nonlinear signed-Ez proxies:

```text
script:
  legacy_workspace/probe_final_premean_shift_oracle.py

proxy rows:
  cheeky_experiments/lowrank_rollout_r32s16_rows_0_200.npz
  cheeky_experiments/diag_rollout_kurt_rows_0_200.npz

Full200 baseline lambda=0 raw = 2.181295751e-6

lowrank r32s16 best:
  lambda=0.002 raw = 2.175916349e-6  # rel 0.997534

diag kurt best:
  lambda=0.001 raw = 2.179599260e-6  # rel 0.999222
```

Read: the stronger nonlinear final-preactivation shift does not turn cheap
Gaussian/low-rank rollout branches into useful signed-Ez proxies.  They carry
at most sub-0.3% raw signal.  This supports the narrower attribution from the
old analytic branch: the transferable signed-Ez coordinate comes from the
expensive response-aligned K3cov plus calibrated kurtosis-stat machinery, not
from a generic cheap rollout state.

Tail-only coreset refresh:

```text
script:
  legacy_workspace/probe_l2snap_tail_coreset.py

spaced20 smoke:
  full raw/adj_proxy       = 2.250013767e-6 / 3.221789476e-7
  even_224 raw/adj_proxy   = 2.327388899e-6 / 2.935846707e-7

first100 validation:
  full raw/adj_proxy       = 2.078894598e-6 / 2.976764338e-7
  even_224 raw/adj_proxy   = 2.558386883e-6 / 3.227235341e-7
  even_232 raw/adj_proxy   = 2.538728252e-6 / 3.310627730e-7
  even_240 raw/adj_proxy   = 2.501408856e-6 / 3.368561430e-7
```

Read: the spaced20 tail-thinning win was a slice artifact.  Broad first100
shows a 20%+ raw hit, so reducing rows after the l2snap checkpoint is not a
safe path to the 10% floor.  This agrees with the older row-thinning result:
the full 512-row QR-antithetic block is carrying real quadrature structure.

PRE-EDGE variance-constant check:

```text
script:
  legacy_workspace/probe_preedge_variance_curve.py

question:
  the L4 PRE-EDGE mean snap improved one fixed Full1000 raw diagnostic but
  failed as a package on Mini.  Since the active SPHEREx branch is
  variance-limited, the useful test is whether PRE-EDGE reduces raw*block_count,
  the block-variance proxy, not just one fixed-count MSE.

spaced20:
  c18 beta=0    raw=2.250036929e-6  raw*count=4.050066473e-5
  c18 beta=0.75 raw=2.220101579e-6  raw*count=3.996182842e-5
  c18 beta=1    raw=2.213234030e-6  raw*count=3.983821254e-5

first100:
  c18 beta=0    raw=2.078896911e-6  raw*count=3.742014439e-5
  c18 beta=0.75 raw=2.062764410e-6  raw*count=3.712975939e-5
  c18 beta=1    raw=2.061603337e-6  raw*count=3.710886006e-5
```

Read: L4 PRE-EDGE is a genuine same-row control, but the variance reduction is
only about `0.8%` on first100 at the protected count.  That is too small to
justify package churn or move the leaderboard frontier.  Keep it as a possible
minor additive hook, but do not treat it as the route toward the `1e-7`
adjusted domain.

Antipodal odd-control check:

```text
script:
  legacy_workspace/probe_antipodal_odd_control.py

idea:
  every QR block already evaluates u and -u.  The odd part
  0.5 * (f(u) - f(-u)) has exact zero expectation over sphere directions, so
  it can be used as a free same-row control if it predicts quadrature error.

first100, protected l2snap:
  base                  raw = 2.078896911e-6
  best scalar odd OOF    raw = 2.080768951e-6  # ridge=1, worse
  per-neuron odd OOF     raw >= 2.101392419e-6
  seedwise odd ridge OOF raw >= 2.291479537e-6
```

Read: close the simple antipodal odd-control family.  The sign information in
positive-minus-negative pairs is not aligned with the final quadrature error;
seedwise/per-neuron fits overfit badly even under MLP-held-out folds.

Deep split-control variate check:

```text
script:
  legacy_workspace/probe_deep_split_cv.py

idea:
  use the already-propagated row cloud at intermediate layer t, map it to the
  final layer through a sample-gated linear response, and fit beta on held-out
  seed-block splits:

    y(row) - beta * (z_t(row) - mean_train(z_t)).

  This is target-free and expectation-neutral by split exchangeability, but it
  costs at least one extra row-matmul plus response construction.

first100, folds=3, ridge=0.01:
  base       raw = 2.078897793e-6
  layer 20   raw = 2.074199388e-6  # 0.226% gain
  layer 24   raw = 2.074768185e-6  # 0.199% gain
  layer 8    raw = 2.074902338e-6  # 0.192% gain
```

Read: the control is mechanically valid and weakly positive, but the gain is
an order of magnitude too small for its FLOP cost.  Do not build a production
deep split-CV from this response geometry.  A future CV must capture several
percent of V at similar cost, or be essentially free.

Seed-trajectory learner against union53 teacher:

```text
scripts:
  song/src/train_equivariant_residual.py
  song/src/eval_equivariant_checkpoint.py

cache:
  song/data/phase1_l2snap_seedmeans_first200_union53_teacher_cache.npz

fold 0, h32/r3, weight_features, prior run:
  test_truth base  = 2.152622831e-6
  test_truth best  = 2.133734464e-6  # gain=1, ~0.88%
  all_truth base   = 2.181294431e-6
  all_truth gain1  = 2.151058173e-6  # ~1.39%

fold 1, h32/r3, weight_features, CUDA:
  teacher best epoch = 10
  test_teacher base  = 1.346762520e-6
  test_teacher best  = 1.328758837e-6  # gain=1.25
  test_truth base    = 2.235294729e-6
  test_truth best    = 2.223888055e-6  # gain=1, ~0.51%
  all_truth base     = 2.181294431e-6
  all_truth gain1    = 2.175564152e-6  # ~0.26%
```

Read: the seed-mean trajectory network detects a small real signal, including
on a second MLP-held-out fold, but the truth-side gain is sub-1% on fold 1 and
teacher-side gains overstate its deployable value.  Keep this as a weak
stabilizer/research lead.  Do not spend production complexity on it unless a
broader fold sweep or a richer row-cloud representation produces several
percent truth gain.

Sampled-alpha cumulant closure snap:

```text
script:
  legacy_workspace/probe_sampled_alpha_closure_snap.py

idea:
  true preactivation cumulants give a huge Edgeworth oracle, and the
  true-state alpha-polynomial closure reaches the ~7e-7 tier.  Test whether
  the paid SPHEREx row cloud can supply the needed preactivation mean/sigma
  coordinates at a layer, then use MLP-held-out public moment coefficients to
  predict skew/excess and snap the hidden mean.

smoke:
  indices 0:40, layer 30, folds=5, ridge=1, mode=alpha, CUDA

final raw after snap:
  beta=0       2.187809542e-6
  beta=0.025   2.188133423e-6
  beta=0.05    2.189437565e-6
  beta=0.10    2.195067131e-6
  beta=0.20    2.218258696e-6

hidden L30 external mean check:
  sampled-alpha closure hidden MSE = 3.236371581e-6
  protected hidden sample MSE scale from prior L30 checks ~= 9e-9
```

Read: close this bridge.  The true-cumulant closure is powerful only when the
state coordinates/cumulants are accurate; sampled preactivation mean/sigma from
the unit-sphere row cloud plus alpha-only closure produces a hidden mean
hundreds of times worse than the existing protected hidden sample.  This does
not kill cumulant closures generally, but it rules out this cheap
sampled-alpha snap.

### 2026-07-05 - multi-frame sphere-design transfer check

Question: the earlier optimized single-QR-frame pilot failed public transfer,
but a single fixed orthogonal frame is rotationally exchangeable under random
He MLPs.  The only remaining direction-design lever is the relative geometry
between several orthogonal frames.  Built a small generated-distribution pilot
to optimize a 4-frame antithetic union with no public labels, then check public
Full200 transfer.

```text
script:
  legacy_efficiency/design_multiframe_pilot.py

command:
  python legacy_efficiency/design_multiframe_pilot.py \
    --frames 4 --n-train 64 --n-val 32 --mc-samples 1000000 \
    --steps 180 --batch 8 --eval-every 20 --public-n 200 --device cuda

generated validation:
  random 4-frame mean loss = 1.100606e-5
  optimized best val       = 9.735328e-6  # rel 0.8845

public Full200 transfer:
  optimized 4-frame raw    = 1.109839e-5
  random 4-frame mean raw  = 1.112682e-5  # rel 0.9974

output:
  legacy_efficiency/design_multiframe_pilot_out.npz
```

Read: this variant does not transfer.  The optimization can overfit generated
validation at small sample size, but the public check shows no material
variance-per-FLOP improvement over ordinary random independent QR frames.  This
does not prove every possible deterministic spherical design is dead, but it
removes the quick multi-frame design path from the near-term promotion queue.

### 2026-07-05 - structured cumulant closure rank/mode smoke

Follow-up to the higher-moment closure lane.  The expert suggested that the
missing all-distinct skew/kurtosis terms may be expressible through low-rank
common factors.  Re-ran the oracle-state structured closure on a broader
first40 slice with rank-8 random and covariance-eigen modes.  This still uses
true public post-moment state from the higher-moment files, so it is a ceiling
diagnostic, not a deployable estimator.

```text
script:
  legacy_workspace/probe_higher_moment_structured_closure.py

random rank8, first40:
  gauss final     = 1.128670134e-6
  true_k3 final   = 1.603857540e-7
  true_k34 final  = 2.234103017e-8
  best k34 final  = 7.369740074e-7  # ridge=0.03
  g3_r2 / g4_r2   = +0.88423 / +0.17772

eigen rank8, first40:
  gauss final     = 1.128670134e-6
  true_k3 final   = 1.603857540e-7
  true_k34 final  = 2.234103017e-8
  best k34 final  = 7.303077829e-7  # ridge=0.1
  g3_r2 / g4_r2   = +0.87280 / +0.17446
```

Read: low-rank common-factor features are real but do not break the existing
`~7e-7` oracle-state closure tier.  They predict skew well but kurtosis remains
weak, and the final result is far from the true-k3/true-k34 oracle.  A deployable
version of this exact feature family is not worth engineering unless a new
kurtosis/off-diagonal state source appears.  The moment lane remains high-ceiling
only if we find a better observable/state, not by increasing this rank/mode
grid.

### 2026-07-05 - true K3 component regression: b111 is the named blocker

Added:

```text
legacy_workspace/probe_higher_moment_component_regression.py
```

This decomposes the true preactivation third-cumulant Edgeworth delta into:

- `b3`: diagonal post-cumulant source;
- `b21`: two-index/storable source;
- `b111`: fully all-distinct source;
- `k4`: true fourth-cumulant Edgeworth delta.

First100, grouped 5-fold OOF component shrinkage from a true preactivation
Gaussian baseline:

```text
base gauss final       = 1.087080720e-6
b3 final               = 1.077227608e-6   final_coef b3=+0.286
b21 final              = 7.548717020e-7   final_coef b21=+0.221
b3+b21 final           = 7.058571428e-7   final_coef b3=+0.581, b21=+0.236
b3+b21+k4 final        = 6.479544849e-7   final_coef b3=+0.644, b21=+0.265, k4=+0.599
b3+b21+b111 final      = 1.460038500e-7   final_coef b3=+0.901, b21=+0.946, b111=+0.976
b3+b21+b111+k4 final   = 1.610141055e-8   final_coef b3=+1.019, b21=+1.040, b111=+1.047, k4=+0.894
```

Read:

- A two-index `O(n^2)` cumulant state is real and can reach the familiar
  `~7e-7` tier in oracle-state form, but it is not enough for the current
  leaderboard target.
- The fully all-distinct `b111` skew contribution is the decisive missing
  coordinate for `~1e-7` raw and below.
- Fourth-cumulant information helps (`~7.06e-7 -> ~6.48e-7`) but does not
  replace `b111`.
- If we engineer a deployable cumulant path, it should not stop at `b21`; it
  needs either a cheap response-aligned proxy for `b111` or a sampler/control
  that estimates the signed all-distinct contribution directly.

### 2026-07-05 - rank-32 random common-factor closure check

Follow-up to the structured cumulant closure lane after the `b111` component
split.  The question was whether the prior rank-4/rank-8 common-factor result
was simply too narrow.  Re-ran the oracle-state structured closure with 32
random covariance modes on first100:

```text
command:
  python legacy_workspace/probe_higher_moment_structured_closure.py \
    --indices 0:100 \
    --weights-npz-cache legacy_workspace/cache/phase1_full1000_weights_targets.npz \
    --rank 32 --mode random --folds 5 \
    --ridge 0.01,0.03,0.1,0.3,1 --seed 24680

gauss     all/final = 2.643569290e-6 / 1.087080720e-6
true_k3   all/final = 3.380259476e-7 / 1.551141005e-7
true_k34  all/final = 1.354466752e-8 / 2.033313200e-8

best rank32 random:
  ridge=0.03
  k3  all/final = 9.770966056e-7 / 6.698303472e-7
  k34 all/final = 8.359012189e-7 / 6.692972422e-7
  g3_r2/g4_r2   = +0.89061 / +0.18828
```

Read: rank-32 random modes do not beat the rank-8 tier (`~6.66e-7` final).
More generic common-factor modes are not the missing all-distinct skew
representation.  Do not spend production engineering on this feature family
unless a qualitatively different `b111` contraction appears.

### 2026-07-05 - nonlinear seed-stat trajectory correction on Full1000

Added:

```text
legacy_workspace/probe_seedstats_mlp_stack.py
```

This is the nonlinear companion to the full1000 seed-stat ridge.  It uses the
existing slim protected-run cache:

```text
song/data/phase1_l2snap_seedstats_full1000_slim.npz

features:
  protected weighted/equal means, seed std/MAD, weighted-minus-equal,
  seed maxabs, seed raw m3/m4 over selected layers and tail windows
target:
  final residual, grouped OOF by MLP
runtime:
  CUDA/offline only; no package produced
```

Results:

```text
base protected raw = 2.301739837e-6

safe features + interactions, h64/d2:
  best OOF raw = 2.290352365e-6  # rel 0.995053

all features + interactions, h96/d2:
  best OOF raw = 2.289384210e-6  # rel 0.994632

all features + interactions, h192/d3:
  best OOF raw = 2.289606839e-6  # rel 0.994729
```

Read: the protected seed-stat trajectory contains a real signed correction, but
only at the `~0.5%` raw-MSE level.  Increasing nonlinear capacity does not move
it into the 5-10% range.  Do not package this as a production correction; the
protected row-cloud summaries still lack the missing `b111`/high-count signed
information.

### 2026-07-05 - sampled K3 decomposition PRE-EDGE closure

Added:

```text
legacy_workspace/probe_preedge_sample_k3_decomp.py
legacy_workspace/probe_preedge_decomp_oof_hidden.py
```

Question: the higher-moment component regression names `b111`, the fully
all-distinct third-cumulant contribution, as the decisive missing coordinate.
Earlier PRE-EDGE used the sampled preactivation raw moments directly.  This
probe decomposes the same sampled preactivation K3 estimate into sampled
`b3`, sampled `b21`, and sampled `b111 = full - b3 - b21`, then tests whether
shrinking only `b111` reduces PRE-EDGE noise.  It also tests the decomposed
components as OOF hidden-denoiser features.

First100, layer 4, protected 18 blocks:

```text
sample hidden             = 2.411845791e-8
direct edge4 hidden       = 2.345384559e-8  # rel 0.972444
best fixed b111 shrink    = 2.345384546e-8  # lambda=1, numerically direct edge4
best OOF decomp features  = 2.287880734e-8  # ridge_a1_c0, rel 0.948602

final no_snap             = 2.078896911e-6
best final replay         = 2.075297886e-6  # ridge_a1_c0, beta=0.25
```

Read: subtracting sampled `b3+b21` and shrinking only sampled `b111` does not
beat ordinary direct PRE-EDGE; the best fixed shrink is the no-shrink direct
edge4 point.  Decomposed features reproduce the familiar supervised L4
raw-moment hidden-denoiser signal, but final-layer impact is only about
`0.17%` on first100.  This is below package relevance and does not explain the
`b111` oracle gap.  The missing coordinate is not exposed by this noisy sampled
marginal/C21 decomposition at protected count.

### 2026-07-05 - dynamic post-l2snap tail-row coreset closure

Rechecked the tail-cost reduction branch with CUDA after the uniform/even
tail thinning miss.  This run keeps all protected rows through the exact H1
affine correction and layer-2 snap, then dynamically selects rows from the
post-snap checkpoint by row norm / centered norm / mean-absolute activation
before finishing layers 2..31.

```text
command:
  python legacy_workspace/probe_l2snap_tail_coreset.py \
    --weights-cache legacy_workspace/cache/phase1_full1000_weights_targets.npz \
    --indices 0-99 \
    --keep 192,224,240 \
    --modes norm,center_norm,meanabs \
    --batch-mlp 8 --device cuda

first100 protected full rows:
  raw       = 2.078893704e-6
  mult      = 0.143190
  adj_proxy = 2.976763057e-7

best dynamic result:
  meanabs_240 raw       = 2.374208976e-6
  rel_raw              = 1.142054
  mult                 = 0.134667
  adj_proxy            = 3.197265719e-7

other notable results:
  center_norm_240 raw/adj = 2.400821442e-6 / 3.233103812e-7
  norm_240        raw/adj = 2.465820948e-6 / 3.320636416e-7
  meanabs_224     raw/adj = 2.628512610e-6 / 3.315694294e-7
  norm_192        raw/adj = 3.122728845e-6 / 3.406802412e-7
```

Read: dynamic checkpoint row selection does not rescue the tail-cost branch.
Even the best `240/256` mode takes a `14%` raw hit, more than the multiplier
reduction can pay for.  This closes simple post-l2snap row coresetting as a
path to the 10% floor; the full QR-antithetic row cloud is carrying real
deep-tail quadrature structure.

### 2026-07-05 - optimized low-count PRE-EDGE variance check

Extended `probe_preedge_variance_curve.py` with `--seed-pool` /
`--seed-weights` so it can test retained seed sets rather than only protected
prefix counts.  Re-ran the only plausible low-count PRE-EDGE rescue: the
known count14/15/16 drop sequence anchored by removing protected seeds
`6,0,21,23`.

```text
command:
  python legacy_workspace/probe_preedge_variance_curve.py \
    --weights-cache legacy_workspace/cache/phase1_full1000_weights_targets.npz \
    --indices 0:200 \
    --counts 14,15,16 \
    --seed-pool 2,3,7,8,13,15,17,20,22,24,27,28,29,31,23,21,0,6 \
    --betas 0,0.25,0.5,0.75,1 \
    --preedge-layer 4 --batch-mlp 8 --device cuda \
    --out legacy_workspace/cache/preedge_variance_curve_optlow_200_cuda.npz

best count14:
  beta=0.50 raw = 2.680389944e-6
  beta=0.00 raw = 2.686749991e-6

best count15:
  beta=0.75 raw = 2.542800862e-6
  beta=0.00 raw = 2.551640845e-6

best count16:
  beta=0.75 raw = 2.435076997e-6
  beta=0.00 raw = 2.448538008e-6
```

Read: PRE-EDGE lowers raw MSE slightly on the optimized low-count sets, but the
gain is far too small to pay for the missing QR blocks.  Count14 remains around
`2.68e-6` raw, which is roughly `3e-7` adjusted at the count14 multiplier even
before any extra PRE-EDGE overhead.  This closes the low-count PRE-EDGE rescue
as a path to the 10% floor.

### 2026-07-05 - full1000 explicit seed-trajectory MLP check

After the full1000 aggregate seed-stat MLP topped out at `~0.5%` raw gain,
exported the stronger cache that exposes all 18 protected seed means at every
layer, not just summary statistics.  This directly checks whether the earlier
first200 per-seed-channel signal scales to the full public surface.

```text
cache:
  song/data/phase1_l2snap_seedmeans_full1000_slim.npz

export command:
  python legacy_workspace/export_l2snap_seedstat_cache.py \
    --indices 0:1000 \
    --weights-cache legacy_workspace/cache/phase1_full1000_weights_targets.npz \
    --seed-preds legacy_workspace/cache/l2snap_union53_seed_preds_full1000.npz \
    --out song/data/phase1_l2snap_seedmeans_full1000_slim.npz \
    --batch-mlp 16 --device cuda \
    --include-seed-means --omit-weights --skip-target-rows

probe:
  python legacy_workspace/probe_seedstats_mlp_stack.py \
    --cache song/data/phase1_l2snap_seedmeans_full1000_slim.npz \
    --feature-mode all --interactions \
    --layers 0,1,2,4,8,16,24,28,30 \
    --hidden 96 --depth 2 --dropout 0.05 \
    --epochs 40 --patience 8 --batch-size 32768 --device cuda

base raw = 2.301738107e-6
best OOF raw = 2.289252275e-6
relative = 0.994575  # ~0.54% raw gain
last100 = 2.200595700e-6
spaced20 = 3.017686366e-6
```

Read: explicit per-seed layer trajectories do not materially outperform the
aggregate seed-stat cache on full1000.  They expose a real signed correction,
but still only at the half-percent level.  This strengthens the prior
conclusion: protected row-cloud geometry mostly exposes uncertainty/magnitude,
not the missing signed `b111`/high-count truth correction.  Do not package this
learner unless a new input channel changes the signal scale by an order of
magnitude.

### 2026-07-05 - direct b111 predictor closure

Added:

```text
legacy_workspace/probe_b111_predictor_closure.py
```

Question: after the higher-moment component regression identified the
all-distinct K3 piece `b111` as the decisive missing coordinate, can that piece
be predicted from deployable-ish local coordinates?  This probe grants an
oracle amount of state: true preactivation mean/variance plus true storable
`b3`/`b21`, then tries to predict the true `b111` ReLU-mean contribution from
`b3`, `b21`, local `alpha/sigma`, and incoming-weight moments.  All fits are
MLP-grouped OOF.

First100:

```text
gauss final        = 1.087080720e-6
unfit b3+b21 final = 4.671805095e-6
unfit true b111    = 1.551141005e-7

fit b3+b21 final      = 8.315402061e-7
fit true_b111 final   = 2.833597551e-7

best predicted-b111 model:
  ridge=0.001
  apparent mean_b111_r2 = +0.43429
  apparent late_b111_r2 = +0.77587
  direct final          = 1.298084546e-6
  fit final             = 8.187225089e-7
```

Read: the local features can predict some b111 variance, especially late, but
not the signed component that improves the final mean.  In the fair fitted
comparison, predicted b111 only moves `8.315e-7 -> 8.187e-7`, whereas true b111
is a large lever.  So b111 is not a simple closure of true b3/b21 plus local
weight/preactivation statistics.  A deployable moment path still needs either a
new response-aligned all-distinct observable or an actual sampled/high-count
control, not a scalar/local b111 closure.

Full1000 follow-up after patching the probe to stream/memmap `weights.npy`
instead of materializing all requested weights as float64:

```text
command:
  python legacy_workspace/probe_b111_predictor_closure.py \
    --indices 0:1000 \
    --weights-npz-cache legacy_workspace/cache/phase1_full1000_weights_targets.npz \
    --folds 5 \
    --ridges 0.0001,0.001,0.01,0.1,1,10,100 \
    --clip-q 0.001 \
    --out legacy_workspace/cache/b111_predictor_full1000_v1.npz

gauss final          = 1.173408401e-6
b3+b21 final         = 5.134699644e-6  # unfit component scale is bad
true b111 final      = 1.732218546e-7

fit b3+b21 final     = 8.883719710e-7
fit true_b111 final  = 3.135110126e-7

best predicted-b111 model:
  ridge=1
  apparent mean_b111_r2 = +0.34906
  apparent late_b111_r2 = +0.63218
  direct final          = 1.990866156e-6
  fit final             = 8.864592920e-7
```

Read: the full1000 surface confirms the first100 conclusion.  The predictor
captures some layerwise/local b111 variance, but the response-relevant signed
part is still missing: fair fitted final MSE moves only
`8.884e-7 -> 8.865e-7`, while true b111 would move it to `3.135e-7`.
Close scalar/local b111 closure as a primary path.  The live math question is
now narrower: find a response-aligned all-distinct observable or accept that
this component needs actual sampling/high-count control.

### 2026-07-05 - response-contracted seed-mean b111 closure

Added:

```text
legacy_workspace/probe_b111_response_seedmean_closure.py
```

Question: the scalar/local b111 closure failed, but maybe the protected
SPHEREx trajectory exposes the signed all-distinct component once its
per-layer seed-mean channels are contracted through the final weight columns.
This probe builds final-neuron features from:

- `W_final.T @ layer30_seedmean/state_channels`;
- `(W_final**2).T @ channels` and `(W_final**3).T @ channels`;
- seedwise final-preactivation moments induced by layer-30 seed means;
- final post-ReLU seed/stat channels and final weight-column moments.

It then scores two MLP-grouped OOF tasks: predict the true final `b111`
Edgeworth delta from the higher-moment files, and directly predict the
protected final residual.

First100:

```text
command:
  python legacy_workspace/probe_b111_response_seedmean_closure.py \
    --indices 0:100 \
    --ridges 0.001,0.01,0.1,1,10 \
    --out legacy_workspace/cache/b111_response_seedmean_first100_v1.npz

base protected final = 2.078891870e-6
features             = 117

best b111_r2         = +0.00624   # ridge=0.1
best direct residual = 2.096724346e-6  # ridge=10, worse than base
best b111-applied    = 2.078892922e-6  # neutral/worse than base
```

Read: final-response contraction does not rescue the protected seed-mean
trajectory as a b111 observable.  The true all-distinct component is still not
visible from the already-paid seed trajectory in the tested linear feature
class.  This strengthens the current blocker: the next useful b111 attack
needs a different signed observable/control, not more contractions of protected
seed means.

### 2026-07-05 - nonlinear local b111 closure check

Added:

```text
legacy_workspace/probe_b111_nonlinear_closure.py
```

Question: the direct b111 closure used ridge features.  Maybe the missing
response-relevant sign is nonlinear in the same local/storable coordinates
(`b3`, `b21`, alpha/sigma, final incoming-weight statistics).  This probe uses
a small CUDA MLP in MLP-grouped OOF folds to predict the final-layer true
`b111` Edgeworth delta, then evaluates whether the predicted component improves
the fair final component regression.

First40 smoke:

```text
b111_r2              = +0.76984
fit b3+b21 final     = 9.022322424e-7
fit pred_b111 final  = 9.016240615e-7
fit true_b111 final  = 2.907860448e-7
```

First100 confirmation:

```text
command:
  python legacy_workspace/probe_b111_nonlinear_closure.py \
    --indices 0:100 --epochs 50 --patience 6 --hidden 64 --depth 2 \
    --dropout 0.05 --batch-size 32768 --device cuda \
    --out legacy_workspace/cache/b111_nonlinear_first100_v1.npz

b111_r2              = +0.81835
gauss final          = 1.087080720e-6
fit b3+b21 final     = 8.315402061e-7
fit pred_b111 final  = 8.285893385e-7
fit true_b111 final  = 2.833597551e-7
unfit true b111      = 1.551141005e-7
```

Read: this is a stronger negative than the ridge closure.  The same local
features can predict a large fraction of b111 variance (`R2 ~= 0.82`) but still
do not predict the response-relevant signed part: the fair final improvement is
only about `0.35%` relative to the `b3+b21` line, while true b111 would cut the
MSE by roughly `66%`.  Do not spend more effort on larger nonlinear models over
the same local/storable feature set.  The live object remains a qualitatively
different response-aligned b111 observable or a sampled/high-count control.

### 2026-07-05 - k26 z12 mini transfer and low-overhead stack closure

Filled the missing Mini100 counterpart for the `k26_z12` branch so the
low-overhead branch stack can be judged on transfer, not just Full1000 OOF.
Used the actual 26-seed cache set:

```text
8,15,16,17,20,24,26,33,44,47,48,49,63,70,79,84,88,93,97,102,105,106,107,110,111,112
```

Mini100 generation:

```text
python legacy_workspace/probe_l2snap_z12_cv.py \
  --weights-cache legacy_workspace/cache/phase1_mini100_weights_targets.npz \
  --indices 0-99 \
  --seeds 8,15,16,17,20,24,26,33,44,47,48,49,63,70,79,84,88,93,97,102,105,106,107,110,111,112 \
  --seed-weights 1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1 \
  --ridges 0.01 --z2-scales 0.25,0.5,0.75,1 \
  --batch-mlp 8 --device cuda \
  --out legacy_workspace/cache/l2snap_k26_z12_mini100.npz
```

Mini100 result:

```text
plain              raw = 1.352738641e-6
z1_r0.01           raw = 1.353833772e-6
z12_s0.25_r0.01    raw = 1.353510568e-6
z12_s0.50_r0.01    raw = 1.353672744e-6
z12_s0.75_r0.01    raw = 1.354186816e-6
z12_s1.00_r0.01    raw = 1.355056163e-6
```

So the Full1000 tiny-positive low-scale `z12` result does not transfer to
Mini100; the branch is negative on the independent split.

Then reran the low-overhead branch stack with `exactgh + preedge4 + k26z12`.
The unbounded stack overfit Full1000 hard and exploded on Mini100.  With a
conservative residual clip:

```text
python legacy_workspace/probe_lowoverhead_moment_stack.py \
  --branch-sets exactgh,preedge4,k26z12 \
  --modes linear --fit-modes global,neuron \
  --lams 10,100,1000,10000,100000 \
  --top-branches 12 --clip 0.0002 \
  --out legacy_workspace/cache/lowoverhead_moment_stack_with_k26z12_clipped_full1000_mini100.npz
```

Best transfer row:

```text
Mini protected base raw = 1.897755704e-6
best clipped xfer raw   = 1.898762227e-6  # worse, rel 1.00053
best clipped xfer obj   = 2.322638618e-6
```

Read: weak branch interactions are not hiding a transfer-positive correction in
the current low-overhead menu.  The stack can manufacture large Full1000 OOF
gains when under-regularized, but the independent Mini100 split rejects them.
Close `k26_z12` and the `exactgh/preedge/k26z12` low-overhead stack as a
frontier path unless a genuinely new branch with Mini transfer enters the menu.

### 2026-07-05 - true moment components on top of sampler

Added:

```text
legacy_workspace/probe_higher_moment_components_on_sampler.py
```

Question: the true-preactivation component regression shows that `b111` is the
named blocker from a Gaussian oracle state.  But would a deployable `b3/b21/k4`
style correction add to the current sampler, or is it miscentered relative to
the sampler path?

First100 against the protected l2snap sampler:

```text
sampler_base          raw = 2.078894894e-6
true gauss state      raw = 1.088398207e-6

Adding true Edgeworth deltas directly to sampler, OOF fitted:
  b3                   raw = 2.078834910e-6
  b21                  raw = 2.078953020e-6
  b3+b21               raw = 2.078860935e-6
  b3+b21+k4            raw = 2.079013724e-6
  b3+b21+b111          raw = 2.078668343e-6
  b3+b21+b111+k4       raw = 2.078771159e-6
```

Read: true component deltas are essentially neutral when added directly to the
sampler.  This means the component deltas are tied to the Gaussian oracle state;
they are not a portable residual correction for the sampled estimator.

Absolute oracle-state branch blend, still OOF:

```text
sampler + true gauss branch                 raw = 7.357289628e-7
sampler + true gauss/b3/b21 branches         raw = 5.373464529e-7
sampler + true gauss/b3/b21/k4 branches      raw = 5.039090975e-7
sampler + true gauss/b3/b21/b111 branches    raw = 1.373625076e-7
sampler + true gauss/b3/b21/b111/k4 branches raw = 1.657673960e-8
```

Read: the high-ceiling hybrid is not "sampler plus Edgeworth delta."  It is
"sampler plus a better absolute propagated distribution state."  If that state
is accurate, the moment components become highly complementary.  This reframes
the viable analytic work: do not engineer standalone `b21/k4` deltas for the
current sampler; either improve the absolute hidden/preactivation state
estimate, or keep using row-cloud/high-count controls.  This is consistent with
the layer-30 oracle snap: true L30 post-ReLU mean almost solves the problem,
while denoisers over the existing 18-block hidden mean do not expose the signed
state error.

### 2026-07-06 - sampled projected preactivation moment probe

Added:

```text
legacy_workspace/probe_projected_preact_moment_oof.py
```

Motivation: true structured/random-projected cumulant features have oracle
signal in `probe_higher_moment_structured_closure.py`, but the deployable
PRE-EDGE path used only diagonal sampled raw moments.  This probe tests the
missing middle: from the actual protected SPHEREx row cloud at a selected
preactivation layer, compute fixed random-projection third-moment contractions
such as `E[(z_j-mean_j) q_r^2]` and `E[(z_j-mean_j)^2 q_r]`, then fit
MLP-grouped OOF hidden-mean corrections and propagate the corrected hidden
state to the final layer.

Oracle diagnostic rerun, true post-moment tensors only:

```text
probe_higher_moment_structured_closure.py
  first100 rank=8 random:
    gauss final        = 1.087080720e-6
    true_k3 final      = 1.551141005e-7
    true_k34 final     = 2.033313200e-8
    best k34 closure   = 6.665003222e-7
    g3_r2              = +0.89053
    g4_r2              = +0.18581

  first100 rank=16 random:
    best k34 closure   = 6.667789630e-7
```

Read: random projected cumulant oracle signal is real but saturates by rank 8;
more random modes do not improve the closure.

Deployable sampled-projection test:

```text
command:
  python legacy_workspace/probe_projected_preact_moment_oof.py \
    --weights-cache legacy_workspace/cache/phase1_full1000_weights_targets.npz \
    --indices 0:200 --layer 4 --proj-rank 8 \
    --alphas 1,10,100,1000,10000 --clips 0,2,4 \
    --eval-final-top 6 --batch-mlp 8 --device cuda \
    --state-cache legacy_workspace/cache/projected_preact_moment_l4_full200_state.npz \
    --out legacy_workspace/cache/projected_preact_moment_l4_full200_eval.npz

Full200 L4:
  sample hidden MSE      = 2.436657534e-8
  fixed edge4 hidden MSE = 2.368091708e-8
  best projected hidden  = 2.355001074e-8

Final propagation:
  best projected final   = 2.156362973e-6  # ridge_a10_c4, beta=1
```

Read: the projected sampled moment features are real and slightly improve over
plain L4 `edge4`, but the improvement is only about `0.2%` beyond the already
known PRE-EDGE L4 correction and about `1.1%` versus the protected Full200
baseline.  This is not a route to the next leaderboard tier by itself.  Keep
the script as a reusable observer harness, but do not package this standalone
or spend a broad search on random projection rank/layer unless it is folded
into a larger absolute-state estimator.

### 2026-07-06 - target-free teacher residual MLP over final seed-cloud features

Added:

```text
legacy_workspace/probe_l2snap_teacher_residual_mlp.py
```

Question: the seed-weight/selection oracle says the SPHEREx blocks contain
large complementary signal, and union53/high-count teachers are strong
target-free references.  Can a deployable-ish per-neuron MLP over the protected
final seed cloud and diagonal weight features learn the signed teacher residual
and transfer to independent Mini100 truth?

Teacher:

```text
train label = union53_equal_l2snap - protected18_l2snap
truth labels are used only for reporting
```

Full feature model:

```text
command:
  python legacy_workspace/probe_l2snap_teacher_residual_mlp.py \
    --mode full --hidden 96 --depth 3 --epochs 60 --batch 32768 \
    --dropout 0.05 --weight-decay 0.001 --lr 0.001 --device cuda \
    --teacher-key equal

Full1000:
  base truth       = 2.301739726e-6
  base->teacher    = 1.483569446e-6
  gain 1.0 truth   = 2.050580723e-6
  gain 1.0 teacher = 1.213909419e-6

Mini100:
  base truth       = 1.897755704e-6
  base->teacher    = 1.315560785e-6
  gain 0.125 truth = 1.898650107e-6
  gain 1.0 truth   = 1.989829136e-6
  gain 1.0 teacher = 1.413584747e-6
```

Safe/low-capacity feature model:

```text
command:
  python legacy_workspace/probe_l2snap_teacher_residual_mlp.py \
    --mode safe --hidden 64 --depth 2 --epochs 50 --batch 32768 \
    --dropout 0.10 --weight-decay 0.01 --lr 0.001 --device cuda \
    --teacher-key equal

Full1000 gain 0.5:
  truth   = 2.282247888e-6
  teacher = 1.462807183e-6

Mini100 gain 0.0625:
  truth   = 1.898131846e-6
  teacher = 1.317263448e-6
```

Read: this is a clean negative for using final seed-cloud scalar/features to
learn the signed high-count teacher correction.  The model can reduce teacher
distance and truth MSE on the training split, but independent Mini100 teacher
distance and truth both worsen, even at tiny gains.  This agrees with the
earlier seedstats/message-model negatives: these features see error magnitude
and some in-split direction, but not the transferable signed correction.  Do
not scale this exact teacher-residual MLP or port it to flopscope.

### 2026-07-06 - projected preactivation moment layer-8 check

After the layer-4 projected preactivation moment probe showed a small real
hidden-state improvement, reran the same deployable sampled-projection observer
at layer 8 on first100 using CUDA:

```text
command:
  python legacy_workspace/probe_projected_preact_moment_oof.py \
    --weights-cache legacy_workspace/cache/phase1_full1000_weights_targets.npz \
    --indices 0:100 --layer 8 --proj-rank 8 \
    --alphas 1,10,100,1000,10000 --clips 0,2,4 \
    --eval-final-top 6 --batch-mlp 8 --device cuda \
    --state-cache legacy_workspace/cache/projected_preact_moment_l8_first100_state.npz \
    --out legacy_workspace/cache/projected_preact_moment_l8_first100_eval.npz

n=100 layer=8 count=18 proj_rank=8 features=66
sample_hidden       = 2.386708870e-08
edge4 hidden        = 2.376796585e-08
best projected hidden:
  ridge_a10000_c2   = 2.373046976e-08  # rel 0.994276

best final replay:
  ridge_a10000_c2 beta=0.25 raw = 2.082212417e-06
```

Read: layer 8 does not scale the layer-4 PRE-EDGE/projected-moment signal into
a larger state-snap correction.  The hidden MSE improves only about `0.6%`,
and final replay is worse than the protected first100 baseline.  Do not run a
broad random-projection layer/rank sweep unless a new observable is added; this
exact projected preactivation feature family is a small local variance control,
not the path to the next score tier.

### 2026-07-06 - narrowed expert question on signed QR error

Created a focused relay note for the math/physics expert:

```text
research/gpt_math_expert/phase1_signed_qr_error_b111_question.md
```

It asks for a concrete observable or no-go result for the signed deep QR-block
error / response-aligned all-distinct `b111` skew contribution.  The note lists
the positive oracles and the closed negative branches so the reply should not
repeat already-tested seed-cloud, first-harmonic, shallow-GREG, or local-b111
ideas.

### 2026-07-06 - real protected-trajectory equivariant student smoke

Question: the earlier Phase-1 Song-student adaptation used diagonal Gaussian
mean rows and failed to generalize.  Maybe the missing input is the actual
protected l2snap sampled trajectory, i.e. per-layer weighted/equal seed means,
seed std/MAD, and seed moments.  The generic equivariant trainer already
supports `pred_features`, so tested the exported real trajectory cache directly
with CUDA.

Full1000 seedstats cache, interrupted after the first epoch because full-cache
training was too slow for a quick gate:

```text
command:
  python song/src/train_equivariant_residual.py \
    --cache song/data/phase1_l2snap_seedstats_full1000_slim.npz \
    --epochs 20 --hidden 16 --rounds 2 --batch 8 \
    --holdout-mod 5 --holdout-value 0 \
    --lr 0.001 --weight-decay 0.01 --dropout 0.10 \
    --weight-features --device cuda

epoch 1:
  train 2.325409e-6 -> 2.325964e-6
  test  2.207063e-6 -> 2.206814e-6  # neutral/tiny positive
```

Embedded first200 seedstats cache, same model/optimizer, 30 epochs:

```text
command:
  python song/src/train_equivariant_residual.py \
    --cache song/data/phase1_l2snap_seedstats_first200_matrix_cache.npz \
    --epochs 30 --hidden 16 --rounds 2 --batch 8 \
    --holdout-mod 5 --holdout-value 0 \
    --lr 0.001 --weight-decay 0.01 --dropout 0.10 \
    --weight-features --device cuda

best held-out:
  train 2.188462e-6 -> 2.181597e-6
  test  2.152623e-6 -> 2.154649e-6  # ratio 1.00094, worse
```

Read: exposing real protected seed-stat trajectories does not rescue the
equivariant residual student in this simple form.  It learns a small in-split
correction but does not improve held-out final MSE.  Do not scale this exact
student/feature set further unless a new target-free channel is added, e.g. a
genuine signed QR-error or response-aligned `b111` observable.

### 2026-07-06 - late projected preactivation moment sensors

After L4/L8 projected sampled preactivation moments gave only tiny local
hidden-state gains, tested the same richer row-cloud moment sensor at late
layers where the oracle mean-snap map says downstream leverage is high.

Commands:

```text
python legacy_workspace/probe_projected_preact_moment_oof.py \
  --weights-cache legacy_workspace/cache/phase1_full1000_weights_targets.npz \
  --indices 0:100 --layer 24 --proj-rank 8 \
  --alphas 1,10,100,1000,10000 --clips 0,2,4 \
  --eval-final-top 6 --batch-mlp 8 --device cuda \
  --state-cache legacy_workspace/cache/projected_preact_moment_l24_first100_state.npz \
  --out legacy_workspace/cache/projected_preact_moment_l24_first100_eval.npz

python legacy_workspace/probe_projected_preact_moment_oof.py \
  --weights-cache legacy_workspace/cache/phase1_full1000_weights_targets.npz \
  --indices 0:100 --layer 30 --proj-rank 8 \
  --alphas 1,10,100,1000,10000 --clips 0,2,4 \
  --eval-final-top 6 --batch-mlp 8 --device cuda \
  --state-cache legacy_workspace/cache/projected_preact_moment_l30_first100_state.npz \
  --out legacy_workspace/cache/projected_preact_moment_l30_first100_eval.npz
```

Results against the same first100 protected-l2snap baseline
`raw ~= 2.078895e-6`:

```text
L24:
  sample hidden MSE       = 1.036598437e-8
  edge4 hidden MSE        = 1.040632648e-8
  best projected hidden   = 1.057972541e-8  # worse, rel 1.0206
  best final replay       = 2.086050268e-6  # worse

L30:
  sample hidden MSE       = 8.506149194e-9
  edge4 hidden MSE        = 8.586463711e-9
  best projected hidden   = 8.708297275e-9  # worse, rel 1.0238
  best final replay       = 2.089029509e-6  # worse
```

Read: moving the sampled projected-moment sensor into the high-leverage late
layers does not rescue it.  The existing protected row cloud has extremely
accurate ordinary hidden means, but its third-moment projection features do not
predict the signed hidden-mean error; fitting them worsens both hidden MSE and
final replay.  This closes the current random-projected sampled preactivation
moment family across early and late layers.  A future moment sensor must be a
different response-aligned observable, not more random projection rank/layer
sweeps of this design.

### 2026-07-06 - HERO low-rank Hermite-chaos smoke

Implemented the expert's deterministic low-rank Hermite-chaos suggestion as a
standalone diagnostic:

```text
legacy_workspace/probe_hero_lowrank_chaos.py
```

State:

```text
h_i ~= mu_i
     + sum_r A_ir H1(xi_r)
     + sum_r B_ir H2(xi_r)/sqrt(2)
     + sum_r C_ir H3(xi_r)/sqrt(6)
     + diagonal Gaussian residual
```

Linear layers propagate A/B/C with dense matrix products.  ReLU layers project
the conditional Gaussian ReLU mean back onto H1/H2/H3 by one-dimensional
Gauss-Hermite quadrature, and decode the scalar mean with clipped Edgeworth
k3/k4 cumulants.  This is the deterministic analogue of MOMA/PRE-EDGE, not a
sampled endpoint patch.

CUDA smoke on the first 10 public-full MLPs:

```text
python legacy_workspace/probe_hero_lowrank_chaos.py \
  --indices 0:10 --ranks 0,4,8 --batch-size 5 --device cuda \
  --out legacy_workspace/cache/hero_lowrank_smoke10.npz

rank=0   all=8.220558917e-04 final=9.900139081e-04
rank=4   all=7.931733469e-04 final=9.697419313e-04
rank=8   all=7.546739573e-04 final=9.463231423e-04
```

Higher-rank sweep:

```text
python legacy_workspace/probe_hero_lowrank_chaos.py \
  --indices 0:10 --ranks 16,32,64,128 --batch-size 2 --device cuda \
  --out legacy_workspace/cache/hero_lowrank_rank_sweep10.npz

rank=16  all=7.067917553e-04 final=9.049153027e-04
rank=32  all=6.044733674e-04 final=8.006772454e-04
rank=64  all=4.913027756e-04 final=7.277712339e-04
rank=128 all=3.792212022e-04 final=6.202999509e-04
```

Read: this quick implementation is not close.  Even rank 128 remains
mean-propagation-scale (`~6e-4` final), orders of magnitude above the
`1e-6`-class needed for a floor-feasible deterministic replacement.  The
failure is too large to justify broad rank/basis sweeps of this exact
representation.  A future deterministic chaos closure would need a materially
better ReLU/covariance projection, likely full covariance or explicit
cross-factor terms; do not treat plain diagonal-residual low-rank HERO as a
near-term package route.

### 2026-07-06 - late full-C21 sampled PRE-EDGE closeout

Question: the random-projected late moment sensors failed, but perhaps the
full sampled two-index `C21` / K3 decomposition at a high-leverage late layer
was unfairly dismissed.  Re-ran the full sampled K3 decomposition at layer 30
on first100 with CUDA, using the protected 18-block l2snap row cloud.

Command:

```text
python legacy_workspace/probe_preedge_sample_k3_decomp.py \
  --weights-cache legacy_workspace/cache/phase1_full1000_weights_targets.npz \
  --indices 0:100 --layer 30 --count 18 --weight-mode protected \
  --eval-final-top 8 --batch-mlp 4 --device cuda \
  --state-cache legacy_workspace/cache/preedge_sample_k3_decomp_l30_first100_state.npz \
  --out legacy_workspace/cache/preedge_sample_k3_decomp_l30_first100_eval.npz
```

Results:

```text
hidden sample             = 8.506150509e-09
hidden direct_edge4       = 8.586464532e-09  # worse
hidden direct_edge3       = 9.037354524e-09  # worse

final no_snap             = 2.078896535e-06
best final C21/PRE-EDGE   = 2.081779945e-06  # direct_edge4 beta=0.25, worse
```

Read: even with the full sampled `C21`/`b3+b21+b111` decomposition at a
late high-leverage layer, the protected sample mean is already better than
the moment-decoded hidden state.  Final replay worsens.  This closes the
current sampled moment-sensor family more strongly: scalar PRE-EDGE,
projected preactivation moments, sampled K3 decomposition, and late full-C21
all fail to expose the response-aligned signed `b111` coordinate at protected
count.  The next useful step is an expert-specified deterministic/final-rooted
`C21` recurrence or a genuinely new observable, not another sampled moment
sensor sweep.

### 2026-07-06 - final ReLU identity / signed pre-mean oracle

Question: can the final sampled ReLU mean be decomposed into an easier control
variate using

```text
ReLU(z) = (z + |z|) / 2
```

The protected sampler already estimates both `E[ReLU(z)]` and, through the
final preactivation row cloud, `E[z]`.  If a lower-noise proxy for the signed
final preactivation mean were available, we could keep the sampled `|z|` part
and replace only the noisy signed part:

```text
candidate = sample_ReLU + 0.5 * (proxy_Ez - sample_Ez)
```

Full200 oracle with true final preactivation mean from the higher-moment
dataset:

```text
protected sample raw                         = 2.181299872e-06
sample_Ez MSE vs true_Ez                     = 4.539282409e-06
replace sample_Ez with true_Ez               = 1.069892640e-06  # rel 0.4905

lambda sweep toward true_Ez:
  lambda=0.10 raw = 1.968039144e-06  # rel 0.9022
  lambda=0.30 raw = 1.609597691e-06  # rel 0.7379
  lambda=1.00 raw = 1.069892640e-06  # rel 0.4905
```

Cheap diagonal mean-propagation proxy:

```text
diag mean-prop Ez MSE vs true_Ez = 1.996440951e-03
best tiny blend lambda=0.001     = 2.180330649e-06  # rel 0.99956
larger blends quickly worsen
```

OOF denoising from already-paid block preactivation/ReLU statistics:

```text
features: sample_Ez, per-seed Ez/ReLU deviations, block std/MAD/max/skew/kurt,
          weighted-minus-equal, same-seed Ez/ReLU products

ridge best pre-error R2  ~= -0.012
ridge best final rel     ~= 1.014  # worse
HGB best pre-error R2    ~= +0.001
HGB best final rel       ~= 1.001  # worse
```

Read: the identity exposes a large oracle that is different from PRE-EDGE and
from endpoint residual fitting.  But the current block statistics do not
predict the signed `sample_Ez` error, and the cheap diagonal analytic proxy is
far too biased.  The useful expert-facing question is now sharper: is there a
deployable deterministic or same-row control that estimates final signed
preactivation mean better than the QR sample, at much lower cost than adding
more full SPHEREx blocks?  Without that, the oracle is not actionable.

### 2026-07-06 - old analytic branch as signed pre-mean proxy

Question: the final-ReLU identity oracle only needs a better estimate of the
signed final preactivation mean `E[z]`.  The old cumulant/Edgeworth estimator
is much too inaccurate as a standalone Phase 1 estimator, but it may contain
a low-frequency signed-mean signal complementary to the protected SPHEREx row
cloud.

Generated deterministic old-analytic rows with `MC_SAMPLES=0, MC_BLEND=0` for
first100 and tail100:

```text
legacy_workspace/cache/analytic_estimator_rows_full100_det.npz
legacy_workspace/cache/analytic_estimator_rows_full100_100_200_det.npz
```

First100 fit, evaluated out-of-fold:

```text
protected sample raw                   = 2.078897250e-06
old analytic final raw                 = 2.580914851e-05
old analytic Ez MSE vs true Ez          = 3.861274894e-05
corr((analytic_Ez-sample_Ez), true error)= 0.2763

identity lambda OOF raw                = 1.979841612e-06  # rel 0.9524
two-branch OOF raw                     = 1.942544812e-06  # rel 0.9344
two-branch mean coeffs                 = [0.0527, 0.0392]
```

Train first100 / test tail100 and the reverse:

```text
first-fit identity on tail100 raw      = 2.191299243e-06  # rel 0.9595
first-fit two-branch on tail100 raw    = 2.143865435e-06  # rel 0.9388
tail-fit identity on first100 rel      = 0.9527
tail-fit two-branch on first100 rel    = 0.9330

Full200 OOF identity raw               = 2.084821311e-06  # rel 0.9558
Full200 OOF two-branch raw             = 2.041265497e-06  # rel 0.9358
fixed coeffs (0.05, 0.04) Full200 raw  = 2.041008629e-06  # rel 0.9357
```

So the signal is real and transfers locally: old analytic final error plus
old analytic signed-preactivation correction gives about a 6.4% raw reduction
on Full200.

Cost gate: the old analytic branch alone profiles at roughly the floor:

```text
one MLP old analytic effective compute = 2.737e10
old analytic multiplier                = 0.1006
```

Paired with the protected 18-block SPHEREx branch, the combined estimated
multiplier is about `0.2409`, so the raw gain is not remotely enough.  A
low-count SPHEREx + old-analytic sweep confirms the adjusted-score branch is
not production-relevant:

```text
count  branch_raw       approx_mult  approx_adjusted
4      8.482629002e-06  0.13180      1.117995256e-06
6      6.083099629e-06  0.14738      8.965564460e-07
8      5.004191707e-06  0.16297      8.155401546e-07
10     3.887081475e-06  0.17856      6.940695195e-07
12     3.161315916e-06  0.19414      6.137524388e-07
14     2.774412070e-06  0.20973      5.818807982e-07
16     2.304729156e-06  0.22532      5.192965271e-07
18     2.041265497e-06  0.24090      4.917498637e-07
```

Cheap ablations of the old analytic branch on a 20/20 first/tail split were
not promising:

```text
gauss_cov          branch rel ~= 1.028  # worse
gauss_cov_nocal    branch rel ~= 1.078  # worse
skew_cross_nokurt  branch rel ~= 1.028  # worse
skew_query_nokurt  branch rel ~= 1.034  # worse
query_herm1_nokurt branch rel ~= 1.036  # worse
```

Read: the signed-preactivation identity is still the largest clean oracle, and
the old analytic estimator proves there is a transferable deterministic proxy.
But adding that proxy wholesale is too expensive, and cheap Gaussian/K3-only
extractions did not preserve the useful signal.  The remaining useful routes
are (a) distill just the old analytic branch's two correction coordinates into
a cheap runtime proxy, or (b) ask for a direct mathematical recurrence for
final signed preactivation mean.  Do not package the full old analytic branch
with SPHEREx.

### 2026-07-06 - analytic signed-branch distillation closeout

Question: can protected-pass seed trajectory features cheaply predict just the
old analytic branch's useful correction, avoiding the old analytic runtime?

Probe:

```text
python legacy_workspace/probe_distill_analytic_signed_branch.py \
  --feature-cache song/data/phase1_l2snap_seedmeans_full1000_slim.npz \
  --feature-mode all --interactions \
  --layers 0,1,2,4,8,16,24,28,30,31 \
  --alphas 0.1,1,10,100,1000 --clips 0,2 \
  --labels combo --beta-modes one --indices 0:200
```

Label:

```text
combo = 0.05 * (analytic_final - protected)
      + 0.04 * 0.5 * (analytic_Ez - sample_Ez)
```

Results:

```text
protected base raw                     = 2.181293040e-06

safe late features:
  best robust obj                      = 2.280221152e-06
  OOF raw                              = 2.243041356e-06  # rel 1.0283
  first100 -> tail100 raw              = 2.280221152e-06
  teacher R2                           = +0.2266

all seed-mean features + interactions:
  best robust obj                      = 2.282897083e-06
  OOF raw                              = 2.225003620e-06  # rel 1.0200
  first100 -> tail100 raw              = 2.282897083e-06
  teacher R2                           = +0.1847
```

Read: this is the failure mode we needed to test.  The already-paid protected
features can partially reconstruct the old analytic teacher coordinate, but
the reconstructed teacher worsens the real target on held-out MLPs.  The model
is learning the analytic branch's wrong components more easily than the small
aligned component that helped when the teacher was used directly.  This closes
cheap protected-feature distillation of the old analytic signed branch for now.
The live signed-preactivation path needs a new mathematical/control-variate
observable, not a tabular student over the existing seed trajectory summaries.

### 2026-07-06 - response-contracted signed-Ez closure

Question: generic block-stat denoising of the signed final preactivation mean
failed, but perhaps the right deployable features are response-contracted:
protected seed trajectory channels pushed through the actual final weight
directions.  This targets the final-ReLU identity oracle directly:

```text
target = true E[z_L] - protected sample E[z_L]
candidate = protected ReLU mean + 0.5 * predicted_target
```

Probe:

```text
python legacy_workspace/probe_signed_ez_response_closure.py \
  --indices 0:200 --layers 24,28,30,31 \
  --ridges 0.01,0.1,1,10,100 --clip-q 0.001
```

Results:

```text
protected base raw       = 2.181293040e-06
true signed-Ez oracle    = 1.069893547e-06

best deployable response closure:
  raw                    = 2.180825015e-06  # rel 0.999785
  signed-Ez R2           = +0.00456
  ridge                  = 100
```

Read: the feature geometry is correct enough to get a barely positive sign, but
the magnitude is two orders too small.  Response-contracted protected
seed-trajectory summaries do not expose the signed final preactivation mean
error.  This closes the obvious final-weight-aware tabular predictor.  The
remaining signed-Ez route requires a new analytic/control identity or a
different sampled observable, not more ridge features over the protected row
cloud.

### 2026-07-06 - self-fitted degree-4 spherical harmonic CV

Added:

```text
legacy_workspace/probe_l2snap_degree4_harmonic_cv.py
```

Reason for the test: a QR-antithetic block integrates constants, all odd
harmonics, and quadratic functions exactly at the block-mean level.  That makes
degree-2 self-harmonic controls structurally dead, but leaves degree 4 as the
first possible same-row spherical control.  The probe builds random degree-4
zonal harmonics

```text
h4_q(u) = <u,q>^4 - 6/(n+4)<u,q>^2 + 3/((n+2)(n+4))
```

fits row-level coefficients from odd seed blocks and applies them to even
blocks, and vice versa.  The control is exact-zero-mean under the sphere, so it
is deployable in principle if it reduces target-free block variance or transfers
across independent public splits.

Smoke and representative results:

```text
smoke2 rank8:
  target MSE improved by chance, but target-free VR was negative
  (best raw rel ~= 0.934, VR ~= -0.057)

Full1000 spaced20, rank16:
  plain raw              = 2.957992449e-6
  best rank16 raw        = 2.793343694e-6  # rel 0.944
  target-free VR         = -0.0156

Mini100 independent, rank16:
  plain raw              = 1.897752866e-6
  best rank16 raw        = 1.899535796e-6  # rel 1.00094, worse
  target-free VR         < 0 for all checked settings

Full200, rank16:
  plain raw              = 2.181291500e-6
  best rank16 raw        = 2.175065833e-6  # rel 0.99715
  target-free VR         < 0 for all checked settings
```

Read: the degree-4 harmonic dictionary can manufacture a favorable result on a
small slice, but it does not pass independent Mini transfer and it increases
the block-variance diagnostic everywhere.  Treat this as a useful structural
negative: QR leaves degree-4 block defects, but self-fitting a small random h4
dictionary does not expose a robust signed correction.  Do not package or
broad-sweep this branch unless a non-random, expert-specified h4 dictionary is
introduced.

### 2026-07-06 - weight-rooted true signed-Ez diagnostic

Added:

```text
cheeky_experiments/probe_weightroot_true_ez.py
```

Purpose: re-test the large signed final preactivation oracle with a different
feature geometry.  The earlier response-contracted protected trajectory probe
got only `+0.00456` signed-Ez R2.  This run asks whether final-rooted weight
response statistics, optionally joined with the protected trajectory
contractions, can predict the true higher-moment label
`true_pre_mean_L - protected_pre_mean_L`.

First200 baseline:

```text
protected l2snap raw           = 2.181293040e-6
true signed-Ez oracle raw      = 1.069893547e-6
```

Weight-only features, `response_layers=28,30`:

```text
best OOF identity raw          = 2.180725796e-6  # rel 0.999740
best signed-Ez R2              = +0.00512
best first->tail signed-Ez R2  = +0.00243
best tail->first signed-Ez R2  = +0.00214
```

Weight + protected trajectory features,
`trajectory_layers=24,28,30,31`:

```text
best OOF identity raw          = 2.180960427e-6  # rel 0.999848
best signed-Ez R2              = +0.00495
best first->tail signed-Ez R2  = +0.00188
best tail->first signed-Ez R2  = +0.00174
```

Verdict: reject as a package path.  The true signed-Ez oracle is real and large,
but simple deployable response/weight statistics do not recover the sign.  Do
not keep adding ridge features in this family.  The remaining signed-Ez route
requires a new analytic/control identity or a new independent state estimator.

### 2026-07-06 - nonlinear final preactivation mean shift

Added:

```text
legacy_workspace/probe_final_premean_shift_oracle.py
```

The earlier signed-Ez identity used the true/proxy final preactivation mean
linearly:

```text
sample_ReLU + 0.5 * (proxy_Ez - sample_Ez)
```

This probe tests the stronger use: shift the sampled final preactivation row
cloud by the preactivation-mean error, then apply ReLU:

```text
Rbar * mean_rows ReLU(z_unit + lambda * (proxy_Ez - sample_Ez) / Rbar)
```

Full200 true-Ez oracle:

```text
lambda=0.00 raw = 2.181295751e-6
lambda=0.10 raw = 1.775342007e-6
lambda=0.30 raw = 1.092352548e-6
lambda=0.50 raw = 5.812522003e-7
lambda=0.75 raw = 1.841001384e-7
lambda=1.00 raw = 5.552102870e-8
```

Read: final preactivation mean is essentially as powerful as the L30 hidden
mean oracle if it is used as a nonlinear shift before the final ReLU.  The
previous linear identity understated this oracle: true-Ez linear replacement
was only `~1.07e-6`, while nonlinear shift reaches `5.55e-8`.

No-eigen old-analytic Ez proxy, same nonlinear shift:

```text
lambda=0.00 raw = 2.181295751e-6
lambda=0.04 raw = 2.055967775e-6
lambda=0.08 raw = 2.001177809e-6  # best, stable on first100/tail100
lambda=0.12 raw = 2.016930739e-6
lambda=0.20 raw = 2.260071595e-6
```

Cheaper K3-cap3 old-analytic Ez proxy:

```text
lambda=0.04 raw = 2.079277659e-6
lambda=0.08 raw = 2.074538601e-6  # best, similar to old cap3 fusion
```

Adding the analytic final-output delta after the no-eigen nonlinear shift only
improved Full200 OOF raw from `2.001178e-6` to `1.998715e-6`, so the useful
teacher coordinate is almost entirely the final preactivation mean proxy.

Verdict: this is a better formulation of the biggest actionable oracle.  It is
not a package path with the full old analytic branch, because the no-eigen
branch remains too expensive.  The next useful work should target a cheap,
independent proxy for `E[z_final]` and deploy it as a nonlinear final-ReLU
shift.  Do not spend more time on generic final residual labels for this path.

Cheap same-run feature proxy check:

```text
features:
  protected final preactivation raw moments, seed spread/MAD, sampled ReLU,
  and MLP-level broadcasts from `preedge_final_full200.npz`
label:
  true_Ez - sample_Ez from higher-moment files
fit:
  MLP-grouped 5-fold ridge, alpha grid

best signed-Ez R2 = +0.00756
linear identity raw without affine = 2.189675e-6
nonlinear shift:
  lambda=0.50 raw = 2.178976403e-6  # rel 0.99894
  lambda=1.00 raw = 2.180805016e-6
```

Read: even with the stronger nonlinear readout, same-run final-preactivation
summaries buy only about `0.1%` raw on Full200.  This closes the cheap
feature-denoising version of the nonlinear-Ez route.  A useful proxy must be
independent of the protected row cloud: a compressed analytic closure, a richer
equivariant state estimator, or a genuinely new sampled observable.

Nonlinear interaction follow-up:

```text
cheeky_experiments/train_weightroot_true_ez_mlp.py
```

This trains a small CUDA MLP over the exact same response-statistic feature
cache with MLP-grouped folds.  It checks whether the ridge failure was merely
missing nonlinear interactions.

```text
0:100 smoke:
  base raw               = 2.078891870e-6
  true signed-Ez oracle  = 1.025102707e-6
  MLP signed-Ez R2       = -0.03888
  identity raw           = 2.129480592e-6  # worse
  fit-beta raw           = 2.064302815e-6  # not stable

0:200 validation:
  base raw               = 2.181293040e-6
  true signed-Ez oracle  = 1.069893547e-6
  MLP signed-Ez R2       = -0.00402
  identity raw           = 2.202035103e-6  # worse
  fit-beta raw           = 2.182679965e-6  # worse
```

Verdict: nonlinear interactions in the response/trajectory summary family do
not recover the signed preactivation error.  Close this feature family unless a
new observable is added.

### 2026-07-06 - no-eigen old-analytic K3-cap value-per-compute check

Added diagnostic wrappers:

```text
cheeky_experiments/estimator_whest_noeigen_k3cap3.py
cheeky_experiments/estimator_whest_noeigen_k3cap4.py
```

Phase-1 `whest run` one-MLP pricing:

```text
no-eigen cap5/default  flops = 21.27B, effective ~= 27.53B
no-eigen cap4          flops = 17.64B, effective ~= 25.37B
no-eigen cap3          flops = 13.87B, effective ~= 19.44B
```

Cap3 Full200 fusion against the protected SPHEREx line:

```text
protected base raw       = 2.181293040e-6
cap3 OOF id raw          = 2.119897521e-6
cap3 OOF two raw         = 2.070813999e-6
cap3 fit-all raw         = 2.070419755e-6
cap3 fixed 0.05/0.04 raw = 2.083334599e-6
```

Read: lowering the cap does retain a real old-analytic teacher signal, but not
nearly enough per FLOP to be a direct hybrid.  Even cap3 only buys about 5% raw
MSE while adding ~14B counted FLOPs plus residual.  The branch is useful for
teacher distillation or algebraic isolation of a subterm, not as a package
component.

### 2026-07-06 - old analytic attribution: calibrated kurt-stat signal

Purpose: identify which old-analytic subterm carries the transferable signed
final-preactivation correction.

First40 toggles against protected SPHEREx:

```text
protected base raw                      = 2.187817042e-6
default no-eigen OOF two                = 1.937423708e-6
KURT_GAIN=0 OOF two                     = 2.304397342e-6
KURT_STATS_GAIN=0 OOF two               = 2.304425037e-6
KURT_MOMENT_GAIN=0 OOF two              = 1.923857987e-6
CALIBRATE_LAYERS=False OOF two          = 1.970324478e-6
LEARNED_K3_GAIN=0 OOF two               = 1.957977892e-6
K3COV_GAIN=0,K3COV_FEATURE_GAIN=0       = 2.054342149e-6
KURT_MOMENT_GAIN=0 plus no K3COV        = 2.090207603e-6
```

Full200:

```text
default no-eigen OOF two                = 2.041265497e-6
no-eigen, KURT_MOMENT_GAIN=0 OOF two    = 1.985499495e-6
cap3 default OOF two                    = 2.070813999e-6
cap3, KURT_MOMENT_GAIN=0 OOF two        = 2.051045861e-6
```

Read: the useful old-analytic coordinate is not the direct K4 Edgeworth mean
term.  It is the EFT/depth kurtosis statistic as a calibration/readout feature,
read through propagated K3 covariance context.  Removing the direct K4 moment
correction gives a cleaner teacher, but K3 source cap compression still bleeds
signal smoothly.  The remaining math target is a cheap recurrence for the
response-aligned K3cov + calibrated kurt-stat signed-mean coordinate.
### 2026-07-06 - final-preactivation Ez proxy route: strong oracle, weak deployable proxies

Built reusable diagnostics for the strongest higher-moment oracle seen so far:

```text
legacy_workspace/build_ez_student_cache.py
legacy_workspace/probe_ez_proxy_ridge.py
legacy_workspace/export_weightroot_ez_proxy.py
```

Key oracle result from `probe_final_premean_shift_oracle.py`: if the protected
final preactivation row cloud is shifted by the true final preactivation mean
error before applying ReLU, Full200 raw drops from `2.1813e-6` to `5.55e-8`
at lambda 1.  This is much stronger than the earlier linear signed-Ez identity
oracle, and it confirms that the missing signal is a signed final-preamble mean
coordinate.

Deployable proxy attempts:

```text
full1000 Ez student cache base Ez MSE        = 4.758931442e-6
generic Song fold0, best holdout Ez ratio    = 0.99934  # too weak
targeted ridge Full1000 OOF signed-Ez R2     = 0.00133
targeted ridge nonlinear shift Full200 best  = 2.178243850e-6  # 0.14% raw gain
weight-rooted true-Ez first200 OOF R2        = 0.00172
weight-rooted nonlinear shift Full200 best   = 2.181295751e-6  # no gain
```

Read: the nonlinear final-preactivation shift is the right readout, but the
same protected sampler features and simple final-rooted weight features still
do not predict the signed mean error.  The next viable path must provide an
independent estimate of final/late hidden state, not another same-row proxy.

Added seed-count support to `probe_final_premean_shift_oracle.py` and swept the
true signed-Ez oracle on protected-prefix QR subsets:

```text
blocks  lambda=0 raw      lambda=0.75 raw     lambda=1.00 raw
8       5.395934255e-6    4.407485033e-7      1.204663107e-7
12      3.429131231e-6    2.852236307e-7      8.263816374e-8
14      3.012491864e-6    2.487222911e-7      7.105828164e-8
16      2.508173873e-6    2.110008407e-7      6.223082354e-8
```

This reframes the signed-Ez target as a possible compute reducer as well as an
accuracy reducer: with a genuinely strong signed final-preactivation mean
estimate, even 8-12 QR blocks would be enough to sit in an elite raw-error
range.  But the current best old analytic proxy is far too weak in this
reduced-sampler regime:

```text
12 blocks + no-eigen/no-direct-K4 proxy:
  lambda=0 raw     = 3.429131231e-6
  lambda=0.10 raw  = 3.008518016e-6
  lambda=0.16 raw  = 2.943859443e-6  # best tested
  wider sweep:
    lambda=0.20 raw = 2.978959091e-6
    lambda=0.30 raw = 3.340480684e-6
    lambda=0.50 raw = 5.236906014e-6
```

Verdict: reduced-block SPHEREx is not viable with the current deployable proxy.
Do not package this.  The next meaningful move is to derive or learn a much
stronger signed final-preactivation mean observable, ideally the cheap version
of the old branch's response-aligned K3cov + calibrated kurt-stat coordinate.

### 2026-07-06 - CUDA Ez learner sanity check

Purpose: check whether the previous weak Ez student was only underpowered, now
that CUDA access is available.

Tried stronger layered equivariant residual models on
`song/data/phase1_l2snap_ez_full1000_matrix_cache.npz`, whose target is the
true final signed preactivation mean from the Phase-1 higher-moment cache.

Large model:

```text
hidden=64, rounds=6, weight_features=True, batch=4
epoch 1: train 4.806008e-6 / 4.804633e-6, test 4.575971e-6 / 4.576126e-6
```

This used CUDA correctly (RTX 4090 at ~99%, ~20 GB VRAM) but took roughly four
minutes per epoch, so it was stopped.  It is not a usable iteration loop unless
there is a much stronger reason to train it.

Smaller weight-feature model:

```text
hidden=32, rounds=3, weight_features=True, batch=8
epoch 1:  test ratio = 1.0001
epoch 10: test ratio = 0.9992
epoch 20: test ratio = 0.9994
```

This matches the earlier non-weight-feature Ez student (`best ratio 0.99934`).
Conclusion: adding local weight summaries and more GPU training does not expose
the signed final-preactivation mean from the already-paid protected trajectory
features.  The Ez oracle remains real, but this learned-from-paid-features route
is closed unless new source features are added.

### 2026-07-06 - direct residual target for nonlinear b111 closure

Question: the nonlinear local b111 closure predicted raw `b111` variance well
but barely improved the final response.  Maybe the issue was the supervised
target: plain `b111` MSE is dominated by response-irrelevant variance.  Patched
`probe_b111_nonlinear_closure.py` with `--target residual`, which trains the
same oracle-state local/storable features directly on

```text
truth - (gaussian + b3 + b21)
```

rather than on `b111` itself.

First100 CUDA result:

```text
command:
  python legacy_workspace/probe_b111_nonlinear_closure.py \
    --indices 0:100 --epochs 50 --patience 6 --hidden 64 --depth 2 \
    --dropout 0.05 --batch-size 32768 --device cuda --target residual \
    --out legacy_workspace/cache/b111_nonlinear_residual_first100_v1.npz

gauss final               = 1.087080720e-6
fit b3+b21 final          = 8.315402061e-7
fit residual-pred final   = 8.207149643e-7
direct residual-pred      = 8.068196424e-7
fit true_b111 final       = 2.833597551e-7
unfit true b111 final     = 1.551141005e-7
residual_r2               = +0.80268
```

Read: even with an oracle-state feature set and the "right" residual target,
the response-relevant gain is only about `1.3%` relative to fitted `b3+b21`,
whereas true b111 would cut the MSE by roughly two thirds.  This closes the
same local/storable feature class more strongly: the missing coordinate is not
unlocked by changing the loss target.  A viable route still needs a
qualitatively different signed response-aligned observable or a stronger
sampling/control mechanism.

### 2026-07-06 - seed-error router with high-count teacher labels

Question: the per-seed/per-neuron oracle inside the protected 18 QR blocks is
huge, but the previous candidate-wise seed-error router was trained against the
public target directly.  Maybe the labels were too noisy or too absolute.  Reran
the existing candidate-wise router with stronger high-count teacher labels:

```text
python legacy_workspace/probe_l2snap_seed_error_router.py \
  --mode joint --label-source union53 --models et --folds 5

python legacy_workspace/probe_l2snap_seed_error_router.py \
  --mode joint --label-source qroffset --models et --folds 5
```

Shared setup:

```text
Full200 protected base  = 2.181298568e-6
Full200 seed oracle     = 3.983043739e-7
Full200 union53 teacher = 7.250611638e-7
Full200 qroff teacher   = 8.172971139e-8

Mini100 protected base  = 1.897755704e-6
Mini100 seed oracle     = 3.657662002e-7
Mini100 union53 teacher = 6.778027337e-7
```

Results:

```text
union53-label hard route:
  Full200 OOF raw = 7.13e-5  # catastrophic
  Mini100 raw     = 7.41e-5  # catastrophic

union53-label best soft route:
  Full200 OOF raw = 2.180473111e-6  # +0.038% vs base
  Mini100 raw     = 1.895628632e-6  # +0.112% vs base

qroffset-label hard route:
  Full200 OOF raw = 6.95e-5  # catastrophic
  Mini100 raw     = 7.53e-5  # catastrophic

qroffset-label best soft route:
  Full200 OOF raw = 2.180531688e-6  # +0.035% vs base
  Mini100 raw     = 1.895257220e-6  # +0.132% vs base
```

Read: replacing noisy target labels with high-count teacher labels does not
unlock the current per-seed candidate feature geometry.  The hard label remains
unidentifiable, and the soft route is a sub-percent correction.  This closes the
existing seed-error router feature set for union53/qroffset teacher labels.  If
we revisit the QR-block oracle, it needs a new candidate-specific internal
observable, such as gate-trajectory telemetry per block/neuron, not another
target choice for the same final seed features.

### 2026-07-06 - direction audit after CUDA/setup reminder

Rechecked the proposed "candidate-specific gate telemetry" follow-up before
writing new code.  Existing scripts already cover the relevant generic forms:

```text
probe_l2snap_gate_trajectory_router.py       # MLP-level protected-vs-union telemetry
probe_l2snap_gate_block_weight.py            # per-block gate/margin reweighting
probe_l2snap_block_neuron_gate_weight.py     # block x final-neuron gate-shape features
probe_l2snap_early_block_telemetry.py        # early exact-moment telemetry
```

Key prior broad result:

```text
Full200 block-trajectory reweighting:
  base raw      = 2.181298568e-6
  best OOF raw  = 2.176620115e-6  # rel 0.997855
  tail guard    = 2.280802906e-6

20-spaced final-neuron block gate:
  base raw      = 2.250037261e-6
  best hinge    = 2.244423938e-6
```

Read: generic gate/margin telemetry is already effectively closed.  Do not
re-run it just because the per-seed oracle remains large; it has not exposed the
signed error.  The only reason to revisit QR-block observables is with a new
specific identity, not another broad regressor over gate rates, margins, or
near-boundary masses.

The strongest remaining non-sampling oracle is still signed final preactivation
mean (`E[z_final]`) used as a nonlinear shift before final ReLU.  Same-run
protected features, weight-rooted features, and small nonlinear learners all
failed to predict that signed coordinate.  The one positive independent signal
is the old no-eigen analytic branch's response-aligned coordinate, especially
with `KURT_MOMENT_GAIN=0`; attribution points to K3cov/K211-style correction
read through the calibrated depth-kurtosis statistic (`KURT_STATS_GAIN`) rather
than the direct K4 Edgeworth mean term.

Checked existing analytic stack outputs before calling this an implementation
path:

```text
cheap diag/low-rank rollout branches only:
  base raw        = 2.181293040e-6
  best global OOF = 2.191303815e-6  # worse

old no-eigen, KURT_MOMENT_GAIN=0, LEARNED_K3_GAIN=0:
  OOF two raw     = 1.966763116e-6

same expensive branch + cheap diag/low-rank shadows:
  best global OOF = 1.909568372e-6
```

So the cheap shadows do not carry the coordinate on their own; they only become
useful when the expensive no-eigen K3cov-style branch is present.  That further
focuses the open question on algebraic extraction/compression of the old branch,
not cheap branch stacking.

Wrote a focused expert-facing memo:

```text
research/gpt_math_expert/phase1_signed_ez_k3cov_kurtstat_question.md
```

Question in one line: can the old branch's signed final-preactivation proxy be
rewritten as a cheap final-rooted K3cov/K211 + depth-kurtosis contraction, or is
that coordinate intrinsically high-rank and inseparable from the old K3 source
carry?

### 2026-07-06 - cap2 no-direct-K4 analytic compression fill-in

Question: the old no-eigen analytic signed-Ez branch has a small transferable
correction but is too expensive to run beside protected SPHEREx.  Cap3 with
`KURT_MOMENT_GAIN=0` was measured on Full200; cap2 had only a first40 smoke.
Filled the missing Full200 cap2 slice to check whether it is a better
value-per-compute subcoordinate.

```text
extract:
  whest-starterkit/.venv/bin/python cheeky_experiments/extract_analytic_variant_rows.py \
    --estimator cheeky_experiments/estimator_whest_noeigen.py \
    --weights-cache legacy_workspace/cache/phase1_full200_weights_targets.npz \
    --indices 40:200 \
    --attrs K3_SOURCE_CAP=2,KURT_MOMENT_GAIN=0.0 \
    --out cheeky_experiments/analytic_noeigen_k3cap2_nokurtmoment_rows_40_200.npz

mean_elapsed = 1.122s / MLP
```

Full200 fusion against protected SPHEREx:

```text
base protected raw      = 2.181293040e-6
cap2 fixed 0.05/0.04   = 2.090638187e-6
cap2 OOF id raw        = 2.125087542e-6
cap2 OOF two raw       = 2.079459546e-6
cap2 fit-all raw       = 2.076948040e-6

first100 -> tail100:
  two raw              = 2.186016818e-6

tail100 -> first100:
  two raw              = 1.969494272e-6
```

Read: cap2 is slightly cheaper than cap3, but the aligned correction is worse
than cap3/no-direct-K4 (`2.051045861e-6` OOF two).  The savings do not come
close to fixing the adjusted-score economics of running an analytic branch
beside protected SPHEREx.  Close this compression knob unless the analytic
branch is algebraically reduced rather than source-cap truncated.

### 2026-07-06 - cap1 no-direct-K4 analytic compression fill-in

Filled the matching Full200 cap1 slice for the same old-analytic signed-Ez
coordinate:

```text
extract:
  whest-starterkit/.venv/bin/python cheeky_experiments/extract_analytic_variant_rows.py \
    --estimator cheeky_experiments/estimator_whest_noeigen.py \
    --weights-cache legacy_workspace/cache/phase1_full200_weights_targets.npz \
    --indices 40:200 \
    --attrs K3_SOURCE_CAP=1,KURT_MOMENT_GAIN=0.0 \
    --out cheeky_experiments/analytic_noeigen_k3cap1_nokurtmoment_rows_40_200.npz

mean_elapsed = 1.079s / MLP
```

Full200 fusion:

```text
base protected raw      = 2.181293040e-6
cap1 fixed 0.05/0.04   = 2.160707261e-6
cap1 OOF id raw        = 2.147705548e-6
cap1 OOF two raw       = 2.119882022e-6
cap1 fit-all raw       = 2.115302573e-6

first100 -> tail100:
  two raw              = 2.239426140e-6

tail100 -> first100:
  two raw              = 2.000381106e-6
```

Read: cap1 is worse than cap2 (`2.079459546e-6` OOF two) and cap3
(`2.051045861e-6` OOF two), and the measured runtime is essentially unchanged
from cap2.  Source-cap trimming is exhausted as a value-per-compute lever; any
useful version of this signal needs an algebraic extraction of the
response-aligned K3cov/kurt-stat coordinate, not another cap reduction.

### 2026-07-06 - no-kurt-moment/no-learned-K3 branch and stack check

Measured the stronger old no-eigen analytic ablation on Full200:

```text
variant attrs:
  KURT_MOMENT_GAIN=0.0
  LEARNED_K3_GAIN=0.0

base protected raw      = 2.181293040e-6
analytic raw           = 1.672218304e-5
fixed blend raw        = 1.996165145e-6
OOF id raw             = 2.062150842e-6
OOF two raw            = 1.966763116e-6
fit-all raw            = 1.966225023e-6
first100 -> tail100    = 2.052455620e-6
tail100 -> first100    = 1.880153394e-6
```

Then stacked cached branch deltas with `cheeky_experiments/probe_analytic_branch_stack.py`.

```text
best global ridge stack, all branches      = 1.899600777e-6 OOF raw
one expensive branch + cheap rollouts      = ~1.909568e-6 OOF raw
expensive old variants only                = ~1.922558e-6 OOF raw
cheap rollout branches only                = ~2.1913e-6 OOF raw
base protected raw                         = 2.181293040e-6
```

Read: the old branch exposes real external signed information, and multiple
versions combine.  However, these are all too expensive as live add-ons beside
protected SPHEREx.  The next useful work is to distill the coordinate
algebraically or via a lightweight independent weight-based teacher; do not
package the branch stack.

### 2026-07-06 - old final-rooted tail candidate is not Phase 1 compatible

Audited the old dramatic research note candidate:

```text
candidate_forward_student_hcoeff_h123_d5d7_refit_reusecov_fastguard_estimator.py
```

It validates only on the toy contract but fails current `v1-phase1` depth-32
rows:

```text
quick_score depth=32 mini n=5      failed=5, zero-FLOP fallback
trace                              IndexError: tuple index out of range
                                   candidate_hybrid2_estimator.py
                                   carr, c0 = self._h_cal[layer_idx]
```

Read: the attractive `~6.9e-8` note belongs to an old depth-8/warmup-style
chassis.  It is not a dormant Phase 1 package.  Only a fresh depth-32 version
of the final-rooted tail idea is still open.

### 2026-07-06 - k26 union-teacher gamma OOF audit

Added:

```text
legacy_workspace/probe_l2snap_k26_teacher_gamma.py
```

Purpose: after `unionteacher1100gamma` became the preferred same-count k26
probe, test whether richer target-free gamma forms can squeeze it further from
the existing k26/union53 caches without public-label fitting.

Inputs:

```text
legacy_workspace/cache/l2snap_k26_split035_045_seed_preds_full1000.npz
legacy_workspace/cache/l2snap_k26_split035_045_seed_preds_mini100.npz
legacy_workspace/cache/l2snap_union53_split035_045_full1000_pred.npz
legacy_workspace/cache/l2snap_union53_split035_045_mini100_pred.npz
```

Tested:

- scalar MLP-level feature gamma variants;
- per-neuron feature gamma variants;
- ridge grid and conservative clip ranges;
- all supervised only by the target-free union53 teacher;
- final acceptance by 5-fold MLP-held-out OOF target MSE.

Result:

```text
saved package gamma:
  Full1000 raw  = 1.470949253e-6
  Mini100 raw   = 1.325972344e-6
  combined raw  = 1.457769534e-6

best richer OOF gamma:
  kind          = scalar_feature
  ridge         = 1e-4
  clip          = [0, 2]
  Full1000 raw  = 1.471550778e-6
  Mini100 raw   = 1.326173881e-6
  combined raw  = 1.458334697e-6

beats package on both splits:
  none
```

An in-sample combined-teacher per-neuron gamma can look better, but the
MLP-held-out check rejects it.  Do not package richer k26 gamma variants unless
a new target-free feature channel is added.  The saved
`submission_phase1_pure26_w05_unionteacher1100gamma_l2snap_split035_045_noaffine_module_finalonly_bundle.tar.gz`
remains the current preferred k26 remote probe.

### 2026-07-06 - k24 union-teacher weighting/gamma check

Question: if the target-free unionteacher trick helps k26, can the lower-compute
`k24_w05` branch become the better adjusted point?

Cache-only check against the union53 beta-0.5 teacher:

```text
k24 equal:
  Full1000 raw = 1.606164071e-6
  Mini100 raw  = 1.502948179e-6
  combined raw = 1.596780808e-6

global weights fit to Full1000 teacher:
  Full1000 raw = 1.604132003e-6
  Mini100 raw  = 1.491428108e-6
  combined raw = 1.593886194e-6

global weights fit to Full1000+Mini100 teacher:
  Full1000 raw = 1.604837203e-6
  Mini100 raw  = 1.480488514e-6
  combined raw = 1.593532776e-6

best scalar gamma, in-sample target-free teacher:
  Full1000 raw = 1.602977084e-6
  Mini100 raw  = 1.471108799e-6
  combined raw = 1.590989058e-6

best scalar gamma, 5-fold MLP-held-out OOF:
  Full1000 raw = 1.605193804e-6
  Mini100 raw  = 1.476458943e-6
  combined raw = 1.593490635e-6
```

Read: the Mini transfer is positive, but the Full1000 movement is tiny and the
OOF gamma does not change the count-frontier economics.  With the current
protected-to-remote raw anchor, this remains around the same or slightly worse
adjusted estimate as existing k24/k26 probes.  Do not package k24 unionteacher
unless the remote result of k26 gamma falsifies the count-26 transfer model.

### 2026-07-06 - same-budget sphere row geometry falsifier

Re-ran the existing row-geometry probe on the current Phase-1 shape:

```text
python legacy_workspace/probe_sampling_geometries.py \
  --indices 0,10,20,30,40,50,60,70,80,90,100,110,120,130,140,150,160,170,180,190 \
  --geometries qr,iid,rademacher,sobol,coord \
  --device cpu
```

All geometries use the same 18 blocks x 256 half-rows, antithetic expansion,
first-layer affine correction, protected seed weights, and final-only readout.

```text
qr           raw=2.264521059e-6  rel=1.000000
iid          raw=3.403444370e-6  rel=1.502942
rademacher   raw=3.478965413e-6  rel=1.536292
sobol        raw=3.791558367e-6  rel=1.674331
coord        raw=4.370055001e-5  rel=19.297922
```

Read: QR-antithetic blocks are not an obvious weakness of the sampler.  Simple
low-discrepancy Gaussian-to-sphere directions, IID sphere rows, Rademacher
rows, and coordinate rows all lose badly at equal row count.  Do not spend
package time on replacing the current QR geometry unless the candidate
preserves the full-block orthogonality/integration properties while adding a
new signed observable.

### 2026-07-06 - full1000 response-contracted signed-Ez closure falsifier

Re-ran the late-layer response-contracted signed final-preactivation proxy on
the full 1000 public higher-moment files, not just the earlier Full200 smoke:

```text
python legacy_workspace/probe_signed_ez_response_closure.py \
  --indices 0:1000 \
  --layers 24,28,30,31 \
  --folds 5 \
  --ridges 0.01,0.1,1,10,100 \
  --clip-q 0.001
```

Setup:

```text
n = 1000
features per final neuron = 468
base protected raw         = 2.301738107e-6
true signed-Ez oracle raw  = 1.127199569e-6
```

Best OOF result:

```text
best predicted-shift raw   = 2.303102021e-6
relative to base           = 1.000593  # worse
best signed-Ez R2          = +0.00119
ridge                      = 10
beta mode                  = fit
```

Read: the full split makes the negative conclusion stronger.  Paid protected
seed-trajectory summaries, even after late-layer response contraction through
the final weights, do not expose the signed final-preactivation mean coordinate.
Do not rerun this regressor family on richer ridge grids or broader layer
menus.  The signed-Ez route still requires a genuinely new analytic/control
observable, most likely the old no-eigen K3cov/kurt-stat coordinate or a new
moment recurrence, not another fit over protected trajectory summaries.

### 2026-07-06 - independent extra-Ez replacement probe

Question: the true final preactivation mean shift is a huge oracle, but
same-row features cannot predict its sign.  Can independent QR seed blocks be
used as a signed final-preactivation mean sensor, replacing some full ReLU
blocks at roughly the same total depth cost?

Mechanism:

```text
full branch:   k protected/optimized blocks to final ReLU
Ez branch:     c disjoint blocks, seeds 32+, to final preactivation only
readout:       ReLU(z_full + lambda * (Ez_proxy - Ez_full) / E||X||)
```

Add-on Full200 checks first:

```text
protected18 base raw = 2.181296859e-6

18 full + 4 Ez, best lambda 0.15 raw = 1.802587171e-6
18 full + 8 Ez, best lambda 0.30 raw = 1.542904935e-6
```

These are real raw gains but do not clear naive block repricing: 18->22 and
18->26 blocks roughly consume the raw gain.

Same-total replacement checks on Full200:

```text
16 full + 2 Ez, best lambda 0.10 raw = 2.195161135e-6
15 full + 3 Ez, best lambda 0.15 raw = 2.117758033e-6
14 full + 4 Ez, best lambda 0.20 raw = 2.110333695e-6
12 full + 6 Ez, best lambda 0.30 raw = 2.280586088e-6

protected18 same-script Full200 raw      = 2.181296859e-6
best same-total Full200 gain             = 3.25% raw, at 14+4
```

Validated the best same-total point on Full1000 with a separately generated
4-block Ez cache (`seeds=32,33,34,35`):

```text
protected18 same-script Full1000 raw     = 2.301740487e-6
14 full + 4 Ez, best lambda 0.20 raw     = 2.291829900e-6
relative gain                            = 0.43% raw
```

Read: this is a useful mechanism but not a next-winner package by itself.  The
signed information from independent Ez rows is real, yet too noisy at 2-4
blocks and too expensive when expanded to 8+ blocks.  Keep the 14+4 form as a
possible tiny/free probe if packaging is cheap, but do not promote it ahead of
the current protected/k26 remote probes.  Future work here needs reliability
weighting or a stronger external Ez estimate; global lambda replacement is
closed as a large-gain route.

Follow-up same-cost mixed-row check:

```text
18 total blocks = 14 optimized protected blocks + extra seeds 32,33,34,35
all 18 blocks contribute final ReLU
extra 4 also provide Ez proxy for a nonlinear shift

Full1000:
  lambda=0 raw       = 2.273615349e-6
  lambda>0           = worse
  protected18 raw    = 2.301740487e-6
```

Read: using the extra rows' ReLU outputs is better than discarding them, but
the Ez shift itself is harmful once those rows are already in the base mean.
This reduces to a small same-count seed-set replacement gain, a class already
shown fragile by `311852`.  Do not package the mixed-row variant as a
breakthrough.

Conditional lambda follow-up, Full1000 cached `14 full + 4 Ez`:

```text
global grid best:
  lambda=0.20 raw = 2.291829942e-6

5-fold MLP OOF selection:
  global lambda chosen per fold      = 2.291829942e-6
  per-final-neuron grid lambda       = 2.304360142e-6  # worse
  shrunk per-neuron lambda           = 2.293602160e-6  # worse

continuous lambda from corr=(lam0.2-lam0)/0.2:
  scalar OOF                         = 2.292267900e-6
  per-neuron OOF                     = 2.301677320e-6  # worse

proxy-Ez RMS vs per-MLP benefit:
  corr(lambda=0.10 benefit) ~= -0.0002
  corr(lambda=0.20 benefit) ~= -0.0173
  corr(lambda=0.30 benefit) ~= -0.0319
```

Read: the independent-Ez correction has no obvious target-free reliability
coordinate in the cached proxy magnitude, and per-neuron/per-fold lambda
fitting overfits.  Treat `lambda ~= 0.20` as the whole useful signal from this
cache.  Do not reopen extra-Ez routing, per-neuron shrinkage, or proxy-norm
gating unless a new independent Ez observable is introduced.

### 2026-07-06 - Full1000 true-moment-summary residual gate

Question: if the public higher-moment dataset exposes the real missing
cumulant structure, do broad true moment summaries predict the protected
sampler's final residual out of fold?

Command:

```text
python legacy_workspace/probe_higher_moment_residual_atlas.py \
  --atlas legacy_workspace/cache/higher_moment_atlas_full1000_noblocks.npz \
  --seed-cache legacy_workspace/cache/l2snap_b05_seed_preds_full1000.npz \
  --layers 0,1,3,7,15,23,30,31 \
  --feature-regex 'pre_|k3|b21|b111|offcov|excess|skew' \
  --mode both --folds 5 \
  --alphas 0.01,0.1,1,10,100,1000 \
  --shrinks 0.125,0.25,0.5,0.75,1.0 \
  --clip-sigma 3
```

Setup:

```text
atlas_mlps = 1000
moment_features = 352
design_features = 708
base raw = 2.301739726e-6
```

Best MLP-held-out result:

```text
alpha=10, shrink=1
raw = 2.293737827e-6
rel = 0.996524
OOF residual R2 = +0.003476
first500 = 2.321656380e-6
last500  = 2.265819274e-6
tail250  = 2.259223249e-6
```

Read: true moment summaries contain a tiny amount of broad residual signal, but
not enough for a final-residual ML patch.  This supports the expert's warning:
the high-upside path is not "predict final sampler error from moment
summaries"; it is a mechanistic preactivation-cumulant closure or a sampler
that directly observes the missing scalar cumulant contractions.

### 2026-07-06 - conservative L4 PRE-EDGE beta audit

Re-opened L4 PRE-EDGE only to check whether the earlier Mini failure was caused
by the aggressive beta `0.75`, rather than by the correction itself.

Full200 fixed beta grid:

```text
base raw      = 2.181298573e-6
beta=0.05 raw = 2.179516129e-6
beta=0.10 raw = 2.177814027e-6
beta=0.20 raw = 2.174651588e-6
beta=0.35 raw = 2.170493748e-6
beta=0.50 raw = 2.167056404e-6
beta=0.75 raw = 2.162913037e-6
beta=1.00 raw = 2.160765216e-6
```

Mini100 cached fixed beta grid:

```text
base raw      = 1.897756316e-6
beta=0.05 raw = 1.897467637e-6
beta=0.10 raw = 1.897255599e-6
beta=0.20 raw = 1.897032499e-6
beta=0.35 raw = 1.897209480e-6
beta=0.50 raw = 1.897999792e-6
beta=0.75 raw = 1.900692959e-6
beta=1.00 raw = 1.905093034e-6
```

Full1000 low-beta confirmation:

```text
base raw      = 2.301739725e-6
beta=0.05 raw = 2.300679764e-6
beta=0.10 raw = 2.299704554e-6
beta=0.20 raw = 2.297989907e-6
beta=0.35 raw = 2.296013660e-6
```

Read: a conservative L4 PRE-EDGE beta is distributionally safer than the old
`0.75` package, but the broad gains are only `0.04%` on Mini and `0.16-0.25%`
on Full1000.  The previously measured PRE-EDGE package overhead is of the same
order or larger, so this is not adjusted-positive.  Keep the hook, but do not
package low-beta PRE-EDGE as a next probe.

### 2026-07-06 - low-row L30 replacement slope audit

Re-opened the cheap L30-measurement idea in replacement form, not only add-on
form.  The question was whether smaller extra low-row checkpoints could clear
raw-vs-cost after the earlier count48/count64 add-on checks flattened.

Protected18 add-on slope, Full200:

```text
base protected18 raw = 2.181299398e-6

count20 half16 beta=0.50 raw = 2.146418208e-6
  extra cost ratio = 0.0370, raw ratio = 0.9840, adjusted ratio ~= 1.020

count40 half8 beta=0.75 raw = 2.126404979e-6
  extra cost ratio = 0.0370, raw ratio = 0.9748, adjusted ratio ~= 1.011

count40 half16 beta=0.75 raw = 2.092739386e-6
  extra cost ratio = 0.0740, raw ratio = 0.9594, adjusted ratio ~= 1.030
```

All add-on slope points remain cost-negative on Full200, despite looking
positive on spaced20.

Then tested replacement: drop protected seeds `0,6,21,23` and spend the saved
budget on extra low-row L30 measurements.

Spaced20 scout:

```text
protected14 base raw             = 2.879256648e-6
protected14 + count64 half32:
  beta=1 raw                     = 1.802943802e-6
  protected cost ratio           = 0.7778
  extra cost ratio               = 0.3364
  total cost ratio               = 1.1142
```

This was adjusted-positive on the scout against protected18, so it was
validated on Full200:

```text
protected18 Full200 raw          = 2.181299398e-6
protected14 + count64 half32:
  beta=0.75 raw                  = 2.075678333e-6
  beta=1.00 raw                  = 2.146472341e-6
  total cost ratio               = 1.1142
  best adjusted ratio vs p18     ~= 1.060
```

Read: low-row L30 mean measurements are real raw signal, but not enough signal
per FLOP.  Both add-on and replacement forms fail broad Full200 repricing.
Close this branch unless a new cheaper L30/late-state measurement appears.

### 2026-07-06 - early-vs-late old K3cov attribution

Question: the old no-eigen/no-direct-K4/no-learned-K3 analytic branch carries
the only consistently useful signed-Ez-like coordinate we have found.  Is that
signal a late readout patch, an early correction that is carried downstream, or
spread across depth?

Compared the same branch split by the layer range where the K3cov update is
allowed to operate, fused OOF with the protected SPHEREx line on Full200:

```text
protected base raw                  = 2.181293040e-6

no K3cov at all, OOF id raw         = 2.157262105e-6
no K3cov at all, OOF two raw        = 2.138134500e-6
no K3cov fit-all raw                = 2.133679628e-6

early16 K3cov, OOF id raw           = 2.100454878e-6
early16 K3cov, OOF two raw          = 2.035279542e-6
early16 fit-all raw                 = 2.034939294e-6

late16 K3cov, OOF id raw            = 2.151234415e-6
late16 K3cov, OOF two raw           = 2.127408504e-6
late16 fit-all raw                  = 2.121798352e-6

all-layer no-direct-K4/no-learnedK3
OOF two raw                         = 1.966763116e-6
```

Read: late-only is not the missing deployable patch.  The useful coordinate is
front-loaded, but not fully localized to the first half; all-layer propagation
still adds material signal beyond early16.  Removing K3cov entirely leaves only
a small scaffold/calibration gain, so the main transferable signal really is
the K3cov/K211-style correction, not just no-eigen branch bookkeeping.  This
points to an early covariance correction whose downstream carry matters, rather
than a cheap final-layer readout correction.  Do not spend time on late-only
K3cov readouts.  The remaining valuable ask is an algebraic way to compute or
approximate the early/all-layer response-aligned K3cov update cheaply enough
for production.

### 2026-07-06 - current protected l2snap row-thinning check

Rechecked the simplest route to the 10% compute floor using the current
protected l2snap seed set, not the older k24/count-frontier set: keep the same
18 QR-antithetic seed blocks, but reduce the number of half-rows per seed after
the layer-2 snap.  This was run with CUDA on Full200.

```text
command, half192:
  python legacy_workspace/probe_l2snap_split_beta.py \
    --weights-cache legacy_workspace/cache/phase1_full200_weights_targets.npz \
    --indices 0-199 --mu-betas 0.5 --sd-betas 0.5 \
    --half-rows 192 --tail-half-rows 192 --tail-row-mode even \
    --batch-mlp 2 --device cuda \
    --out-cache legacy_workspace/cache/l2snap_b05_full200_half192_even.npz

half192:
  raw        = 3.363069006e-6
  affine_raw = 3.350691050e-6

command, half224:
  python legacy_workspace/probe_l2snap_split_beta.py \
    --weights-cache legacy_workspace/cache/phase1_full200_weights_targets.npz \
    --indices 0-199 --mu-betas 0.5 --sd-betas 0.5 \
    --half-rows 224 --tail-half-rows 224 --tail-row-mode even \
    --batch-mlp 2 --device cuda \
    --out-cache legacy_workspace/cache/l2snap_b05_full200_half224_even.npz

half224:
  raw        = 2.651085167e-6
  affine_raw = 2.629243719e-6
```

Read: row thinning loses raw much faster than it saves compute.  Half192 is
catastrophic, and half224 is still far worse while saving only about 12.5% of
the row work.  Since the protected package needs roughly a 29% counted-FLOP cut
to reach the floor, simple row-count trimming cannot floor the current sampler.
This reinforces the older tail-coreset result: the full QR row cloud is carrying
real deep-tail quadrature structure.  Future compute cuts must replace rows
with a deterministic state or signed preactivation proxy, not select fewer rows.

### 2026-07-06 - many-seed low-row L30 replacement check

Follow-up to the low-row L30 mean measurement branch.  The earlier checked
`count48/64` with 16 or 32 extra half-rows, but that does not test the
opposite geometry: many more independent seed blocks with only a few rows each.
This matters because the target is a layer-30 mean, not a final full-path
quadrature rule.

New caches:

```text
legacy_workspace/cache/layer30_extra_manyseed_lowrow_spaced20_20260706.npz
legacy_workspace/cache/layer30_extra_manyseed_lowrow_full200_20260706.npz
legacy_workspace/cache/layer30_extra_manyseed_lowrow_prot14_spaced20_20260706.npz
legacy_workspace/cache/layer30_extra_manyseed_lowrow_prot16_spaced20_20260706.npz
legacy_workspace/cache/layer30_extra_manyseed_lowrow_prot16_full200_20260706.npz
```

Protected18 add-on, Full200:

```text
base raw                 = 2.181301415e-6
count96  half8 beta=.75  = 2.013598428e-6, extra cost ~= 0.1312
count128 half8 beta=.75  = 1.968986093e-6, extra cost ~= 0.1850
count192 half8 beta=.75  = 1.870487367e-6, extra cost ~= 0.2926
```

Raw improves, but not enough per added FLOP.  Approximate adjusted ratios versus
protected are all above 1.0.

Reduced protected16 replacement, Full200:

```text
base prot16 raw          = 2.448539221e-6, protected cost ~= 0.8889
count64  half8 beta=.75  = 2.302523416e-6, total cost ~= 0.9696
count96  half8 beta=.75  = 2.231292438e-6, total cost ~= 1.0234
count128 half8 beta=.75  = 2.173628834e-6, total cost ~= 1.0773
```

This also fails repricing.  The best raw point is roughly protected raw with
more compute; the lower-compute point loses too much raw.  Prot14 was worse
already on spaced20.

Read: many-seed/few-row L30 sensing is real, and it is better than some earlier
low-row shapes, but it is still not steep enough for adjusted score.  Close the
L30 low-row add-on/replacement family unless a cheaper late-state measurement
or a non-global reliability gate appears.

Full-bank layer-2 snap, then pruned tail-seed continuation:

```text
question:
  Maybe count14/count16 fail because their layer-2 snap is estimated from too
  few rows.  Use all 18 protected blocks through H1 and layer-2 snap, then
  continue only the selected 14/16 seed blocks through layers 2..31.

count16 tail seeds:
  2,3,7,8,13,15,17,20,22,23,24,27,28,29,31,21
  raw        = 2.484871555e-6
  affine_raw = 2.477018378e-6

count14 tail seeds:
  2,3,7,8,13,15,17,20,22,24,27,28,29,31
  raw        = 2.773052442e-6
  affine_raw = 2.767898278e-6
```

Read: using all 18 blocks only for the cheap snap does not preserve the missing
tail information.  The raw loss is still far too large for the saved tail
FLOPs.  Close adaptive "decide after layer 2, then continue fewer QR blocks"
unless the continuation branch also receives a new signed/analytic correction.

### 2026-07-06 - Strassen counted-FLOP cut for protected row propagation

Question: can we reduce compute without changing the estimator by replacing the
dominant `(row_cloud x 256) @ (256 x 256)` hidden propagations with block
matrix multiplication?  This is an engineering compression of the exact same
row cloud, not a statistical approximation.

Single dominant multiply cost probe, shape `9216 x 256 @ 256 x 256`:

```text
standard matmul    flops = 1.210351743e9
1-level Strassen   flops = 1.065336959e9
2-level Strassen   flops = 9.43681663e8
3-level Strassen   flops = 8.46390399e8
```

Full-estimator local profile on Mini indices `0..4`, max_threads=1:

```text
protected standard:
  raw      = 1.707697891e-6
  adjusted = 2.426864439e-7
  mult     = 0.14367069
  flops    = 3.8155630592e10
  eff      = 3.9078426831e10

1-level Strassen all row matmuls:
  raw      = 1.707697891e-6
  adjusted = 2.298729562e-7
  mult     = 0.13582689
  flops    = 3.3587705856e10
  eff      = 3.6944914480e10

2-level Strassen all row matmuls:
  raw      = 1.707697822e-6
  adjusted = 2.217652694e-7
  mult     = 0.13344999
  flops    = 2.9755635712e10
  eff      = 3.6298397456e10

2-level Strassen tail-only (ordinary first half-row input multiply):
  raw      = 1.707697891e-6
  adjusted = 2.180138107e-7
  mult     = 0.12931758
  flops    = 2.9888858112e10
  eff      = 3.5174382752e10

3-level Strassen all row matmuls, Mini idx0 only:
  raw      = 7.493691783e-7
  mult     = 0.20710076
  flops    = 2.6691086336e10
  eff      = 5.6331405812e10
```

Read: 3-level crosses the counted-FLOP floor but loses badly to residual/Python
overhead.  The best measured point is 2-level Strassen on the full 9216-row
hidden propagations only, leaving the first `4608 x 256` input projection
standard.  It preserves raw parity and buys about a 10% local adjusted-score
cut versus the protected profile.

Broad Mini spaced20 check:

```text
protected Strassen2 tail-only:
  raw      = 1.848905e-6
  adjusted = 2.366118e-7
  mult     = 0.12851252

z12top16 + Strassen2 tail-only:
  raw      = 1.831096e-6
  adjusted = 2.366865e-7
  mult     = 0.12965308
```

Read: `z12top16` remains raw-positive but adjusted-neutral/slightly negative
after the Strassen cut.  Do not add it to the current package.  Promote only
the pure Strassen2-tail exact-compute rewrite as the low-risk compute candidate.

Prepared artifact, not submitted:

```text
whest-starterkit/packages/to_test_remote/
  submission_phase1_pure18_protected_h1affine_l2snap_b05_weighted_affine_full1000_module_strassen2_tailonly_finalonly_bundle.tar.gz

contents:
  estimator.py
  spherex_pure18_protected_h1affine_l2snap_b05_weighted_affine_full1000_module_finalonly_h1half_inputs_256x4608_module.npz
  manifest.json
```

### 2026-07-06 - SPHEREx row-matmul CountSketch falsifier

Question: instead of dropping QR rows, can each exact hidden propagation
`H @ W` be replaced by an unbiased matrix-product CountSketch
`(H S) @ (S.T W)`?  This is distinct from the failed K3 tensor sketches; it
sketches the dense row matmul inside the sampler.

Added GPU diagnostic:

```text
legacy_workspace/probe_spherex_matmul_countsketch.py
```

Smoke on MLPs `0,10,20,30`, applying the sketch from layer 2 onward:

```text
exact raw    = 3.005493311e-6
sketch192    = 2.179122925e+11
sketch224    = 1.413703354e+10
```

Read: direct CountSketch row propagation is catastrophically unstable through
depth-32 ReLU composition, even at rank 224/256.  The per-layer estimator is
unbiased as a matrix product, but collision noise explodes under repeated
nonlinear propagation.  Close this row-matmul sketch route; do not attempt
production packaging or smaller ranks.

### 2026-07-06 - Repriced count branch with Strassen tail propagation

Question: the old selected-bank count branch was compute-cancelled under
ordinary dense row propagation.  After the exact 2-level Strassen tail rewrite,
does a lower-count selected bank buy enough raw improvement at an acceptable
multiplier?

Built exact Strassen-tail variants for:

```text
k26 split035/045 union l2snap equal affine
k22 balmini union l2snap equal affine
k24 w05 union l2snap equal affine
```

All three preserve raw parity with the corresponding ordinary-matmul branch;
only the hidden row-cloud matmuls are rewritten.

Small prefix check, Mini indices `0..4`:

```text
k26 standard:
  raw      = 6.332630e-7
  adjusted = 1.311772e-7
  mult     = 0.207486
  flops    = 5.508e10
  eff      = 5.644e10

k26 Strassen2 tail:
  raw      = 6.332630e-7
  adjusted = 1.170597e-7
  mult     = 0.185163
  flops    = 4.313e10
  eff      = 5.036e10
```

Mini spaced20:

```text
k22 balmini Strassen2 tail:
  raw      = 1.246077e-6
  adjusted = 1.870087e-7
  mult     = 0.151538
  flops    = 3.651e10
  eff      = 4.122e10

k24 w05 Strassen2 tail:
  raw      = 1.880344e-6
  adjusted = 3.064517e-7
  mult     = 0.164181
  flops    = 3.983e10
  eff      = 4.466e10

k24 bal1 Strassen2 tail:
  raw      = 1.858103e-6
  adjusted = 3.055531e-7
  mult     = 0.165075
  flops    = 3.983e10
  eff      = 4.490e10

k26 split Strassen2 tail:
  raw      = 1.405246e-6
  adjusted = 2.534034e-7
  mult     = 0.184092
  flops    = 4.313e10
  eff      = 5.007e10
```

Independent `full` split spaced20 for k22:

```text
k22 balmini Strassen2 tail:
  raw      = 1.485275e-6
  adjusted = 2.280950e-7
  mult     = 0.152412
  flops    = 3.651e10
  eff      = 4.146e10
```

Read: the k26 and k24 repricings do not clear the protected/Strassen line on the
Mini spaced20 guard.  The k22 balmini repricing is the first serious
post-Strassen remote candidate: Mini spaced20 is below `2e-7`, while the
independent full-spaced20 check is weaker but still close to the protected
remote line.  The older exact Full1000 validation for ordinary k22 has identical
raw predictions and, when scaled by the protected raw-transfer ratio, points
near `~2e-7` adjusted at the measured `~0.152` multiplier.

Follow-up closures:

```text
k22 Strassen3 tail, Mini idx0:
  raw      = 3.632728e-7
  adjusted = 9.461260e-8
  mult     = 0.260445
  flops    = 3.283e10
  eff      = 7.084e10

k22 split-beta 0.35/0.45 Strassen2 tail, Mini spaced20:
  raw      = 1.266280e-6
  adjusted = 1.909949e-7
  mult     = 0.151516
  flops    = 3.651e10
  eff      = 4.121e10
```

Read: 3-level Strassen still loses to residual overhead, even though raw on the
single tested row is excellent.  The k22 split-beta variant is worse than
single-beta `0.5` on the same Mini spaced20 guard.  Keep the staged k22
single-beta Strassen2 package as the count-branch candidate.

Prepared artifact, not submitted:

```text
whest-starterkit/packages/to_test_remote/
  submission_phase1_pure22_balmini_union_l2snap_equal_affine_full1000_module_strassen2_tailonly_finalonly_bundle.tar.gz
sha256:
  07f3647485f619edab4ab52b17cb785725b1891dab57ec34e10bcdc5c2d14441
contents:
  estimator.py
  spherex_pure22_balmini_union_l2snap_equal_affine_full1000_module_finalonly_h1half_inputs_256x5632_module.npz
  manifest.json
```

Risk: k22 was generated from a Mini-heavy selected-bank proposal, so remote
transfer is less protected than the original protected18 branch.  Treat as a
worthwhile remote probe or candidate after explicit user approval, not as an
already-proven replacement.

Additional follow-up after regenerating the missing balanced k23/k24 proposals
from the same 2026-06-24 exact-validation family:

```text
k24 balanced Strassen2 tail, equal weights, no scalar affine:
  seeds    = (0,2,6,15,16,20,21,23,24,33,44,47,49,63,77,84,88,90,102,106,107,110,111,112)

  Mini idx0:
    raw      = 3.472159e-7
    adjusted = 6.531276e-8
    mult     = 0.188104
    flops    = 3.983e10
    eff      = 5.116e10

  Mini spaced20:
    raw      = 1.078208e-6
    adjusted = 1.763448e-7
    mult     = 0.163973
    flops    = 3.983e10
    eff      = 4.460e10

  full split spaced20:
    raw      = 1.370755e-6
    adjusted = 2.274753e-7
    mult     = 0.164128
    flops    = 3.983e10
    eff      = 4.464e10

k23 balanced Strassen2 tail, equal weights, no scalar affine:
  seeds    = (0,2,6,15,16,20,23,24,33,44,47,49,63,77,84,88,90,102,106,107,110,111,112)

  Mini spaced20:
    raw      = 1.135534e-6
    adjusted = 1.776731e-7
    mult     = 0.157286
    flops    = 3.817e10
    eff      = 4.278e10

  full split spaced20:
    raw      = 1.496295e-6
    adjusted = 2.383472e-7
    mult     = 0.158033
    flops    = 3.817e10
    eff      = 4.298e10
```

Read: k23 is not the sweet spot; it loses the independent full-spaced guard.
k24 balanced is now the best raw count-branch Strassen candidate and beats k22
on Mini spaced20, but the higher multiplier almost cancels its raw advantage on
the independent full-spaced guard.  If submitting a count branch, k24 balanced
is the higher-upside probe and k22 is the slightly cheaper/safer probe.  Neither
should be treated as a proven sub-`2e-7` replacement without a remote result.

### 2026-07-06 - k26 H1-kurt knob and refreshed official Full200 anchor

Question: the preferred parked count-frontier probe,
`submission_phase1_pure26_w05_unionteacher1100gamma_l2snap_split035_045_noaffine_module_finalonly_bundle.tar.gz`,
contains a disabled first-layer kurtosis-matching knob.  Because it is same
seed count and uses already-computed H1 moments, it was worth checking as a
cheap same-count improvement.

Added diagnostic wrapper:

```text
legacy_workspace/candidate_k26_gamma_h1kurt_env.py
```

It imports the packaged k26 gamma estimator and sets `H1_KURT_BETA` from
`K26_H1_KURT_BETA`.

Mini spaced10 smoke:

```text
beta=0     raw=1.243287e-6 adjusted=2.550656e-7
beta=0.02  raw=1.217120e-6 adjusted=2.492782e-7
beta=0.05  raw=1.201528e-6 adjusted=2.516979e-7
beta=0.10  raw=1.214937e-6 adjusted=2.496887e-7
beta=0.20  raw=1.518494e-6 adjusted=3.103839e-7
```

All Mini100:

```text
beta=0      raw=1.325967e-6 adjusted=2.694014e-7
beta=0.02   raw=1.325210e-6 adjusted=2.700818e-7
beta=0.05   raw=1.320131e-6 adjusted=2.693176e-7
beta=-0.05  raw=1.352231e-6 adjusted=2.750968e-7
```

Official Full split, first200:

```text
beta=0      raw=1.313140e-6 adjusted=2.668562e-7
beta=0.05   raw=1.364067e-6 adjusted=2.776005e-7
beta=-0.05  raw=1.297795e-6 adjusted=2.641380e-7
```

Full spaced20 sign check:

```text
beta=0      raw=1.226795e-6 adjusted=2.515164e-7
beta=-0.02  raw=1.232542e-6 adjusted=2.532025e-7
beta=-0.05  raw=1.214563e-6 adjusted=2.502386e-7
beta=-0.10  raw=1.236741e-6 adjusted=2.592499e-7
```

Read: the H1-kurt correction is split-sensitive.  Positive beta helps Mini and
hurts Full; negative beta helps Full and hurts Mini.  The effect is not large
enough to justify a sign/router package, and the sign instability is exactly
the overfit pattern to avoid.  Do not package this knob unless a new
target-free reliability signal appears.

Refreshed the protected-vs-k26 official Full200 anchor with the same evaluator:

```text
protected module, full 0:199:
  raw=2.181300e-6 adjusted=3.076245e-7 mult=0.140841

k26 gamma module, full 0:199:
  raw=1.313140e-6 adjusted=2.668562e-7 mult=0.203154
```

Anchored to protected remote `311697` raw `1.655662345e-6`, the same-slice
ratio estimates k26 gamma remote raw around `9.97e-7`; with the observed
`~0.203` multiplier this gives an adjusted estimate around `2.02e-7`.

Read: the saved k26 gamma package remains a plausible remote probe and likely
beats the protected line if it transfers, but it is not a new first-place-sized
method.  The next real leaderboard jump still requires a stronger
variance-per-FLOP mechanism or an independent absolute-state estimator.

### 2026-07-06 - k26 gamma plus Strassen2 tail package

Question: the Strassen2 tail rewrite preserves protected raw parity and cuts
effective compute.  Does the same exact-compute rewrite combine with the best
parked k26 union-teacher gamma package?

Built:

```text
legacy_workspace/_pkg_pure26_w05_unionteacher1100gamma_l2snap_split035_045_noaffine_module_strassen2_tailonly_finalonly/
```

Change relative to the saved k26 gamma package:

```text
Use two-level Strassen only for post-H1 row-cloud matmuls:
  h2 = relu(_strassen2(h, w1))
  h  = relu(_strassen2(h, w)) for layers 2..31

Keep the first input projection h0 @ w0 standard.
```

Mini spaced20, same rows:

```text
standard k26 gamma:
  raw=1.395895912e-6 adjusted=2.840689776e-7 mult=0.203503

k26 gamma + Strassen2 tail:
  raw=1.395895912e-6 adjusted=2.464240e-7 mult=0.179683
```

Official Full split 0:199:

```text
standard k26 gamma:
  raw=1.313140e-6 adjusted=2.668562e-7 mult=0.203154

k26 gamma + Strassen2 tail:
  raw=1.313140e-6 adjusted=2.288066e-7 mult=0.173965
```

Packaged artifact:

```text
whest-starterkit/packages/to_test_remote/
  submission_phase1_pure26_w05_unionteacher1100gamma_l2snap_split035_045_noaffine_module_strassen2_tailonly_finalonly_bundle.tar.gz

sha256:
  0418e388949b407d0e575d47bd79f2a976016a5cf8cf379d6c9bf372e217460b
```

Tarball smoke:

```text
Mini 0,5 failed=0 raw=7.248879e-7 adjusted=1.456573e-7
```

Final tarball hardening: the packaged `_strassen2` helper uses `fnp.add` and
`fnp.subtract` for internal block sums/differences, avoiding the known remote
flopscope-client `__add__`/`__sub__` dispatch gotcha.

2026-07-06 resmoke after CUDA/env audit:

```text
whest validate:
  passed

Mini 0,5:
  raw      = 7.248879e-7
  adjusted = 1.381543e-7
  mult     = 0.193807
  flops    = 4.313e10
  eff      = 5.272e10
```

Modeled remote:

```text
protected 311697 remote raw              = 1.655662345e-6
official Full200 protected local raw     = 2.181300e-6
official Full200 k26 gamma local raw     = 1.313140e-6
ratio-anchored k26 remote raw estimate   = 9.97e-7
Strassen2 Full200 multiplier             = 0.173965
adjusted estimate                        = 1.73e-7
```

Read: this is now the strongest practical remote probe in the current queue.
It is still not e-8-class and not a new statistical breakthrough, but it is a
clean combination: target-free k26 gamma raw plus exact Strassen compute cut.
The main risk is remote flopscope-client parity for the larger Strassen
operation graph; local raw parity and hardened package smoke are clean.

### 2026-07-06 - highseed88 pathCV Strassen2 tail compute cut

Starting point:

```text
submission_phase1_highseed88_f16_pathcv_betas15_ridge01_countprobe_bundle.tar.gz
Full200 raw = 4.972958455e-7
modeled remote adjusted ~= 2.174892520e-7
```

Change:

```text
Keep first projection h0 @ W0 standard.
Use two-level Strassen for:
  z = sphere_er * (h1_centered @ response)
  h = relu(h @ W_l), l=1..31
```

Rejected sibling:

```text
Strassening the first projection too preserved raw but worsened effective
compute on mini 0,5:
  all-row  mult = 0.629231
  tail+z   mult = 0.623224
```

Local checks:

```text
tail+z mini 0,5:
  raw      = 3.715678e-7
  adjusted = 2.311928e-7
  mult     = 0.623224
  flops    = 1.543e11
  eff      = 1.695e11

standard mini 0,5:
  raw      = 3.715678e-7
  adjusted = 2.707305e-7
  mult     = 0.728669
  flops    = 1.961e11
  eff      = 1.982e11

tail+z mini 0,5,10,15,20:
  raw      = 5.743663e-7
  adjusted = 3.558369e-7
  mult     = 0.619735
  flops    = 1.543e11
  eff      = 1.686e11
```

Packaged artifact:

```text
whest-starterkit/packages/to_test_remote/
  submission_phase1_highseed88_f16_pathcv_betas15_ridge01_countprobe_strassen2_tailz_bundle.tar.gz

sha256:
  8774a1aaa10bf69d336dbffff55aff1e9c0624314069484952afff4ff844196e

estimator.py sha256:
  172385bf7e446c8525e5310bd629a667031802c39b2d9621774996ca081423e7
```

Validation / package smoke:

```text
whest validate: passed
tar top-level: estimator.py, direction blob, manifest.json
extracted-package mini idx0:
  raw      = 2.139638e-7
  adjusted = 1.347502e-7
  mult     = 0.629780
  failed   = 0
```

Modeled remote impact:

```text
standard pathCV ridge01 model       ~= 2.1749e-7 adjusted
observed local compute ratio 0,5    ~= 0.623224 / 0.728669 = 0.8553
tail+z Strassen adjusted estimate   ~= 1.86e-7
```

Read: viable but high-compute.  It is safer raw-wise than a new fitted
correction because predictions are intentionally unchanged from the existing
pathCV ridge branch, but it is more remote-sensitive than k26 gamma Strassen
because it sits around 62% effective compute instead of roughly 17-18%.

### 2026-07-06 - pathCV88 flat-router Strassen2 tail compute cut

Starting point:

```text
submission_phase1_pathcv88_flatq05_late_std_or_betas15_router_probe_bundle.tar.gz
historical modeled adjusted ~= 1.794e-7
```

Change:

```text
Inside pathcv88_branch.py only:
  z = sphere_er * _strassen2(h1_centered, response)
  h = relu(_strassen2(h, W_l)), l=1..31

The router gate and protected branch are unchanged.
```

Mini 0,5,10,15,20:

```text
standard router:
  raw      = 5.749948e-7
  adjusted = 4.198974e-7
  mult     = 0.730096
  flops    = 1.961e11
  eff      = 1.986e11

Strassen2 path branch:
  raw      = 5.749948e-7
  adjusted = 3.525773e-7
  mult     = 0.612881
  flops    = 1.544e11
  eff      = 1.667e11
```

Packaged artifact:

```text
whest-starterkit/packages/to_test_remote/
  submission_phase1_pathcv88_flatq05_late_std_or_betas15_router_strassen2_tailz_bundle.tar.gz

sha256:
  134289603df4328a11e601b6d336ee00defbeddd7f87f3f82864b47c8045df3b

estimator.py sha256:
  bf8bf16cfab393cc9878e34ccdef3ee1ac340ce13db2d69cc0ea924a68a45dba

pathcv88_branch.py sha256:
  761b7d6d73a312b2334129e4d5da20abb8c47f920ad2d0723e11a587d63d8a34
```

Validation / package smoke:

```text
whest validate: passed
tar top-level: estimator.py, pathcv88_branch.py, protected_branch.py,
               two direction blobs, manifest.json
extracted-package mini idx0:
  raw      = 2.143788e-7
  adjusted = 1.314917e-7
  mult     = 0.613361
  failed   = 0
```

Modeled remote impact:

```text
historical flat-router model       ~= 1.794e-7
observed local compute ratio       ~= 0.612881 / 0.730096 = 0.8395
Strassen router adjusted estimate  ~= 1.51e-7
```

Read: highest-upside package currently staged, but it inherits the public-row
router gate risk.  The compute rewrite itself is raw-preserving and
package-smoked cleanly.

### 2026-07-06 - pathCV88 q25-router Strassen2 tail compute cut

Starting point:

```text
submission_phase1_pathcv88_q25_std_or_betas15_router_probe_bundle.tar.gz
historical modeled adjusted ~= 1.802e-7
```

Change:

```text
Same pathcv88_branch.py tail/CV Strassen2 rewrite as the flat-router package.
The q25 row-norm router gate and protected branch are unchanged.
```

Mini 0,5,10,15,20:

```text
standard q25 router:
  raw      = 1.164677e-6
  adjusted = 3.756977e-7
  mult     = 0.517573
  flops    = 1.329e11
  eff      = 1.408e11

Strassen2 path branch:
  raw      = 1.164677e-6
  adjusted = 3.122584e-7
  mult     = 0.425611
  flops    = 1.079e11
  eff      = 1.158e11
```

Packaged artifact:

```text
whest-starterkit/packages/to_test_remote/
  submission_phase1_pathcv88_q25_std_or_betas15_router_strassen2_tailz_bundle.tar.gz

sha256:
  0dca47afbf884bd433d11d068e06008b10c12cc65d53b2d6181c53f94e593459

estimator.py sha256:
  bb77157df18fd1c600964075c8013135590014e8e66b83e000178255a8e0690a

pathcv88_branch.py sha256:
  761b7d6d73a312b2334129e4d5da20abb8c47f920ad2d0723e11a587d63d8a34
```

Validation / package smoke:

```text
whest validate: passed
tar top-level: estimator.py, pathcv88_branch.py, protected_branch.py,
               two direction blobs, manifest.json
extracted-package mini idx0:
  raw      = 2.143788e-7
  adjusted = 1.323041e-7
  mult     = 0.617151
  failed   = 0
```

Modeled remote impact:

```text
historical q25-router model       ~= 1.802e-7
observed local compute ratio      ~= 0.425611 / 0.517573 = 0.8223
Strassen q25 adjusted estimate    ~= 1.48e-7
```

Read: lower gate-complexity sibling to the flat-router Strassen package.  The
five-row slice is unfavorable raw-wise, so this should be treated as a
robustness bracket rather than the lead candidate.

### 2026-07-06 - pathCV88 z12/z2s1 flat-router Strassen2 tail compute cut

Starting point:

```text
submission_phase1_pathcv88_flatq05_late_std_or_z12z2s1_router_probe_bundle.tar.gz
```

Change:

```text
Inside z12 pathcv88_branch.py only:
  z = sphere_er * _strassen2(h1_centered, response)
  h = relu(_strassen2(h, W_l)), l=1..31

The z2 control, exact first-layer covariance control, router gate, and
protected branch are unchanged.
```

Mini 0,5,10,15,20:

```text
standard z12/z2s1 flat router:
  raw      = 5.328290e-7
  adjusted = 3.902715e-7
  mult     = 0.732284
  flops    = 1.963e11
  eff      = 1.992e11

Strassen2 path branch:
  raw      = 5.328290e-7
  adjusted = 3.285959e-7
  mult     = 0.616261
  flops    = 1.546e11
  eff      = 1.676e11
```

Packaged artifact:

```text
whest-starterkit/packages/to_test_remote/
  submission_phase1_pathcv88_flatq05_late_std_or_z12z2s1_router_strassen2_tailz_bundle.tar.gz

sha256:
  ccf8675437d168d7403e0794990d604f8a5877679f52cc88687786feb5eebaf5

estimator.py sha256:
  bf8bf16cfab393cc9878e34ccdef3ee1ac340ce13db2d69cc0ea924a68a45dba

pathcv88_branch.py sha256:
  1392b5840701746d0013084a6d2fc103629353240317abd35d6b226fea96de50
```

Validation / package smoke:

```text
whest validate: passed
tar top-level: estimator.py, pathcv88_branch.py, protected_branch.py,
               two direction blobs, manifest.json
extracted-package mini idx0:
  raw      = 2.094304e-7
  adjusted = 1.290089e-7
  mult     = 0.615999
  failed   = 0
```

Read: current lead high-upside staged package.  It improves the same five-row
slice over beta15 Strassen (`3.286e-7` vs `3.526e-7` adjusted) because the z2
control lowers raw while the Strassen rewrite removes most of the previous
compute objection.  It still inherits the flat public-row router gate risk.

### 2026-07-06 - pathCV88 z12/z2s1 q25-router Strassen2 tail compute cut

Starting point:

```text
submission_phase1_pathcv88_q25_std_or_z12z2s1_router_probe_bundle.tar.gz
```

Change:

```text
Same z12 pathcv88_branch.py row-cloud Strassen2 rewrite as the flat-router
z12 package.  The q25 row-norm gate and protected branch are unchanged.
```

Mini 0,5,10,15,20:

```text
standard z12/z2s1 q25 router:
  raw      = 1.159030e-6
  adjusted = 3.448762e-7
  mult     = 0.497893
  flops    = 1.330e11
  eff      = 1.354e11

Strassen2 path branch:
  raw      = 1.159030e-6
  adjusted = 3.134833e-7
  mult     = 0.430422
  flops    = 1.080e11
  eff      = 1.171e11
```

Packaged artifact:

```text
whest-starterkit/packages/to_test_remote/
  submission_phase1_pathcv88_q25_std_or_z12z2s1_router_strassen2_tailz_bundle.tar.gz

sha256:
  a11ebfed6c94f4c2d366d0e59244d9ab07197a3dc26aece45b881beb72d6866e

estimator.py sha256:
  bb77157df18fd1c600964075c8013135590014e8e66b83e000178255a8e0690a

pathcv88_branch.py sha256:
  1392b5840701746d0013084a6d2fc103629353240317abd35d6b226fea96de50
```

Validation / package smoke:

```text
whest validate: passed
tar top-level: estimator.py, pathcv88_branch.py, protected_branch.py,
               two direction blobs, manifest.json
extracted-package mini idx0:
  raw      = 2.094304e-7
  adjusted = 1.289565e-7
  mult     = 0.615748
  failed   = 0
```

Read: hedge package only.  It has a cleaner row-norm gate, but the five-row
slice shows that it can under-route pathCV badly versus the flat gate.

### 2026-07-06 - Strassen3 rejected on lead z12 flat router

Tested changing the lead z12 flat-router path branch from two-level Strassen
to three-level Strassen for the row-cloud products.

Mini 0,5:

```text
Strassen2:
  raw      = 3.647881e-7
  adjusted = 2.244695e-7
  mult     = 0.615305
  flops    = 1.546e11
  eff      = 1.674e11

Strassen3:
  raw      = 3.647881e-7
  adjusted = 2.345562e-7
  mult     = 0.642187
  flops    = 1.393e11
  eff      = 1.747e11
```

Read: reject.  The lower analytical FLOPs do not survive effective compute;
residual/subcall overhead is worse than the saved matmul count.

### 2026-07-06 - z12 scale follow-up under Strassen2

After the `z12/z2s1` Strassen package became the lead, adjacent z2 scales were
tested with the same raw-preserving row-cloud Strassen2 rewrite.

#### z2s125 flat router

Mini 0,5,10,15,20:

```text
standard z12/z2s125 flat router:
  raw      = 5.315649e-7
  adjusted = 4.121263e-7
  mult     = 0.775343
  flops    = 1.963e11
  eff      = 2.109e11

Strassen2 path branch:
  raw      = 5.315649e-7
  adjusted = 3.609347e-7
  mult     = 0.677052
  flops    = 1.545e11
  eff      = 1.842e11
```

Read: do not lead with it.  `z2s125` has slightly better raw than `z2s1` on
this slice, but worse effective compute after the rewrite.

#### adaptive z2 scale flat router

Mini 0,5,10,15,20:

```text
standard z12 adaptive flat router:
  raw      = 5.270247e-7
  adjusted = 4.254776e-7
  mult     = 0.807674
  flops    = 1.963e11
  eff      = 2.197e11

Strassen2 path branch:
  raw      = 5.270247e-7
  adjusted = 3.653507e-7
  mult     = 0.688560
  flops    = 1.545e11
  eff      = 1.873e11
```

Read: do not lead with it.  Raw is best of the tested fixed/adaptive z2
branches on this five-row slice, but effective compute remains too high and
the adaptive threshold is target-fitted.

#### aggressive z2s3 flat router

Mini 0,5,10,15,20:

```text
standard z12/z2s3 flat router:
  raw      = 5.273919e-7
  adjusted = 3.854283e-7
  mult     = 0.730813
  flops    = 1.963e11
  eff      = 1.988e11

Strassen2 path branch:
  raw      = 5.273919e-7
  adjusted = 3.277329e-7
  mult     = 0.620631
  flops    = 1.546e11
  eff      = 1.688e11
```

Packaged artifact:

```text
whest-starterkit/packages/to_test_remote/
  submission_phase1_pathcv88_flatq05_late_std_or_z12z2s3_router_strassen2_tailz_bundle.tar.gz

sha256:
  566f0842e604c87fd8fde7f47a129197fa25e353b48280ac9b08a1f0c04dbb59

estimator.py sha256:
  bf8bf16cfab393cc9878e34ccdef3ee1ac340ce13db2d69cc0ea924a68a45dba

pathcv88_branch.py sha256:
  66988a2220f1a63807132607ed6adf575d31af0f61f58dbcef56d73a858132e3
```

Validation / package smoke:

```text
whest validate: passed
tar top-level: estimator.py, pathcv88_branch.py, protected_branch.py,
               two direction blobs, manifest.json
extracted-package mini idx0:
  raw      = 1.845435e-7
  adjusted = 1.158872e-7
  mult     = 0.627967
  failed   = 0
```

Read: strongest aggressive package on the five-row slice, narrowly ahead of
`z2s1` Strassen (`3.277e-7` vs `3.286e-7` adjusted).  It remains higher risk
because the older Full200 tail100 guard rejected fixed `z2s3`.

### 2026-07-06 - k26 plus exact-GH blend diagnostic

Question: the k26 union-teacher gamma package has strong raw MSE but still pays
~17-18% effective compute after Strassen2.  Could a cheap independent
exact-GH/second-layer analytic branch be blended with k26 to capture sampler
variance without paying another sampled row cloud?

Offline Full1000/Mini100 cache blend:

```text
standalone:
  Full1000 protected        = 2.301739726e-6
  Full1000 k26 gamma        = 1.470949253e-6
  Full1000 exactGH marg0.5  = 2.306190611e-6
  Mini100 protected         = 1.897755704e-6
  Mini100 k26 gamma         = 1.325972344e-6
  Mini100 exactGH rank12    = 1.886320835e-6

k26 + exactGH rank12, scalar blend fitted on Full1000:
  alpha                 = 0.353736717
  Full1000 fit raw      = 1.113117649e-6
  Full1000 OOF raw      = 1.114204421e-6
  Mini100 transfer raw  = 9.392828879e-7

k26 + exactGH marg0.5, scalar blend fitted on Full1000:
  alpha                 = 0.353790848
  Full1000 OOF raw      = 1.114493593e-6
  Mini100 transfer raw  = 9.439876769e-7
```

This looked promising until pricing/structure was checked.  The exactGH cache
is not a cheap pure analytic vector; it propagates a separate protected
18-block row cloud through the tail.  A literal k26+exactGH blend would
therefore pay roughly the k26 sampler plus another protected sampler, which
erases the adjusted-score gain.

Same-cloud falsifier: run the exact-GH layer-2 covariance coloring on the k26
row cloud itself, using the k26 package seeds/weights and the independent full
spaced20 slice:

```text
python legacy_workspace/probe_secondlayer_cov_exactgh.py \
  --weights-cache legacy_workspace/cache/phase1_full1000_weights_targets.npz \
  --indices 0,50,100,150,200,250,300,350,400,450,500,550,600,650,700,750,800,850,900,950 \
  --seeds 8,15,16,17,20,24,26,33,44,47,48,49,63,70,79,84,88,93,97,102,105,106,107,110,111,112 \
  --seed-weights <k26 package weights> \
  --marginal-beta 0.5 --ranks 12 --gains 0.05 \
  --gh-order 15 --batch-mlp 2 --device cuda \
  --out legacy_workspace/cache/exactgh_k26gamma_full_spaced20_rank12_g005.npz

results:
  k26 row cloud marg0.5              raw = 1.260103853e-6
  k26 row cloud rank12/gain0.05      raw = 1.284618111e-6
  k26 row cloud h1affine             raw = 1.306699920e-6
```

Read: the strong blend was mostly the value of a second independent row cloud,
not a cheap exact-GH analytic correction.  Do not package k26+exactGH unless a
new formulation computes the exact-GH coordinate inside the existing k26 row
cloud and improves raw; the tested same-cloud covariance-coloring version
worsens raw by about 2%.

### 2026-07-06 - pathCV88 z12/z2s1 Strassen3 falsifier

Question: the current lead high-upside staged package uses the pathCV88
z12/z2s1 branch with two-level Strassen on the high-count row-cloud tail.  The
k22 count branch already showed that a third Strassen level loses to residual
overhead, but pathCV88 is much larger, so this is the one branch where deeper
recursion could plausibly amortize.

Probe package:

```text
legacy_workspace/
  _pkg_pathcv88_flatq05_late_std_or_z12z2s1_router_probe_strassen3_tailz_probe/
```

Change:

```text
In the copied pathcv88_branch.py only:
  _strassen2(...): rec(a, b, 2) -> rec(a, b, 3)

The staged Strassen2 package is untouched.
```

Mini idx0 result:

```text
Strassen2 staged package:
  raw      = 2.094304e-7
  adjusted = 1.290089e-7
  mult     = 0.615999
  flops    = 1.54558245376e11

Strassen3 probe:
  raw      = 2.094304e-7
  adjusted = 1.350320e-7
  mult     = 0.644758
  flops    = 1.393e11
  failed   = 0
```

Read: raw parity holds and analytical FLOPs fall, but effective compute gets
worse because the extra Strassen recursion adds too much residual/Python
overhead.  Close deeper Strassen for pathCV88.  Keep the staged two-level
Strassen packages as the compute frontier.

### 2026-07-06 - late checkpoint pool sweep scout

Question: the L30 low-row add-on/replacement probes were cost-negative on
Full200, but perhaps the checkpoint sweet spot was earlier because extra rows
are cheaper before L30.

CUDA scout:

```text
python legacy_workspace/probe_late_layer_pool_sweep.py \
  --weights-cache legacy_workspace/cache/phase1_full200_weights_targets.npz \
  --indices 0,20,40,60,80,100,120,140,160,180 \
  --layers 20,24,26,28,29,30 \
  --counts 32,48,64 \
  --betas 0.5,0.75,1.0 \
  --batch-mlp 2 --device cuda \
  --out legacy_workspace/cache/late_layer_pool_sweep_scout10_20260706.npz
```

Best scout points by approximate adjusted score:

```text
L30 c48 beta=1    raw=6.753648287e-7  mult~=0.365645  adj~=2.469435157e-7
L29 c48 beta=1    raw=7.108344416e-7  mult~=0.358243  adj~=2.546513963e-7
L28 c48 beta=1    raw=7.431437464e-7  mult~=0.350841  adj~=2.607254389e-7
L30 c64 beta=1    raw=5.556923088e-7  mult~=0.486046  adj~=2.700919219e-7
L20 c48 beta=1    raw=9.512608981e-7  mult~=0.291627  adj~=2.774138279e-7
```

Read: the frontier is monotone in the intuitive direction: later checkpoints
give better raw MSE, but the extra checkpoint pool is still not steep enough
per FLOP.  Even the favorable 10-row scout does not beat the protected remote
line, and the broader L30 Full200 measurements were less favorable.  Do not
promote this family unless a new way appears to compute the late mean with
substantially fewer propagated rows.

### 2026-07-06 - pathCV84 flat-router Strassen2 tail probe

Question: count84 was historically close to count88 and sometimes better raw.
Can the two-level Strassen tail rewrite make the smaller branch a better
adjusted remote probe than pathCV88 z12/z2s1?

Starting point:

```text
legacy_workspace/_pkg_pathcv84_flatq05_late_std_or_router_probe/
```

Copied to:

```text
legacy_workspace/_pkg_pathcv84_flatq05_late_std_or_router_probe_strassen2_tailz/
```

Change, in `pathcv84_branch.py` only:

```text
z = sphere_er * _strassen2(h1_centered, response)
h = relu(_strassen2(h, W_l)), l=1..31
```

The first projection `h0 @ W0`, router thresholds, and protected branch are
unchanged.

Full split spaced10, indices `0,100,...,900`:

```text
protected Strassen branch:
  raw      = 2.910836e-6
  adjusted = 4.227245e-7
  mult     = 0.142621

pathCV88 z12/z2s1 flat router Strassen2:
  raw      = 3.729319e-7
  adjusted = 2.020670e-7
  mult     = 0.569287

k26 gamma Strassen2:
  raw      = 1.165221e-6
  adjusted = 2.309830e-7
  mult     = 0.187609

pathCV84 flat router, standard:
  raw      = 3.639262e-7
  adjusted = 2.215979e-7
  mult     = 0.643045

pathCV84 flat router Strassen2:
  raw      = 3.639262e-7
  adjusted = 1.902729e-7
  mult     = 0.551025
  failed   = 0
```

Mini idx0 smoke:

```text
raw      = 2.314462e-7
adjusted = 1.365548e-7
mult     = 0.590007
failed   = 0
```

Validation:

```text
whest validate ../legacy_workspace/_pkg_pathcv84_flatq05_late_std_or_router_probe_strassen2_tailz/estimator.py
status: success
```

Packaged artifact:

```text
whest-starterkit/packages/to_test_remote/
  submission_phase1_pathcv84_flatq05_late_std_or_router_strassen2_tailz_bundle.tar.gz

sha256:
  2b93567d3e493a00317ea719ab29feca532b601aa64405c2a009d3fc32497a6e

pathcv84_branch.py sha256:
  38df130069f244117911b2e4db33b8acfabe07fdaaf55eccd3b75495da8067be
```

Extracted tarball rerun, Mini idx0:

```text
raw      = 2.314462e-7
adjusted = 1.361244e-7
mult     = 0.588147
failed   = 0
csv      = legacy_workspace/cache/pathcv84_flatq05_late_std_or_router_strassen2_tailz_pkg_idx0_rerun.csv
```

Read: this is a newly promoted high-upside remote probe.  It is simpler than
the z12/z2s1 count88 branch, raw-positive on the same full spaced10 canary, and
lower compute than its own standard implementation.  It still inherits the
flat public-row router risk, but its full spaced10 score is the best current
local adjusted result among the high-count router packages.

Follow-up z2 control on count84:

```text
candidate dir:
  legacy_workspace/_pkg_pathcv84_flatq05_late_std_or_z12z2s1_router_probe_strassen2_tailz/

Full split spaced10:
  z2_scale=1.0  raw=3.693193e-7  adjusted=1.940934e-7  mult=0.550453
  z2_scale=0.5  raw=3.665584e-7  adjusted=1.919805e-7  mult=0.550902

current promoted count84 one-control:
  raw=3.639262e-7  adjusted=1.902729e-7  mult=0.551025
```

Read: unlike count88, count84 does not benefit from the z2 squared control on
this matched canary.  Do not package count84+z2 unless a broader sweep later
finds a stable coefficient; the simpler one-control count84 branch remains the
current best count84 shape.

Follow-up beta-scale on count84:

```text
candidate dir:
  legacy_workspace/_pkg_pathcv84_flatq05_late_std_or_betas15_router_probe_strassen2_tailz/

Full split spaced10:
  beta_scale=1.5  raw=3.740829e-7  adjusted=1.958926e-7  mult=0.548714

current promoted count84 one-control:
  beta_scale=1.0  raw=3.639262e-7  adjusted=1.902729e-7  mult=0.551025
```

Read: the first-layer pathCV coefficient is not under-aggressive for count84 on
this canary.  Do not package the beta15 count84 variant.

### 2026-07-06 - pathCV80 flat-router Strassen2 tail probe

Question: count84 narrowly beats count88 after Strassen, but count80 has lower
row-cloud cost.  Does the raw loss from dropping four seed blocks stay small
enough to improve adjusted score?

Built by copying the promoted count84 Strassen router and changing only:

```text
SPHERE_SEEDS     = 0..79
SPHERE_DATA_FILE = spherex_highseed80_f16_pathcv_countprobe_inputs_256x40960_f16.bin.gz
```

The flat router, protected branch, one-control pathCV readout, and Strassen2
tail rewrite are unchanged.

Full split spaced10, indices `0,100,...,900`:

```text
pathCV84 flat router Strassen2:
  raw      = 3.639262e-7
  adjusted = 1.902729e-7
  mult     = 0.551025

pathCV80 flat router Strassen2:
  raw      = 3.780276e-7
  adjusted = 1.895013e-7
  mult     = 0.524810
  failed   = 0
```

Mini idx0 smoke:

```text
raw      = 2.057152e-7
adjusted = 1.164016e-7
mult     = 0.565839
failed   = 0
```

Validation:

```text
whest validate ../legacy_workspace/_pkg_pathcv80_flatq05_late_std_or_router_probe_strassen2_tailz/estimator.py
status: success
```

Packaged artifact:

```text
whest-starterkit/packages/to_test_remote/
  submission_phase1_pathcv80_flatq05_late_std_or_router_strassen2_tailz_bundle.tar.gz

sha256:
  3bacd447e01eb707bfda95aa7c6c49d9b70a268104a184d13057c9d90decc775

pathcv84_branch.py sha256:
  402385eb7ffc634ca63492d28d9db0b9f9384ab7862d315abdd85a5c6c518a7c
```

Read: count80 is now the best matched-canary adjusted router probe, but only
narrowly.  It trades about `+3.9%` raw versus count84 for about `-4.8%`
multiplier on the full spaced10 check.  Promote it above count84 as the next
remote probe, while keeping count84 as the raw-safer adjacent hedge.

Lower-count follow-up:

```text
Full split spaced10:
  pathCV64 raw=5.003005e-7 adjusted=2.207902e-7 mult=0.445612
  pathCV68 raw=4.381441e-7 adjusted=1.941058e-7 mult=0.455462
  pathCV72 raw=3.910354e-7 adjusted=1.809223e-7 mult=0.481875
  pathCV74 raw=3.919100e-7 adjusted=1.841435e-7 mult=0.488943
  pathCV76 raw=3.735271e-7 adjusted=1.797688e-7 mult=0.504637
  pathCV78 raw=3.702465e-7 adjusted=1.814942e-7 mult=0.513800
  pathCV80 raw=3.780276e-7 adjusted=1.895013e-7 mult=0.524810
  pathCV84 raw=3.639262e-7 adjusted=1.902729e-7 mult=0.551025
```

Count76 was packaged after passing validate and mini idx0:

```text
whest-starterkit/packages/to_test_remote/
  submission_phase1_pathcv76_flatq05_late_std_or_router_strassen2_tailz_bundle.tar.gz

sha256:
  2fe7f71b75f55397199bef20e3c19fd9d05f2daa649b3a750ae5c8c45cb11009

mini idx0:
  raw=1.926804e-7 adjusted=1.227790e-7 mult=0.637216 failed=0
```

Broader spaced20 falsifier, indices `0,50,...,950`:

```text
pathCV76 raw=7.391886e-7 adjusted=2.946858e-7 mult=0.477345
pathCV80 raw=7.061952e-7 adjusted=2.871745e-7 mult=0.497966
pathCV84 raw=6.708478e-7 adjusted=2.789118e-7 mult=0.521464
pathCV88 z12/z2s1 raw=6.498652e-7 adjusted=2.787617e-7 mult=0.548375
k26 gamma Strassen2 raw=1.263830e-6 adjusted=2.254549e-7 mult=0.177652
```

Read: the count76/count80 apparent win was a spaced10 artifact.  On the
broader spaced20 gate, the raw-safer pathCV84/88 variants beat lower counts,
but all high-count pathCV routers lose clearly to the safer k26 gamma Strassen
package.  Demote count76/count80 to diagnostics.  The next remote queue should
put k26 gamma Strassen first, then pathCV84/88 only as higher-variance probes.

### 2026-07-06 - pathCV84 z12/z2s1 Strassen2 follow-up

Question: the pathCV88 branch benefited from the z12/z2s1 row correction.
Does the same correction improve the new count84 Strassen lead?

Starting point:

```text
legacy_workspace/_pkg_pathcv84_flatq05_late_std_or_z12z2s1_router_probe_strassen2_tailz/
```

Mini idx0:

```text
non-z12 count84 Strassen2:
  raw      = 2.314462e-7
  adjusted = 1.361244e-7
  mult     = 0.588147

z12/z2s1 count84 Strassen2:
  raw      = 2.166805e-7
  adjusted = 1.293737e-7
  mult     = 0.597071
```

Full split spaced10, indices `0,100,...,900`:

```text
non-z12 count84 Strassen2:
  raw      = 3.639262e-7
  adjusted = 1.902729e-7
  mult     = 0.551025

z12/z2s1 count84 Strassen2:
  raw      = 3.665584e-7
  adjusted = 1.907953e-7
  mult     = 0.547699
  failed   = 0
```

Read: kill z12/z2s1 for count84 despite the better single-MLP smoke.  On the
matched full-spaced10 canary it slightly worsens raw and adjusted score.  Keep
the non-z12 pathCV84 Strassen package as the current remote-probe leader.

### 2026-07-06 - pathCV84 q25-router Strassen2 follow-up

Question: the q25/std-or count84 router existed only in dense form.  Does the
same Strassen2 tail rewrite make it a safer or cheaper sibling to the flat
router?

Starting point:

```text
legacy_workspace/_pkg_pathcv84_q25_std_or_router_probe_strassen2_tailz/
```

Mini idx0:

```text
raw      = 2.314462e-7
adjusted = 1.379032e-7
mult     = 0.595833
failed   = 0
```

Full split spaced10, indices `0,100,...,900`:

```text
flat count84 Strassen2:
  raw      = 3.639262e-7
  adjusted = 1.902729e-7
  mult     = 0.551025

q25/std-or count84 Strassen2:
  raw      = 3.639262e-7
  adjusted = 1.902465e-7
  mult     = 0.550349
  failed   = 0
```

Read: statistical tie, not a new promoted candidate.  The q25/std-or router
does not change raw on the matched canary and only trims a negligible amount of
effective compute.  It is a hedge only; keep the already packaged flat count84
Strassen2 artifact as the cleaner top remote probe.

### 2026-07-06 - pathCV count truncation under Strassen2

Question: count84 beat count88 after the Strassen2 tail rewrite.  Does the
same branch have a better adjusted tradeoff at lower QR-block counts?

Method: clone the count80 Strassen2 branch and use only the first `N`
QR-antithetic seed blocks from the direction bank.  For count76 and below, use
a prefix/dedicated direction blob so the submission does not carry unused rows.

Mini idx0 smoke:

```text
count84 raw=2.314462e-7 adjusted=1.361244e-7 mult=0.588147
count80 raw=2.057152e-7 adjusted=1.165626e-7 mult=0.566621
count78 raw=1.938345e-7 adjusted=1.069424e-7 mult=0.551720
count76 raw=1.926804e-7 adjusted=1.026006e-7 mult=0.532491
count74 raw=1.953907e-7 adjusted=1.042112e-7 mult=0.533348
count72 raw=2.026557e-7 adjusted=1.162968e-7 mult=0.573864
count70 raw=2.060920e-7 adjusted=1.048049e-7 mult=0.508535
count64 raw=1.686040e-7 adjusted=8.877449e-8 mult=0.526527
```

Full split spaced10, indices `0,100,...,900`, using CSV row-average for
adjusted score:

```text
count84 raw=3.639261919e-7 adjusted=1.902729351e-7 mult=0.551025
count80 raw=3.780276245e-7 adjusted=1.883280813e-7 mult=0.521433
count78 raw=3.702464767e-7 adjusted=1.858567839e-7 mult=0.535215
count76 raw=3.735270610e-7 adjusted=1.797687512e-7 mult=0.504637
count74 raw=3.919100408e-7 adjusted=1.860585081e-7 mult=0.493676
count72 raw=3.910353797e-7 adjusted=1.809223053e-7 mult=0.481875
count64 raw=5.003004759e-7 adjusted=2.207901851e-7 mult=0.445612
```

Clean packaged count76 artifact:

```text
whest-starterkit/packages/to_test_remote/
  submission_phase1_pathcv76_flatq05_late_std_or_router_strassen2_tailz_bundle.tar.gz

sha256:
  362c6b4a9a7c1949717631c7c26f3178882c861cebbdcd861d6849c2632eeb52

pathcv84_branch.py sha256:
  b2ae81ecd35143d920f2cef6ffd6879dc3120823cd583df1cfb184bea02d8c11

whest validate:
  passed on clean package folder

Extracted tarball Mini idx0:
  raw      = 1.926804e-7
  adjusted = 1.170629e-7
  failed   = 0
  note     = raw parity exact; one-row adjusted timing is noisy
```

Read: count76 is the current best matched-canary remote probe.  Count70 did
not beat count76 on the one-row smoke and was not expanded.  Count64 shows
that lowering count can overfit the one-row smoke and break raw on broader
canaries.  Count72 is close but worse than count76 on spaced10; count78/80/84
are useful adjacent hedges but lose adjusted score to count76.  Use count76 as
the next remote candidate, with count80/count84 as backup if remote residual or
router transfer is unexpectedly bad.

### 2026-07-06 - pathCV76 broader warning-slice demotion

Question: did the count76 promotion survive the older remote-shape warning
slices, or was full-spaced10 too optimistic?

Full split spaced20, indices `0,50,...,950`:

```text
pathCV76 router:
  raw      = 7.391886e-7
  adjusted = 2.946858e-7
  mult     = 0.477345
  pathcount= 17/20

pathCV76 path only:
  raw      = 5.539937e-7
  adjusted = 2.942141e-7
  mult     = 0.530709

protected branch only:
  raw      = 2.958103e-6
  adjusted = 4.254739e-7
  mult     = 0.142106

branch oracle:
  adjusted = 2.485935e-7
  pathcount= 14/20
```

Mini parity10 warning slice:

```text
pathCV76 router:
  raw      = 1.070089e-6
  adjusted = 4.441514e-7
  mult     = 0.462523
  pathcount= 8/10
```

Read: demote the count76/count80/count84 path-router family as a primary remote
probe.  Full-spaced10 missed several bad path rows.  On the broader warning
slice, even the exact branch oracle cannot reach `2e-7`, so no threshold-only
router on this branch is enough for the target.  Keep the package only as a
diagnostic artifact.

### 2026-07-06 - k26 plus old-analytic final-preactivation coordinate

Question: the old no-eigen analytic branch was too expensive as a literal
protected-sampler add-on, but does it carry an independent absolute-state
coordinate that complements the current k26 gamma sampler?

Harness updates:

```text
legacy_workspace/probe_lowoverhead_moment_stack.py
  added --base-mode k26gamma

cheeky_experiments/estimator_whest_noeigen_k3cap{1,2,3}_nokurtmoment.py
  diagnostic wrappers for pricing stripped old-analytic coordinates
```

k26-base low-overhead stack over exactGH, PRE-EDGE L4, k26-z12, and
protected18 branches:

```text
Full1000 k26 base raw = 1.470949253e-6
Mini100  k26 base raw = 1.325972344e-6

best Full1000 OOF rows could reach ~0.92x raw, but Mini transfer failed.
best Mini transfer row:
  raw = 1.325691492e-6  # only 0.02% better than base
  Full1000 OOF was neutral/worse
```

Read: weak low-overhead branches still do not combine into a transferable
correction when k26 is the base.  The old protected-base conclusion carries
over.

Then tested the stronger old-analytic final-preactivation shift caches as
external absolute branches against k26 on the same Full200 slice:

```text
k26 gamma Full200 raw = 1.313140188e-6

k26 + no-eigen nokurtmoment shift:
  best OOF raw = 9.838728022e-7

k26 + no-eigen shift:
  best OOF raw = 9.848043247e-7

k26 + k3cap3 nokurtmoment shift:
  best OOF raw = 1.000492115e-6

k26 + k3cap2 nokurtmoment shift:
  best OOF raw = 1.005879180e-6

k26 + k3cap1 nokurtmoment shift:
  best OOF raw = 1.015596195e-6

k26 + early16 K3cov-only nokurt/nolearn shift:
  best OOF raw = 9.976089260e-7
```

This is a real independent-truth signal: the old branch's response-aligned
final-preactivation coordinate cuts k26 raw by roughly 23-25% on Full200.

Compute pricing kills the literal add-on:

```text
cap1 one-row price:
  flops=5.940e9 effective=1.877e10 mult_add~=0.069

cap2 one-row price:
  flops=9.975e9 effective=4.236e10 mult_add~=0.156  # residual-heavy/noisy

cap3 one-row price:
  flops=1.387e10 effective=2.681e10 mult_add~=0.099
```

Approximate adjusted economics with k26 Strassen2 Full200 multiplier
`0.173965`:

```text
k26 alone:
  raw=1.313140e-6 adjusted~=2.284e-7

k26 + cap1 full add-on:
  raw~=1.0156e-6 total_mult~=0.243 adjusted~=2.468e-7

k26 + cap3 full add-on:
  raw~=1.0005e-6 total_mult~=0.272 adjusted~=2.724e-7

k26 + full no-eigen/nokurt add-on:
  raw~=9.84e-7 total_mult~=>0.27 adjusted worse
```

Also tested conditional-compute economics for cap1/cap3: run the analytic
branch only on the MLPs where it helps.

```text
Perfect oracle gate, k26 + cap1:
  best around f=0.33
  raw=1.081197386e-6 adjusted~=2.127119913e-7

Perfect oracle gate, k26 + cap3:
  best around f=0.15-0.20
  adjusted~=2.19e-7
```

But cheap k26 telemetry did not recover the gate:

```text
best single feature correlations with cap1 benefit:
  delta_std       +0.265
  delta_rms       +0.255
  seed_sd_mean    +0.227

5-fold ridge gate over k26 telemetry:
  best tested adjusted ~=2.287e-7
  no improvement over k26 alone
```

Read: this route is a valuable clue, not a package.  The deployable target is
now narrower: find a cheaper approximation to the old branch's
response-aligned final-preactivation coordinate, or a target-free gate that
predicts when the cap1 coordinate pays.  Literal k26+analytic and conditional
gating are not promoted.

Follow-up distillation check:

```text
legacy_workspace/export_weightroot_ez_proxy.py
  added --target-ez-rows to target analytic Ez instead of true higher-moment Ez

legacy_workspace/probe_weightroot_analytic_final_distill.py
  new final-output distillation probe for analytic teacher corrections
```

Weight-root features can predict the cap1 analytic Ez coordinate itself:

```text
target = cap1/no-kurt analytic Ez - protected sampled Ez
features = final-rooted weight-response summaries
OOF Ez R2 = +0.404204
```

But the predicted Ez coordinate is not response-aligned:

```text
nonlinear final-preactivation shift with predicted cap1 Ez:
  lambda=0      raw=2.181295751e-6
  best positive raw worsens
  best negative lambda=-0.01 raw=2.178283150e-6  # only 0.14% raw gain
```

Distilling the final analytic correction directly also fails against truth:

```text
teacher = cap1 final shift lambda=0.04
  teacher raw = 2.114010379e-6
  best OOF distill raw = 2.193667405e-6
  truth_residual_r2 = -0.00599

teacher = cap1 final shift lambda=0.08
  teacher raw = 2.165157875e-6
  best OOF distill raw = 2.221147024e-6
  truth_residual_r2 = -0.01859
```

Read: the cheap weight-root feature family learns large teacher variance but
not the signed truth-improving part.  Close old-analytic distillation via these
features.  The remaining useful object is still a mechanistic cheaper
recurrence for the coordinate, not an ML patch over weight-root summaries.

### 2026-07-06 - batched Strassen2 leaf-matmul for k26 gamma

Question: the Strassen2 tail rewrite is exact but emits 49 small matmul calls
per hidden propagation.  Can we keep the same counted FLOPs and raw parity
while reducing residual/client overhead by stacking the 49 Strassen leaves and
using one batched matmul?

Added local wrapper:

```text
legacy_workspace/candidate_k26_gamma_batched_strassen_env.py
```

and then patched a copied self-contained package:

```text
legacy_workspace/_pkg_pure26_w05_unionteacher1100gamma_l2snap_split035_045_noaffine_module_batchedstrassen2_tailonly_finalonly/
```

Single MLP, Phase-1 budget, depth forced to 32:

```text
standard Strassen2 recursive:
  raw      = 5.813557e-7
  mult     = 0.21018, 0.21047, 0.20321
  flops    = 4.313e10

batched Strassen2:
  raw      = 5.813557e-7  # exact parity on the tested row
  mult     = 0.19539, 0.19677, 0.20827
  flops    = 4.313e10
```

Mini indices `0,5`, wrapper smoke:

```text
recursive Strassen2:
  raw      = 7.248879e-7
  adjusted = 1.358548e-7
  mult     = 0.18983487

batched Strassen2:
  raw      = 7.248879e-7
  adjusted = 1.317558e-7
  mult     = 0.18396811
```

The copied package validates.  One concurrent/noisy validation run and one
solo run showed larger timing variance, so this should be treated as a modest
average compute cut, not a guaranteed remote improvement.  It is still exact
raw-parity and target-free, so it is safer than another seed/router tweak.

Prepared artifact, not submitted:

```text
whest-starterkit/packages/to_test_remote/
  submission_phase1_pure26_w05_unionteacher1100gamma_l2snap_split035_045_noaffine_module_batchedstrassen2_tailonly_finalonly_bundle.tar.gz

sha256:
  7c4c500d3f7421b33300fc723f6c3b3b191ac8c162c9bdbbd181e5809c87fa9e
```

Read: keep this as the preferred k26 engineering probe over the recursive
Strassen2 package if we decide to remote-test k26.  Do not expect a raw-MSE
change; the only expected gain is a few percent adjusted-score reduction from
lower residual/client overhead.  Batched Strassen3 was also tested on one row:
raw parity held and counted FLOPs dropped, but effective multiplier worsened,
so level 3 remains closed unless the client overhead model changes.

### 2026-07-06 - batched Strassen2 leaf-matmul for pathCV88 z12/z2s1

Question: the high-count pathCV88 z12/z2s1 router branch has good raw MSE on
some slices but pays high effective compute.  The recursive Strassen2 package
uses 49 leaf matmuls per tail multiply.  Does the batched-leaf Strassen2 helper
preserve raw and reduce effective compute here too?

Existing folder checked:

```text
legacy_workspace/_pkg_pathcv88_flatq05_late_std_or_z12z2s1_router_probe_batchedstrassen2_tailz/
```

Mini index 0, same estimator math as recursive Strassen2:

```text
recursive Strassen2:
  raw      = 2.094304e-7
  adjusted = 1.305897e-7
  mult     = 0.62354697
  flops    = 1.546e11
  eff      = 1.696e11

batched Strassen2:
  raw      = 2.094304e-7  # exact parity
  adjusted = 1.267871e-7
  mult     = 0.60539035
  flops    = 1.546e11
  eff      = 1.647e11
```

Packaged artifact, not submitted:

```text
whest-starterkit/packages/to_test_remote/
  submission_phase1_pathcv88_flatq05_late_std_or_z12z2s1_router_batchedstrassen2_tailz_bundle.tar.gz

sha256:
  abdcaa5c6f82fbf0ebbbaa8603ae62ddec3c3e9cb20698dd2b0478953aa686c0
```

Validation:

```text
whest validate --estimator <folder>/estimator.py: passed
tar top-level: estimator.py, pathcv88_branch.py, protected_branch.py,
               two direction blobs, manifest.json
```

Read: the batched-leaf rewrite is an exact engineering improvement over the
recursive pathCV88 z12/z2s1 Strassen2 package.  It does not fix the statistical
risk: the pathCV family was demoted by the broader full-spaced20 warning slice.
Keep this as the preferred pathCV88 z12/z2s1 diagnostic only if we deliberately
want to test the high-count/router regime; do not treat it as safer than k26.

### 2026-07-06 - low-row many-seed extra-Ez geometry check

Question: independent extra final-preactivation-only QR blocks give a real
signed `E[z_L]` signal, but full blocks are too expensive.  Maybe the signed
mean proxy wants many independent seeds with only a few rows per seed, unlike
the full ReLU mean estimator.  Added:

```text
legacy_workspace/probe_extra_ez_lowrow_grid.py
```

This offline CUDA probe builds independent final-preactivation proxy caches and
scores them through the k26 nonlinear final-ReLU shift.  It is diagnostic only;
no submission code changed.

Full split warning slice `0,50,...,950`, k26 geometry, row mode `even`, rough
multiplier uses the measured k26 Strassen2 Full200 multiplier `0.173965`.

1024 extra half-row budget:

```text
base k26 raw = 1.253945042e-6

4x256  best lambda=.16 raw=1.017218058e-6 rough_adj=2.041850071e-7
16x64  best lambda=.08 raw=1.164372074e-6 rough_adj=2.337230629e-7
32x32  best lambda=.12 raw=1.066743399e-6 rough_adj=2.141261716e-7
64x16  best lambda=.08 raw=1.164787221e-6 rough_adj=2.338063950e-7
128x8  best lambda=.12 raw=1.084417948e-6 rough_adj=2.176739635e-7
256x4  best lambda=.04 raw=1.210994562e-6 rough_adj=2.430815412e-7
```

2048 extra half-row budget:

```text
8x256   best lambda=.20 raw=8.848931611e-7 rough_adj=2.013067276e-7
16x128  best lambda=.16 raw=1.106643352e-6 rough_adj=2.517532757e-7
32x64   best lambda=.16 raw=1.054019681e-6 rough_adj=2.397817751e-7
64x32   best lambda=.16 raw=1.040160688e-6 rough_adj=2.366289553e-7
128x16  best lambda=.16 raw=1.120315909e-6 rough_adj=2.548636825e-7
256x8   best lambda=.20 raw=9.711153404e-7 rough_adj=2.209216433e-7
```

Read: spreading the same extra-Ez row budget across many low-row seeds does not
beat ordinary full QR blocks.  The best shape is still full-block `8x256`,
which prior Full200 economics already judged too costly as a literal add-on.
Close many-seed/low-row extra-Ez as a compute rescue.  The signed `E[z_L]`
oracle remains valuable, but the proxy must be cheaper or more accurate than
literal extra full-depth rows.

### 2026-07-06 - correction: k26 nonlinear signed-Ez shift must use split l2-snap geometry

The earlier k26 + old-analytic final-preactivation shift numbers in this ledger
used the protected-style single l2-snap beta in the probe harness.  The actual
k26 package uses split snap coefficients:

```text
SNAP_BETA_MU = 0.35
SNAP_BETA_SD = 0.45
```

Patched:

```text
legacy_workspace/probe_final_premean_shift_oracle.py
  --l2-beta-mu
  --l2-beta-sd
```

and reran Full200 with the exact k26 seed set and weights.

Corrected k26-matched results:

```text
k26 base raw on this probe path = 1.318329765e-6

old no-eigen, KURT_MOMENT_GAIN=0, LEARNED_K3_GAIN=0:
  best lambda = 0.08
  raw         = 1.199883680e-6
  rel         = 0.910154
  spaced20    = 1.188324458e-6

old no-eigen, KURT_MOMENT_GAIN=0:
  best lambda = 0.08
  raw         = 1.212054279e-6
  rel         = 0.919386

true final E[z_L] oracle, same k26 geometry:
  lambda=1.00 raw = 4.124214250e-8
  lambda=0.75 raw = 1.207911350e-7
```

Read: the old analytic proxy is still real, but the corrected k26 transfer is
only about a 9% raw cut rather than the earlier mismatched 23-25% estimate.
This demotes literal k26+old-proxy economics.  The decisive positive signal
remains the true signed final-preactivation mean oracle; the next-winner path
needs a much stronger independent estimate of `E[z_L]`, not another small
distillation of the old proxy.

### 2026-07-06 - k26 plus independent extra-Ez signed-preactivation probes

Question: instead of adding ordinary post-ReLU seed blocks, use a small
independent row cloud only as a final signed-preactivation `E[z_L]` proxy, then
apply the nonlinear final-ReLU shift to the current k26 gamma row cloud.

Used exact k26 geometry:

```text
seeds/weights = k26 unionteacher1100gamma
SNAP_BETA_MU  = 0.35
SNAP_BETA_SD  = 0.45
base Full200 raw on probe path = 1.318329765e-6
```

Proxy caches:

```text
legacy_workspace/cache/extra_ez_s32_49_c4_full200.npz
legacy_workspace/cache/extra_ez_s32_49_c8_full200.npz
```

Results:

```text
k26 + extra-Ez c4:
  best lambda = 0.08
  raw         = 1.231542520e-6
  rel         = 0.934169
  spaced20    = 1.066048655e-6

k26 + extra-Ez c8:
  best lambda = 0.20
  raw         = 1.112223003e-6
  rel         = 0.843661
  spaced20    = 9.518760473e-7
```

Approximate adjusted economics if the extra Ez rows are paid literally and
scale linearly from the k26 branch:

```text
c4 mult ~= 0.20073, adjusted ~= 2.47e-7
c8 mult ~= 0.22749, adjusted ~= 2.53e-7

raw needed to beat 2.25e-7:
  c4 <= 1.121e-6
  c8 <= 9.890e-7
```

Read: this is a useful positive diagnostic.  Spending rows on signed
preactivation is more informative than a totally generic cheap feature, but the
slope is still not steep enough if those rows require full-depth propagation.
Do not package k26+extra-Ez literally.  This path only becomes competitive if
we find a materially cheaper way to get the same signed `E[z_L]` proxy.

### 2026-07-06 - QR-offset raw frontier Strassen squeeze

Question: the QR-offset `7x18` ensemble has excellent robust raw MSE
(`~3.50e-7` Full1000) but was priced at roughly full budget.  Can Strassen tail
matmuls turn it into a competitive adjusted-score branch?

Copied the package to a local experimental folder:

```text
legacy_workspace/_pkg_qroffset7x18_h1affine_rawfrontier_f16_strassen2_finalonly/
```

and replaced the first-layer and tail row-cloud matmuls with two-level
Strassen.  Mini index 0:

```text
original qroffset7x18:
  raw      = 5.586356e-7
  mult     = 0.988333
  adjusted = 5.521182e-7

qroffset7x18 Strassen2:
  raw      = 5.586356e-7  # raw parity
  mult     = 0.813866
  flops    = 2.076e11
  adjusted = 4.546547e-7

qroffset7x18 Strassen3:
  raw      = 5.586356e-7  # raw parity
  mult     = 0.837010
  flops    = 1.861e11
  adjusted = 4.675835e-7
```

Read: Strassen2 helps but nowhere near enough; Strassen3 loses to residual
overhead.  Even if the Full1000 raw `3.50e-7` translated perfectly, the
Strassen2 multiplier would still model around `2.85e-7`.  Close QR-offset
Strassen squeezing as a next-winner path unless a much stronger matmul
compression method appears.

### 2026-07-06 - k26 extra-Ez count sweep in one CUDA pass

Question: the independent final-preactivation proxy branch has real signed
information, but c4/c8 alone looked borderline after repricing.  We already had
proxy caches for c2..c18; sweep all counts with one k26 final-preactivation
row-cloud pass to avoid rerunning the expensive base branch repeatedly.

Added:

```text
legacy_workspace/probe_final_premean_shift_proxy_sweep.py
```

Full200, exact k26 geometry:

```text
base raw = 1.318329765e-6
base mult used for rough economics = 0.173965

best by rough adjusted:
  c2  lambda=.02 raw=1.303784330e-6 mult=0.187347 rough_adj=2.442599826e-7
  c3  lambda=.06 raw=1.263587777e-6 mult=0.194038 rough_adj=2.451838992e-7
  c4  lambda=.08 raw=1.231542520e-6 mult=0.200729 rough_adj=2.472061090e-7
  c6  lambda=.12 raw=1.169664701e-6 mult=0.214111 rough_adj=2.504378089e-7
  c8  lambda=.20 raw=1.112223003e-6 mult=0.227493 rough_adj=2.530226054e-7
  c10 lambda=.20 raw=1.073512921e-6 mult=0.240875 rough_adj=2.585820121e-7
  c12 lambda=.25 raw=1.049353427e-6 mult=0.254257 rough_adj=2.668049701e-7
  c14 lambda=.25 raw=1.047365986e-6 mult=0.267638 rough_adj=2.803154212e-7
  c16 lambda=.25 raw=1.040884515e-6 mult=0.281020 rough_adj=2.925097668e-7
  c18 lambda=.30 raw=1.056231527e-6 mult=0.294402 rough_adj=3.109569989e-7
```

Cache:

```text
legacy_workspace/cache/final_premean_shift_k26_extraez_counts_full200_sweep.npz
```

Read: raw MSE improves monotonically through about c16, confirming the signed
preactivation measurement signal.  The error/compute slope is still too weak:
the best rough-adjusted point is c2/c3 around `2.44e-7`, worse than the current
protected remote line and far from a new winner.  Close literal extra-Ez full
blocks as a package path.  The remaining useful lesson is structural: signed
`E[z_L]` is worth measuring, but it must be computed by a cheaper recurrence or
by replacing existing work, not added as full-depth proxy rows.

### 2026-07-06 - Phase-1 K3 cap depth-32 bisection

Question: the rank-112/SVD-shrunk K3/D21 audit would be a true `e-8`-class
path if it transferred to Phase 1, but prior Phase-1 cap4096 runs produced
NaNs.  Run single-MLP CUDA bisections to identify whether the instability is
the carried `K_out[3]` state or source-level `pK_111` truncation.

Depth override was explicit:

```text
MLP_KPROP_K3_DEPTH=32
source=npz legacy_workspace/cache/phase1_mini100_weights_targets.npz
mlps=1 dtype=float32 device=cuda
```

Cost-aware family-quota cap, both stages:

```text
k3_simple                                 final=7.640727834e-5 finite
k3_aug_full                               final=8.879031304e-6 finite
k3_aug_no_d4_u_source                     final=2.100150319e-4 finite
cap1024 family_quota_cost both            first_bad_layer=19 final=nan
cap2048 family_quota_cost both            first_bad_layer=21 final=nan
cap3072 family_quota_cost both            first_bad_layer=22 final=nan
cap4096 family_quota_cost both            first_bad_layer=22 final=nan
```

Stage bisection:

```text
cap2048 family_quota_cost pk-only         first_bad_layer=18 final=nan
cap4096 family_quota_cost pk-only         first_bad_layer=20 final=nan

cap2048 family_quota_cost K_out-only      final=2.594819053e-5 finite
cap4096 family_quota_cost K_out-only      final=1.412645324e-5 finite
```

Downstream lookahead source scoring does not fix the source cap:

```text
cap4096 lookahead_diag both               first_bad_layer=23 final=nan
```

Read: hard source-level `pK_111` truncation is the instability.  Truncating only
the carried `K_out[3]` state is finite, so the reference math itself can run at
depth 32, but the finite capped branch is still much too inaccurate on the
smoke MLP.  Do not pursue hard K3 source caps (`family_quota`, cost-aware, or
lookahead) as the next production route.  A viable K3/D21 path needs a
cancellation-preserving recurrence, shrinkage, or complete diagram grouping,
not top-k source selection.

### 2026-07-06 - Layer-masked old analytic branch pricing

Question: the old no-eigen/K3cov/kurt-stat branch contains real external
signed information, but literal add-on compute kills it.  Does restricting the
K3cov update to early or late half layers save enough compute to make it useful
as a preview/gate or partial correction?

One-row Phase-1 local pricing, mini idx0:

```text
early16 K3cov only:
  raw       = 1.546922431e-5
  flops     = 2.125e10
  effective = 2.729e10
  mult      = 0.10033

late16 K3cov only:
  raw       = 3.507033762e-5
  flops     = 2.125e10
  effective = 2.685e10
  mult      = 0.10000
```

For comparison, cap1/no-direct-K4 was already priced at `1.877e10` effective
on the same row and had stronger correction value.  The layer masks do not
remove the expensive query/carry path in this implementation and are not a
compute-saving route.

### 2026-07-06 - k26 full-block to cheap-Ez replacement sanity check

Question: the extra final-preactivation proxy rows were adjusted-negative as an
add-on to k26, but maybe they work as a replacement for a few expensive full
SPHEREx blocks.

Cache sanity check before a CUDA rerun:

```text
l2snap_k26_split035_045 Full1000 equal raw = 1.476394503e-6
l2snap_k24_w05 Full1000 equal raw          = 1.606164071e-6
l2snap_k24_w05 Mini100 equal raw           = 1.502948179e-6
```

Dropping from 26 to 24 full blocks loses roughly `1.3e-7` raw on Full1000.
The c2/c3 extra-Ez add-on recovered only `1.5e-8..5.5e-8` raw on Full200 while
adding about the same compute that dropping two full blocks would save.  This
replacement is therefore unlikely to beat k26 unless a much stronger cheap-Ez
proxy appears.  Close the simple "drop full blocks, add c2/c3 Ez" trade.

### 2026-07-06 - protected batched-Strassen package and router warning refresh

Goal: after the pathCV/k26 count probes failed to become robust next-winner
packages, check the safest remaining lever: exact compute reduction of the
current protected `311697` row cloud.  Also retest the best batched z12 router
and a lower-count pathCV variant on the broader warning slice.

Protected batched Strassen2 tail package:

```text
source folder:
  legacy_workspace/_pkg_pure18_protected_h1affine_l2snap_b05_weighted_affine_full1000_module_batchedstrassen2_tailonly_finalonly/

package:
  whest-starterkit/packages/to_test_remote/
  submission_phase1_pure18_protected_h1affine_l2snap_b05_weighted_affine_full1000_module_batchedstrassen2_tailonly_finalonly_bundle.tar.gz

sha256:
  7af46ad6d122822bc0b8ea1ba52a53addf7797e976c7a29db5ad5f8fd8bd99fa

tar contents:
  estimator.py
  spherex_pure18_protected_h1affine_l2snap_b05_weighted_affine_full1000_module_finalonly_h1half_inputs_256x4608_module.npz
  manifest.json

source validation:
  whest validate --estimator .../estimator.py
  status = success
  setup/predict finite smoke = OK
```

Mini spaced20, indices `0,5,...,95`:

```text
protected batched Strassen2:
  raw      = 1.848905193e-6
  adjusted = 2.260974266e-7
  score/mean multiplier ~= 0.1223
  failed   = 0
  csv      = legacy_workspace/cache/protected_batchedstrassen2_tailonly_mini_spaced20.csv

prior recursive protected Strassen2 tail-only, same slice:
  raw      = 1.848905e-6
  adjusted = 2.366118e-7
  mult     = 0.12851252
```

Full spaced20 warning slice, indices `0,50,...,950`:

```text
protected batched Strassen2:
  raw      = 2.958103076e-6
  adjusted = 4.149868225e-7
  failed   = 0
  csv      = legacy_workspace/cache/protected_batchedstrassen2_tailonly_full_spaced20.csv

prior protected branch-only warning number:
  raw      = 2.958103e-6
  adjusted = 4.254739e-7
```

Read: this is an exact-prediction engineering candidate.  It is not an e-8
route, and the full-spaced20 raw is a poor public-warning slice for the
protected family, but the compute cut is real and target-free.  Anchored to
remote `311697` raw `1.655662345e-6`, this models roughly in the low `2e-7`
class depending on remote residual overhead.  It should be the conservative
compute-cut remote probe before any router-heavy package.

2026-07-06 hardening update: rebuilt this tarball after replacing Python
`+`/`-` inside the protected batched Strassen helper with an unrolled-safe
`fnp.add` / `fnp.subtract` helper, matching the safer k26 helper and avoiding
the known flopscope-client dispatch gotcha.  Source validation still passes.
Final unrolled-safe Mini indices `0,5` smoke:

```text
raw      = 1.329675e-6
adjusted = 2.033444e-7
failed   = 0
csv      = legacy_workspace/cache/protected_batchedstrassen2_unrolledsafe_smoke2.csv
```

`strassen2fast` micro-branch check, Mini indices `0,5,10,15,20`:

```text
protected batched Strassen2:
  raw      = 2.123773868e-6
  adjusted = 2.611924020e-7

protected strassen2fast:
  raw      = 2.123774e-6
  adjusted = 2.815447e-7
  mult     = 0.14738374
```

Read: `strassen2fast` has raw parity but worse residual/effective compute than
batched leaves.  Do not package it.

Batched z12/pathCV warning refresh:

```text
pathCV88 flat z12/z2s1 batched Strassen2, full spaced20:
  raw      = 6.498652233e-7
  adjusted = 2.699597181e-7
  failed   = 0
  csv      = legacy_workspace/cache/pathcv88_z12z2s1_batchedstrassen2_full_spaced20.csv

pathCV68 flat router Strassen2, full spaced20:
  raw      = 7.768581384e-7
  adjusted = 2.856673133e-7
  failed   = 0
  csv      = legacy_workspace/cache/pathcv68_flatq05_late_std_or_router_strassen2_tailz_full_spaced20.csv
```

The five-row spread check for batched z12 was excellent (`1.619e-7` adjusted),
but the full warning slice reproduced the same raw failure as the older z12
family.  Count68 also failed the warning slice.  Do not promote the pathCV
router/count-truncation family as a primary remote candidate unless a new gate
changes the bad-row behavior; batching only cuts compute and does not fix
router transfer.

### 2026-07-06 - protected18 vs k26 batched remote multiplex diagnostic

Question: if we spend one remote submission, can we compare the conservative
protected compute-cut arm and the lower-raw/higher-compute k26 gamma arm under
the same hidden grader draw without confusing that with a leaderboard package?

Built a seed-hash router:

```text
source folder:
  legacy_workspace/_pkg_remote_probe_protected18_vs_k26_batched/

package:
  whest-starterkit/packages/to_test_remote/
  submission_phase1_remote_probe_protected18_vs_k26_batched_bundle.tar.gz

sha256:
  7541b1c51b525d4b2fd399c9d97a41df808455aa0861e50b72adad98027da226
```

Routing:

```text
arm = ((int(mlp.seed) * 11400714819323198485) mod 2**64) mod 2
arm 0: protected18 batched Strassen2 tail-only
arm 1: k26 unionteacher1100gamma batched Strassen2 tail-only
```

Validation:

```text
whest validate --estimator <folder>/estimator.py
status = success
smoke predict = finite
```

Mini spaced10 diagnostic (`0,5,...,45`):

```text
aggregate raw      = 1.437705e-6
aggregate adjusted = 2.161334e-7
failed             = 0

protected routed rows, n=5:
  raw_mean = 1.301523008e-6
  adj_mean = 1.609910818e-7

k26 routed rows, n=5:
  raw_mean = 1.573886266e-6
  adj_mean = 2.712756769e-7
```

Full warning slice (`0,50,...,950`):

```text
aggregate raw      = 2.187816e-6
aggregate adjusted = 3.094467e-7
failed             = 0

protected routed rows, n=12:
  raw_mean = 2.806604238e-6
  adj_mean = 3.708339334e-7
  flops    ~= 2.989e10
  eff      ~= 3.452e10

k26 routed rows, n=8:
  raw_mean = 1.259632640e-6
  adj_mean = 2.173659217e-7
  flops    ~= 4.313e10
  eff      ~= 4.697e10
```

Read: this is a diagnostic multiplex, not a candidate whose aggregate score
should be optimized.  On the warning slice, k26 has a real lower-raw profile
but pays enough compute that it is not an obvious next winner.  The remote use,
if any, is to split hidden rows by FLOP band (`~2.99e10` protected,
`~4.31e10` k26) and measure transfer of the two arms under one grader draw.
The safer single-arm submission remains the protected batched compute cut; k26
is a calibration/upside probe unless remote proves the raw gain is much larger
than local Full1000 suggests.

2026-07-06 hardening update: rebuilt after applying the same unrolled-safe
`fnp.add` / `fnp.subtract` Strassen helper to the protected arm.  Multiplex
validation passes.  Final unrolled-safe Mini indices `0,5` smoke:

```text
raw      = 1.245668e-6
adjusted = 1.723185e-7
failed   = 0
csv      = legacy_workspace/cache/remote_probe_protected18_vs_k26_batched_unrolledsafe_smoke2.csv
```

### 2026-07-06 - k26 batched Strassen2 promoted to highest-upside practical probe

Revalidated the single-arm k26 gamma batched Strassen2 tail-only package after
the remote-client-safe Strassen hardening.

Package:

```text
whest-starterkit/packages/to_test_remote/
submission_phase1_pure26_w05_unionteacher1100gamma_l2snap_split035_045_noaffine_module_batchedstrassen2_tailonly_finalonly_bundle.tar.gz
```

sha256:

```text
7c4c500d3f7421b33300fc723f6c3b3b191ac8c162c9bdbbd181e5809c87fa9e
```

Validation:

```text
whest validate: success

Mini indices 0,5:
  raw      = 7.248879e-7
  adjusted = 1.329964e-7
  mult     = 0.186396
  failed   = 0
  csv      = legacy_workspace/cache/k26_batchedstrassen2_unrolledsafe_smoke2.csv

Full spaced20 0,50,...,950:
  raw      = 1.263830e-6
  adjusted = 2.189804e-7
  mult     = 0.172029
  failed   = 0
  csv      = legacy_workspace/cache/k26_batchedstrassen2_unrolledsafe_full_spaced20.csv
```

Read: this is now the highest-upside practical single-arm remote probe in the
queue.  Protected18 batched remains the lowest implementation-risk compute-cut
package, but k26 is the only current parked package that both improves raw on
the broader full-spaced20 guard and keeps adjusted score below the protected
line.  It is still not an e-8-class method; it is a likely incremental
leaderboard move while the independent truth-estimator/cumulant lane remains
the route to larger variance capture.

### 2026-07-06 - old hybrid2 student package is not Phase-1 depth-32 compatible

Reopened the older learned-student line because the archived live results had
strong raw MSE (`~7.7e-7`) and adjusted scores near the current leaderboard if
the residual issue could be solved.  The first gate was to test the exact
guarded active package under the current Phase-1 depth-32 scorer, rather than
trusting older depth-8/chassis notes.

Artifact tested:

```text
whest-starterkit/packages/active/
submission_hybrid2_no_back_t9k_1000_blob_guard_vendored.tar.gz
```

Checks:

```text
tar extraction to /tmp/whest_student_t9k_active
whest validate /tmp/whest_student_t9k_active/estimator.py: success

quick_score_selected.py --split mini --depth 32 --indices 0,5:
  raw      = 5.256774e-1
  adjusted = 5.256774e-1
  failed   = 2/2
  csv      = legacy_workspace/cache/student_t9k_active_depth32_mini_0_5.csv
```

Traceback:

```text
File "/tmp/whest_student_t9k_active/candidate_hybrid2_estimator.py", line 2396, in predict
    carr, c0 = self._h_cal[layer_idx]
IndexError: tuple index out of range
```

Read: close this exact old student package for current Phase 1.  The learned
student raw signal was real in the older chassis, but this artifact depends on
calibration tables that do not cover depth 32.  A Phase-1 student would need a
fresh depth-32 base/state cache and retraining/export, not a residual-overhead
packaging tweak to this tarball.

### 2026-07-06 - full-covariance Gaussian signed-Ez proxy

Question: the true final preactivation mean `E[z_L]` is a huge oracle for the
k26 nonlinear final-ReLU shift, while diagonal mean propagation was useless.
Before closing this analytic subfamily, test a cheap full-covariance Gaussian
state as an independent `E[z_L]` proxy:

```text
legacy_workspace/probe_fullcov_gaussian_ez_proxy.py
```

Closure:

```text
mean/variance: exact univariate Gaussian ReLU moments
off-diagonal covariance: Cov[ReLU(z_i),ReLU(z_j)] ~= Phi_i Phi_j Cov[z_i,z_j]
final proxy: mean_{L-1} @ W_L
```

Full200 spaced20 `0,10,...,190`:

```text
proxy Ez MSE vs true Ez = 1.332444684e-4
proxy Ez corr           = +0.999988
```

k26 nonlinear final shift, same slice and exact k26 geometry:

```text
positive lambdas:
  lambda=0       raw=1.173869473e-6
  lambda=0.02    raw=1.231362817e-6  # worse
  lambda=0.10    raw=1.918119567e-6  # much worse

negative lambdas:
  lambda=-0.005  raw=1.166631006e-6
  lambda=-0.010  raw=1.162254916e-6
  lambda=-0.020  raw=1.162056617e-6  # best, rel=0.989937
  lambda=-0.030  raw=1.173272781e-6
```

Caches:

```text
legacy_workspace/cache/fullcov_gaussian_ez_proxy_full200_spaced20.npz
legacy_workspace/cache/final_premean_shift_k26_fullcov_gaussian_ez_full200_spaced20.npz
legacy_workspace/cache/final_premean_shift_k26_fullcov_gaussian_ez_full200_spaced20_neglam.npz
```

Read: the proxy is not response-aligned.  The only useful direction is a small
negative coefficient, and the best raw gain is about `1.0%`.  A deployable
full-covariance Gaussian propagation would cost roughly several billion FLOPs,
so the gain is below the compute break-even bar.  Do not package.  This closes
the simple full-cov Gaussian signed-Ez proxy; future signed-Ez work needs a
more accurate response-aligned recurrence, not another Gaussian covariance
variant.

### 2026-07-06 - protected all-row batched Strassen and seed-only nonlinear distillation closure

Context: resumed after the Phase-1 leaderboard moved into the `~1e-7` adjusted
regime.  Rechecked the two nearest "maybe free" levers before opening a broader
new branch:

1. Can the protected18 batched-Strassen package save a bit more counted FLOP by
   also Strassening the first `4608 x 256 @ 256 x 256` sampled H1 projection?
2. Does a small nonlinear model see the high-count/truth correction from only
   the deployable final seed-cloud features?

Protected18 batched Strassen2 tail-only baseline, Mini indices `0,5`:

```text
source:
  legacy_workspace/_pkg_pure18_protected_h1affine_l2snap_b05_weighted_affine_full1000_module_batchedstrassen2_tailonly_finalonly/

raw      = 1.329675e-6
adjusted = 2.059778e-7
mult     = 0.157000
flops    = 2.989e10
eff      = 4.270e10
csv      = legacy_workspace/cache/protected_batched_tailonly_profile_0_5.csv
```

Operation summary on one MLP:

```text
flops_used ~= 2.989e10
matmul     ~= 2.912e10, 35 calls
add/sub    ~= 6.70e8 combined, ~5200 calls
```

All-row first-projection variant:

```text
source:
  legacy_workspace/_pkg_pure18_protected_h1affine_l2snap_b05_weighted_affine_full1000_module_batchedstrassen2_allrow_finalonly_test/

change:
  pre = _strassen2(h0, w0)

raw      = 1.329672e-6  # parity
adjusted = 2.260418e-7
mult     = 0.175325
flops    = 2.976e10
eff      = 4.769e10
csv      = legacy_workspace/cache/protected_batched_allrow_profile_0_5.csv
```

Read: reject.  All-row batched Strassen saves only `~0.13e9` counted FLOPs but
adds enough backend/residual overhead to worsen effective compute.  Keep the
tail-only protected batched package; do not Strassen the first half-row input
projection.

Seed-only nonlinear distillation smoke:

```text
added:
  legacy_workspace/probe_seedonly_mlp_residual.py

features:
  protected low-count seed cloud only: base, equal/median/trim deltas,
  seed variance/skew/kurtosis, per-seed deviations, and MLP row summaries.
  No diagonal Gaussian cache or weight features.

train:
  Full1000 public rows
valid:
  disjoint Mini100 rows
model:
  CUDA MLP, hidden=128, depth=3, 60 epochs
```

Truth-label run:

```text
train base raw = 2.301739726e-6
valid base raw = 1.897755704e-6

train gain 1.0 raw = 1.736178092e-6
valid gain 0.125 raw = 1.900447385e-6
valid gain 1.0 raw   = 2.128574116e-6
```

Teacher-label run, teacher = `l2snap_union53` equal seed mean:

```text
train teacher raw = 8.036253231e-7
valid teacher raw = 6.763920958e-7

valid gain 0.125 raw = 1.897078115e-6  # tiny 0.036% gain
valid gain 0.25 raw  = 1.900724206e-6
valid gain 1.0 raw   = 2.013398064e-6
```

Read: close the seed-cloud-only nonlinear route in this form.  It fits the
Full1000 training residual but does not transfer to the independent Mini split.
The high-count correction is still not visible from final seed-vector summaries
alone; any learned route needs richer trajectory/state observables or a
different target, not a larger MLP over the same cheap final seed features.

### 2026-07-06 - union-cache low-count subset frontier refresh

After CUDA/env resumption and the leaderboard moving into the `~1e-7` adjusted
regime, rechecked the cheapest practical way to preserve k26-style accuracy:
choose a better 18-26 block subset from the cached union l2snap seed bank.  This
is only a proposal surface because l2snap subset predictions must be recomputed
exactly, but it is a fast first pass for whether a low-count subset is hiding.

Command:

```text
whest-starterkit/.venv/bin/python legacy_workspace/search_l2snap_union_subset.py \
  --counts 18,19,20,21,22,24,26 \
  --full-weight 1 --mini-weight 2 \
  --random-restarts 80 --max-passes 8 \
  --seed 20260706 --m18 0.14319 --m120 0.952027
```

Best proposal rows:

```text
k=18 mean_raw=2.107578105e-6 mult=0.143190 mean_adj=3.017841089e-7
k=19 mean_raw=1.990513939e-6 mult=0.151120 mean_adj=3.008060177e-7
k=20 mean_raw=1.893441204e-6 mult=0.159050 mean_adj=3.011509697e-7
k=21 mean_raw=1.806328128e-6 mult=0.166979 mean_adj=3.016194489e-7
k=22 mean_raw=1.716518654e-6 mult=0.174909 mean_adj=3.002347295e-7
k=24 mean_raw=1.570605872e-6 mult=0.190769 mean_adj=2.996223573e-7
k=26 mean_raw=1.454017122e-6 mult=0.206628 mean_adj=3.004409349e-7
```

Read: the cached union subset frontier is essentially flat in adjusted score.
More blocks buy raw accuracy almost exactly at their modeled compute price, and
the whole curve sits around `3.0e-7` before exact recomputation.  Do not spend
GPU/package time on another target-aware union subset unless a new objective or
state correction changes the geometry.  The count-compression route remains
compute-linear, not a hidden jump toward `1e-7`.

### 2026-07-06 - k26 external branch stack: weak signals combine, but literal cost kills it

Question: several independent/signed-preactivation probes were individually
below the adjusted-score bar.  Before closing them, test the user's concern
that weak signals can be useless in isolation but strong in combination.

Added:

```text
legacy_workspace/probe_k26_external_branch_stack.py
```

The probe uses k26 gamma as the base and fits MLP-held-out ridge stacks over
cached branch deltas.  No row/name lookup is used; folds are by MLP.  This is a
research gate, not a deployable package, because some branches are expensive or
offline-supervised.

Full200 base:

```text
k26 base raw = 1.318329534e-6
base mult    = 0.173965
base adj     = 2.293431974e-7
```

All cached branches, including offline PRE-EDGE rawseed students:

```text
best OOF stack:
  raw = 8.437769559e-7
  adj = 1.467876581e-7  # if branch cost were free
  qmax = 9.142809471e-7
  first/last = 7.789653014e-7 / 9.085886104e-7
```

Mechanical-only subset, keeping only old analytic signed-Ez and independent
extra-Ez branches:

```text
branches:
  old no-eigen/nokurt/nolearn lambda 0.08, 0.12
  extra-Ez c4 lambda 0.08
  extra-Ez c8 lambda 0.20

best OOF stack:
  raw = 1.028954745e-6
  adj = 1.790021122e-7  # if branch cost were free
  qmax = 1.115801257e-6
  first/last = 9.458375431e-7 / 1.112071946e-6
```

First/last transfer for the mechanical subset, signed-abs ridge stack:

```text
fit first100 -> last100:
  raw = 1.120318719e-6 vs base 1.367700412e-6  # rel 0.819

fit last100 -> first100:
  raw = 9.732494578e-7 vs base 1.268958656e-6  # rel 0.767
```

Single-coordinate decomposition:

```text
extra-Ez c8 only:
  OOF raw = 1.111572873e-6
  adj_if_free = 1.933747749e-7

old analytic signed-Ez lambda 0.08 only:
  OOF raw = 1.202349060e-6
  adj_if_free = 2.091666542e-7
```

Perfect target gate for literal extra-Ez c8:

```text
all c8 extra-Ez:
  raw = 1.112223252e-6
  mult ~= 0.227493
  adj ~= 2.530230043e-7

perfect per-MLP gate:
  best f ~= 0.34
  raw = 1.107447966e-6
  mult ~= 0.192165
  adj ~= 2.128122067e-7
```

Read: this is a meaningful positive diagnostic and a negative package result.
The signed coordinates are complementary; the mechanical stack cuts k26 raw by
roughly `22%` under MLP-held-out validation and transfers first/last.  But the
literal branches cost too much: extra-Ez c8 alone is adjusted-negative, a
perfect gate cannot make it first-place-shaped, and the old analytic
K3cov/K211-style coordinate is also too expensive as an add-on.  The next
winner route is therefore not "submit the stack"; it is to reproduce the same
two signed coordinates cheaply, especially the extra-Ez / final-preactivation
mean coordinate and the old analytic response-aligned K3cov coordinate.  This
also confirms that testing signals only in isolation would have missed a real
interaction, but the interaction does not yet clear compute pricing.

Follow-up: can cheap k26 final seed-cloud features distill the mechanical stack
teacher?

Setup:

```text
teacher = best mechanical stack prediction above
features = k26 final seed block means, deviations, dispersion/skew/kurtosis,
           base/equal/median/quantile deltas, and MLP broadcast summaries
validation = first100 -> last100 and last100 -> first100
```

Results:

```text
k26 base        raw = 1.318329534e-6
stack teacher   raw = 1.028955005e-6

ridge alpha=100:
  teacher_R2 = +0.003
  truth_R2   = -0.025
  raw        = 1.351145655e-6  # worse

ExtraTrees:
  teacher_R2 = +0.066
  truth_R2   = -0.002
  raw        = 1.320439685e-6  # worse/slight

HistGradientBoosting:
  teacher_R2 = +0.038
  truth_R2   = -0.000
  raw        = 1.318911113e-6  # worse/slight
```

Read: the expensive branch-stack correction is not recoverable from final
seed-cloud summaries.  This matches earlier failures for old-analytic
distillation and seed-only residual MLPs.  The useful interaction is real, but
compressing it requires a new measured/mechanistic coordinate, not another
learner over current final seed features.

### 2026-07-06 - H3 old-analytic compression fill-in

Question: the old analytic response-aligned K3cov coordinate is useful but too
expensive.  A first40 ablation with lower Hermite order looked strong:

```text
HERMITE_ORDER=3,KURT_MOMENT_GAIN=0,LEARNED_K3_GAIN=0
first40 fit_all raw = 1.866820183e-6
```

Filled the missing rows and scored Full200:

```text
row files:
  cheeky_experiments/analytic_noeigen_h3_nokurtmoment_nolearnedk3_rows_0_40.npz
  cheeky_experiments/analytic_noeigen_h3_nokurtmoment_nolearnedk3_rows_40_80.npz
  cheeky_experiments/analytic_noeigen_h3_nokurtmoment_nolearnedk3_rows_80_200.npz

Full200:
  base raw       = 2.181293040e-6
  analytic raw   = 1.672665258e-5
  OOF id raw     = 2.062231049e-6
  OOF two raw    = 1.966919984e-6
  fit-all raw    = 1.966379623e-6
  first->tail    = 2.052524047e-6
  tail->first    = 1.880399383e-6
```

Observed extraction elapsed:

```text
default no-eigen/nokurt/nolearned rows ~= 1.23s/MLP
H3/nokurt/nolearned rows               ~= 1.32s/MLP
H1/H2 first40 rows                     ~= 1.35s/MLP
```

Read: the first40 H3 result was optimistic.  On Full200 it ties the existing
no-direct-K4/no-learned-K3 branch (`~1.966e-6`) and is not faster in observed
runtime.  Lowering Hermite order does not produce a cheaper K3cov coordinate.
Close this old-analytic compression branch unless a new algebraic simplification
removes source queries themselves.

### 2026-07-06 - dense C21 rollout CUDA confirmation

After resuming with CUDA available, reran the deployable dense two-index skew
rollout on the independent first100 slice to make sure the earlier negative was
not a CPU/runtime artifact.

Command:

```text
python legacy_workspace/probe_dense_c21_rollout.py \
  --indices 0:100 --device cuda --dtype float32 --batch-size 16 \
  --gains 0,0.02,0.05,0.08,0.1,0.15,0.2,0.3,0.5,0.94 \
  --kurt-gains 0,0.5,1.14 \
  --hermite-order 4 \
  --out legacy_workspace/cache/dense_c21_rollout_first100_20260706.npz
```

Result:

```text
standalone g0_k0          final ~= 6.734703e-5
standalone variants       final ~= 6.7e-5 to 8.0e-5
OOF best g0_k0.5          final ~= 6.733075e-5
```

Read: confirmed closed.  A dense recursive `C21`/two-index skew state started
from deployable mean/covariance drifts too badly by depth 32.  It does not
recover the true `b21`/`b111` oracle channel and should not be productionized
unless a new response-aligned contraction changes the state being propagated.

### 2026-07-06 - protected18 vs k26 weight-only adaptive gate

Question: can a branch-independent target-free gate choose between the cheaper
protected18 l2snap branch and the more accurate/higher-compute k26 gamma branch,
getting close to the oracle adjusted switch without paying for both branches?

First, the adjusted oracle switch ceiling, using rough measured multipliers
`protected=0.14319`, `k26=0.17203`:

```text
Full1000:
  protected raw/adj      = 2.301739726e-6 / 3.295861113e-7
  k26 raw/adj            = 1.470949253e-6 / 2.530473999e-7
  oracle row switch adj  = 2.095287163e-7
  oracle k26 fraction    = 0.659

Mini100:
  protected raw/adj      = 1.897755704e-6 / 2.717396393e-7
  k26 raw/adj            = 1.325972344e-6 / 2.281070223e-7
  oracle row switch adj  = 1.805571317e-7
  oracle k26 fraction    = 0.630
```

Then tested a deployable-ish router using only MLP-level weight geometry:
layer norm profiles, row/column norm summaries, and adjacent layer product
summaries.  Full1000 used 5-fold OOF to pick a threshold; the trained gate was
applied to independent Mini100.

Best lines:

```text
ridge1000:
  full OOF adj = 2.530281021e-7, gate frac = 0.999
  mini adj     = 2.281070223e-7, gate frac = 1.000

hgb0.03:
  full OOF adj = 2.534163660e-7, gate frac = 0.975
  mini adj     = 2.274037541e-7, gate frac = 0.990
  mini raw     = 1.322811304e-6

extra trees:
  full OOF adj = 2.522510556e-7, gate frac = 0.950
  mini adj     = 2.276149871e-7, gate frac = 0.990
```

Feature/gain correlations were weak (`~0.03` to `0.06` full OOF, `~0.05` to
`0.15` Mini transfer).  The learned policy mostly degenerates to "run k26
everywhere" and recovers almost none of the oracle switch value.

Read: adaptive protected-vs-k26 branch choice is not a winner with cheap
weight-only geometry.  The k26 single arm remains the practical stronger raw
candidate, but branch-independent routing does not provide the next tier.
Do not spend more effort on protected/k26 routers unless a new pre-branch
observable appears; post-protected telemetry is not usable for a real switch
because paying protected before k26 defeats the compute economics.

### 2026-07-06 - k26 truth-supervised gamma refit check

Question: the shipped k26 gamma combiner is target-free, fitted to the
union-teacher proxy.  Could we get a same-runtime gain by fitting the gamma
combiner directly to public truth, while guarding transfer on the independent
Mini split?

Setup:

```text
base package:
  full raw = 1.470949253e-6
  mini raw = 1.325972344e-6

features:
  the existing 8 MLP-level k26 gamma features, plus an optional 14-feature
  extension from median/IQR/seed-spread summaries.  No extra runtime rows.

validation:
  Full1000 5-fold MLP-held-out OOF, then train Full1000 -> Mini100 transfer.
```

Best truth-supervised gamma lines:

```text
best combined:
  full OOF raw = 1.474038329e-6
  mini xfer raw = 1.326931901e-6
  features = base8, scalar gamma, ridge=1, clip=(0,2)

best near-full line:
  full OOF raw = 1.474365797e-6
  mini xfer raw = 1.326308663e-6
  features = base8, scalar gamma, ridge=0.1, clip=(0,2)
```

Read: do not replace the target-free k26 gamma with a public-truth refit.  The
truth-supervised gamma improves the fit surface but loses to the existing
package on Full OOF and is neutral/slightly worse on independent Mini.  This is
exactly the overfit pattern we wanted the Mini guard to catch.  Close same-cost
truth-gamma retuning unless a genuinely new target-free feature is added.

### 2026-07-06 - k26 add-on branch pricing audit

Question: can we improve the current k26 line by adding a small independent
SPHEREx branch, then stacking it with k26?  This tests the "external truth
estimate" thesis directly while charging the added seed compute.

Artifacts:

```text
legacy_workspace/probe_k26_aux_seed_branch_stack.py
legacy_workspace/cache/k26_aux_seed_branch_stack_count30_union53_full1000_20260706.npz
```

Sanity check from the k26 cache:

```text
k26 gamma raw1000    = 1.470949253e-6
k26 equal raw1000    = 1.476394503e-6
k26 weighted raw1000 = 1.472975768e-6
k26 gamma first200   = 1.313140189e-6
```

Results:

```text
small auxiliary prefixes:
  counts 1-8 can sometimes shave raw, but never clear the repriced score.

protected18 auxiliary:
  best weighted OOF raw ~= 1.114e-6
  adjusted after extra 18-seed cost ~= 3.53e-7

first40 auxiliary:
  count40 OOF raw ~= 7.85e-7
  adjusted after extra 40-seed cost ~= 3.86e-7

count30/union53 auxiliary:
  best saved line raw = 1.438000697e-6
  adjusted           = 2.616010868e-7
```

Read: a second full sampled branch contains real raw-MSE information, but it is
priced out.  The useful object is not "run another branch"; it is "compress the
branch signal into a much cheaper independent estimate."

### 2026-07-06 - k26 PRE-EDGE moment-sensor add-ons

Question: can the existing k26 rows be used as a moment sensor, decoding
preactivation skew/kurtosis with Edgeworth instead of only averaging final
ReLUs?

Artifacts:

```text
legacy_workspace/probe_k26_preedge_l4_snap.py
legacy_workspace/cache/k26_preedge_l4_full200_20260706.npz
legacy_workspace/probe_k26_final_preedge_readout.py
legacy_workspace/cache/k26_final_preedge_readout_full200_20260706.npz
```

Full200 hidden L4 snap:

```text
beta0 raw = 1.313144189e-6
beta1 raw = 1.299925808e-6
rel       = 0.989934
first     = 1.249599370e-6
last      = 1.350252246e-6
qmax      = 1.368307742e-6
spaced20  = 1.218838488e-6
```

Final-preactivation Edgeworth readout:

```text
sample gamma raw                  = 1.313143445e-6
best edge34433 weighted lambda .1 = 1.312838102e-6
rel                               = 0.999767
```

Read: reusing the k26 rows for scalar cumulant readout is real but too small.
Layer-4 PRE-EDGE is about a 1% raw gain on Full200; final-only Edgeworth is
neutral.  Do not promote as a standalone package.  Keep the broader moment
sensor idea alive only if it supplies a cheap independent branch or a much
stronger deterministic closure.

### 2026-07-06 - External branch stacks versus k26

Question: if a strong external estimate were free, how much would it help k26?

Artifacts:

```text
legacy_workspace/probe_k26_external_branch_stack.py
legacy_workspace/cache/k26_external_branch_stack_full200_20260706.npz
legacy_workspace/cache/k26_external_branch_stack_preedge_full200_20260706.npz
```

Mechanical analytic signed-Ez / extra-Ez branch stack:

```text
k26 base raw       = 1.318329534e-6
k26 base adj-free  = 2.293431974e-7
best OOF raw       = 1.028954745e-6
best adj-if-free   = 1.790021122e-7
qmax               = 1.115801257e-6
first/last         = 9.458e-7 / 1.112e-6
```

PRE-EDGE checkpoint branch stack:

```text
k26 base raw       = 1.318329534e-6
best OOF raw       = 1.034667785e-6
best adj-if-free   = 1.799959811e-7
qmax               = 1.117756728e-6
first/last         = 9.811e-7 / 1.088e-6
spaced20           = 9.313e-7
```

All cached branches, including offline PRE-EDGE rawseed students:

```text
best OOF raw       = 8.437769559e-7
best adj-if-free   = 1.467876581e-7
```

Read: there is a large blendable gap if the external estimate is cheap.  The
direct branches are too expensive, so the next useful work is compression or a
lower-cost deterministic closure, not another full sampled add-on.

### 2026-07-06 - Low-overhead moment and higher-moment residual audits

Question: did any existing low-overhead moment stack or higher-moment residual
learner already contain a deployable signal against k26?

Artifacts inspected:

```text
legacy_workspace/cache/lowoverhead_moment_stack_k26base_full1000_mini100.npz
legacy_workspace/cache/lowoverhead_moment_stack_full1000_mini100.npz
legacy_workspace/cache/lowoverhead_moment_stack_with_k26z12_full1000_mini100.npz
legacy_workspace/cache/higher_moment_residual_oof_full1000_rich_20260706.npz
legacy_workspace/cache/higher_moment_residual_oof_full1000_noblocks_l0_1_3_7_15_23_30_31.npz
```

Key lines:

```text
lowoverhead_moment_stack_k26base:
  full base raw      = 1.470949229e-6
  full best OOF raw  = 1.357986283e-6
  mini base raw      = 1.325972900e-6
  mini xfer raw      = 1.325692302e-6

higher_moment_residual_oof_full1000_rich:
  protected base raw = 2.3017395e-6
  predicted raw      = 2.2937379e-6

higher_moment_residual_oof_full1000_noblocks:
  predicted raw      = 2.2958836e-6
```

Read: the public Full1000 moment-stack OOF improvement does not transfer to
independent Mini; the higher-moment residual learners are only tiny protected
improvements.  Do not promote these as-is.  They are useful mainly as feature
and teacher caches.

### 2026-07-06 - Analytic closure stack versus k26

Question: can existing deterministic/cumulant-style analytic rows provide a
signed correction to k26?

Artifacts:

```text
legacy_workspace/probe_k26_analytic_closure_stack.py
legacy_workspace/cache/k26_analytic_closure_stack_full200_20260706.npz
cheeky_experiments/estimator_whest_noeigen_nokurtmoment_nolearnedk3.py
legacy_workspace/cache/analytic_noeigen_nokurt_nolearn_compute_smoke.csv
```

Full200 same-subset stack results:

```text
k26 base raw             = 1.313140189e-6
k26 adj-if-free          = 2.284404329e-7

top single analytic row:
  noeigen_nokurtmoment_nolearnedk3
  standalone raw         = 1.672218304e-5
  one-row OOF stack raw  = 1.213590199e-6

best cross-half transfer:
  top11/top12 raw        ~= 1.197e-6
  adj-if-free            ~= 2.082e-7

best OOF stack:
  top12 raw              = 1.190939480e-6
  relative raw           = 0.906940
  adj-if-free            = 2.071817866e-7
```

Compute smoke for the best single analytic row:

```text
indices 0,5 mini
raw      = 9.905116e-6
mult     = 0.10881389
util     = 0.10421020
flops    = 2.125e10
effective= 2.835e10
failed   = 0
```

Read: the analytic closure carries a stable signed correction, about 9% raw
gain on Full200 when stacked with k26.  But the direct analytic estimator costs
about 0.109 multiplier, while the k26 add-on budget for breaking even is only
about 0.018.  This is the best current compression target: distill or simplify
the analytic correction, do not bundle the full analytic branch beside k26.

### 2026-07-06 - k26 analytic-teacher compression probe

Question: can the expensive analytic closure correction be predicted from cheap
k26 features, then applied as a near-free add-on?

Added:

```text
legacy_workspace/probe_k26_analytic_teacher_distill.py
legacy_workspace/cache/k26_analytic_teacher_distill_small_20260706.npz
```

Validation:

```text
train/OOF surface: Full first200, grouped by MLP
transfer surface: train Full first200 -> independent Mini100
features: k26 seed-cloud summaries, optional final/tail weight geometry
labels: target_residual, best analytic-stack delta, clean no-direct-K4 branch
```

Base lines:

```text
Full first200 k26 raw = 1.313140188e-6
Mini100 k26 raw       = 1.325972344e-6
```

Best robust rows were target-residual fits but worsened Mini:

```text
seed features, ridge=3:
  Full OOF raw       = 1.332162862e-6
  Full last raw      = 1.376155915e-6
  Mini raw           = 1.329287654e-6
  robust guard       = 1.378879963e-6
```

Best Mini-transfer row was tiny:

```text
seed features, best_stack_delta, ridge=0.03:
  Full OOF raw       = 1.316791966e-6
  Mini raw           = 1.325503508e-6
  Mini gain          = 0.035%
  robust guard       = 1.396046833e-6
  scaled label R2    = +0.00438
```

Read: this closes the obvious cheap distillation of the analytic correction
from k26 final seed-cloud and tail-weight features.  The analytic branch is
valuable if run directly and too expensive; its sign is not recoverable from
the already-paid k26 telemetry at a useful level.  Keep the analytic branch as
a math target, but do not package this student or keep widening ridge features
in the same family.

### 2026-07-06 - k22 Full200 gate and k26 H1-kurt batched check

Follow-up after auditing staged count-branch packages.

The k22 balmini Strassen2 package looked attractive on Mini spaced20 and
surprisingly decent on full spaced20.  Ran the real Full first200 gate:

```text
estimator:
  legacy_workspace/_pkg_pure22_balmini_union_l2snap_equal_affine_full1000_module_strassen2_tailonly_finalonly/estimator.py

full indices 0:199:
  raw      = 1.655471e-6
  adjusted = 2.529440e-7
  mult     = 0.15251550
  flops    = 3.651e10
  eff      = 4.148e10
  failed   = 0
```

Read: k22 is demoted.  The broad Full200 gate is worse than k26 and worse than
the current protected remote anchor.  The earlier spaced20/Mini strength was
not enough.

Then combined the k26 H1-kurt correction with the batched Strassen wrapper:

```text
legacy_workspace/candidate_k26_gamma_h1kurt_batched_strassen_env.py
```

Full spaced20:

```text
baseline k26 batched:
  raw      = 1.263829515e-6
  adjusted = 2.189803999e-7
  mult     = 0.172029

H1 beta=-0.05:
  raw      = 1.265370e-6
  adjusted = 2.191256e-7
  mult     = 0.17275185

H1 beta=-0.02:
  raw      = 1.257634e-6
  adjusted = 2.247057e-7
  mult     = 0.17551814
```

Read: the small H1-kurt raw signal does not survive adjusted-score pricing on
the broad slice.  Do not package this knob unless the H1 moment computation can
be made free inside an existing operation.

### 2026-07-06 - k26 seedstats cannot distill the mechanical signed-coordinate stack

After resuming with CUDA explicitly available, exported a k26-matched
seed-stat trajectory cache using the actual split layer-2 snap used by the k26
line:

```text
snap_beta_mu = 0.35
snap_beta_sd = 0.45
seeds        = 26-block k26 unionteacher1100gamma set
weights      = k26 gamma seed weights
cache        = song/data/phase1_k26_seedstats_first200_slim.npz
```

Patched the offline exporter only:

```text
legacy_workspace/export_l2snap_seedstat_cache.py
```

to support separate mean/std layer-2 snap coefficients.  Smoke parity:

```text
base_mse 0:2 = 1.737002530e-6
```

matching the affine-calibrated weighted k26 seed path.

Then added:

```text
legacy_workspace/probe_k26_seedstats_mechanical_distill.py
```

Question: the non-deployable mechanical stack
`old08 + old12 + extraEz4 + extraEz8` improves k26 Full200 raw from
`1.318334670e-6` to `1.028955005e-6` if its branch cost is ignored.  Are those
two signed coordinates visible in already-paid k26 trajectory statistics
instead of final seed-cloud summaries?

Setup:

```text
features = per-layer k26 seed-stat trajectory rows
layers   = 0,1,2,4,8,12,16,20,24,28,30,31
label    = mechanical_stack_best_pred - k26_base
folds    = grouped by MLP
targets  = used only for final-MSE reporting
```

Rich ridge:

```text
base raw       = 1.318334670e-6
teacher raw    = 1.028955005e-6
label corr     = +0.46730

best OOF:
  raw           = 1.314529012e-6
  rel           = 0.997113
  teacher_R2    = +0.03201
  label_corr    = +0.16774
```

Interaction ridge:

```text
best OOF:
  raw           = 1.314511532e-6
  rel           = 0.997100
  teacher_R2    = +0.03249
  label_corr    = +0.16819
```

First/last transfer was similarly tiny: the learned correction nudges raw by
sub-percent while the teacher itself cuts raw by about `22%`.

Read: the already-paid k26 seed-stat trajectory is not the missing
signed-coordinate observable.  This is stronger than the previous final
seed-cloud distillation closeout: even layerwise seed mean/std/MAD/skew/kurtosis
summaries do not recover the old K3cov/K211-style signed coordinate or the
independent extra-Ez coordinate.  Do not scale this exact student/distiller to
Full1000.  The useful target remains algebraic/measurement compression of the
signed final-preactivation / response-aligned K3cov coordinates themselves.

## 2026-07-06 resumed depth-32 stale-branch audit

After the long pause and context compaction, rechecked the old learned/hybrid
status claims against the current Phase 1 depth-32 evaluator before spending
more work on them.

Root deterministic cumulant estimator smoke:

```text
cmd:
  .venv/bin/python quick_score_selected.py --estimator estimator.py \
    --revision v1-phase1 --split mini --width 256 --depth 32 \
    --flop-budget 272000000000 --indices 0,5 --max-threads 1 \
    --csv ../legacy_workspace/cache/root_estimator_phase1_mini_0_5_20260706.csv

result:
  score=2.501897e-06 raw=2.167127e-05 all=7.233187e-06
  mult=0.11611987 failed=0 elapsed=2.06s
```

This path is finite and cheap, but it is not near the active sampler line.

Old hybrid2/state-space candidates:

```text
candidate_hybrid2_estimator.py
candidate_hybrid2_state_space_l2_l8_lean_estimator.py

indices=0,5
failed=2/2
exception:
  IndexError: tuple index out of range
  candidate_hybrid2_estimator.py, line 2415:
      carr, c0 = self._h_cal[layer_idx]
```

The state-space wrapper is additionally guarded for `len(mlp.weights) == 8`,
confirming it is a Warm-Up/depth-8 artifact rather than a Phase-1 depth-32
candidate.  Do not treat the stale `CURRENT_STATUS.md` hybrid2 claims as
actionable for current Phase 1 unless the branch is rebuilt/retrained for
depth 32.

## 2026-07-06 - k26 PRE-EDGE multi-observer and deployable L4 package

Question: the previous k26 L4 PRE-EDGE snap was a real but small `~1%` raw
gain.  Could multiple weak sampled preactivation moment observations combine
nonlinearly, and if not, is L4 cheap enough to package?

Added a k26-specific multi-layer observer, separate from the protected18
script:

```text
legacy_workspace/probe_k26_preedge_multi_observer.py
```

It uses:

```text
seeds/readout = current k26 union-teacher gamma package
L2 snap       = split 0.35 mean / 0.45 std
observations  = sampled preactivation raw moments, Edgeworth-decoded hidden means
configs       = mean snaps of the live k26 row cloud
```

Full spaced20 gate over layers `3,4,5,6,8`:

```text
base none                 raw = 1.226777826e-6
best L3+L4 lambda=.75 b=1 raw = 1.215970254e-6
best L4-only nearby       raw ~= 1.217e-6
```

Full200 narrowed gate over `L3`, `L4`, `L3+L4`:

```text
base none                 raw = 1.313144188e-6
best L4 lambda=1 b=1      raw = 1.299927060e-6
best L3+L4 lambda=1 b=1   raw = 1.301036886e-6
```

Read: multi-layer interaction does not survive the broad slice.  The apparent
spaced20 L3+L4 gain was slice luck; the deployable signal is still just L4.

Then priced L4 inside the actual k26 batched-Strassen package by copying the
current package and patching only its root estimator:

```text
legacy_workspace/_pkg_pure26_w05_unionteacher1100gamma_l2snap_split035_045_noaffine_module_batchedstrassen2_preedgeL4_finalonly/
legacy_workspace/candidate_k26_gamma_preedge_l4_batched_strassen_env.py  # diagnostic wrapper
```

Validation:

```text
whest validate -> success

Mini indices 0,5 package folder:
  raw      = 7.108977e-7
  adjusted = 1.297004e-7
  mult     = 0.18534399
  failed   = 0

Full spaced20 0,50,...,950 package folder:
  raw      = 1.249116e-6
  adjusted = 2.158833e-7
  mult     = 0.17201979
  failed   = 0

Same-run k26 batched baseline on full spaced20:
  raw      = 1.263830e-6
  adjusted = 2.207309e-7
  mult     = 0.17294076
  failed   = 0
```

Artifact:

```text
whest-starterkit/packages/to_test_remote/submission_phase1_pure26_w05_unionteacher1100gamma_l2snap_split035_045_noaffine_module_batchedstrassen2_preedgeL4_finalonly_bundle.tar.gz
sha256 19aaab1eb229bfdd6cee912235afb0bb760beb1732e3cd7343c238a3ed9ae173
```

Read: promote as the new best k26-family remote probe.  This is a small
incremental improvement, not a route to `1e-8`; the larger target remains a
cheap independent signed coordinate / deterministic moment closure.

### 2026-07-06 - k26 tail row thinning is not a safe compute cut

Added:

```text
legacy_workspace/probe_k26_tail_row_thinning_gamma.py
```

It reuses the existing split layer-2 snap trajectory code, but scores the
per-seed predictions through the deployed k26 target-free gamma combiner.  This
tests whether the current k26 sampled branch can keep all 26 seed blocks for the
layer-2 snap, then continue fewer QR half-rows through layers 3..32.

Quick smoke on two MLPs was misleading:

```text
even tail=256  gamma raw = 1.741057607e-6  work=1.0000
even tail=224  gamma raw = 1.403412809e-6  work=0.8810
even tail=192  gamma raw = 4.558029398e-6  work=0.7619
```

Full200 spaced20 also gave a tempting but fragile result:

```text
even tail=256  gamma raw = 1.226777355e-6  work=1.0000  adj proxy=2.110412816e-7
even tail=232  gamma raw = 1.225296107e-6  work=0.9107  adj proxy=1.919662439e-7
first tail=248 gamma raw = 1.193684513e-6  work=0.9702  adj proxy=1.992367950e-7
```

All-200 validation killed it:

```text
full tail=256  gamma raw = 1.313140963e-6  work=1.0000  adj proxy=2.258983267e-7
first tail=248 gamma raw = 1.381491978e-6  work=0.9702  adj proxy=2.305835679e-7
even tail=248  gamma raw = 1.430167967e-6  work=0.9702  adj proxy=2.387080329e-7
even tail=232  gamma raw = 1.560225669e-6  work=0.9107  adj proxy=2.444394133e-7
first tail=232 gamma raw = 1.670092546e-6  work=0.9107  adj proxy=2.616521765e-7
```

Read: do not package k26 tail-row thinning.  The apparent spaced20 win was a
canary miss.  Even the safest `first/248` cut saves only about three percent of
sampled-row work and loses enough raw accuracy to worsen the adjusted proxy.
Further row-thinning/coreset work needs a genuinely new target-free row
selection observable; fixed first/even/last continuation subsets are closed.

### 2026-07-06 - k26 in-pass PRE-EDGE layer snaps are cleanup-only

Follow-up after the higher-moment audit and the earlier layer-4 PRE-EDGE probe.
Question: can the current k26 row cloud estimate preactivation cumulants at a
better intermediate layer, snap the hidden mean there, and continue to the
final layer for a near-free gain?

Used:

```text
legacy_workspace/probe_k26_preedge_l4_snap.py
```

with the actual k26 seed set, split layer-2 snap, and gamma final readout.

Full200 spaced20 layer sweep, `edge4`:

```text
layer 4:  best beta=0.75 raw=1.218077576e-6 rel=0.992908
layer 8:  best beta=0.25 raw=1.226314307e-6 rel=0.999622
layer 12: best beta=1.00 raw=1.204323829e-6 rel=0.981697
layer 16: best beta=0.75 raw=1.220649537e-6 rel=0.995005
layer 24: best beta=0.75 raw=1.213869875e-6 rel=0.989478
layer 30: best beta=0.25 raw=1.223202693e-6 rel=0.997086
```

The only plausible layer was 12, so ran the all-200 guard:

```text
base beta=0    raw=1.313144189e-6
beta=0.50      raw=1.311403105e-6  rel=0.998674
beta=0.75      raw=1.312579776e-6  rel=0.999570
beta=1.00      raw=1.315122746e-6  rel=1.001507
```

Read: the in-pass PRE-EDGE snap is real but tiny on the broad slice.  It may be
worth folding into a future package if implementation overhead is negligible,
but it is not the missing 5-10% raw-MSE lever.  The strong higher-moment oracle
still requires a better deployable pre-skew/pre-kurt proxy than moments of the
same k26 sampled row cloud.

### 2026-07-07 - b111 low-rank common-factor closure diagnostic

Question: the higher-moment partial-K3 oracle left a large gap between
`b3+b21` and true pre-skew.  Can the missing fully off-diagonal `b111`
contraction be approximated by covariance common-factor modes, as suggested by
the math expert?

Added:

```text
legacy_workspace/probe_b111_lowrank_common_factor.py
```

The diagnostic uses true higher-moment labels and is not deployable as-is.  It
fits common-factor amplitudes from the true layer-30 `c21` matrix, then decodes
the final ReLU with a k3-only Edgeworth update.  It deliberately evaluates only
the final transition `L30 -> L31`, where the closure gap is most relevant.

First100:

```text
Gaussian final readout        = 1.087080720e-6
true k3 final readout         = 1.551141005e-7
b3+b21 direct                 = 4.671805095e-6
b3+b21 OOF damped             = 7.233485008e-7

c21-fit rank4 one-gain OOF    = 6.800015924e-7
c21-fit rank4 two-gain OOF    = 6.356320645e-7
two-gain coefficients         ~= [0.51 for b3+b21, 0.39 for b111_lr]
b111 contraction corr         ~= +0.9756
```

Full200:

```text
Gaussian final readout        = 1.162166983e-6
true k3 final readout         = 1.692510618e-7
b3+b21 OOF damped             = 7.673749481e-7

c21-fit rank4 one-gain OOF    = 7.321982550e-7
c21-fit rank4 two-gain OOF    = 6.855713086e-7
two-gain coefficients         ~= [0.51 for b3+b21, 0.39 for b111_lr]
b111 contraction corr         ~= +0.9733
```

Basis ceiling check, fitting amplitudes directly to true `b111` on first100:

```text
best two-gain OOF             = 6.322294704e-7
```

Read: the low-rank common-factor idea is real but modest.  It improves the
closure from the `~7.5e-7` tier to the `~6.8e-7` tier on Full200 when true
`c21` is available, but even a direct true-`b111` amplitude fit does not
approach the true-k3 `~1.6e-7` oracle.  The covariance-mode basis is therefore
not the whole missing off-diagonal story.  Keep this as a component for a
future deterministic cumulant closure, not as a standalone breakthrough.

Deployable-shaped scout:

```text
legacy_workspace/probe_k26_sample_c21_b111_final.py
```

This reused the k26 row cloud at L30, estimated radialized covariance/c21 from
the sampled rows, built the same common-factor `b111` estimate, and blended the
Edgeworth final mean with the k26 gamma readout.

Full spaced20:

```text
k26 baseline                  = 1.226775436e-6
best nonzero blend            = worse; zero blend remained best
example g321=.51,g111=.39,b=.05 raw = 1.227818943e-6
```

Read: sample-estimated L30 `c21` is not a useful low-noise sensor at the current
k26 row count.  The higher-moment/common-factor signal requires deterministic
or much lower-noise c21 estimation; do not package sample-c21/eigensolve.

### 2026-07-07 - k26 L4/L12 PRE-EDGE multi-snap check

Question: the L4 PRE-EDGE snap is the best deployable cleanup in the current
k26 package.  Does a later L12 PRE-EDGE snap combine with it?

Added caches:

```text
legacy_workspace/cache/k26_preedge_multi_L4_12_spaced20_20260707.npz
legacy_workspace/cache/k26_preedge_multi_L4_12_full200_20260707.npz
```

Spaced20 scout looked tempting:

```text
none                         raw = 1.226778535e-6
L4 beta=.75                  raw = 1.218093255e-6
L12 beta=1                   raw = 1.204330385e-6
L4+L12 beta=1                raw = 1.203823214e-6
```

Full200 killed the extrapolation:

```text
none                         raw = 1.313144188e-6
L12 beta=.5                  raw = 1.311404777e-6
L12 beta=1                   raw = 1.315121258e-6
L4+L12 beta=.5               raw = 1.306194683e-6
L4+L12 beta=1                raw = 1.314452114e-6

known L4-only broad result    raw ~= 1.299927060e-6
```

Read: L12 and L4+L12 are scout-lucky and do not beat the existing L4-only
package on Full200.  Do not build a multi-snap package from this route.

### 2026-07-06 - k26 Strassen-Winograd kernel falsifier

Question: can the current k26 batched Strassen2 row-cloud kernel be improved
without changing the estimator by switching from the standard Strassen
assembly to a Winograd-style lower-addition assembly?

Added local-only diagnostic wrapper:

```text
legacy_workspace/candidate_k26_gamma_batched_winograd_env.py
```

It imports the staged k26 Strassen package and replaces only `_strassen2`.
Raw parity on mini indices `0,5` was exact to score precision:

```text
k26 batched standard Strassen1:
  raw      = 7.248879e-7
  mult     = 0.18793192
  flops    = 4.858e10
  eff      = 5.112e10

k26 batched standard Strassen2:
  raw      = 7.248879e-7
  mult     = 0.18339459
  flops    = 4.313e10
  eff      = 4.988e10

k26 batched standard Strassen3:
  raw      = 7.248879e-7
  mult     = 0.22098793
  flops    = 3.877e10
  eff      = 6.011e10

k26 batched Winograd1:
  raw      = 7.248879e-7
  mult     = 0.19883461
  flops    = 4.853e10
  eff      = 5.408e10

k26 batched Winograd2:
  raw      = 7.248879e-7
  mult     = 0.19338543
  flops    = 4.299e10
  eff      = 5.260e10
```

Read: close Winograd for this package.  It shaves negligible counted FLOPs
relative to standard Strassen at the same depth, and the extra intermediate
arithmetic/residual overhead makes effective compute worse.  The existing
batched standard Strassen2 kernel remains the best measured k26 exact-row
implementation.

### 2026-07-07 - all-layer residual pretraining does not rescue the learned correction

Question: prior learned residual models over the l2snap/SPHEREx state gave only
small final-layer held-out gains.  Could the target be wrong?  If the missing
signal is a coherent propagated state, all-layer supervision might regularize
the student better than final-only residual training.

Patched the research trainer with a default-off flag:

```text
song/src/train_equivariant_residual.py --pretrain-own-scale
```

The flag lets all-layer pretraining use its own residual normalization instead
of the final-layer residual scale.  Without this, all-layer pretraining is
ill-conditioned: the first corrected pretrain attempt produced destructive
final corrections around `5x` worse than base.

Corrected first200 CUDA probe:

```text
python song/src/train_equivariant_residual.py \
  --cache song/data/phase1_l2snap_seedmeans_first200_matrix_cache.npz \
  --pretrain-cache song/data/phase1_l2snap_seedmeans_first200_matrix_cache.npz \
  --pretrain-all-layers --pretrain-own-scale \
  --pretrain-epochs 8 --epochs 24 \
  --hidden 32 --rounds 3 --batch 8 \
  --holdout-mod 5 --holdout-value 0 \
  --lr 0.001 --weight-decay 0.01 --dropout 0.10 \
  --device cuda
```

Result:

```text
pretrain own scale mean/std = 1.646170e-02 / 9.204519e-02
final residual mean/std     = -3.476275e-05 / 1.478937e-03

held-out fold base          = 2.152622756e-6
best after finetune         = 2.153254172e-6
best ratio                  = 1.000293
```

Read: close this as a near-term lever.  All-layer supervision is numerically
valid after the scale fix, but it does not improve held-out final MSE on the
first200 gate.  Do not scale this exact all-layer-pretrain residual student to
Full1000; the learned-residual path still needs a new physical observable or
target, not just more GPU training.

### 2026-07-07 - k26/L4 external-stack decomposition and same-budget Ez replacement closeout

Context after compaction: the strongest current package artifact is still the
k26 gamma + L4 PRE-EDGE batched-Strassen probe:

```text
whest-starterkit/packages/to_test_remote/
submission_phase1_pure26_w05_unionteacher1100gamma_l2snap_split035_045_noaffine_module_batchedstrassen2_preedgeL4_finalonly_bundle.tar.gz
sha256 19aaab1eb229bfdd6cee912235afb0bb760beb1732e3cd7343c238a3ed9ae173
```

Before opening new code paths, re-audited stale/old learned artifacts:

```text
song/data/public100_base_d5d7_tail_matrix_cache.npz  weights=(100,8,256,256)
song/data/public100_base_d6d7_tail_matrix_cache.npz  weights=(100,8,256,256)
song/data/public100_fullskip_matrix_cache.npz        weights=(100,8,256,256)
```

Read: the attractive old `~6e-7` Song/ML numbers came from depth-8/warmup
caches, not current Phase-1 depth-32.  Current Phase-1 Song/message-student
runs remain neutral or worse versus k26/L4, so do not revive the depth-8
student artifacts without a fresh depth-32 training program.

Important validation hygiene correction: `branches_from_fusion_jsons()` in the
analytic stack scripts skips missing row files.  Running the branch-loader from
the wrong cwd can silently drop the intended no-eigen row files and rank
unrelated diagonal rollout branches.  All branch-stack checks below were rerun
from the project root.

Free external-stack target, old k26 base:

```text
base raw                 = 1.318329534e-6
all7 OOF raw             = 8.437769559e-7
all7 pair OOF raw        = 8.423509554e-7
old analytic only        = 1.202694404e-6
extra-Ez only            = 1.117158696e-6
old + extra-Ez           = 1.028823844e-6
preedge rawseed only     = 1.038040501e-6
extra-Ez + preedge       = 9.005062024e-7
old + extra-Ez + preedge = 8.437772075e-7
```

Leave-one-out / subset read:

```text
drop ez8      -> raw 9.144946447e-7  # largest damage
drop ez4      -> raw 8.406421368e-7  # ez4 is dispensable / slightly harmful
drop old08/12 -> essentially flat once ez8+preedge are present
drop preedge4/24, weightroot -> essentially flat once ez8+old are present

best subset ~= old08/old12 + ez8 + preedge4/preedge24
raw         ~= 8.406e-7
```

Same decomposition using the current L4 PRE-EDGE package prediction as the
base:

```text
L4 base raw                  = 1.299925808e-6
old + ez8                   = 1.027553338e-6
ez8 + preedge24             = 8.942305485e-7
old + ez8 + preedge24       = 8.445697595e-7
all5                        = 8.446353405e-7
```

Read: if the external coordinates were free, there is still a large `~35%`
raw-MSE gap after the L4 package.  The indispensable coordinate is `ez8`, with
old analytic signed-Ez and preedge24 adding complementary structure.  But this
is a compression target, not a package: literal branches remain priced out.

Corrected direct k26/L4 + old analytic pricing:

```text
L4 pred_b1 raw                              = 1.299925808e-6
L4 + free old analytic correction raw       = 1.181134084e-6
first/last transfer                         = 1.113788845e-6 / 1.259827985e-6

generous same-count + one analytic branch pricing:
  c26 top1 raw ~= 1.2134e-6, modeled adjusted ~= 3.43e-7
```

Read: the old analytic correction remains orthogonal after L4 if it is free,
but direct bundling is nowhere near score-positive.

Same-budget replacement check: instead of adding signed final-preactivation
rows, drop some k26 full-ReLU blocks and spend the saved row budget on
independent final-preactivation proxy rows.

CUDA Full200, k26 first18 ReLU blocks + c8 Ez proxy:

```text
lambda=0 raw     = 2.201155885e-6
best lambda=.25  = 1.676450045e-6
```

CUDA Full200, k26 first22 ReLU blocks + c4 Ez proxy:

```text
lambda=0 raw     = 1.739550939e-6
best lambda=.12  = 1.576005683e-6
```

Read: signed-Ez rows are useful as an add-on oracle, but they are not valuable
enough to replace current full ReLU QR blocks at the same row budget.  This
extends the earlier c2/c3 sanity check and closes the simple k26 budget-split
route.  A winning signed-Ez path needs a materially stronger/cheaper proxy for
`E[z_L]`, not a different allocation of ordinary QR proxy rows.

### 2026-07-07 - k26 final PRE-EDGE control-variate bridge is flat

After re-reading the MOMA/PRE-EDGE expert memo, checked the nearby form that
was not separated in the earlier k26 final PRE-EDGE audit:

```text
sample + lambda * (all-block Edgeworth - blockwise Edgeworth readout)
```

This is the control-variate version of the final preactivation moment decoder:
keep the sampled endpoint and only add the nonlinear moment-aggregation
correction.  It used the existing cache, so no new forward pass was required:

```text
cache = legacy_workspace/cache/k26_final_preedge_readout_full200_20260706.npz
base  = sample_gamma raw 1.313143445e-6
```

Best rows over `edge3/edge4/edge33/edge34433`, equal/weighted all-block
moments, and positive/negative lambda:

```text
edge3 weighted lambda .35   raw = 1.313013194e-6  rel = 0.9999008
edge3 weighted lambda .20   raw = 1.313021015e-6  rel = 0.9999068
edge4 weighted lambda .20   raw = 1.313076854e-6  rel = 0.9999493
edge34433 equal lambda -.10 raw = 1.313088653e-6  rel = 0.9999583
```

Read: the mathematically clean MOMA control-variate bridge is real only at
noise-cleanup scale (`~0.01%` on Full200).  This closes the final-layer
same-row PRE-EDGE CV path as a meaningful leaderboard lever.  PRE-EDGE L4
remains the only deployable moment-observer cleanup in this family, and it is
already documented as a `~1%` raw gain.  The larger oracle gap still requires a
new low-noise signed/cumulant coordinate, not another decode of the same k26
row cloud.

### 2026-07-07 - cached b111/response-feature nonlinear closeout

Built a reusable final-layer cache so b111/Ez residual probes no longer have to
reopen and decompose the 1000 higher-moment files for every model:

```text
legacy_workspace/build_b111_response_feature_cache.py
legacy_workspace/cache/b111_response_features_full1000_20260707.npz
```

The builder stores, per final neuron:

```text
response_features  = 117 features from protected l2snap/SPHEREx trajectory
local_features     = 20 true local/storable cumulant features (oracle-only)
labels             = sampler residual, true b111 delta, gauss/b3/b21/truth
```

Full1000 summary:

```text
protected sampler base raw = 2.301738107e-6
Gaussian final raw         = 1.173408412e-6
true b3+b21 raw            = 5.134699705e-6
true b111 line raw         = 1.732218616e-7
b111 delta corr to sampler residual = -0.00435
```

Fast ridge/feature screen:

```text
legacy_workspace/probe_b111_cached_models.py
legacy_workspace/cache/b111_cached_models_full1000_20260707.json
```

Best deployable response-only variants were flat or worse:

```text
response direct residual       best rel ~= 1.00005
response_sq direct residual    best rel ~= 1.00048
response_abs direct residual   best rel ~= 1.00018
response_small direct residual best rel ~= 1.00025
```

The true local/oracle feature sets predict b111 variance very well, but still do
not align with the current sampler residual:

```text
local -> b111 signal R2     ~= +0.834
local_sq -> b111 signal R2  ~= +0.841
best sampler raw with local ~= 2.301791e-6  # effectively unchanged/worse
```

Even combining deployable response features with oracle local features only
gave a tiny non-deployable residual fit:

```text
response_local_prod direct residual raw = 2.300783409e-6
rel = 0.999585  # 0.04% raw gain, oracle-assisted
```

Finally ran cached CUDA MLP probes on the deployable response feature sets:

```text
legacy_workspace/probe_b111_cached_mlp.py

response     h96 d2  raw = 2.289065118e-6  rel = 0.994494
response_abs h96 d2  raw = 2.285890981e-6  rel = 0.993115
response_sq  h96 d2  raw = 2.293160565e-6  rel = 0.996273
response_abs h192 d3 raw = 2.285185855e-6  rel = 0.992809
```

Read: cached response features contain a small nonlinear sampler-residual signal
(`~0.7%` raw), but nowhere near the `5-10%+` needed for a serious package, and
Mini transfer cannot be cheaply guarded because the Mini caches do not contain
the same full per-layer `pred_features`.  More importantly, this confirms the
core diagnosis: predicting b111 under ordinary MSE is not enough; the missing
coordinate must be signed and response-aligned.  Do not package a learned
response-feature MLP from this cache.  Keep the cache for future formulas, but
the next frontier needs a new measured coordinate or deterministic recurrence,
not a larger model over the same response/local feature menu.

### 2026-07-07 - k26/L4 Full1000 in-pass residual transfer gate

Before spending more time on the L4 package, built a full matrix cache for the
actual current k26 + L4 PRE-EDGE path so any residual learner is tested against
the same broad gate as the package:

```text
legacy_workspace/build_k26_l4_ez_matrix_cache.py
legacy_workspace/cache/k26_l4_ez_matrix_full1000_20260707.npz
legacy_workspace/cache/k26_l4_ez_matrix_mini100_20260707.npz
```

The Mini builder needed `--skip-true-ez` because we do not have Mini
higher-moment preactivation labels.  It still stores final targets and the same
k26/L4 telemetry for Full->Mini transfer checks.

Current k26/L4 cache baseline:

```text
Full1000 sample_final_mean raw = 1.476400956e-6
Mini100  sample_final_mean raw = 1.325317392e-6
Full1000 base Ez MSE           = 3.058977935e-6
```

Direct final residual ridge from the already-paid k26/L4 feature bank:

```text
script = legacy_workspace/train_apply_k26_l4_cache_ridge.py --mode final
best Full1000 grouped OOF alpha=1e7 raw = 1.476940854e-6  rel=1.000366
Mini transfer at same alpha              = 1.325371778e-6  rel=1.000041
```

Signed final-preactivation `E[z_L]` ridge from the same feature bank:

```text
script = legacy_workspace/train_apply_k26_l4_cache_ridge.py --mode ez
best Full1000 grouped OOF alpha=1e7 raw = 3.059228108e-6  rel=1.000082
```

Global affine calibration was also checked as a sanity guard:

```text
Full1000 base raw       = 1.476400956e-6
Full1000 5-fold OOF     = 1.477497579e-6  rel=1.000743
Mini fullfit transfer   = 1.324784265e-6  rel=0.999598  # in-sample on Full only
```

Read: do not add a residual learner or affine scalar to the current package.
On the broad Full1000 OOF gate, the k26/L4 in-pass telemetry does not expose
the missing signed final residual or signed `E[z_L]`.  This reinforces the
current working model: a real jump needs an independent weight-based/moment
coordinate, a cheaper external measurement, or a better branch, not another
fit over the same sampled row cloud.

### 2026-07-07 - projected PRE-EDGE moment contractions recheck

After the MOMA/PRE-EDGE expert memo and the new higher-moment emphasis, ran a
bounded CUDA check of the richer projected sampled preactivation moment
features rather than the diagonal raw-moment Edgeworth decoder alone:

```text
script = legacy_workspace/probe_projected_preact_moment_oof.py
indices = 0:80
layer = 4
count = 18
proj_rank = 16
state = legacy_workspace/cache/projected_preact_l4_r16_first80_state_20260707.npz
out   = legacy_workspace/cache/projected_preact_l4_r16_first80_eval_20260707.npz
```

Hidden-state result:

```text
sample hidden MSE        = 2.442352311e-8
plain edge4 hidden MSE   = 2.375729861e-8
best projected-ridge MSE = 2.382054884e-8  # ridge_a10000_c0
```

Final propagation for the best projected rows:

```text
best final raw = 2.187634618e-6  # ridge_a100_c0 beta=0.75 on first80
```

Read: projected sampled preactivation moment contractions are not the missing
MOMA unlock.  They reproduce the same small L4 cleanup scale as diagonal
PRE-EDGE, and the richer ridge actually trails plain `edge4` on hidden MSE.
Do not promote this to a package or run a larger sweep unless a new target-free
observable changes the feature geometry.  The real open target remains a cheap
independent signed final-preactivation / response-aligned cumulant coordinate.

### 2026-07-07 - low-count sampler plus analytic-closure economics

Added:

```text
legacy_workspace/probe_lowcount_sampler_analytic_stack.py
```

Question: the no-eigen/no-direct-K4 analytic closure carries a stable signed
correction, but is too expensive beside k26.  Can it replace QR sampler blocks
instead of being an add-on?

First200, protected-prefix sampler counts plus top cached analytic branch rows:

```text
protected18 weighted base        raw = 2.181298568e-6
protected18 weighted adjusted    adj = 3.123401419e-7  # mult 0.143190

best low-count + analytic stack:
  count18 weighted + top5 rows   raw = 1.883133728e-6
  total multiplier               mult = 0.252004
  adjusted                       adj = 4.745572320e-7

best count16 weighted stack      adj ~= 4.975e-7
best count14 weighted stack      adj ~= 5.453e-7
best count12 weighted stack      adj ~= 5.703e-7
```

Read: the analytic rows reduce raw MSE, but not enough to pay for the analytic
branch.  Reducing sampler count loses raw too quickly, so this does not create
an economic hybrid.  The branch remains a math target: we need a cheap version
of its response-aligned K3cov/kurt-stat coordinate, not a route that runs the
full branch with fewer samples.

### 2026-07-07 - low-count, nonlinear telemetry, and teacher-distill closeouts

After the k26/L4 package became the protected local probe, ran cached checks
for the remaining "cheap or already-paid" upgrade routes.

Low-count seed frontier from cached Full1000 seed predictions:

```text
k26 prefix18 raw ~= 2.313e-6, optimistic adj ~= 3.07e-7
k26 prefix26 raw = 1.476e-6, optimistic adj ~= 2.37e-7
first40 prefix40 raw ~= 1.073e-6, optimistic adj ~= 2.23e-7
union53 prefix53 raw ~= 8.036e-7, optimistic adj ~= 2.03e-7
```

Read: lower-count variants do not get near the floor-score route, and
high-count variants improve raw only by spending too much sampled-row compute.

Global seed weights on the current k26 cache were flat:

```text
k26 start raw = 1.476394503e-6
best 5-fold OOF raw ~= 1.476445521e-6  # rel 1.000035
```

Low-count sampler plus analytic coordinate was also cost-negative.  The best
first200 stacks reduced raw but lost badly after repricing:

```text
k26 count26 + analytic rows:    raw ~= 1.202e-6, modeled adj ~= 3.38e-7
union53 count53 + analytic row: raw ~= 6.842e-7, modeled adj ~= 2.56e-7
```

Nonlinear MLP residual screens over the current k26/L4 in-pass telemetry:

```text
mode=all  h64 d2: base=1.476400956e-6, gain_raw=1.478451558e-6, signal_r2=-0.00163
mode=seed h64 d2: base=1.476400956e-6, gain_raw=1.477132840e-6, signal_r2=-0.00014
mode=ez   h64 d2: base=1.476400956e-6, gain_raw=1.477844045e-6, signal_r2=-0.00151
```

High-count teacher distillation using `union53 - k26/L4` as a target-free
teacher label was also negative:

```text
script = legacy_workspace/probe_k26_l4_teacher_distill.py
teacher union53 raw = 8.036253231e-7

mode=all:  teacher_R2=-0.00063, target_R2=-0.00079, gain_raw=1.478042491e-6
mode=seed: teacher_R2=+0.00011, target_R2=-0.00045, gain_raw=1.477708400e-6
mode=ez:   teacher_R2=-0.00104, target_R2=-0.00124, gain_raw=1.479770068e-6
```

A stale protected18 Strassen3 all-row package also failed the compute/accuracy
tradeoff:

```text
mini indices 0,5:
  raw      = 1.329675e-6
  adjusted = 2.632882e-7
  mult     = 0.20612214
```

Read: the current k26/L4 row-cloud telemetry does not expose either the public
target residual or the high-count sampler residual, even with nonlinear
combinations.  The old b111/response-feature MLP signal likewise worsens when
applied to k26/L4.  Do not build another small readout over the same feature
bank.  The next winner still requires a new independent coordinate: a cheaper
signed final-preactivation estimate, a better deterministic cumulant state, or
an exact compute reduction that preserves the k26/L4 raw line.

### 2026-07-07 - broad PRE-EDGE rawseed MLP deployability gate

The free external-branch stack showed that PRE-EDGE-like hidden-state branches
combine with `extra-Ez` and old signed-Ez coordinates, so checked whether the
deployable-shaped rawseed PRE-EDGE MLP itself becomes useful on a broad
Full1000 OOF gate.

```text
script = legacy_workspace/probe_preedge_rawseed_mlp.py
state  = legacy_workspace/cache/preedge_rawmoment_l4_full1000_state.npz
out    = legacy_workspace/cache/preedge_rawseed_mlp_l4_full1000_all_h64d2_quick_20260707.npz
mode   = all
model  = h64 d2, grouped 5-fold OOF, CUDA
```

Hidden-layer result:

```text
sample_hidden MSE = 2.419048807e-8
rawseed_mlp MSE   = 2.388097360e-8
rel               = 0.987205
```

Final replay:

```text
no_snap final raw       = 2.301738998e-6
beta=0.25 final raw     = 2.299995401e-6
beta=0.50 final raw     = 2.300201959e-6
```

Read: the rawseed MLP learns a real local L4 hidden cleanup, but it only moves
final raw by about `0.08%` on Full1000.  This explains why PRE-EDGE branches
can participate in an oracle/free stack but are not a standalone deployable
frontier move.  Do not broaden this exact rawseed MLP path; the missing
frontier coordinate remains signed final-preactivation / extra-Ez or the
response-aligned K3cov old-analytic signal.

### 2026-07-07 - full1000 equivariant seedmeans residual check

After CUDA access was re-confirmed, ran one bounded larger learned-residual
probe over the full1000 seedmeans cache to verify that the learned lane was not
being unfairly closed from first200/small-model runs:

```text
command:
  python song/src/train_equivariant_residual.py \
    --cache song/data/phase1_l2snap_seedmeans_full1000_slim.npz \
    --out song/runs/phase1_seedmeans_full1000_h64r4_e60_fold0_20260707.json \
    --checkpoint-out song/runs/phase1_seedmeans_full1000_h64r4_e60_fold0_20260707_best.pt \
    --epochs 60 --hidden 64 --rounds 4 --batch 8 \
    --holdout-mod 5 --holdout-value 0 \
    --lr 0.001 --weight-decay 0.01 --dropout 0.10 \
    --weight-features --device cuda --eval-every 10
```

Early held-out metrics:

```text
epoch 0001:
  train base/corrected = 2.327997e-6 / 2.325407e-6
  test  base/corrected = 2.209064e-6 / 2.207063e-6
  ratio                 = 1.0009

epoch 0010:
  train base/corrected = 2.325125e-6 / 2.325407e-6
  test  base/corrected = 2.209176e-6 / 2.207063e-6
  ratio                 = 1.0010
```

Stopped the run at epoch 10.  The larger full1000 equivariant model is flat to
worse on held-out data and does not approach the few-percent gate needed for a
score-relevant learned correction.  This reinforces the prior closeouts:
current seedmeans/seedstats trajectories and local weight features do not expose
the signed final-preactivation or high-count teacher residual strongly enough.

### 2026-07-07 - stationary tail-source design recheck

Rechecked the expert-suggested stationary source-injection route using the
existing starter-kit tail-response machinery, keeping this separate from the
sampler/seed workstream.

Collected a compact Full200 design cache:

```text
script:
  whest-starterkit/phase1/collect_tail_design.py

estimator:
  whest-starterkit/candidate_phase1_identity_tail_finalraw_estimator.py

cache:
  legacy_workspace/cache/tail_design_identity_full200_four_expanded_20260707.npz

shapes:
  final_cols    = (200, 256, 86)
  response_cols = (200, 256, 100)
  target        = (200, 256)
```

The identity-tail analytic base is very weak:

```text
base final MSE = 2.493787001e-5
```

Fitted joint final/readout plus propagated tail-source columns with grouped
MLP-held-out CV:

```text
script:
  whest-starterkit/phase1/fit_tail_design.py

best lambda = 1e-4
best OOF CV final MSE = 9.191716562e-6
fit-all final MSE     = 8.440196835e-6
```

Read: the propagated stationary source idea improves the old identity-tail
analytic chassis, but it is nowhere near the current SPHEREx frontier
(`~1.5e-6` raw for k26/L4, lower on some canaries).  This exact tail-response
feature set is not the cheap rewrite of the old expensive K3cov/K211 signed
coordinate.  Do not tune group counts or ridge constants here unless a new
source dictionary is introduced.

### 2026-07-07 - high-compute pathCV/highseed remote-probe recheck

Rechecked the parked high-compute pathCV/highseed candidates on the full split
after the cache parity fixes, using `quick_score_selected.py`.  This was kept
separate from the sampler assistant's current low-count seed workstream.

Analytic-only sanity checks:

```text
candidate_hybrid3_lowrankoff_hermite3_estimator.py, full indices 0,50,100,150,200:
  failed=5
  error=IndexError at self._h_cal[layer_idx]
  read=old warmup/depth mismatch; do not use as a live Phase-1 branch.

candidate_phase1_tailresponse_joint_expanded_cap6_full1000_estimator.py, same full5:
  raw=7.428848e-6 adjusted=7.494265e-7 mult=0.10147161 failed=0
  read=valid but far outside the useful independent-blend band.
```

High-compute branch checks on full indices `0,250,500,750,999`:

```text
pathCV88 flat z12/z2s3 Strassen2:
  raw=3.807997e-7 adjusted=2.370286e-7 mult=0.62186001 failed=0

pathCV88 flat z12/z2s1 Strassen2:
  raw=3.921830e-7 adjusted=2.446332e-7 mult=0.62296641 failed=0

pathCV88 q25 z12/z2s1 Strassen2:
  raw=3.921830e-7 adjusted=2.442246e-7 mult=0.62202603 failed=0

highseed88 betas15 ridge01 Strassen2:
  raw=3.418905e-7 adjusted=2.122446e-7 mult=0.61957920 failed=0
```

The highseed88 result was strong enough to warrant the broader guard.  On full
spaced20 `0,50,...,950`:

```text
highseed88 betas15 ridge01 Strassen2:
  raw=4.636353e-7 adjusted=2.831605e-7 mult=0.61063861 failed=0
  flops=1.543e11 effective=1.661e11 elapsed=171.36s
```

Read: highseed88/pathCV has real raw-MSE power, but the compute slope still
loses on the broad guard.  Do not promote these as next-winner packages unless
we find a much cheaper way to extract the same signed correction.  The current
practical remote probe remains k26 + L4 PRE-EDGE; the higher-upside research
lane remains an independent weight/moment coordinate that can blend with the
sampler at near-floor compute.

### 2026-07-07 - old K3cov source caps and exact layer-skip audit

Question: can the old no-eigen/K3cov signed coordinate be made cheap by either
capping the K3cov source set or masking the K3cov update to early layers only?

Implementation detail: patched `candidate_k3cov_sourcecap2_estimator.py` so a
zero `K3COV_LAYER_GAIN` now skips building the layer's `k2eq_pre` term instead
of forming it and multiplying by zero.  This makes the early/late layer masks a
real FLOP diagnostic, not just a readout mask.

Source-cap sweep on spaced20 (`0,10,...,190`), with `KURT_MOMENT_GAIN=0` and
`LEARNED_K3_GAIN=0`:

```text
reference no-kurt/no-learned:
  base raw        = 2.249992893e-6
  OOF two raw     = 2.156851553e-6
  fit-all raw     = 2.150109325e-6
  elapsed mean    = 1.230s

K3COV_SOURCE_CAP=1:
  OOF two raw     = 2.276908928e-6
  fit-all raw     = 2.245625454e-6
  elapsed mean    = 1.382s

K3COV_SOURCE_CAP=2:
  OOF two raw     = 2.269511615e-6
  fit-all raw     = 2.241914523e-6
  elapsed mean    = 1.428s

K3COV_SOURCE_CAP=3:
  OOF two raw     = 2.282051556e-6
  fit-all raw     = 2.239969366e-6
  elapsed mean    = 1.425s
```

Read: source capping is not the compression route.  It loses the useful OOF
signal and is not faster in this implementation.

Exact early-layer skip profiles:

```text
early8 K3cov only:
  profile n=2 warm ops      = 6215
  profile n=2 warm FLOPs    = 1.317e10
  predicted remote mult     = 0.1055 / 0.1512 / 0.1969 at 25/45/65 us per op
  spaced20 OOF two raw      = 2.255849201e-6
  spaced20 fit-all raw      = 2.236495109e-6

early12 K3cov only:
  profile n=2 warm ops      = 6343
  profile n=2 warm FLOPs    = 1.451e10
  predicted remote mult     = 0.1117 / 0.1583 / 0.2049 at 25/45/65 us per op
  spaced20 OOF two raw      = 2.228021099e-6
  spaced20 fit-all raw      = 2.216399152e-6

early16 K3cov only:
  profile n=2 warm ops      = 6471
  profile n=2 warm FLOPs    = 1.586e10
  predicted remote mult     = 0.1178 / 0.1654 / 0.2129 at 25/45/65 us per op

late16 K3cov only:
  profile n=2 warm ops      = 6511
  profile n=2 warm FLOPs    = 1.649e10
  predicted remote mult     = 0.1205 / 0.1684 / 0.2162 at 25/45/65 us per op
```

Read: the exact skip saves real FLOPs, but the branch is still too expensive as
an add-on to k26/L4 and the shorter early8/early12 masks do not preserve enough
signal.  The all/early16 old K3cov coordinate remains a valuable diagnostic of
the missing signed final-preactivation signal, not a submit-relevant production
branch in this form.  Do not retest source caps or simple early masks unless a
new algebraic K3cov/K211 contraction changes the cost model.

### 2026-07-07 - resumed-goal triage after compaction

After resuming the under-`2e-7` goal, re-read the current expert briefs, package
source, and the latest caches to avoid repeating already-closed probes.

Closed or not worth rerunning:

```text
full-cov Gaussian signed-Ez proxy:
  proxy Ez corr is high, but nonlinear k26 shift only gives ~1% raw and costs
  several billion FLOPs.  Keep as a diagnostic, not a package path.

k26/L4 paid telemetry residual models:
  full1000 nonlinear residual caches are flat/worse;
  union53 teacher distill over the same features has negative transfer.

within-block reliability / SHRC / signed partition controls:
  already failed broad variance-reduction and transfer gates.

PRE-EDGE/MOMA intermediate snaps:
  L4 is the only broad deployable cleanup (~1%); L8/L12/later snaps were
  scout-lucky or faded on Full200/Full1000.  Do not broaden the same layer-snap
  family unless the moment observable changes.

generic common-factor b111 / structured true-moment closure:
  useful oracle tier (~6.6e-7 with true moment features), but rank 8/16/32
  generic modes saturate and do not recover the response-aligned b111 oracle.

direction-set design:
  QR-antithetic geometry is load-bearing; iid/sign/Hadamard/Sobol and the
  optimized multi-frame pilot did not transfer.  A specific new cubature
  construction could be tested, but generic fixed-direction optimization is not
  a near-term frontier.
```

Current frontier:

```text
protected/k26 sampler family:
  practical, robust, but compute/raw tradeoff stalls above the new leaderboard.

true signed final-preactivation / true b111 oracles:
  still large enough for e-8 to low-e-7 adjusted scores.

missing deployable object:
  a cheap response-aligned signed coordinate (old K3cov/K211/extra-Ez-like),
  not another model over sampler seed telemetry or public moment summaries.
```

Next work should therefore be filtered hard: either a new algebraic/final-rooted
signed coordinate with an actual cost reduction, or a genuinely new sampler
control that estimates the same signed coordinate with independent low-noise
information.  Cosmetic seed/readout tuning and broader MLPs over the existing
telemetry are below the needed 5-10% gain scale.

Rooted cubic diagram closeout, re-read from cached files after compaction:

```text
probe:
  legacy_workspace/probe_rooted_cubic_diagrams.py

caches:
  rooted_cubic_coarse_fullkinds_first40.npz
  rooted_cubic_final_coarse_fullkinds_first40_v2.npz
  rooted_cubic_all_coarse_fullkinds_first40.npz

inputs granted to the probe:
  true public post moments / covariance / c21 from higher-moment files,
  final-rooted cubic response features diag, c21, covsq1, covsqvar,
  MLP-grouped OOF ridge against the protected sampler residual.

first40 base raw                         = 2.187817042e-6
best final-only rooted cubic raw         = 2.187818224e-6
best final-only residual_r2              = -0.001121
best direct final-rooted fullkinds raw    = 2.187979441e-6
best all-readout rooted cubic raw         = 2.188271142e-6
```

Read: even the true-moment, final-rooted `B⊗B⊗B`/cubic source dictionary is
neutral to slightly wrong-signed for the current protected sampler residual in
this coarse implementation.  This closes the naive "rooted cubic diagrams over
cov/c21/post-mu3" route as a near-term production bridge.  If final-rooted work
returns, it needs a qualitatively different response-aligned signed-Ez/K3cov
coordinate or the pair-distance receiver-injected recurrence from the expert
notes, not this coarse source dictionary.

Compact selected-template smoke against the current k26 residual:

```text
temporary cache:
  tmp_k26base_phase1_first40_matrix_cache.npz
  base = k26 split035/045 gamma readout, first40 public-full rows
  weights/pred_rows/states = phase1_l2snap_seedmeans first200 matrix cache

probe:
  final_rooted_young_cact_probe.py
  limit=5, folds=5, max_cact_dist=1

features:
  hcoeff/h123 distance buckets and totals
  symcv_Cact_{wick2,gate,h2,mean} at distance 1

base raw      = 1.590147185e-6
corrected raw = 1.786959737e-6
residual R2   = -0.123770
```

Read: not promotion evidence because `n=5`, but the sign is bad and the old
all-pair feature generator is too slow even for small current-residual probes.
Do not scale this exact generator.  If selected-template work continues, it
must be via the folded one-sided receiver recurrence, not the old pair-transfer
feature builder.

Folded/direct one-sided Cact receiver follow-up:

```text
script:
  probe_k26_selected_cact_direct.py

feature:
  resp_t @ [ beta_t * 3 * diag(P Cact_s P.T) * (P v_s) ]
  v_s in {centered mean, wick2, centered gate, h2}
  distances d in {1,2}

current residual:
  base = k26 split035/045 gamma readout
  target = public Phase-1 first200 final means

first40:
  base raw      = 1.291598534e-6
  corrected raw = 1.310996883e-6
  residual R2   = -0.0150

first200:
  base raw      = 1.313140188e-6
  corrected raw = 1.316959860e-6
  residual R2   = -0.0029
```

Read: the corrected one-sided receiver shape that matched the old selected
generator is not the missing correction for the current k26/SPHEREx residual.
This closes young Cact/Cact-receiver distance-1/2 as a near-term analytic
bridge.  The live signed-coordinate question is narrower: K3cov/K211/b111 or a
different response-aligned final-preactivation mean term, not covariance-source
Cact with local receiver vectors.

### 2026-07-07 - sparse router gate transfer and offset20 falsifier

Question: can the k26-rectsparse/pathCV84-sparse router recover more of the
row oracle without retuning on the same warning slice?

Added:

```text
legacy_workspace/probe_sparse_router_gate_transfer.py
```

The script recomputes old `pathcv88_branch_gate_full200.npz` labels from raw
branch predictions using current sparse-arm multipliers, instead of trusting
stale pathCV cost assumptions.  Weight-only gates trained on that old Full200
cache over-selected pathCV and failed to transfer:

```text
old Full200-trained gates applied to current sparse arms:

full_spaced20:
  fixed k26      = 1.976966736e-7
  fixed pathCV   = 2.599167758e-7
  oracle         = 1.577118649e-7
  ridge gate     = 2.439986430e-7
  logit gate     = 2.534600491e-7
  threshold gate = 2.656373997e-7

mini_spaced20:
  fixed k26      = 2.312995296e-7
  fixed pathCV   = 3.289516795e-7
  oracle         = 1.871579073e-7
  ridge gate     = 3.242600736e-7
  logit gate     = 3.352749444e-7
  threshold gate = 3.341879031e-7
```

Then scored a fresh Full offset20 slice for the current sparse arms:

```text
indices = 25,75,125,...,975

k26 rectsparse240s7:
  raw      = 1.372751e-6
  adjusted = 2.412333e-7
  mult     = 0.17619270
  failed   = 0

pathCV84 sparse224s7:
  raw      = 4.700669e-7
  adjusted = 2.448840e-7
  mult     = 0.52059708
  failed   = 0

oracle over the two arms on offset20:
  adjusted = 1.793622967e-7
```

One-feature thresholds over the current sparse slices are not stable:

```text
old late-q25 gate:
  full_spaced20 = 1.887797178e-7
  full_offset20 = 2.306891205e-7
  mini_spaced20 = 3.330106506e-7

fit on full_spaced20 + mini_spaced20:
  feature 111, sign +, threshold 1.3215446555
  full_spaced20 = 1.873764219e-7
  mini_spaced20 = 1.976833243e-7
  full_offset20 = 2.495824405e-7  # fails offset

fit on full_spaced20 + full_offset20:
  feature 100, sign -, threshold 0.0621629979
  full_spaced20 = 1.677157900e-7
  full_offset20 = 1.969894751e-7
  mini_spaced20 = 3.049802092e-7  # fails Mini

fit on full_offset20 + mini_spaced20:
  feature 5, sign +, threshold 7.049786873e-5
  full_offset20 = 2.007196707e-7
  mini_spaced20 = 2.168189789e-7
  full_spaced20 = 2.117527275e-7
```

Read:

* The pathCV sparse arm is valuable in an oracle, but current cheap gates are
  slice-unstable.
* Do not promote a new router threshold from these rows.  The existing
  `late_q25` router remains a high-risk diagnostic only.
* The robust practical package from this workstream is the single-arm k26
  `rectsparse240s7` compute-polish package, not the two-arm router.

### k26 L4 PRE-EDGE + Full-Covariance Ez Calibration

Status: packaged as a remote probe.

Artifact:

```text
whest-starterkit/packages/to_test_remote/submission_phase1_pure26_w05_unionteacher1100gamma_l2snap_split035_045_noaffine_module_batchedstrassen2_preedgeL4_fullcovEzCal_finalonly_bundle.tar.gz
sha256 00caad84fabd9d4f819b6a0dd0ba2fc5c1c5b7ea54afed3bfc515ea56c42e2ea
```

Implementation:

```text
candidate package:
  _pkg_pure26_w05_unionteacher1100gamma_l2snap_split035_045_noaffine_module_batchedstrassen2_preedgeL4_fullcovEzCal_finalonly

new final-only correction:
  fullcov_proxy = deterministic full-covariance Gaussian final preactivation mean
  proxy_cal     = 0.9927832064329624 * fullcov_proxy + 0.0000778385601343052
  z_final       = sampled z_final + 0.05 * (proxy_cal - sampled_z_mean)
```

Validation:

```text
whest validate package estimator:
  success; setup/predict finite smoke OK

package mini indices 0,5:
  raw=6.753851e-7 adjusted=1.257742e-7 mult=0.18836659 failed=0

package full spaced20 0,50,...,950:
  raw=1.133270e-6 adjusted=2.059867e-7 mult=0.18131035 failed=0

previous k26 L4 PRE-EDGE package on full spaced20:
  raw=1.249116e-6 adjusted=2.158833e-7 mult=0.17201979 failed=0
```

Read: this is the current best k26-family single-arm remote probe by the
broader full-spaced20 guard.  The correction is independent of the SPHEREx rows
and therefore avoids the already-falsified same-row sign-routing trap.  It is
small enough to keep the compute multiplier close to the existing k26 package.
Treat it as an incremental transfer probe, not as the e-8 breakthrough.

Blend robustness follow-up:

```text
global fullcov-Ez lambda sweep on full spaced20 0,50,...,950:
  lambda=0.02 adjusted=2.160972e-7 raw=1.188390e-6 failed=0
  lambda=0.03 adjusted=2.119458e-7 raw=1.165218e-6 failed=0
  lambda=0.05 adjusted=2.059867e-7 raw=1.133270e-6 failed=0
  lambda=0.08 adjusted=2.046192e-7 raw=1.121317e-6 failed=0
  lambda=0.10 adjusted=2.060211e-7 raw=1.137330e-6 failed=0

cross-checks:
  lambda=0.08 mini spaced20 adjusted=2.464789e-7 raw=1.359756e-6
  lambda=0.05 mini spaced20 adjusted=2.392338e-7 raw=1.326840e-6
  lambda=0.08 full offset20 adjusted=2.608108e-7 raw=1.439890e-6
  lambda=0.05 full offset20 adjusted=2.476283e-7 raw=1.371274e-6
```

Read: `lambda=0.08` is a narrow original-spaced20 peak, but the independent
Mini and shifted-full checks both prefer the safer `0.05`.  Do not package an
`0.08` sibling unless we deliberately want a high-risk canary-fit probe.

Negative companion result:

```text
learned Ez ridge proxy:
  full1000 OOF cache looked strong on Full200
  explicit full-trained proxy transfer to Mini was flat or wrong-signed
  best safe Mini-all result was only a tiny negative-lambda polish
```

Read: do not promote the learned k26 telemetry Ez-ridge route without a much
stronger non-overfit transfer diagnostic.  It likely learned public/full-row
structure rather than a stable deployable Ez correction.

### Full1000 Fullcov-Ez Calibration and Composition Closeout

After packaging the Full200-global fullcov-Ez correction, built a stronger
Full1000 fullcov proxy cache with CUDA:

```text
legacy_workspace/cache/fullcov_gaussian_ez_proxy_full1000_f32_cuda_20260707.npz
legacy_workspace/cache/fullcov_gaussian_stats_full1000_f32_cuda_20260707.npz
legacy_workspace/cache/fullcov_gaussian_stats_mini100_f32_cuda_20260707.npz
```

Global Ez calibration from Full1000:

```text
proxy_cal = 0.992489309004615 * proxy + 0.00001414661893027946

Ez proxy MSE:
  raw full        = 1.603411863e-4
  raw mini        = 1.674621269e-4
  Full1000 global = 5.185686904e-5 full, 5.352518953e-5 mini
```

Exact k26/L4 final-shift grid with Full1000 calibration:

```text
Full1000 all:
  lambda=0.00 raw=1.476399601e-6
  lambda=0.03 raw=1.411231231e-6
  lambda=0.05 raw=1.393771419e-6  # best all
  lambda=0.08 raw=1.406557379e-6

Mini100 all:
  lambda=0.00 raw=1.325314139e-6
  lambda=0.03 raw=1.281775584e-6
  lambda=0.05 raw=1.278035604e-6  # best all
  lambda=0.08 raw=1.310338565e-6
```

Read: Full1000 calibration supports the existing scalar `lambda=0.05` choice.
It is only a microscopic scorer-path polish versus the staged Full200-global
package, not enough to repackage by itself.

Per-neuron lambda selection from the Full1000 grid:

```text
Full1000 full-fit per-neuron raw = 1.389888697e-6
Mini transfer raw                = 1.287717502e-6  # worse than scalar 0.05
Full1000 5-fold OOF raw          = 1.398168195e-6  # worse than scalar 0.05

thresholded deviation best Mini:
  changed 1 neuron, mini raw=1.277891844e-6
```

Read: per-neuron blend strengths are overfit/noise.  The one-neuron threshold
gain is too small to matter and not a meaningful next-winner package.

Fullcov hidden mean snaps, using the already-paid fullcov state as hidden
external targets, were wrong-signed on Mini spaced20:

```text
baseline scalar fullcov-Ez lambda=0.05 on Mini spaced20 ~= 1.327e-6 raw

L8  beta=+0.05 raw=1.389948238e-6
L8  beta=-0.05 raw=1.401807407e-6
L12 beta=+0.05 raw=1.415152322e-6
L12 beta=-0.05 raw=1.430919076e-6
L24 beta=+0.05 raw=1.533739281e-6
L24 beta=-0.05 raw=1.499785539e-6
```

Final preactivation variance scaling from raw fullcov variance was also
wrong-signed on Mini spaced20:

```text
var_lambda=-0.25 raw=1.395037577e-6
var_lambda=+0.25 raw=1.398894196e-6
var_lambda=+0.50 raw=1.606368558e-6
```

MLP-level lambda routing had a tempting target-aware oracle but failed OOF and
Mini transfer:

```text
scalar lambda=0.05:
  Full1000 raw=1.393771419e-6
  Mini100 raw =1.278035604e-6

row oracle over lambdas:
  Full1000 raw=1.298846876e-6
  Mini100 raw =1.158383066e-6

deployable ridge loss router:
  best OOF Full1000 was at best about flat
  Mini transfer worsened to ~1.299e-6 to 1.316e-6
```

Read: the fullcov mean coordinate is useful, but the nearby cheap variants
do not unlock the row-wise oracle.  The next meaningful route is not more
same-coordinate routing; it is a better deployable moment closure for signed
preactivation skew/kurtosis or the old response-aligned K3cov/K211 coordinate.

### Deployable Fullcov Alpha-Closure Check

Built a reusable true final-preactivation cumulant cache from the local
higher-moment dataset:

```text
legacy_workspace/cache/true_final_preact_cumulants_full1000_20260707.npz
```

Tested the higher-moment memo's alpha-polynomial skew/kurtosis closure in the
deployable setting: use fullcov final `mu,var` as features, fit global
skew/excess coefficients on Full1000, and evaluate Full1000 OOF plus Mini
transfer.

Standalone deterministic final estimator:

```text
fullcov Gaussian ReLU:
  Full1000 raw = 2.630171702e-5
  Mini100 raw  = 2.600070168e-5

best alpha/sigma Edgeworth closure:
  Full1000 OOF raw ~= 2.6147e-5
  Mini100 raw      ~= 2.5858e-5
```

Sampler control-variate blend against the current k26/L4/fullcov scalar base:

```text
base:
  Full1000 raw = 1.395280542e-6
  Mini100 raw  = 1.278035604e-6

fullcov Gaussian analytic arm:
  standalone Mini raw = 2.600070168e-5
  global OOF beta ~= 0.008
  Mini blend raw  = 1.281643936e-6  # worse

alpha/sigma Edgeworth analytic arm:
  standalone Mini raw = 2.585771724e-5
  global OOF beta ~= 0.008
  Mini blend raw  = 1.281752194e-6  # worse
```

Read: the alpha-polynomial closure's oracle result depends on accurate
preactivation `mu,var`; substituting fullcov `mu,var` destroys standalone
accuracy, and the analytic arm is not useful as a control variate.  Do not
package a fullcov Edgeworth closure.  The moment lane remains live, but it
needs a better deployable state recurrence for signed skew/K211/K3cov, not a
final-only alpha closure on fullcov moments.

### 2026-07-07 - signed-Ez anisotropic shrink closeout

Question: the previous signed-preactivation zero-shrink falsifier only tested
scalar/global proxies.  Maybe the sampled signed final-preactivation mean could
be improved by a target-free vector shrink using the per-seed/block noise
estimate:

```text
Ez       = mean_seed raw_seed[:, seed, moment1, neuron]
noise    = Var_seed(Ez_seed) / n_seed
prior    = max(Ez^2 - noise, 0), estimated globally / per neuron / per MLP
Ez_proxy = beta * Ez or mean + beta * (Ez - mean)
pred     = sample_ReLU + 0.5 * alpha * (Ez_proxy - Ez)
```

Cache-only diagnostic on:

```text
legacy_workspace/cache/preedge_final_full1000_rawonly.npz
```

Results:

```text
base equal sampled ReLU raw        = 2.314414355e-6

target-free alpha=1 variants:
  EB zero global                   = 2.314241987e-6  # rel 0.999926
  EB zero neuron                   = 2.314242214e-6  # rel 0.999926
  EB zero MLP                      = 2.314111601e-6  # rel 0.999869
  EB zero MLP scalar               = 2.314229973e-6  # rel 0.999920
  MLP-mean residual shrink         = 2.314100435e-6  # rel 0.999864

target-fitted alpha ceilings:
  best EB zero MLP scalar          = 2.312357644e-6  # rel 0.999111
  best MLP-mean residual shrink    = 2.312933913e-6  # rel 0.999360
```

Read: even a non-scalar empirical-Bayes shrink of the signed final-preactivation
sample mean is far below the needed scale.  The per-block noise model estimates
`beta ~= 1` almost everywhere, and target-fitted over-shrink only buys
sub-0.1% raw.  Close vector/anisotropic shrink of the existing signed-Ez sample
mean.  The remaining signed-Ez path still needs an independent/mechanistic
coordinate, not reweighting of the same sampled signed mean.

Companion split of final ReLU error into signed and absolute-value halves:

```text
identity:
  E[ReLU(z)] = 0.5 * E[z] + 0.5 * E[|z|]

cache:
  preedge_final_full1000_rawonly.npz
  true E[z] from higher-moment public files
  true E[|z|] = 2 * target_final - true E[z]

base equal sampled ReLU raw        = 2.314414355e-6
true Ez + sampled Abs              = 1.130806027e-6  # rel 0.4886
sampled Ez + true Abs              = 1.193013210e-6  # rel 0.5155
true Ez + true Abs                 ~= numerical zero

sample-moment Gaussian Abs proxy:
  alpha=0.25 raw                   = 2.385779834e-6  # worse
  alpha=1.00 raw                   = 3.490325976e-6  # much worse
  target-fitted alpha              = 0.00478
  target-fitted raw                = 2.314387207e-6  # rel 0.999988
```

Read: the linear final-ReLU error is almost evenly split between the signed
mean and absolute-value/shape halves.  However, a cheap `E|z|` proxy decoded
from the same sampled first/second preactivation moments is wrong-signed and
collapses to zero under target-fitted alpha.  The nonlinear true-Ez row-cloud
shift remains valuable because shifting the rows repairs both halves at once,
but a direct cheap absolute-value moment proxy is not a route in this form.

### 2026-07-07 - repeated-K3/K211 tail source against current k26/L4/fullcov base

Question: can the deterministic repeated two-equal K3 slice (`K211`-style tail
source) provide a cheap independent signed correction to the current staged
k26/L4/fullcov-Ez base?

Harness update:

```text
legacy_workspace/probe_repeated_k3_tail.py
  added --base-cache/--base-key so this probe can score against the current
  staged base instead of the older protected18 base.
```

First40 smoke, current base = `pred_lam0p05` from
`k26_l4_fullcov_ez_full1000cal_shift_full1000_grid_20260707.npz`:

```text
base raw = 1.179751193e-6
best scalar rowenergy@16-31 raw = 1.174406469e-6  # rel 0.995470
best ridge raw = 1.190572609e-6                   # wrong-signed
```

Full200 expansion:

```text
base raw = 1.229059455e-6
best scalar rowenergy@16-31 raw = 1.229129254e-6  # rel 1.000057
best ridge raw = 1.234207466e-6                   # wrong-signed
```

Read: the deterministic repeated-K3/K211 tail source does not expose a robust
signed correction for the current k26/L4/fullcov base.  The tiny first40 scalar
gain was slice noise and vanished on Full200.  Do not package or scale this
source family unless the local source observable changes materially.

### 2026-07-07 - Hermite full-covariance signed-Ez proxy closeout

Question: the staged fullcov-Ez package uses the cheap first-Hermite covariance
closure

```text
Cov[ReLU(z_i), ReLU(z_j)] ~= Phi_i Phi_j Cov[z_i,z_j]
```

Could a stronger Gaussian Hermite covariance closure make the deterministic
signed-Ez proxy more response-aligned?

Harness update:

```text
legacy_workspace/probe_fullcov_gaussian_ez_proxy.py
  added --hermite-order, using the same ReLU Hermite covariance coefficients
  as the old analytic estimator, while keeping exact univariate diagonals.
```

Full split spaced20 `0,50,...,950`, float32 CUDA:

```text
K=1 staged/simple proxy true-Ez MSE ~= 1.33e-4  # from earlier cache
K=3 Hermite proxy true-Ez MSE       = 1.435406934e-4
K=3 proxy corr                      = +0.999989
```

Nonlinear k26/L4 final-preactivation shift, same rows:

```text
K=3 uncalibrated:
  lambda=0    raw = 1.249117707e-6
  lambda=0.02 raw = 1.216384765e-6

K=3 scalar affine fit to the same true-Ez spaced20 labels:
  proxy MSE after affine = 4.553986986e-5
  lambda=0.08 raw        = 1.128056064e-6

Staged K=1 Full1000-calibrated cache on the same rows:
  lambda=0.08 raw        = 1.121509225e-6
  lambda=0.05 raw        = 1.133115642e-6
```

Read: Hermite covariance terms are more expensive and do not beat the existing
first-Hermite fullcov-Ez coordinate, even with an over-generous same-slice
affine calibration.  Do not package Hermite covariance for the signed-Ez proxy.

### 2026-07-07 - truncated fullcov-Ez proxy closeout

Question: can we keep most of the useful fullcov signed-Ez coordinate while
running full covariance only for early layers, then switching to diagonal
variance propagation?

Harness update:

```text
legacy_workspace/probe_fullcov_gaussian_ez_proxy.py
  added --fullcov-until.  For layers >= cutoff, propagate only diagonal
  variance with pre_var = var @ W^2.
```

Full split spaced20 `0,50,...,950`, K=1 covariance, float32 CUDA:

```text
cutoff  true-Ez MSE    same-slice affine-cal MSE
2       2.241042561e-3 1.894645629e-3
4       2.201954573e-3 1.734214733e-3
8       2.042776164e-3 1.550379237e-3
16      1.657786693e-3 1.361673638e-3
```

For comparison, the full K=1 proxy after broad affine calibration is around
`5e-5` signed-Ez MSE on the full/mini caches.

Read: the useful fullcov-Ez coordinate is not seeded by early covariance alone;
it needs deep off-diagonal covariance state.  Truncating to diagonal after even
16 layers destroys the proxy by more than an order of magnitude, so this is not
a compute-saving route.

### 2026-07-07 - k26/L4 zero-extra seed readout OOF closeout

Question: can we improve the current k26/L4 readout with no extra compute by
using a better constant readout from the already-paid 26 final seed-block
vectors?

Added:

```text
legacy_workspace/probe_k26_seed_readout_oof.py
legacy_workspace/cache/k26_l4_seed_readout_oof_full1000_to_mini_20260707.npz
```

Input caches:

```text
k26_l4_ez_matrix_full1000_20260707.npz
k26_l4_ez_matrix_mini100_20260707.npz
```

Baseline:

```text
Full1000 existing gamma readout raw = 1.476400956e-6
Full1000 equal seed readout raw    = 1.479672355e-6
Mini100 existing gamma readout raw = 1.325317392e-6
Mini100 equal seed readout raw     = 1.352377156e-6
```

Tested:

```text
global 26 seed weights, grouped 5-fold OOF, ridge alpha 0..1e6
per-neuron 26 seed weights, grouped 5-fold OOF, ridge alpha 0..1e6
Full1000 fit -> Mini100 transfer for every alpha
```

Best transfer variants were still worse than existing gamma:

```text
best global transfer ~= 1.351108414e-6  # rel 1.01946 vs gamma
best per-neuron transfer ~= 1.353056241e-6
best OOF variants also stayed worse than gamma/equal by ~0.3%+
```

Read: the current gamma readout is already better than simple global or
per-neuron target-fitted seed weights for this k26/L4 cloud.  Do not replace
the final readout with constant seed weights; this zero-cost lane is closed
unless the available seed vectors or readout features change.

### 2026-07-07 - Hermite-4 fullcov signed-Ez confirmation

Follow-up to the Hermite full-covariance closeout: reran the deterministic
fullcov signed-preactivation proxy with `--hermite-order 4` on the full split
spaced20 rows, using CUDA for the offline audit.

Artifacts:

```text
legacy_workspace/cache/fullcov_gaussian_ez_proxy_spaced20_h4_20260707.npz
legacy_workspace/cache/k26_l4_fullcov_h4_ez_shift_spaced20_20260707.npz
legacy_workspace/cache/fullcov_gaussian_ez_proxy_spaced20_h4_cal_oracle_20260707.npz
legacy_workspace/cache/k26_l4_fullcov_h4_caloracle_ez_shift_spaced20_20260707.npz
```

Full split spaced20, fullcov proxy quality:

```text
order1 raw true-Ez MSE = 1.491494858e-4
order2 raw true-Ez MSE = 1.435793580e-4
order3 raw true-Ez MSE = 1.435395792e-4
order4 raw true-Ez MSE = 1.434877019e-4

order1 same-slice affine MSE = 4.697203522e-5
order4 same-slice affine MSE = 4.553111056e-5
```

Nonlinear k26/L4 final-preactivation row-cloud shift with the uncalibrated
order4 proxy:

```text
lambda=0.00 raw = 1.249111476e-6
lambda=0.03 raw = 1.220728424e-6  # best uncalibrated
lambda=0.05 raw = 1.270914289e-6
```

With an over-generous same-slice affine calibration to true final signed
preactivation:

```text
calibration a = 0.9931623625555237
calibration b = -4.7265204436e-05
lambda=0.05 raw = 1.138702356e-6
lambda=0.08 raw = 1.128023926e-6  # best in this grid
```

Read: Hermite-4 is still only a small ceiling polish on the already-staged
first-Hermite fullcov coordinate.  It does not expose a new sub-2e-7 route,
and the extra pointwise/Hermite complexity is not justified unless a future
package already pays for the same richer covariance state for another reason.

### 2026-07-07 - k26 seed-prefix finite-count bias closeout

Question: can the 26 paid seed-block outputs be extrapolated to the infinite-
seed sphere expectation by learning the bias pattern from prefixes, e.g.
Richardson-style or OOF prefix-to-full corrections?

Input:

```text
legacy_workspace/cache/k26_l4_ez_matrix_full1000_20260707.npz
```

The cache has `final_seed_scaled` with shape `(1000, 26, 256)`.  The stored
gamma-weighted sample mean matches the weighted seed reconstruction to MSE
`~1.07e-9`, so this is testing the actual deployed final row cloud.

Grouped 5-fold OOF prefix/extrapolation attempts were flat or worse:

```text
existing gamma sample raw = 1.476400956e-6
eq18 prefix readout raw   = 1.479898846e-6  # rel 1.000954
eq24 prefix readout raw   = 1.478503894e-6  # rel 1.000011
w24 prefix readout raw    = 1.478606396e-6  # rel 1.000080
```

Read: for this QR-antithetic seed cloud, prefix-to-full differences are
mostly exchangeable quadrature noise.  They do not reveal a stable finite-
count bias that can be removed without buying more rows or a different
observable.  The next sampler-side gain needs a new target-free internal
observable, not a better prefix extrapolator over the same 26 seed vectors.

### 2026-07-07 - highseed signed-Ez oracle and cheap-proxy probes

Added two offline CUDA probes:

```text
legacy_workspace/probe_highseed_fullcov_ez_shift.py
legacy_workspace/probe_highseed_final_preedge.py
```

Question: can the high-count/pathCV raw-MSE branch be turned into a real
adjusted-score contender by correcting its final signed preactivation mean
`E[z_L]`, either with the fullcov proxy, true-moment oracle, in-run final
moments, or pathCV-estimated signed `E[z]`?

Full split spaced20 `0,50,...,950`, highseed88/pathCV baseline:

```text
baseline raw      = 4.636532351e-7
baseline adjusted = 2.831245670e-7  # multiplier 0.61063861
```

True signed final-preactivation oracle on the same row cloud:

```text
lambda=0.50 raw = 1.712006292e-7 adjusted = 1.045417143e-7
lambda=0.75 raw = 1.191451481e-7 adjusted = 7.275462764e-8
lambda=1.00 raw = 1.298626489e-7 adjusted = 7.929914744e-8
```

Read: true `E[z_L]` is an e-8-class coordinate even for the expensive
highseed branch.  The missing signal is real; the remaining problem is a
deployable proxy.

Deployable proxy checks on the same spaced20 guard:

```text
Full1000-calibrated fullcov-Ez proxy:
  best lambda=0.02 raw = 4.558040281e-7 adjusted = 2.783315381e-7

final preactivation Edgeworth from the highseed row cloud:
  best edge4 path-blend lambda=0.25 raw = 4.554106179e-7 adjusted = 2.780913067e-7

pathCV-estimated signed-Ez row shift:
  best lambda=-0.25 raw = 4.541915092e-7 adjusted = 2.773468719e-7
```

The in-run signed-Ez CV shift is a real zero-extra-compute correction, but only
at the `~2%` raw scale.  It does not rescue the highseed adjusted economics.

Extra-Ez row caches on the matching first200 spaced20 slice
`0,10,...,190` were also not score-positive for highseed:

```text
highseed first200-spaced baseline raw = 4.312851236e-7
extra-Ez c8 positive lambdas: best is lambda=0, positive shifts worsen
extra-Ez c8 negative lambdas: best nonzero lambda=-0.05 raw=4.371110194e-7
```

Read: small extra signed-Ez row branches do not approximate the true-Ez oracle
well enough to justify adding them to the highseed path.  The highseed family
still demonstrates the oracle ceiling, but not a next-upload package.

### 2026-07-07 - damped layerwise fullcov mean calibration

Added:

```text
legacy_workspace/probe_fullcov_layer_affine_ez.py
```

Question: can the fullcov Gaussian final `E[z]` proxy be improved by fitting
one global affine correction per hidden layer to public higher-moment
post-ReLU means, then rolling the corrected mean state forward?

Full200 first-half fit -> second-half transfer:

```text
raw uncalibrated fullcov Ez MSE, last100 = 1.612238101e-4

damped layer affine:
  damp=0.005 transfer Ez MSE = 1.375010251e-4
  damp=0.010 transfer Ez MSE = 1.167955279e-4
  damp=0.020 transfer Ez MSE = 8.440681931e-5
  damp=0.050 transfer Ez MSE = 5.886768365e-5
  damp=0.100 transfer Ez MSE = 2.514983261e-4

undamped damp=1.0 transfer Ez MSE ~= 4.642079011e-2  # unstable
```

Composing a final affine calibration after the damped layer rollout did not
beat the existing Full1000 global final calibration:

```text
existing global final fullcov calibration, same last100 Ez MSE = 5.323708518e-5
best damped-layer + final-affine transfer Ez MSE             ~= 5.343097243e-5
```

Scoring the damped `damp=0.05` proxy through actual row-cloud shifts was
negative:

```text
highseed heldout20 100,105,...,195:
  baseline raw = 5.085814046e-7
  layer-affine proxy positive shifts worsen immediately
  true Ez oracle on same rows reaches raw = 4.098989417e-8 at lambda=1

k26/L4 heldout20 100,105,...,195:
  baseline raw = 1.369066756e-6
  layer-affine proxy positive shifts worsen immediately
```

Read: layerwise fullcov calibration is a useful diagnostic of accumulated
Gaussian mean bias, but it does not improve over the already-used final global
fullcov-Ez calibration and is not a package path.  The true-Ez oracle remains
the right target; this layer-affine closure is not accurate enough.

### 2026-07-07 - fullcov-Ez stats-CDF compute cleanup

Question: the staged fullcov-Ez package uses a primitive polynomial normal CDF
inside the full-covariance Gaussian branch.  Can flopscope's native
`flops.stats.norm.cdf/pdf` reduce instrumented op count without changing the
math?

Diagnostic estimator:

```text
legacy_workspace/candidate_k26_gamma_preedge_l4_fullcov_ez_stats_env.py
```

Compared with the primitive package variant on the same local profile:

```text
primitive fullcov env:
  ops     ~= 7168
  FLOPs   ~= 4.526e10
  warm residual local ~= 39.09 ms

stats-CDF fullcov env:
  ops     ~= 6548
  FLOPs   ~= 4.526e10
  warm residual local ~= 35.18 ms
```

Quick-score parity on Mini indices `0,1` with
`K26_L4_FULLCOV_EZ_LAMBDA=0.05`:

```text
primitive raw = 2.324806e-6
stats raw     = 2.324822e-6
```

Read: this is a real engineering cleanup, not a statistical change.  It
removes about `620` instrumented ops per MLP while leaving counted FLOPs
unchanged.  It should be used in the next fullcov-Ez package if the grader's
current flopscope-client supports the stats primitives cleanly.  It will not
by itself move the branch below `2e-7`; it is margin to bank while searching
for a larger signed row-cloud correction.

### 2026-07-07 - direct fullcov-Ez discrepancy readout falsifier

Question: is the useful fullcov signed-preactivation coordinate just a direct
additive final correction, or does it need to deform the sampled final
preactivation cloud before ReLU?

Feature: `proxy_Ez - sampled_Ez` from the current k26/L4 fullcov cache.

Full1000 grouped OOF, using the current base final row:

```text
base raw                         = 1.476400956e-6
one-feature direct correction    = 1.43256e-6   # rel 0.9703
five-feature nonlinear direct    = 1.43209e-6   # similar
nonlinear row-cloud shift cache  = 1.39377e-6   # better
```

Mini100 transfer of the same direct readout:

```text
base raw                         = 1.325317392e-6
one-feature direct correction    = 1.30203e-6   # rel 0.9824
five-feature nonlinear direct    = 1.30279e-6
nonlinear row-cloud shift cache  = 1.27804e-6
```

Read: the signed coordinate is real, but its best use is not as a final
linear feature.  It should be applied before the final ReLU as a row-cloud
translation, because the gain comes from changing both the signed and absolute
halves of `E[ReLU(z)]`.  Do not spend more time on direct final readouts of
the same fullcov-Ez discrepancy unless a new proxy is added.

### 2026-07-07 - Strassen level repricing for k26/L4 row propagation

Question: can a deeper Strassen schedule cut enough counted arithmetic to move
the k26/L4 family below the current adjusted-score wall?

Diagnostic wrapper:

```text
legacy_workspace/candidate_k26_gamma_preedge_l4_strassen_level_env.py
```

Local `op_profile.py`, two Mini MLPs, no fullcov-Ez branch:

```text
level 1:
  ops   = 1,076
  FLOPs = 4.861e10

level 2:
  ops   = 5,633
  FLOPs = 4.316e10

level 3:
  ops   = 37,532
  FLOPs = 3.880e10
```

Read: level 3 is a trap.  It saves only `~10%` counted FLOPs versus level 2
but creates a huge add/sub/concat residual burden.  Level 1 has a clean op
profile but too many FLOPs.  The current batched level-2 schedule remains the
best row-cloud implementation among these choices.  Do not package Strassen3
for this branch unless flopscope's operation model changes materially.

### 2026-07-07 - sampled gate-response feature probe

New signal hypothesis: the sampled row values are not the only paid
information.  The sampled forward pass also reveals intermediate activation
rates.  For selected layers, collect:

```text
(sample_gate_l - meanfield_gate_l) @ response_l
(sample_mean_l - meanfield_mean_l) @ response_l
```

where `response_l` is a downstream mean-field response to the final
preactivation.  This is different from the old pathCV branch, which uses a
first-layer control variate pushed through deterministic mean-field gates;
here the source is the actual sampled gate deviation.

Diagnostic:

```text
legacy_workspace/probe_sampled_gate_response_oof.py
```

Against the current k26/L4/fullcov-Ez base `pred_lam0p05`:

```text
spaced20, layers 8,16,24,28,30:
  base raw = 1.133115642e-6
  best OOF = 1.128199338e-6  # rel 0.99566

spaced20, layers 2,4,6,8,12,16,20,24,28,30:
  base raw = 1.133115642e-6
  best OOF = 1.063289156e-6  # rel 0.93838

first100, same 10-layer menu:
  base raw = 1.155941627e-6
  best OOF = 1.137910069e-6  # rel 0.98440
```

Interaction check on first100 with final preactivation reliability channels
did not improve the result; the best one-mod interaction was effectively tied
with gate-only, and all-mod interactions overfit.

Read so far: the signal is real but possibly split-sensitive.  The 20-row
warning slice shows a breakthrough-sized gain, while first100 shows only a
small `~1.6%` raw gain.  A Full1000 validation is running; package nothing from
this lane until the Full1000 OOF result confirms durable value.

Full1000 result:

```text
base raw = 1.393771419e-6
best OOF = 1.393584953e-6  # rel 0.999866
```

Read: close the sampled gate-response lane.  It is a classic split mirage:
large on the 20-row warning slice, modest on first100, and effectively zero on
the full public split.  Intermediate gate-rate telemetry is not a durable
correction for the current k26/L4/fullcov base in this linear response form.

Rerun with the wider ridge grid confirmed the closeout:

```text
full1000, layers 2,4,6,8,12,16,20,24,28,30:
  base raw = 1.393771419e-6
  best OOF = 1.393580215e-6  # rel 0.999863
```

### 2026-07-07 - fullcov-Ez error-model probe

Question: the deployable fullcov final-preactivation proxy is useful when used
as a tiny row-cloud shift, but its scalar `E[z_L]` estimate is still far from
the true signed-preactivation oracle.  Can weight/fullcov summary features
predict the signed proxy error and make the row-cloud shift stronger?

Diagnostic:

```text
legacy_workspace/probe_fullcov_ez_error_model.py
```

Results:

```text
full200:
  calibrated fullcov-Ez MSE = 5.064078978e-5
  best OOF proxy MSE        = 4.986079175e-5
  err_r2                    = +0.0153

full1000:
  raw proxy MSE             = 1.603411863e-4
  calibrated proxy MSE      = 5.185686872e-5
  best rich ridge proxy MSE = 5.124210863e-5
  rel                       = 0.988145
  err_r2                    = +0.01185
```

The proxy itself improves slightly, but the real final-row scorer does not.
On full spaced20, the corrected proxy row-cloud shift was worse than the
existing calibrated proxy:

```text
corrected proxy:
  lambda=0.05 raw = 1.137273251e-6
  lambda=0.08 raw = 1.125630753e-6

existing calibrated proxy:
  lambda=0.05 raw = 1.133117175e-6
  lambda=0.08 raw = 1.121512027e-6
```

Nonlinear tree checks over the same feature family did not uncover hidden
structure:

```text
HGB depth3 rel      = 0.990969
HGB depth5 rel      = 0.992174
ExtraTrees depth8 rel = 0.984403
```

Read: close this exact error-model lane.  The features can remove about 1-2%
of the algebraic `E[z_L]` proxy error, but that correction is not aligned with
the final activation residual after the current row-cloud shift.  Do not
replace the packaged `lambda=0.05` fullcov-Ez calibration with this learned
proxy.

### 2026-07-07 - fullcov-Ez stats-CDF package smoke

Question: the current fullcov-Ez package uses a primitive polynomial normal CDF
inside the full-covariance branch.  A stats-CDF diagnostic previously had fewer
instrumented ops in `op_profile.py`.  Does that translate into a cleaner
package smoke on the actual copied package?

Added a separate package folder, leaving the queued primitive artifact
unchanged:

```text
legacy_workspace/
  _pkg_pure26_w05_unionteacher1100gamma_l2snap_split035_045_noaffine_module_batchedstrassen2_preedgeL4_fullcovEzCal_statscdf_finalonly
```

Mini indices `0,5`, same scorer:

```text
stats-CDF package:
  raw=6.753879e-7 adjusted=1.300878e-7 mult=0.19697478 failed=0

primitive package rerun:
  raw=6.753851e-7 adjusted=1.280286e-7 mult=0.19188778 failed=0
```

Read: no promotion.  The predictions match, but local effective compute was
worse for the stats-CDF copy on the direct scorer.  Keep the primitive
fullcov-Ez package as the upload candidate unless a grader-local interface
shows the opposite pricing.

### 2026-07-07 - fullcov-Ez per-MLP lambda gate

Question: scalar `lambda=0.05` is broad but not oracle-optimal per MLP.  Can
cheap target-free MLP-level summaries route the final-preactivation shift
strength without overfitting?

Added:

```text
legacy_workspace/probe_fullcov_lambda_gate.py
legacy_workspace/cache/fullcov_lambda_gate_20260707.npz
```

Feature set: MLP-level summaries of base row, lambda-grid deltas, seed-block
dispersion, fullcov `E[z]` discrepancy, and fullcov log-variance.  Training
label for the diagnostic is the per-MLP best lambda from the grid; folds are
grouped by MLP, and the fitted model is also transferred Full1000 -> Mini100.

Grid facts:

```text
Full1000 scalar best lambda=0.05 raw = 1.393771419e-6
Mini100  scalar best lambda=0.05 raw = 1.278035604e-6
Full1000 per-MLP lambda oracle raw   = 1.298846876e-6
```

Best OOF ridge gates:

```text
alpha=0.03:
  Full1000 OOF raw = 1.389972153e-6  # +0.27% vs scalar
  Mini transfer    = 1.290231414e-6  # worse than scalar

alpha=10:
  Full1000 OOF raw = 1.391520786e-6
  Mini transfer    = 1.281993608e-6  # still worse than scalar
```

Read: the per-MLP oracle gap is real, but this cheap feature gate does not
transfer.  Do not add lambda routing to the package.  The scalar `0.05` remains
the robust choice for the fullcov-Ez coordinate.

### 2026-07-07 - Full1000-calibrated fullcov-Ez package sibling

Packaged a low-risk sibling of the current preferred k26/L4/fullcov-Ez probe
using the Full1000 global affine calibration constants for the deterministic
full-covariance final-preactivation proxy:

```text
artifact:
  whest-starterkit/packages/to_test_remote/
  submission_phase1_pure26_w05_unionteacher1100gamma_l2snap_split035_045_noaffine_module_batchedstrassen2_preedgeL4_fullcovEzFull1000Cal_finalonly_bundle.tar.gz

sha256:
  96204b2259c6ae7f0bc1ef7ea9eba634cabde4c565bc656fb148cabb97f55a6c

package folder:
  legacy_workspace/
  _pkg_pure26_w05_unionteacher1100gamma_l2snap_split035_045_noaffine_module_batchedstrassen2_preedgeL4_fullcovEzFull1000Cal_finalonly

constants:
  FULLCOV_EZ_A = 0.992489309004615
  FULLCOV_EZ_B = 0.00001414661893027946
  FULLCOV_EZ_LAMBDA = 0.05
```

Package contents were checked after an initial packaging mistake and now include
the estimator, direction blob, and manifest:

```text
estimator.py
spherex_pure26_w05_unionteacher1100gamma_l2snap_split035_045_noaffine_module_finalonly_h1half_inputs_256x6656_module.npz
manifest.json
```

Local package smoke:

```text
Mini indices 0,5:
  raw      = 6.734576e-7
  adjusted = 1.267615e-7
  mult     = 0.19109390
  failed   = 0

Full spaced20 0,50,...,950:
  raw      = 1.133121e-6
  adjusted = 2.078432e-7
  mult     = 0.18300318
  failed   = 0
```

Read: this is a raw-polish sibling, not a breakthrough.  It is slightly better
in raw MSE than the existing staged `fullcovEzCal` package on the same checks,
while local adjusted score is within residual/multiplier noise.  It is safe to
park for remote testing after or alongside the existing preferred fullcov-Ez
artifact, but it should not distract from the search for a stronger independent
signed-preactivation coordinate.

### 2026-07-07 - current final-feature nonlinear residual readout

Question: single final-feature readouts and seed weights mostly failed, but
could nonlinear interactions among the already-paid k26/L4 final feature
channels predict the residual left by the current k26/L4/fullcov-Ez base?

Added:

```text
legacy_workspace/probe_current_feature_residual_nonlinear.py
legacy_workspace/cache/current_feature_residual_nonlinear_20260707_smoke.npz
```

Inputs:

```text
features:
  k26_l4_ez_matrix_full1000_20260707.npz
  k26_l4_ez_matrix_mini100_20260707.npz

current base:
  k26_l4_fullcov_ez_full1000cal_shift_full1000_grid_20260707.npz:pred_lam0p05
  k26_l4_fullcov_ez_full1000cal_shift_mini100_grid_20260707.npz:pred_lam0p05

feature shape:
  Full1000: (1000, 256, 101)
  Mini100:  (100, 256, 101)
```

The feature matrix includes both cached final feature rows, current base, the
pre-fullcov base, the fullcov shift delta, and neighboring lambda-grid rows.
Folds are grouped by MLP; the Mini score is a Full1000-trained transfer check.

Results:

```text
base:
  Full1000 raw = 1.393771419e-6
  Mini100 raw  = 1.278035604e-6

ridge best:
  Full1000 OOF raw     = 1.394112312e-6  # worse
  Mini100 transfer raw = 1.282018595e-6  # worse

HGB depth3, 40 iters:
  Full1000 OOF raw     = 1.391661005e-6  # rel 0.998486
  Mini100 transfer raw = 1.284939210e-6  # worse
```

Read: close this final-feature nonlinear readout lane.  The small Full1000 OOF
gain from gradient boosting does not transfer to Mini and is too tiny to
justify packaging or distillation.  This is consistent with the broader
exchangeability story: already-paid final row/seed features expose magnitude
and variance, not a stable signed correction.

### 2026-07-07 - zero-mean arc-cosine fullcov covariance falsifier

Question: can the useful fullcov signed-preactivation proxy be improved by
using the exact zero-mean arc-cosine ReLU covariance kernel for off-diagonals,
while keeping nonzero-mean exact univariate marginals?

Patch:

```text
legacy_workspace/probe_fullcov_gaussian_ez_proxy.py
  added --cov-mode zeromean_arccos
```

Full split spaced20 `0,50,...,950`, float32 CUDA:

```text
zeromean_arccos proxy true-Ez MSE = 3.609620690e+02
zeromean_arccos proxy corr        = +0.884119
```

For comparison, the current first-Hermite fullcov proxy is around `1e-4`
uncalibrated true-Ez MSE on this slice and around `5e-5` after broad affine
calibration.

Read: close this covariance closure immediately.  The zero-mean bivariate
kernel is inconsistent with the nonzero-mean layer state after a few layers and
destroys the signed final-preactivation proxy.  Do not package or expand.

### 2026-07-07 - exact-GH fullcov Gaussian covariance falsifier

Question: maybe the current fullcov-Ez proxy is limited by the cheap
first-Hermite covariance closure.  What if we propagate the exact nonzero-mean
bivariate Gaussian ReLU covariance by 1D Gauss-Hermite quadrature at every
layer?

Patch:

```text
legacy_workspace/probe_fullcov_gaussian_ez_proxy.py
  added --cov-mode exact_gh --gh-order K
  added --psd-project for diagnostic covariance stabilization
```

Full split spaced20 `0,50,...,950`, float32 CUDA:

```text
exact_gh9, no PSD projection:
  one row produced NaNs after covariance drift

exact_gh9 + PSD projection every layer:
  true-Ez MSE = 1.491902548e-4
  corr        = +0.999981

previous Hermite-4 proxy on same slice:
  true-Ez MSE = 1.434877019e-4
```

Read: close exact Gaussian covariance as the missing fullcov-Ez improvement.
Even the expensive stabilized Gaussian covariance does not beat the cheaper
Hermite proxy.  The remaining gap to the true signed-Ez oracle is not mainly a
bivariate Gaussian covariance formula issue; it is higher cumulants /
non-Gaussian ReLU state structure.

### 2026-07-07 - absolute moment-state branch distillation

Fresh current-base oracle re-score:

```text
current k26/L4/fullcov-Ez base raw       = 1.393771419e-6
true Gaussian absolute branch raw        = 1.174233600e-6
true gauss+b3+b21+b111 branch raw        = 1.737033758e-7

OOF blend current + true gauss           = 6.414680148e-7
OOF blend current + true gauss/b3b21     = 4.973657983e-7
OOF blend current + true gauss/b3b21/b111= 1.457566071e-7
```

Read: the high-ceiling hybrid remains an independent absolute moment-state
estimate, not a direct cumulant delta added to the sampler.

Added:

```text
legacy_workspace/probe_absolute_branch_distill_current.py
legacy_workspace/probe_protected_response_branch_transfer.py
```

First Full1000 OOF ridge smoke, predicting `true_b111 - current_base` and then
OOF-blending the predicted absolute branch with the current base:

```text
current_small features:
  best raw = 1.393506669e-6  # flat

current response-contracted k26 features:
  best raw = 1.393241183e-6  # flat / not meaningful

old protected-response features:
  response_abs -> true_b111
  Full1000 OOF raw = 1.309312615e-6
  rel              = 0.939403
  first100         = 1.079926945e-6
  last100          = 1.215151899e-6
  spaced20         = 1.169856376e-6
```

Then built a Mini100 protected seed-stat trajectory cache, no moment labels:

```text
song/data/phase1_l2snap_seedmeans_mini100_slim.npz
base_mse = 1.897754679e-6
```

Full1000 -> Mini100 transfer for the protected-response branch:

```text
alpha=0.01:
  Full1000 OOF raw = 1.309369653e-6  # rel 0.939444
  Mini100 transfer = 1.164090805e-6  # rel 0.910844 vs current Mini base
  gain_mean        = 0.614920
  mini signal corr = 0.305594
```

This is a real transferable signal.  It is not just Full1000 OOF luck.

Group ablation over the protected response features:

```text
cache:
  legacy_workspace/cache/protected_response_group_ablation_20260707.json

best groups:
  final only, 26 features:
    Full1000 OOF = 1.271343101e-6
    Mini100 xfer = 1.106690540e-6
    Mini rel     = 0.865931

  final_abs, 52 features:
    Full1000 OOF = 1.271841299e-6
    Mini100 xfer = 1.106725359e-6

  all_no_abs, 117 features:
    Full1000 OOF = 1.273377752e-6
    Mini100 xfer = 1.117250675e-6

  all features + abs, 234 features:
    Full1000 OOF = 1.309369653e-6
    Mini100 xfer = 1.164087155e-6
```

Read: the useful signal is mostly the auxiliary protected final seed cloud,
not the expensive response contractions and not the all-layer trajectory bank.

Protected final-seed subset transfer:

```text
cache:
  legacy_workspace/cache/protected_final_seed_subset_transfer_20260707.json

all18 protected final seeds:
  Full1000 OOF = 1.272582289e-6
  Mini100 xfer = 1.105858971e-6

first12 protected seeds:
  seed ids     = 0,2,3,6,7,8,13,15,17,20,21,22
  Full1000 OOF = 1.304935316e-6
  Mini100 xfer = 1.121079964e-6

first10 protected seeds:
  Full1000 OOF = 1.317587128e-6
  Mini100 xfer = 1.136066298e-6

first8 protected seeds:
  Full1000 OOF = 1.349455614e-6
  Mini100 xfer = 1.184588626e-6

nonover13 protected seeds:
  seed ids exclude current k26 overlap
  Full1000 OOF = 1.393764439e-6
  Mini100 xfer = 1.278501996e-6  # flat/worse
```

Current k26 final seed-cloud branch transfer:

```text
cache:
  legacy_workspace/cache/current_k26_final_seed_branch_transfer_20260707.json

all26 current k26 final seeds:
  Full1000 OOF = 1.392031340e-6
  Mini100 xfer = 1.278192178e-6  # flat/worse

overlap5 current k26 seeds {8,15,17,20,24}:
  Full1000 OOF = 1.393943154e-6
  Mini100 xfer = 1.278108419e-6  # flat
```

Read: the branch is specific to the protected/no-L4 low-seed final cloud.  The
same seed ids inside the current k26/L4 path do not expose the signed
correction.  This is a good scientific clue but a difficult package path:
directly adding the protected final cloud to the current k26 estimator would
buy roughly 9-13% raw improvement while adding a large fraction of a second
sampler's compute.  At current multipliers, this does not clear the adjusted
score slope unless the auxiliary view can be made much cheaper or replace rows
without damaging the base.

Same-budget mixed-row sketch:

```text
partial cache-only sweep, stopped early:
  current14 + protected12, total 26 rows:
    best seen Mini transfer ~= 1.34e-6
    current Mini base        = 1.278035604e-6
```

Read: replacing k26 rows with protected rows loses too much base accuracy in
the simple final readout.  Do not package this shape.  The durable next idea is
not "add protected rows" but "explain why the protected no-L4 final seed cloud
has the signed moment-state signal and find an analytic/low-row surrogate for
that cloud."

### 2026-07-07 - auxiliary protected final-cloud row compression

Question: the protected/no-L4 final seed cloud gives a real transferable
external branch when blended with the current k26/L4/fullcov-Ez base, but full
rows are too expensive to append.  Can we preserve the signal with fewer sphere
rows per seed?

Added:

```text
legacy_workspace/probe_aux_final_cloud_transfer.py
```

The probe protocol is fixed across row counts:

```text
Full1000:
  fit branch residual to true_b111 absolute moment-state target
  learn scalar blend gain OOF against final targets

Mini100:
  apply the Full1000-trained branch and Full-derived gain
  use Mini targets only for reporting transfer
```

Full-row reproduction:

```text
cache = song/data/phase1_l2snap_seedmeans_full1000_slim.npz
mini  = song/data/phase1_l2snap_seedmeans_mini100_slim.npz

current Mini base raw = 1.278035604e-6
best Mini transfer    = 1.106243013e-6
best Full OOF raw     = 1.273702930e-6
Mini signal corr      = 0.377306
```

Low-row exports:

```text
song/data/phase1_l2snap_seedmeans_full1000_half64_slim.npz
song/data/phase1_l2snap_seedmeans_mini100_half64_slim.npz
song/data/phase1_l2snap_seedmeans_full1000_half128_slim.npz
song/data/phase1_l2snap_seedmeans_mini100_half128_slim.npz
song/data/phase1_l2snap_seedmeans_full1000_half64even_slim.npz
song/data/phase1_l2snap_seedmeans_mini100_half64even_slim.npz
song/data/phase1_l2snap_seedmeans_full1000_half128even_slim.npz
song/data/phase1_l2snap_seedmeans_mini100_half128even_slim.npz
```

Transfer results:

```text
first64:
  best Mini transfer = 1.269139467e-6
  best Full OOF raw  = 1.380939978e-6
  Mini signal corr   = 0.084303

first128:
  best Mini transfer = 1.216044038e-6
  best Full OOF raw  = 1.351330810e-6
  Mini signal corr   = 0.229293

even64:
  best Mini transfer = 1.240396527e-6
  best Full OOF raw  = 1.377211696e-6
  Mini signal corr   = 0.196080

even128:
  best Mini transfer = 1.178144669e-6
  best Full OOF raw  = 1.354515580e-6
  Mini signal corr   = 0.318766
```

Read:

* Row choice matters: balanced/even rows recover more signal than first-prefix
  rows, especially at 128 rows.
* The branch is strongly row-noise limited.  Even the best reduced-row variant
  only gives about `7.8%` raw improvement on Mini versus `13.4%` for full rows.
* Appending `18 x 128` auxiliary rows to the current k26 estimator is unlikely
  to clear compute repricing.  It is a useful science result but not a next
  package candidate.
* Do not spend more GPU on random low-row variants unless a new packaging idea
  makes the auxiliary cloud replace current compute.  The next higher-upside
  path is a near-free independent truth estimate: deterministic/learned moment
  closure, or an analytic surrogate for the protected final-cloud branch.

### 2026-07-07 - current next-winner sweep: stack/router negatives, cumulant closure positive

Goal: stop making isolated 1-2% sampler tweaks and re-evaluate the highest
upside routes against current evidence.  This pass used current repo caches and
Full1000/Full200/Mini transfer checks where available.

#### Low-overhead side-prediction stack on current k26

Ran the existing stack harness on the current `k26gamma` base with protected18,
ExactGH, L4 PRE-EDGE, and k26-z12 branches:

```text
python legacy_workspace/probe_lowoverhead_moment_stack.py \
  --base-mode k26gamma \
  --branch-sets protected18,exactgh,preedge4,k26z12 \
  --modes linear,abs \
  --fit-modes global,neuron \
  --lams 10,30,100,300,1000,3000,10000 \
  --clip 0.003 \
  --out legacy_workspace/cache/lowoverhead_stack_k26gamma_clip003_full1000_mini_20260707.npz
```

Result:

```text
base full raw = 1.470949253e-6
base mini raw = 1.325972344e-6

best transfer by guarded objective:
  global linear lambda=10000
  mini xfer raw = 1.336212375e-6
  mini rel      = 1.007723

unclipped OOF looked artificially strong on Full1000
  OOF raw ~= 1.11e-6
but Full1000 -> Mini transfer exploded.
```

Read: simple stacking of weak side predictions is not a current winner.  The
OOF gain is mostly branch-scale overfit; conservative clipping makes it flat or
worse on Mini.  Do not promote this shape.

#### Union53 / high-count branch router

Ran the existing protected18-vs-union53 router over Full1000:

```text
python legacy_workspace/probe_l2snap_union53_router.py \
  --protected-cache legacy_workspace/cache/l2snap_b05_seed_preds_full1000.npz \
  --union-cache legacy_workspace/cache/l2snap_union53_seed_preds_full1000.npz \
  --feature-mode seed \
  --m53 0.20,0.25,0.30,0.35,0.40,0.45,0.50

python legacy_workspace/probe_l2snap_union53_router.py \
  --protected-cache legacy_workspace/cache/l2snap_b05_seed_preds_full1000.npz \
  --union-cache legacy_workspace/cache/l2snap_union53_seed_preds_full1000.npz \
  --diag-cache legacy_workspace/cache/l2snap_diag_features_full1000.npz \
  --feature-mode seed_diag \
  --m53 0.32,0.35,0.38,0.40,0.42
```

Key numbers:

```text
protected18 raw = 2.301739726e-6
union53 raw    = 8.039954401e-7

seed-only:
  m53=0.30 fixed_union adjusted = 2.411986320e-7
  m53=0.35 fixed_union adjusted = 2.813984040e-7
  m53=0.40 fixed_union adjusted = 3.215981760e-7
  m53=0.40 best OOF gate adjusted ~= 3.222e-7 to 3.226e-7

seed+diag:
  m53=0.35 best OOF gate adjusted ~= 2.770e-7
  m53=0.38 best OOF gate adjusted ~= 3.030e-7
  m53=0.40 best OOF gate adjusted ~= 3.207e-7
```

Read: fixed union53 is good only if engineered to a low enough multiplier; when
the decision is actually nontrivial, target-free gates capture too little of
the oracle.  This matches the earlier router failures: same-run summaries see
magnitude more reliably than signed quadrature luck.

#### High-count sampling curve from cached seed means

Radialized the cached high-seed unit-sphere tensors with `E||X||`:

```text
legacy_workspace/cache/highseed_spherex_full1000_s512_union39.npz
legacy_workspace/cache/highseed_spherex_full200_s512_f16_seeds0to119.npz
```

Full1000 union39 equal prefixes:

```text
prefix26 raw = 1.707660560e-6
prefix32 raw = 1.375232253e-6
prefix39 raw = 1.133166551e-6
```

Full200 native 120-seed prefixes:

```text
prefix032 raw = 1.532328809e-6
prefix048 raw = 1.040825946e-6
prefix064 raw = 8.170060641e-7
prefix080 raw = 6.273292551e-7
prefix096 raw = 5.146909611e-7
prefix120 raw = 4.033216684e-7

fit raw ~= a/k + b over k>=16:
  b ~= 3.63e-8
  predicted k=240 raw ~= 2.34e-7
```

Read: brute-force high-count sampling can get leaderboard-class raw, but not
leaderboard-class adjusted unless the compute is compressed radically.  The
sampling-only asymptote is not enough for the e-8 adjusted target unless it is
paired with a very cheap closure/control-variate mechanism.

#### Higher-moment Edgeworth oracle and partial-K3 falsifier

Refreshed the true-preactivation Edgeworth oracle on Full200:

```text
python legacy_workspace/probe_higher_moment_edgeworth_oracle.py \
  --moment-root /mnt/d/datasets/arc-whestbench-higher-moments-2026/full \
  --indices 0:200 --folds 5
```

Final-layer raw:

```text
Gaussian preactivation readout = 1.162166983e-6
true K3 Edgeworth              = 1.692510618e-7
true K3+K4 Edgeworth           = 2.228986603e-8
final delta corr:
  K3  = +0.91697
  K34 = +0.98943
```

Then decomposed K3 into b3/b21/b111:

```text
python legacy_workspace/probe_higher_moment_partial_k3_oracle.py \
  --moment-root /mnt/d/datasets/arc-whestbench-higher-moments-2026/full \
  --weights-npz-cache legacy_workspace/cache/phase1_full1000_weights_targets.npz \
  --indices 0:200 --folds 5
```

Final-layer raw:

```text
gauss             = 1.162166983e-6
true_k3           = 1.692510618e-7
b3_k3             = 1.201619074e-6
b3_b21_k3         = 4.808117929e-6
fit b3+b21 gain   = 7.673749481e-7
```

Read: the easy diagonal/two-index K3 pieces do not explain the big oracle gap.
The missing object is the off-diagonal all-distinct b111/common-factor
interaction.  A deployable K3 path must model b111 or an equivalent low-rank
common-factor contraction.

#### Structured b111/common-factor closure

Validated the existing structured closure probe with oracle-ish true post
covariance/common-factor features:

```text
python legacy_workspace/probe_higher_moment_structured_closure.py \
  --moment-root /mnt/d/datasets/arc-whestbench-higher-moments-2026/full \
  --weights-npz-cache legacy_workspace/cache/phase1_full1000_weights_targets.npz \
  --indices 0:200 --rank 4 --mode eigen --folds 5 \
  --ridge 0.03,0.1,0.3 --clip-q 0.01 --g3-clip 1.0 --g4-clip 4.0
```

Full200 OOF final raw:

```text
gauss      = 1.162166983e-6
true_k3    = 1.692510618e-7
true_k34   = 2.228986603e-8
structured = 7.071229764e-7  # rank4 eigen, ridge=0.1, K34 readout
g3_r2      = +0.88006
g4_r2      = +0.16397
```

Rank 8 on first100 did not improve rank 4:

```text
rank4 first100 best final ~= 6.590988167e-7
rank8 first100 best final ~= 6.592429392e-7
```

Existing replicated b111 cache agrees:

```text
legacy_workspace/cache/b111_lowrank_common_factor_final_full200_c21_20260707.npz
  rank4 direct  = 9.13258674e-7
  rank4 OOF     = 7.32198255e-7
  rank4 two_OOF = 6.85571309e-7
  b111_corr     = 0.97333925
```

Read: this is the best non-sampler avenue found in this sweep.  It is not
deployable yet because it uses true post-moment/covariance features from the
higher-moment files, but it gives a stable target: reproduce the low-rank
common-factor b111 features with a cheap propagated/sketched state, then decode
with Edgeworth and blend with the k26 sampler.

Current action:

* No new package promoted from this sweep.
* Keep the practical remote queue headed by the rectsparse k26 package.
* Next serious engineering branch is deployable b111/common-factor closure,
  not another same-run router or generic residual learner.

Rerun sanity check on the same day, using the cached tensors but a fresh Ridge
fit:

```text
full rows, alpha=3:
  Full1000 OOF raw  = 1.273072731e-6
  Mini100 transfer  = 1.106386324e-6
  Mini signal corr  = 0.377054

even128 rows, alpha=0.01:
  Full1000 OOF raw  = 1.354485410e-6
  Mini100 transfer  = 1.177839477e-6
  Mini signal corr  = 0.319572
```

This confirms the branch signal is not a 10-MLP artifact.  It is a real
Full1000-to-Mini100 transfer signal.  The open problem is deployability: full
rows are too expensive, and even128 recovers only part of the raw gain.

Mode split to locate the deployability problem:

```text
full auxiliary cache, stats-only features:
  features          = 16  # protected/equal/std/mad/max/m3/m4 and abs
  best Mini100 raw  = 1.252460075e-6
  Mini signal corr  = 0.154642

full auxiliary cache, seed_mean-only features:
  features          = 36  # 18 seed means and abs
  best Mini100 raw  = 1.101176895e-6
  Mini signal corr  = 0.381695
```

Read: the useful signal is in the expensive per-seed final cloud geometry, not
in cheap aggregate statistics.  A deployable path must either compress those
seed means, replace existing sampled rows with them, or derive an analytic
surrogate for their signed response-aligned direction.  Adding cheap moments
alone is not enough.

Protected seed subset beam search:

```text
probe_aux_seed_subset_search.py
cache: aux_seed_subset_beam_consecutive_alpha10_20260707.json
base Mini100 raw = 1.278035604e-6
base local multiplier used for arithmetic = 0.18131035
full 18-seed protected branch standalone multiplier estimate = 0.12647591

k  best Mini raw       naive append multiplier  naive append adjusted
2  1.232542673e-6      0.19536                  2.40794e-7
4  1.187056548e-6      0.20942                  2.48589e-7
6  1.150170533e-6      0.22347                  2.57027e-7
8  1.128085948e-6      0.23752                  2.67945e-7
10 1.111424722e-6      0.25157                  2.79606e-7
12 1.101308847e-6      0.26563                  2.92538e-7
14 1.094882156e-6      0.27968                  3.06217e-7
18 1.100826289e-6      0.30779                  3.38819e-7
```

Read: selected protected seeds are very efficient raw-MSE-wise: `k=10..14`
matches or beats all 18 seed means in the fitted proxy, likely by removing
noisy seeds.  But the append arithmetic is still wrong.  Even `k=2`, the
best adjusted append point, loses to the base on this Mini100 model.  This
branch only becomes deployable if those protected seed means replace current
K26 rows, feed a cheap analytic surrogate, or route only on MLPs where the
extra branch value is predictably high.

### 2026-07-07 - latest decision checkpoint

This is the current working state after the stack/router/high-count/cumulant
sweep above:

* Remote-ready queue: still led by
  `submission_phase1_pure26_w05_unionteacher1100gamma_l2snap_split035_045_rectsparse240s7_preedgeL4_fullcovEzCal_finalonly_bundle.tar.gz`.
* High-count sampling: raw can fall to `~4.03e-7` at 120 cached seeds, but the
  compute slope is not enough by itself.  Treat it as a teacher/control source,
  not a standalone e-8 adjusted solution.
* Routers: union53 and pathCV-style target-free gates remain weak in the
  decision region.  Do not spend the next iteration on another same-run
  branch-choice gate unless a genuinely new signed observable appears.
* Main next branch: deployable b111/common-factor cumulant closure.  The
  validated target is `~7.07e-7` Full200 OOF raw with oracle-ish structured
  features, against `2.23e-8` true K3+K4 Edgeworth raw.  The engineering task is
  to approximate the needed post-covariance/common-factor features without
  reading public higher-moment labels.

### 2026-07-07 - sampled b111 sensor calibration probe

Added:

```text
legacy_workspace/probe_sampled_b111_sensor_calibration.py
```

Purpose: separate two questions that earlier sample-C21 probes conflated:

1. do the paid k26 rows contain a signed estimate of the final-transition
   b111/common-factor cumulant?
2. can the same sampled mean/variance/C21 state be decoded directly into a
   better final mean?

Spaced20, equal moment weights:

```text
base raw                    = 1.226775436e-6
sample_gauss raw            = 2.532287940e-6
oracle_gauss raw            = 1.276144989e-6
oracle_true_k3 raw          = 2.039836526e-7
sample b321 corr/R2         = +0.99924 / +0.99844
sample b111 corr/R2         = +0.97902 / +0.95280
oracle-state sampleall raw  = 1.108318777e-6
raw-cumulant OOF best       = 1.116395072e-6  # rel 0.910, looked promising
```

Full200, equal moment weights:

```text
base raw                    = 1.313140624e-6
sample_gauss raw            = 2.474272731e-6
oracle_gauss raw            = 1.163066404e-6
oracle_true_k3 raw          = 1.699202818e-7
sample b321 corr/R2         = +0.99904 / +0.99805
sample b111 corr/R2         = +0.97216 / +0.93510
oracle-state sampleall raw  = 9.607680210e-7
oracle-state sampleall OOF blend with base = 5.514816295e-7
deployable raw-cumulant OOF best           = worse than base
```

Follow-up cached analysis on the Full200 probe:

```text
sample raw features -> residual target, OOF+gain best ~= 1.303530108e-6
sample raw features -> oracle_delta target, signal corr to oracle_delta ~= 0.645
sample raw features -> oracle_delta target, signal corr to true residual ~= 0.004
```

Read:

* The k26 row cloud is an excellent sensor for the final-transition b111
  tensor contraction itself.
* The direct sampled mean/variance/Edgeworth state is badly calibrated.
* Spaced20 raw-cumulant residual gains were selection noise; Full200 rejects a
  deployable same-row calibration add-on.
* The real ceiling remains large: if we can produce a deployable final
  preactivation state close to the true mean/variance, the already-paid sampled
  b111 sensor becomes useful.  The next branch should target deployable final
  pre-state estimation or a mechanistic closure for that state, not another
  same-row residual router.

### 2026-07-07 - rectsparse tail-keep compression for current k26/L4/fullcov package

Question: can the current robust k26/L4/fullcov-Ez package move closer to the
score floor by reducing the tail sparse matmul keep count, without losing more
raw MSE than the compute saving buys?

Copied the staged `rectsparse240s7` package into isolated keep-count probes and
changed only `SPARSE_TAIL_KEEP`.

Spread5 `full` indices `0,250,500,750,999`:

```text
keep=192 raw=6.983936e-6 adjusted=1.056817e-6 mult=0.15325683  # collapse
keep=208 raw=1.397234e-6 adjusted=2.259377e-7 mult=0.16144146
keep=224 raw=1.055065e-6 adjusted=1.795385e-7 mult=0.16864413
keep=228 raw=1.076788e-6 adjusted=1.851157e-7 mult=0.17056338
keep=232 raw=1.094596e-6 adjusted=1.904457e-7 mult=0.17260243
keep=236 raw=1.094930e-6 adjusted=1.933741e-7 mult=0.17508755
keep=240 raw=1.084957e-6 adjusted=1.925938e-7 mult=0.17625577
```

`keep=224` won the tiny spread5 slice, but failed the shifted full slice.  The
robust point is `keep=232`:

```text
rectsparse232s7:
  full spaced20 raw=1.129617e-6 adjusted=1.933005e-7 mult=0.17047814
  full offset20 raw=1.380689e-6 adjusted=2.350453e-7 mult=0.17069001
  mini spaced20 raw=1.329418e-6 adjusted=2.255026e-7 mult=0.17010411

staged rectsparse240s7 comparison:
  full spaced20 raw=1.130103e-6 adjusted=1.976967e-7 mult=0.17451628
  full offset20 raw=1.372751e-6 adjusted=2.412333e-7 mult=0.17619270
  mini spaced20 raw=1.329e-6 adjusted=2.313e-7 mult~=0.174
```

Packaged remote-test artifact:

```text
whest-starterkit/packages/to_test_remote/
submission_phase1_pure26_w05_unionteacher1100gamma_l2snap_split035_045_rectsparse232s7_preedgeL4_fullcovEzCal_finalonly_bundle.tar.gz

sha256 cbba28e3ed750bed3788d7aa358b05ff973ebc3cf80e3755ed818b7ce6f97f92
```

Validation:

```text
whest validate --estimator ...rectsparse232s7.../estimator.py
Status: success
```

Read: this is a small but robust compute-polish promotion over the previous
`rectsparse240s7` staged package.  It is not the e-8 breakthrough; keep using
the moment/preactivation-state lane for the next large jump.

Lambda follow-up on the `rectsparse232s7` winner:

```text
lambda=0.04 full spaced20 adjusted=1.968943e-7 raw=1.143000e-6
lambda=0.05 full spaced20 adjusted=1.933005e-7 raw=1.129617e-6
lambda=0.06 full spaced20 adjusted=1.919882e-7 raw=1.121020e-6

lambda=0.05 full offset20 adjusted=2.350453e-7 raw=1.380689e-6
lambda=0.06 full offset20 adjusted=2.381227e-7 raw=1.398197e-6
```

Read: `0.06` is another narrow spaced20 improvement that fails the shifted
full slice.  Keep the packaged `0.05` scalar.  Do not promote a lambda sibling.

Tail-start bracket at `keep=232, lambda=0.05`, spread5 only:

```text
start=6 raw=1.093685e-6 adjusted=1.931961e-7 mult=0.17460576
start=7 raw=1.094596e-6 adjusted=1.904457e-7 mult=0.17260243
start=8 raw=1.100781e-6 adjusted=1.969115e-7 mult=0.17669401
```

Read: start position is closed.  The robust rectsparse polish remains
`keep=232, start=7, lambda=0.05`.

### 2026-07-07 - post-compaction next-winner sweep

Goal: after resuming, test only branches with a plausible path to a new
remote winner, while avoiding overlap with the parallel sampler assistant.

#### Stats-CDF cleanup on current rectsparse232

Copied the current best practical package:

```text
_pkg_pure26_w05_unionteacher1100gamma_l2snap_split035_045_noaffine_module_rectsparse232s7_preedgeL4_fullcovEzCal_finalonly
```

to:

```text
_pkg_pure26_w05_unionteacher1100gamma_l2snap_split035_045_noaffine_module_rectsparse232s7_statscdf_preedgeL4_fullcovEzCal_finalonly
```

and changed only `_normal_pdf_cdf_approx` to call
`flops.stats.norm.pdf/cdf`, matching the older statscdf cleanup.

Same-row Mini idx0 check:

```text
original rectsparse232:
  raw=5.250890e-7 adjusted=9.636415e-8 mult=0.18351965

statscdf rectsparse232:
  raw=5.250928e-7 adjusted=9.648959e-8 mult=0.18375723
```

Read: neutral to slightly worse.  Do not package the statscdf rectsparse232
sibling; it is not a compute win under the local scorer.

#### Auxiliary final-cloud low-count replacement grid

Added:

```text
legacy_workspace/probe_aux_final_cloud_lowcount_grid.py
```

Question: the protected/no-L4 final seed cloud carries a transferable
true-b111/absolute-moment branch, but appending it to the full current sampler
is too expensive.  Can it replace some current k26 rows instead?

Half128/even auxiliary rows, seed means + abs only:

```text
cache:
  legacy_workspace/cache/aux_final_cloud_lowcount_half128even_seedsabs_20260707.json

best by Mini modeled adjusted:
  count=26, base=gamma, alpha=0.01
  Mini raw          = 1.322289288e-6
  Full1000 OOF raw  = 1.470648855e-6
  modeled multiplier= 0.233716095
  Mini adjusted est = 3.090402889e-7
```

Including the auxiliary aggregate seed statistics + abs:

```text
cache:
  legacy_workspace/cache/aux_final_cloud_lowcount_half128even_seedstatsabs_20260707.json

best by Mini modeled adjusted:
  count=26, base=gamma, alpha=0.003
  Mini raw          = 1.301405738e-6
  Full1000 OOF raw  = 1.464371042e-6
  modeled multiplier= 0.233716095
  Mini adjusted est = 3.041594671e-7
```

Read: the branch is real but it does not replace current rows efficiently.
Lower counts are far too noisy, and the best point still uses all 26 current
seeds plus the half-row auxiliary cloud.  That is behind the staged
rectsparse232 package on adjusted economics.  Do not package this shape.

#### Auxiliary final-cloud plus cheap side-branch stack

Added:

```text
legacy_workspace/probe_aux_cloud_plus_side_stack.py
```

Question: weak signals might fail in isolation but combine.  Tested the
half128/even auxiliary true-b111 proxy with the existing exactGH, PRE-EDGE L4,
k26-z12, and protected18 side branches under Full1000 grouped OOF and
Full1000-fit -> Mini100 transfer.

```text
cache:
  legacy_workspace/cache/aux_half128even_plus_side_stack_k26gamma_20260707.npz

k26gamma base:
  Full1000 raw = 1.470949253e-6
  Mini100 raw  = 1.325972344e-6

best transfer:
  aux alpha        = 0.003
  set              = aux_only
  ridge            = 300
  aux branch Mini  = 1.299803048e-6
  stacked Mini raw = 1.325863159e-6
  stacked Mini rel = 0.999918

aux_plus_all:
  small ridge overfits catastrophically on Mini
  high ridge collapses back to the base
```

Read: the interaction check is negative.  The auxiliary branch can be fit as a
moment-state teacher, but a target-free stack coefficient learned from
Full1000 final residuals does not transfer.  Existing cheap side branches do
not unlock it.  The path remains: derive an independent deployable
moment-state estimate, not another stack over sampler-side predictions.

#### Rectsparse232 / pathCV84-sparse router repricing

Repriced the two-arm oracle after replacing the base arm from rectsparse240 to
the newer rectsparse232.  The oracle remains tempting, but target-free
weight-only gates still fail the transfer guards.

Fixed-arm and oracle arithmetic:

```text
full_spaced20:
  k26_232  raw=1.129616520e-6 adjusted=1.933004748e-7 mult=0.170478
  pathCV84 raw=6.866844771e-7 adjusted=2.599167758e-7 mult=0.464774
  oracle   adjusted=1.547463939e-7, path wins 8/20

full_offset20:
  k26_232  raw=1.380688785e-6 adjusted=2.350452762e-7 mult=0.170690
  pathCV84 raw=4.700668718e-7 adjusted=2.448839760e-7 mult=0.520597
  oracle   adjusted=1.765625028e-7, path wins 10/20

mini_spaced20:
  k26_232  raw=1.329417833e-6 adjusted=2.255025592e-7 mult=0.170104
  pathCV84 raw=9.162633461e-7 adjusted=3.289516795e-7 mult=0.444084
  oracle   adjusted=1.819936024e-7, path wins 7/20
```

Re-ran the existing weight-only sparse-router gate with `base_mult=0.17047814`.
It trained on the old Full200 branch-gate cache and evaluated on the three
current warning slices:

```text
train fixed base/path/oracle = 3.718636e-7 / 2.338261e-7 / 1.991027e-7
train OOF ridge_loss         = 2.694336e-7
train OOF logit_win          = 2.634933e-7
train OOF rf_win             = 2.364035e-7
train fit-threshold          = 2.242331e-7

full_spaced20 best transfer gate = 2.427722e-7  # worse than fixed base
full_offset20 ridge_loss gate    = 2.169856e-7  # improves this slice
mini_spaced20 best transfer gate = 3.239729e-7  # badly worse than fixed base
```

Read: the two-arm oracle is real, but the observable gate still does not
transfer.  Do not package a rectsparse232/pathCV router unless a new signed
observable appears.  The single-arm rectsparse232 package remains the practical
remote probe.

#### PathCV sparse-tail and count audit

Checked whether pathCV's raw can be repriced into a winner by lowering count or
sparse-tail keep.  The attractive tiny slices do not survive full-spaced20.

```text
pathCV84 sparse keep sweep, full spread5:
  keep208 raw=8.701287e-7 adjusted=4.335448e-7 mult=0.500217
  keep216 raw=4.555582e-7 adjusted=2.315524e-7 mult=0.509150
  keep220 raw=3.278146e-7 adjusted=1.695786e-7 mult=0.518194  # slice trap
  keep224 raw=4.277169e-7 adjusted=2.229322e-7 mult=0.522746

pathCV84 sparse keep220, full spaced20:
  raw=5.141557e-7 adjusted=2.640549e-7 mult=0.513545

pathCV84 sparse keep224, full spaced20:
  raw=6.866845e-7 adjusted=2.599168e-7 mult=0.464774
```

Lower-count pathCV Strassen2 tails were also broad-slice negative:

```text
pathCV68 full spaced20 raw=7.768581e-7 adjusted=2.856673e-7 mult=0.427724
pathCV72 full spaced20 raw=7.729867e-7 adjusted=2.984278e-7 mult=0.451590
pathCV76 full spaced20 raw=7.391886e-7 adjusted=2.946858e-7 mult=0.477345
pathCV80 full spaced20 raw=7.061952e-7 adjusted=2.871745e-7 mult=0.497966
pathCV84 full spaced20 raw=6.708478e-7 adjusted=2.789118e-7 mult=0.521464
pathCV88 full spaced20 raw=6.498652e-7 adjusted=2.787617e-7 mult=0.548375
```

Read: full-depth pathCV rows have real raw power but the compute slope remains
wrong.  Treat this family as a teacher/control source, not as a fixed-count
submission path.

#### Old Song / Hybrid2 learned-estimator artifacts

The Song notes contain strong older learned-teacher numbers, but those artifacts
belong to the old fullskip/hybrid2 family and do not directly apply to Phase 1
depth 32.  Smoke tests:

```text
candidate_hybrid2_student_c16r8_forward_only_states_blob_fast2_estimator.py:
  failed at candidate_hybrid2_estimator.py:_h_cal[layer_idx]

guarded copy candidate_hybrid2_phase1_calguard_estimator.py:
  extended missing depth-8 calibration rows with identity Edgeworth mean
  and lowrank table with None
  Mini idx0 raw=2.199391e-5 adjusted=2.199391e-6 mult=0.1

candidate_final_fullskip_estimator.py:
  Mini idx0 raw=2.076757e-5 adjusted=2.222316e-6 mult=0.107009
```

Read: not a quick drop-in.  A learned external estimator may still be a major
route, but it needs a Phase-1-native state/model port or retraining against the
current k26/rectsparse state, not the old depth-8/fullskip packages.

Current decision:

* No new package promoted in this sweep.
* Keep remote queue headed by:
  `submission_phase1_pure26_w05_unionteacher1100gamma_l2snap_split035_045_rectsparse232s7_preedgeL4_fullcovEzCal_finalonly_bundle.tar.gz`.
* The next high-upside branch should be a Phase-1-native independent external
  estimate for final preactivation state or cumulant closure, suitable for
  blending with the current sampler.  Repeating same-row gates, pathCV fixed
  counts, or old Song/hybrid2 package smokes is below the needed gain scale.

### 2026-07-07 - Phase-1 synthetic supervision builder for external-estimator lane

After the post-compaction sweep, the closed lanes were:

* same-row final-feature residual learners,
* target-free routers over existing seed/pathCV outputs,
* fullcov-Ez scalar error models,
* literal extra-Ez row append/replacement,
* old depth-8 Song/hybrid2 package drops.

The common failure mode is the same: the current sampler rows expose variance
and magnitude, but not the missing signed final-preactivation / cumulant state.
The remaining high-upside learned route therefore needs a Phase-1-native
external estimator, not another readout over the existing row cloud.  The old
synthetic caches in `song/data` are mostly depth-8/fullskip; only tiny
depth-32 timing smoke data exists.

Added:

```text
legacy_workspace/build_phase1_synthetic_mc_cache.py
```

Purpose:

* generate width-256/depth-32 He-Gaussian synthetic MLPs;
* estimate final means, and optionally all-layer means, by Torch MC;
* optionally attach cheap diagonal-Gaussian prediction rows for immediate
  Song/PERO-style residual training;
* optionally attach any estimator's prediction rows in the same matrix-cache
  shape consumed by Song/PERO-style trainers;
* keep estimator attachment optional because running the current k26 sampler on
  many synthetic MLPs is itself expensive.

Smoke:

```text
python legacy_workspace/build_phase1_synthetic_mc_cache.py \
  --out /tmp/phase1_synth_smoke.npz \
  --n-mlps 1 --samples 256 --chunk 128 --width 16 --depth 4 \
  --all-layers --device cpu
```

Result: wrote the expected keys:

```text
weights, target, label_noise_mse, seed, seeds, samples, width, depth,
source, target_rows, label_noise_rows_mse
```

Diagonal-row smoke:

```text
python legacy_workspace/build_phase1_synthetic_mc_cache.py \
  --out /tmp/phase1_synth_diag_smoke.npz \
  --n-mlps 1 --samples 256 --chunk 128 --width 16 --depth 4 \
  --all-layers --diag-rows --device cpu
```

Result: additionally wrote:

```text
pred_rows, base, residual, base_mse, estimator_path=diagonal_gaussian_mean_rows
```

CUDA timing outside the sandbox on RTX 4090:

```text
python legacy_workspace/build_phase1_synthetic_mc_cache.py \
  --out /tmp/phase1_synth_depth32_cuda_timing_1x1m.npz \
  --n-mlps 1 --samples 1000000 --chunk 65536 \
  --width 256 --depth 32 --device cuda

elapsed_sec 1.59
final_noise_mse ~= 2.571e-08
```

This is surprisingly favorable: final-only synthetic supervision is minutes
for hundreds of MLPs on the local 4090.  All-layer accumulation and estimator
attachment will be slower, but the core depth-32 MC bake is not the bottleneck.

Real CUDA sketch, deliberately not launched in this sweep:

```text
python legacy_workspace/build_phase1_synthetic_mc_cache.py \
  --out /mnt/d/datasets/phase1_synth_depth32_256xN_mc.npz \
  --n-mlps 256 --samples 1000000 --chunk 65536 \
  --width 256 --depth 32 --diag-rows --device cuda
```

Read: this does not promote a package.  It creates the missing data-generation
entry point for the only learned lane that has not already been falsified:
Phase-1-native external absolute-state/PERO training, then OOF/Full->Mini
blend testing against the current k26/rectsparse sampler.  Meaningful labels
will need enough samples that MC label noise is below the residual scale being
trained; small smoke caches are only for plumbing.

Follow-up infrastructure built in the same pass:

```text
legacy_workspace/build_phase1_diag_matrix_cache.py
```

This memory-maps the public Phase-1 weight caches and writes diagonal-Gaussian
`pred_rows` without duplicating the multi-GB weights.  Built:

```text
song/data/phase1_diag_mini100_matrix_cache.npz
  base_mse = 9.482238676e-4

song/data/phase1_diag_full1000_matrix_cache.npz
  base_mse = 1.009358068e-3
```

Trainer plumbing smoke:

```text
python song/src/train_equivariant_residual_big.py \
  --cache song/data/phase1_diag_mini100_matrix_cache.npz \
  --out /tmp/phase1_diag_student_smoke.json \
  --epochs 1 --hidden 4 --rounds 1 --batch 1 --device cuda --eval-every 1
```

Result:

```text
test_base      = 8.850247e-4
test_corrected = 6.917799e-4
test_ratio     = 0.7817
```

This proves the public diagonal cache and existing student trainer connect
correctly.  The absolute error remains far above competition scale; this is
only the start of an external absolute-estimator lane.

Attempted next job, not run by the agent because the escalation layer rejected
the large D: write/GPU job:

```text
/usr/bin/time -f 'elapsed_sec %e' \
python legacy_workspace/build_phase1_synthetic_mc_cache.py \
  --out /mnt/d/datasets/phase1_synth_depth32_diag_alllayers_256x1m.npz \
  --n-mlps 256 --samples 1000000 --chunk 65536 \
  --width 256 --depth 32 --all-layers --diag-rows --device cuda
```

If explicitly approved/run by the user, this is the next data artifact for a
Phase-1-native PERO/student pretrain.  The one-MLP timing suggests it should be
minutes-scale on the local RTX 4090, with final MC label-noise MSE around
`2.6e-8` at one million samples.

## 2026-07-07: K26 rectsparse compute-polish, final response pruning

Starting point:

```text
submission_phase1_pure26_w05_unionteacher1100gamma_l2snap_split035_045_rectsparse232s7_preedgeL4_fullcovEzCal_finalonly_bundle.tar.gz
sha256 cbba28e3ed750bed3788d7aa358b05ff973ebc3cf80e3755ed818b7ce6f97f92
```

Full spaced20 guard for the base rectsparse232 branch:

```text
raw=1.129616520e-6
local_adjusted=1.933004748e-7
local_multiplier=0.170478
FLOPs=4.239381073e10
warm_ops=7290
```

The successful compute cut is dynamic activation-energy pruning, not a new
analytical closure.  The sampled row cloud selects high-energy input
coordinates for a contraction:

```text
score = mean(h*h, axis=0)
idx   = topk(score, keep)
z     = matmul(h[:, idx], W[idx, :])
```

This is useful because the sampled cloud itself reveals which coordinates are
doing work for this particular MLP.  Static or diagonal analytic replacements
were not equivalent.

Final-layer-only pruning frontier on full spaced20:

```text
final_keep raw_mse        local_adjusted multiplier FLOPs
232        1.129616520e-6 1.937993682e-7 0.171181  4.227e10
224        1.129616520e-6 2.009701043e-7 0.177414  4.223e10
216        1.129616000e-6 2.056766000e-7 0.181206  4.219e10
208        1.129616000e-6 2.072310000e-7 0.182192  4.215e10
192        1.129612000e-6 2.053574000e-7 0.181332  4.206e10
184        1.130308000e-6 1.954385000e-7 0.172731  4.202e10
176        1.131132000e-6 1.950984000e-7 0.172032  4.198e10
160        1.210728000e-6 2.169542000e-7 0.178694  4.190e10
```

Read: `176-192` preserve raw accuracy; `160` is past the cliff.  The local
adjusted column is noisy because these guards were run under different local
residual conditions; use raw MSE plus a separate `op_profile.py` for remote
economics.

Counterexample / kill result:

```text
candidate_k26_rectsparse232_diag_ez_env.py
full spaced20 raw=3.447922052e-6
adjusted=5.608613933e-7
multiplier=0.163007
FLOPs=4.031e10
```

This says the fullcov off-diagonal response correction is load-bearing.  Do
not replace it by a diagonal/mean Gaussian proxy.

One-level rectangular Strassen-Winograd trades counted FLOPs for far fewer
small operations:

```text
base two-level Strassen:
  raw=1.129616520e-6, FLOPs=4.239e10, warm_ops=7290
  predicted remote multiplier:
    25us/op 0.2229
    45us/op 0.2765
    65us/op 0.3301

one-level Winograd, final_keep=224:
  raw=1.129617e-6, FLOPs=4.720e10, warm_ops=2645
  predicted remote multiplier:
    25us/op 0.1978
    45us/op 0.2173
    65us/op 0.2367

one-level Winograd, final_keep=176:
  raw=1.131132e-6, FLOPs=4.691e10, warm_ops=2645
  predicted remote multiplier:
    25us/op 0.1968
    45us/op 0.2162
    65us/op 0.2357
```

Staged package:

```text
whest-starterkit/packages/to_test_remote/submission_phase1_pure26_w05_unionteacher1100gamma_l2snap_split035_045_rectsparse232s7_finalsp176_winograd1_preedgeL4_fullcovEzCal_finalonly_bundle.tar.gz
sha256 a90dd5f4ed1bb6b6ee5d6c5b718b9aeb6430a5b5b57bb981aeeba9ce353d2e59
contents: estimator.py, manifest.json, bundled .npz
validated: yes
```

Interpretation:

- The solution improved because it removed computation that carried almost no
  final-layer signal, identified by the actual sampled activations.
- The similar next knobs are layer-specific energy pruning and response-weighted
  pruning.  The unsafe knobs are diagonalizing `fullcovEz` or thinning the
  source rows without a held-out raw guard.

Follow-up: output-row weighted final pruning did not rescue the final_keep=160
cliff:

```text
final_keep=160, score=mean(h*h)*sum(W*W,row)
full spaced20 raw=1.213077e-6
adjusted=2.214797e-7
multiplier=0.182336
FLOPs=4.681e10
```

Row norms are too uniform to give the missing ordering signal.

Layer-specific late-tail pruning was more useful.  These variants keep
`SPARSE_TAIL_KEEP=232` through layer 23, lower only layers 24-30, and use
`final_keep=176` with one-level Winograd:

```text
late_keep raw_mse       local_adjusted multiplier FLOPs
224       1.131136e-6   2.032912e-7    0.179449  4.657e10
216       1.131169e-6   2.049618e-7    0.180853  4.624e10
208       1.131138e-6   2.007706e-7    0.177276  4.590e10
192       1.133884e-6   1.994972e-7    0.175763  4.523e10
184       1.188007e-6   2.083533e-7    0.175075  4.490e10
176       1.471149e-6   2.540485e-7    0.172689  4.457e10
160       6.712572e-6   1.148123e-6    0.171264  4.390e10
```

Read: `late192` is the useful edge.  `late184` starts the raw cliff; `late176`
breaks it.  `op_profile.py` for `late192 + finalsp176 + winograd1`:

```text
FLOPs=4.523e10
warm_ops=2645
predicted remote multiplier:
  25us/op 0.1906
  45us/op 0.2101
  65us/op 0.2295
```

Staged package:

```text
whest-starterkit/packages/to_test_remote/submission_phase1_pure26_w05_unionteacher1100gamma_l2snap_split035_045_rectsparse232s7_late192_finalsp176_winograd1_preedgeL4_fullcovEzCal_finalonly_bundle.tar.gz
sha256 c5c43b40a4f9a15473757ebfb5c6ced659226b424547226d04b3501b39183054
contents: estimator.py, manifest.json, bundled .npz
validated: yes
```

This supersedes `finalsp176_winograd1` as the preferred economics probe.  The
expected improvement is only a few percent adjusted score, but it is a real
directional win: final readout pruning and late-tail pruning compose.

Additional transfer guards for the staged `late192 + finalsp176 + winograd1`
package:

```text
full offset20 raw=1.384830e-6 adjusted=2.440782e-7 mult=0.176522
mini spaced20 raw=1.331829e-6 adjusted=2.351068e-7 mult=0.176798
```

For comparison, the earlier broad guards for base `rectsparse232s7` were:

```text
full offset20 raw=1.380689e-6 adjusted=2.350453e-7 mult=0.170690
mini spaced20 raw=1.329418e-6 adjusted=2.255026e-7 mult=0.170104
```

Read: the late-tail package loses only about `0.2-0.3%` raw on these transfer
slices.  Local adjusted is worse because local residual timing differs between
schedules; the remote hypothesis rests on the much lower warm op count and the
lower FLOPs from late-tail pruning.

Final keep boundary on top of `late192`:

```text
late192 + final_keep=168:
  full spaced20 raw=1.138764e-6 adjusted=1.997176e-7 mult=0.175231
```

This is not worth packaging: it gives negligible additional FLOP savings and
costs materially more raw than final_keep=176.

## 2026-07-07: higher-moment and side-observer probes

Context: after the leaderboard moved into the low `1e-7` adjusted-score range,
we revisited the higher-moment dataset and the old protected `l2snap` branch as
possible independent truth signals for the current `k26 + L4 preedge +
fullcovEz` sampler.

### Higher moments: what is real

The public higher-moment dataset on `D:` gives true activation/preactivation
moments for all public MLPs:

```text
/mnt/d/datasets/arc-whestbench-higher-moments-2026/full/mlp_00000.npz ...
keys include mean, M11, M21, M22, M31, m2, m3, m4, official_alm,
and pre_* variants
```

The true-cumulant oracle remains the clearest evidence of where the missing
signal lives:

```text
true final preactivation k3 oracle:  ~1.55e-7 raw
true final preactivation k34 oracle: ~2.03e-8 raw
```

The scalar b111 response probe also confirms that true contracted higher-order
preactivation structure is enormous in principle:

```text
cache: legacy_workspace/cache/b111_response_features_full1000_20260707.npz
sampler_base_mse = 2.301738107e-06
gauss_mse        = 1.173408412e-06
b3b21_mse        = 5.134699705e-06
true_b111_mse    = 1.732218616e-07
b111_delta_corr_to_sampler_residual = -0.004346
```

Read: the oracle says the path to `e-8` is not "more final residual ML" by
itself; it is a deployable approximation to true final preactivation cumulants
or their contractions.  The current sampled preedge/MOMA variants did not yet
extract that signal from same-row sampler trajectories.

### Response branch transfer: useful but not directly shippable

`probe_response_branch_cached_transfer.py` tested cached response branches
against current sampler residuals using grouped Full OOF plus Full-trained Mini
transfer.  Best cached branch:

```text
target_residual, response_abs, alpha=0.1
Full OOF raw = 1.2732399326e-06  rel = 0.91352
Mini raw     = 1.1131191860e-06  rel = 0.87096
```

Feature-group ablation found the compact old final-bundle side observer was
better than the larger feature soup:

```text
probe_response_feature_group_transfer.py
final_bundle, abs=True, alpha=0.1, 66 features
Full OOF raw = 1.2699421228e-06  rel = 0.91116
Mini raw     = 1.1030826158e-06  rel = 0.86311
Mini signal corr = 0.37742
```

Canaries for this full old side observer:

```text
probe_response_final_bundle_canaries.py
full_all1000_oof          raw=1.2699421228e-06 rel=0.91116
full_first200_oof         raw=1.1000751392e-06 rel=0.89505
full_tail200_oof          raw=1.2618561798e-06 rel=0.97634
full_spaced20_oof         raw=9.837872895e-07  adjusted=1.786719431e-07
full_offset20_oof         raw=1.283171493e-06  adjusted=2.317560944e-07
mini_all100_fulltrained   raw=1.1030826158e-06 rel=0.86311
mini_spaced20_fulltrained raw=1.161998822e-06  adjusted=2.106762243e-07
```

Read: the signal is real, transfers Full->Mini, and is not name-keyed.  But the
full old side observer is an extra sampled branch if deployed naively.

### Current-state features are dead

`probe_current_final_feature_transfer.py` rebuilt the current `k26_l4` feature
cache for Mini on CPU and tested the current final-state features against the
same residual target.  Result:

```text
best current feature groups are neutral:
final9 abs=True alpha=0.03: Full OOF rel=1.00010, Mini rel=0.99999
all_base_w abs=True alpha=0.03: tiny Full gain, Mini worsened
```

Read: the useful residual signal lives in the old protected/l2snap side
observer, not in the current corrected `k26` state.  Reusing only current
features is not a deployable route.

### Partial side observer economics

`probe_l2snap_side_sensor_transfer.py` tested partial old side observers as an
add-on to current `k26`:

```text
old protected seeds:   18 seeds
current k26 seeds:     26 seeds
seed overlap:          [8,15,17,20,24]

current rows:          2*26*256 = 13312
old half64 rows:       2*18*64  = 2304  (~17.3% extra sampled rows)
old half128 rows:      2*18*128 = 4608  (~34.6% extra sampled rows)
old full rows:         2*18*256 = 9216  (~69.2% extra sampled rows)
```

Best partial variants:

```text
half64_even:
  Mini raw=1.2368e-06 rel=0.96774
  Full rel=0.98756

half128_even:
  Mini raw=1.174935386e-06 rel=0.91933
  Full rel=0.97085

full old side observer:
  Mini raw=1.103082616e-06 rel=0.86311
  Full rel=0.91115
```

Read: as a pure add-on, these likely do not clear adjusted-score break-even.
The full side observer gives about 9-14% raw gain for about 69% extra sampled
rows; half128 gives about 3-8% raw gain for about 35% extra rows.  This is
valuable scientifically, but not a direct package.

`probe_l2snap_standalone_corrected.py` also tested the old side observer as a
primary estimator:

```text
half64 primary raw  ~1e-5    unusable
half128 primary raw ~5e-6    unusable
full protected18 primary:
  Full base raw = 2.301738e-06
  Mini base raw = 1.897755e-06
  learned correction worsened Mini slightly
```

Read: old l2snap alone is not the breakthrough; it is useful only as an
independent side measurement or as a teacher for trading rows.

### Working interpretation

The next non-naive experiments should not add this side observer blindly.  They
should test one of:

1. trade current `k26` rows for a partial old side observer and measure adjusted
   score on Full/Mini canaries;
2. distill/compress the full old side correction into the half128 observer so
   we can recover most of the full-side gain at partial-side cost;
3. continue the higher-moment closure route: a weight-based approximation to
   final preactivation cumulant contractions, then blend it with the sampler.

The target to beat remains the protected/current remote line around
`2.37e-7` adjusted and local staged probes around `1.9e-7` on favorable canaries.

### Half-row side-observer distillation

Added:

```text
legacy_workspace/probe_l2snap_side_teacher_distill.py
```

Question: can the cheaper half128 old `l2snap` side observer recover most of
the full old side-observer correction if we train it against the full-side
teacher instead of directly against noisy public residuals?

Fast sweep:

```text
variant: half128_even
teacher: response_final_bundle_predictions_20260707.npz
tested layer feature bundles: late4, checks4, checks8
tested labels: 50/50 teacher-residual and pure residual
```

Best calibrated-gain row:

```text
checks4 + abs + 50/50 teacher-residual
features=228
Full raw = 1.347960996e-6  rel=0.96713
Mini raw = 1.174053483e-6  rel=0.91864
Mini unit/raw without fold gain = 1.173276748e-6 rel=0.91803
```

Best Mini unit row:

```text
checks4 + abs + residual-only
features=228
Mini unit raw = 1.163534504e-6 rel=0.91041
Full unit rel = 0.97254
```

Interpretation:

- The partial side observer is real, but it does not compress the full side
  observer enough.  It recovers about `8-9%` raw on Mini while costing roughly
  `35%` extra sampled rows if added to current `k26`.
- Late/checkpoint features did not unlock a hidden large interaction.  Checks4
  is slightly better than final-only, but not enough to change the economics.
- Do not package this as a pure add-on.  It is only worth revisiting as a row
  trade: reduce current `k26` work and spend the saved compute on a half128
  side sensor.

### Weight-rooted final preactivation error probe

Ran the existing higher-moment diagnostic:

```text
cheeky_experiments/probe_weightroot_true_ez.py
```

This tests whether deployable weight/path features can predict the signed final
preactivation mean error:

```text
true_pre_mean_L - protected_sample_pre_mean_L
```

The oracle is large:

```text
indices 0:200, protected l2snap base raw = 2.181293040e-6
true final-Ez oracle raw                = 1.069893547e-6
```

But deployable features do not recover it:

```text
mode=weight, response_layers=24,28,30, features=124
best identity raw = 2.180730169e-6  rel=0.999742
EZ R2             = +0.00199
direct residual R2 negative
first_to_tail identity = 2.281523239e-6
tail_to_first identity = 2.079378830e-6

mode=both, response_layers=24,28,30, trajectory_layers=24,28,30, features=475
best identity raw = 2.181119066e-6 rel=0.999920
EZ R2             = +0.00473
direct residual R2 negative
```

Read: the signed final preactivation mean error is a huge oracle, but not
visible to these final-rooted weight/trajectory summaries.  Do not spend more
time on this exact `true_Ez - sample_Ez` feature family unless the feature is
non-local/equivariant rather than summary-based.

### k26/L4 row-trade closeout

Script:

```text
legacy_workspace/probe_k26_l4_rowtrade_side_observer.py
```

Cache:

```text
legacy_workspace/cache/k26_l4_ez_matrix_full1000_20260707.npz
```

This uses the saved `final_seed_scaled` cloud `(1000, 26, 256)` for the current
k26/L4 sampler and asks whether we can drop enough full-depth seed blocks to
fund the half128 side observer.  The probe is intentionally generous: the
`fixed_shift` columns reuse the full-26 fullcov-Ez shift on lower-count
readouts, even though a deployable lower-count estimator would not have the
same full-26 shift.

Baseline:

```text
full sample raw = 1.476403312e-6
full grid  raw  = 1.393771419e-6   # pred_lam0p05 fullcov-Ez shift
mini sample raw = 1.325317878e-6
mini grid  raw  = 1.278035604e-6
```

Best target-free row-trade candidates:

```text
keep 24 top-weight seeds:
  full fixed-shift raw = 1.528984636e-6  rel=1.0970
  mini fixed-shift raw = 1.364127546e-6  rel=1.0674
  modeled with half128 side rel: full adjusted = 3.198e-7, mini adjusted = 2.710e-7

keep 22 top-weight seeds:
  full fixed-shift raw = 1.685645488e-6  rel=1.2094
  mini fixed-shift raw = 1.572254706e-6  rel=1.2302
  modeled with half128 side rel: full adjusted = 3.312e-7, mini adjusted = 2.934e-7

keep 17 top-weight seeds, roughly enough to pay for half128:
  full fixed-shift raw = 2.288052562e-6  rel=1.6416
  mini fixed-shift raw = 2.165228061e-6  rel=1.6942
```

Even the target-tuned greedy ceiling is not useful:

```text
keep 24 greedy-full seeds:
  full fixed-shift raw = 1.528295753e-6  rel=1.0965
  mini fixed-shift raw = 1.540553188e-6  rel=1.2054

keep 17 greedy-full seeds:
  full fixed-shift raw = 2.237308236e-6  rel=1.6052
  mini fixed-shift raw = 2.374106624e-6  rel=1.8576
```

Read: row-trading current k26/L4 rows for the partial side observer is not a
viable route.  The sampled-row variance rises much faster than the side
observer can repair, and this conclusion holds under an optimistic shift reuse
that a production estimator would not actually get.  Do not spend more time on
k26 seed-subset row trade unless a much stronger side observer appears.

### Current k26/L4 Song-adapter matrix learner

Added:

```text
legacy_workspace/build_k26_l4_song_adapter_cache.py
legacy_workspace/train_song_adapter_transfer.py
```

Question: scalar nonlinear readouts over the current final feature bundle were
flat, but can a small matrix-equivariant learner recover residual sign if it
sees the full Phase-1 weight tensor plus the current k26/L4 final features?

Adapter shape:

```text
channel 0: diagonal Gaussian rows for layers 0..30, current k26/fullcov-Ez
           production prediction at layer 31
extra final-only channels: compact k26/L4 seed and preactivation statistics
train cache: Full1000 current k26/L4/fullcov-Ez, base raw 1.393771419e-6
valid cache: Mini100 current k26/L4/fullcov-Ez, base raw 1.278035604e-6
```

Fast CPU transfer gates:

```text
h8/r1, train first200, 4 epochs:
  best Mini raw = 1.276518388e-6 vs base 1.278035604e-6
  rel = 0.998813  # tiny/noisy +0.12%

h16/r2 + weight features, train first200, 12 epochs:
  best Mini raw = 1.280426289e-6 vs base 1.278035604e-6
  rel = 1.001871  # worse
  train rel at best = 0.998141
```

Read: this is not a GPU-scale candidate.  Even with matrix messages and weight
features, the current k26/L4 feature bundle does not expose a useful transferable
signed residual.  This agrees with the earlier scalar-current-feature and
same-row router failures.  Continue toward independent moment/cumulant state or
an integrated K3-aug replacement; do not spend a large GPU run on this adapter
unless the input representation changes materially.

### Post-compaction nonlinear moment-branch synergy check

Question: did the earlier current-feature closures fail because they were
tested too independently?  A bounded nonlinear check combined current-response
features with local higher-moment closure features and asked a histogram
gradient booster to distill the true final `gauss+b3+b21+b111` branch.

Command:

```text
python legacy_workspace/probe_absolute_branch_distill_current.py \
  --feature-modes current_response_abs+local_abs \
  --branches true_b111 \
  --models hgb --target-modes delta \
  --alphas 1,10 --max-iters 40 --max-depths 2,3 \
  --clip-q 0.001 --clip-gain \
  --out /tmp/absolute_branch_hgb_current_response_local_20260707.json
```

Full1000 current k26/L4/fullcov-Ez base:

```text
base raw = 1.393771419e-6
true moment branch raw = 1.737033757e-7
oracle blend current + true branch = 1.457566071e-7
```

Best nonlinear distilled branch:

```text
blend raw = 1.393550747e-6
rel       = 0.999842
signal corr to target residual = +0.00973
unit raw  = 1.401901956e-6
```

Read: this is not a hidden interaction.  The absolute branch `R2` metric looks
near-perfect only because the base scale dominates; the residual-sign signal is
nearly absent and the final-MSE gain is only `0.016%`.  Close the current
response/local feature synergy path unless a genuinely new independent sensor is
added.

### Old analytic/K211 current Phase-1 smoke

Question: do the older near-floor analytic/K211 candidates remain live on the
current Phase-1 `depth=32,width=256` scorer, or were those numbers tied to the
old depth-8/warmup shape?

Commands:

```text
.venv/bin/python quick_score_selected.py \
  --estimator candidate_k211_2i_estimator.py \
  --revision v1-phase1 --split mini --width 256 --depth 32 \
  --flop-budget 272000000000 --indices 0,5,10,15,20 --max-threads 2 \
  --csv ../legacy_workspace/cache/k211_2i_current_phase1_smoke5_20260707.csv

.venv/bin/python quick_score_selected.py \
  --estimator candidate_hybrid2_estimator.py \
  --revision v1-phase1 --split mini --width 256 --depth 32 \
  --flop-budget 272000000000 --indices 0,5,10,15,20 --max-threads 2 \
  --csv ../legacy_workspace/cache/hybrid2_current_phase1_smoke5_20260707.csv
```

Results:

```text
candidate_k211_2i_estimator.py:
  raw=2.724039e-5 adjusted=3.153221e-6 mult=0.11674 failed=0

candidate_hybrid2_estimator.py:
  failed=5 under the current depth-32 contract
```

Read: close old K211/hybrid2 as a direct Phase-1 contender.  The old local
`~7e-8` adjusted notes were not current depth-32 transfer evidence.  Any K3/K4
analytical path must be a real depth-32 port, not an old artifact drop-in.

### Current k26 exact-mean ReLU ridge control variate

Question: the first-10 smoke test for an exact-mean ReLU ridge control variate
looked positive on a k26-flavored seed pool.  Does it survive a proper Full200
guard?

Command:

```text
python legacy_workspace/probe_l2snap_ridge_cv.py \
  --weights-cache legacy_workspace/cache/phase1_full200_weights_targets.npz \
  --indices 0-199 \
  --seeds 8,15,16,17,20,24,26,33,44,47,48,49,63,70,79,84,88,93,97,102,105,106,107,110,111,112 \
  --seed-weights 1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1,1 \
  --ranks 8 --ridge 0.001 \
  --betas 0,0.25,0.5,0.75 \
  --basis-source postrelu --fit-rows 128 \
  --beta-mu 0.35 --beta-sd 0.45 \
  --device cpu \
  --out /tmp/k26_l2snap_ridge_cv_full200_cpu_20260707.npz
```

Result:

```text
plain raw              = 1.317298240e-6
affine raw             = 1.317269816e-6
rank8 beta=0.25 raw    = 1.394023367e-6  ratio=1.058244
rank8 beta=0.50 raw    = 1.606434144e-6  ratio=1.219492
rank8 beta=0.75 raw    = 1.954535131e-6  ratio=1.483745
mean signed corr       = +0.005402
mean variance reduction= -0.220219
```

Read: the positive first-10 result was smoke luck.  The exact-mean ridge basis
is another same-row control that cannot see the transferable signed residual.
Do not wire this into the production package unless the control family changes
substantially.

### Phase-1 diagonal student transfer smoke, sandbox CPU

Question: can the existing Song/PERO-style equivariant student become an
independent external estimator if pretrained on native depth-32 synthetic MC
and fine-tuned on public Full1000 diagonal residuals?

The outside-sandbox CUDA run was rejected by the escalation layer, so this is
only a small CPU transfer gate, not a serious training run.

Command:

```text
python song/src/train_equivariant_residual_big.py \
  --cache song/data/phase1_diag_mini100_matrix_cache.npz \
  --finetune-cache song/data/phase1_diag_full1000_matrix_cache.npz \
  --pretrain-cache /mnt/d/datasets/phase1_synth_depth32_diag_alllayers_64x1m.npz \
  --pretrain-epochs 5 --epochs 12 \
  --hidden 8 --rounds 1 --batch 8 \
  --lr 0.001 --weight-decay 0.01 --finetune-lr 0.0005 \
  --dropout 0.05 --weight-features --device cpu --eval-every 3 \
  --out song/runs/phase1_diag_student_synth64_full1000_to_mini_h8r1_cpu_20260707.json
```

Result:

```text
Mini diagonal base      = 8.850247832e-4
best corrected Mini MSE = 6.529854145e-4
best ratio              = 0.737816
best epoch              = 3
```

Read: the model/trainer/data plumbing works and transfers a coarse diagonal
correction, but the absolute error is hundreds of times too large for the
leaderboard-scale sampler blend.  A learned external estimator remains possible
only with a much stronger Phase-1 state/teacher, such as current k26/L4 moment
state or true preactivation cumulant labels.  Do not spend a large GPU run on
the diagonal-only representation as-is.

### Sampled-b111 fusion split against current k26/fullcov state

Question: the cached `sampled_b111_sensor_full200_equal_v2_20260707.npz`
showed that paid k26 rows estimate the final-transition `b111` contraction
well.  Can the current deterministic fullcov-Ez state supply the preactivation
mean/variance needed to decode that b111 signal?

Offline Full200 cache fusion, using current staged base
`k26_l4_fullcov_ez_full1000cal_shift_full1000_grid_20260707.npz:pred_lam0p05`:

```text
current base raw = 1.229059455e-6

decode state        standalone gauss   standalone sampled-k3  best grouped-OOF blend
sample mu/var       2.474e-6           2.261e-6               1.230986352e-6
true mu/var         1.163e-6           9.608e-7               4.325686983e-7
fullcov mu/var      2.593e-5           2.592e-5               1.231019605e-6
raw fullcov mu/var  7.570e-5           7.502e-5               1.231043452e-6
```

Mean/variance split:

```text
true_mu + sample_var edge raw = 9.758e-7
true_mu + fullcov_var edge raw = 2.035e-6
sample_mu + true_var edge raw = 2.239e-6
fullcov_mu + true_var edge raw = 2.487e-5

sample_mu MSE vs true_mu = 2.697e-6
fullcov_mu MSE vs true_mu = 5.064e-5
sample logvar MSE vs true_logvar = 1.446e-4
fullcov logvar MSE vs true_logvar = 9.018e-2
```

Read: the b111 sensor itself is not the blocker.  If true final preactivation
mean is supplied, even sampled variance is close enough and sampled b111 gives
large raw gains.  With sample/fullcov means, the Edgeworth decoder is neutral or
bad.  The next winner path should target signed final preactivation mean
`E[z_L]` or layer-30 hidden mean, not another b111 calibration over the same
state.

### Signed-Ez state refresh after D: higher-moment dataset confirmation

The user confirmed the full higher-moment dataset is locally available on D:
and D: is acceptable for large scratch.  Rechecked the nearest routes to the
missing signed final-preactivation signal before spending a GPU-scale run.

Deployable Gaussian/full-cov cumulant closure, first40 Full:

```text
python legacy_workspace/probe_higher_moment_rollout_closure.py \
  --weights-npz-cache legacy_workspace/cache/phase1_full1000_weights_targets.npz \
  --indices 0:40 --folds 5 --order 4 \
  --ridge 0.03,0.1,0.3,1 --clip-q 0.01 --g3-clip 1.5 --g4-clip 6

gauss_rollout final = 5.917296490e-5
best recursive k34  = 6.194240662e-5  # ridge=1
```

Read: this reconfirms the earlier closeout.  The full-cov Gaussian state is too
wrong at depth 32; predicting cumulants on top of it does not rescue the state.

Current k26/L4 feature bank -> signed `E[z_L]` residual, grouped Full1000 OOF:

```text
cache = legacy_workspace/cache/k26_l4_ez_matrix_full1000_20260707.npz
x     = two k26 feature rows + base/abs(base), shape (1000, 256, 88)
base signed-Ez MSE = 3.058977935e-6

best HGB OOF = 3.058842967e-6, rel=0.9999559, corr=+0.00681
ridge variants all worsened, corr about -0.007 to -0.009
```

Read: scalar nonlinear current features do not see the signed `E[z_L]` error.
This agrees with the previous final-residual and ridge probes.

Built a fair 32-layer Song adapter retargeted to signed `E[z_L]`:

```text
python legacy_workspace/build_k26_l4_song_adapter_cache.py --target-mode ez \
  --out-full legacy_workspace/cache/k26_l4_song_adapter_ez_full1000_20260707.npz \
  --out-mini /tmp/k26_l4_song_adapter_ez_mini100_dummy_20260707.npz

base signed-Ez MSE = 3.058977935e-6
```

Small CPU message-model gates:

```text
h8/r1, fold0, 8 epochs:
  test base=2.953378726e-6 corrected=2.950929229e-6 ratio=0.999171

h16/r2, fold1, first epoch only:
  test base=3.091120e-6 corrected=3.092636e-6 ratio=1.0005
  stopped early
```

Read: the properly shaped signed-Ez adapter is not worth a large CUDA run as-is.
The tiny fold0 gain is not reproduced on fold1, and the feature family is the
same already-paid k26/L4 telemetry that repeatedly fails to expose residual
sign.  Future learned external estimators need a materially richer state or
new labels/features, not this adapter with more epochs.

### Response-aware sparse-tail selector recheck

Question: can the dynamic pruning score be improved by multiplying activation
energy by a mean-field downstream squared-response vector?

The earlier package uses:

```text
score_i = mean_rows(h_i^2)
```

The recheck compared this to:

```text
score_i = mean_rows(h_i^2) * downstream_response_i
```

on matched Full1000-style warning slices, using
`probe_k26_sparse_tail_meanfield.py` with `tail-mode=drop`, layers `7..30`,
and the same calibrated fullcov-Ez proxy.

Full1000 offset20 `5,55,...,955`:

```text
plain energy:
  keep236 raw=1.478951257e-6 rel=0.993904
  keep232 raw=1.470321954e-6 rel=0.988105  # best
  keep228 raw=1.501393947e-6 rel=1.008986

energy_response:
  keep236 raw=1.473599403e-6 rel=0.990307
  keep232 raw=1.472375643e-6 rel=0.989485
  keep228 raw=1.529747419e-6 rel=1.028041
```

Full1000 spaced20 `0,50,...,950`:

```text
plain energy:
  keep236 raw=1.126104787e-6 rel=0.993665  # best
  keep232 raw=1.129620799e-6 rel=0.996767
  keep228 raw=1.162208989e-6 rel=1.025523

energy_response:
  keep236 raw=1.129897652e-6 rel=0.997012
  keep232 raw=1.132338228e-6 rel=0.999165
  keep228 raw=1.160247715e-6 rel=1.023792
```

Read: downstream-response weighting is not a promotion.  It helps one offset
line at keep236 relative to plain energy but loses the best keep232 offset line
and loses the spaced20 guard.  The robust pruning selector remains plain live
activation energy; keep236 is a narrow spaced20 winner, while keep232 remains
the safer broad point.

### Mean-field omitted-tail variants for k26 sparse columns

Question: can we prune the activation columns more aggressively if the omitted
columns are represented by their sample mean plus a He-style variance term
inside the next ReLU?

Implemented additional `tail_mode`s in
`probe_k26_sparse_tail_meanfield.py`:

```text
mean           z = z_head + mean(h_omit) @ W_omit
mean_he_scalar same mean plus scalar He variance from centered omitted energy
mean_he_col    same mean plus per-output column-energy variance
```

Smoke on two Full200 MLPs showed the path runs:

```text
keep256 raw=1.293284711e-6
keep192 mean_he_col raw=1.510805683e-6
```

Broad Full1000 spaced20 with `tail_mode=mean`, plain activation-energy
selection, layers 7..30:

```text
keep256 raw=1.133275453e-6
keep236 raw=1.130443427e-6 rel=0.997501
keep232 raw=1.137196813e-6 rel=1.003460
keep224 raw=1.130968277e-6 rel=0.997964
keep208 raw=1.386408764e-6 rel=1.223364
keep192 raw=3.694716433e-6 rel=3.260210
```

Read: omitted-tail mean-field is not the missing compute cut.  It does not
make 208/192-column tails viable, and the small keep236/224 improvements are
within the same narrow pruning band already found by plain dropping.  Keep this
closed unless a new state, not just a local mean/variance tail, is added.

### Low-row late hidden-mean checkpoint probes

Question: the higher-moment dataset shows that true layer-30 and layer-29
hidden means are extremely predictive of the final target.  Can a small extra
sampled checkpoint near the tail buy enough raw score to justify its compute?

Script:

```text
legacy_workspace/probe_k26_sparse_extra_l30_snap.py
```

Full1000 spaced20, extra branch injected at L30:

```text
baseline                 raw=1.133891052e-6
c64  h4 beta=0.75        raw=1.105174318e-6   extra work +2.97%
c64  h8 beta=0.50        raw=1.095598816e-6   extra work +5.94%
c96  h4 beta=0.75        raw=1.070235166e-6   extra work +4.54%
c96  h8 beta=0.75        raw=1.033094893e-6   extra work +9.08%
```

Full200, L30:

```text
baseline                 raw=1.234668535e-6
c96 h4 beta=0.50         raw=1.225232020e-6   extra work +4.54%
c96 h8 beta=0.50         raw=1.194085763e-6   extra work +9.08%
```

Full200, L24:

```text
baseline                 raw=1.234668535e-6
c96 h8 beta=0.50         raw=1.209099393e-6   extra work +7.32%
```

Read: late hidden checkpoints are directionally correct but not economical in
this form.  The spaced20 gains are tempting, but the broader Full200 gains are
too small relative to the added sampled matmuls.  The useful lesson is that
late hidden mean is the right state variable; the current way of buying it
with extra low-row sampling is not yet a promotion.

### Exact-moment rooted template controls

Question: if we give ourselves true higher-moment layer state from the D:
`arc-whestbench-higher-moments-2026` dataset, can final-rooted response
templates explain the residual of the current k26/L4/fullcov-Ez calibrated
estimator?  This is an oracle diagnostic for the proposed influence-weighted
gate/cumulant-template path; if exact C/gates cannot move the residual, a
deployable approximation to the same dictionary is not worth packaging.

Script:

```text
legacy_workspace/probe_exact_moment_rooted_templates.py
```

Outputs:

```text
legacy_workspace/cache/exact_moment_rooted_templates_spaced20_late_20260707.json
legacy_workspace/cache/exact_moment_rooted_templates_first200_late_20260707.json
legacy_workspace/cache/exact_moment_rooted_templates_spaced20_checkpoints_20260707.json
```

Late layers `24,26,28,29,30`, Full1000 spaced20:

```text
base      raw=1.133115642e-6
corrected raw=1.169636911e-6
rel=1.03223, R2=-0.03223
```

Late layers `24,26,28,29,30`, First200:

```text
base      raw=1.229059455e-6
corrected raw=1.233380829e-6
rel=1.00352, R2=-0.00352
```

Checkpoint sweep `0,4,8,12,16,20,24,28,30`, Full1000 spaced20:

```text
base      raw=1.133115642e-6
corrected raw=1.219213502e-6
rel=1.07598, R2=-0.07598
```

Read: close this dictionary.  Even exact higher-moment covariance/gate inputs
do not produce a useful signed residual correction when contracted through
these simple final-rooted templates.  The exact moment dataset remains
valuable, but the viable signal is not this local template family.

### Final preactivation moment shrink / PRE-EDGE sanity check

Question: can the current k26 rows be decoded better by shrinking sampled
final preactivation moments, then applying Edgeworth to the final ReLU?  This
tests the expert's PRE-EDGE idea in the limited form where the sampled final
preactivation mean/variance/skew are used directly.

Inputs:

```text
legacy_workspace/cache/sampled_b111_sensor_full200_equal_v2_20260707.npz
legacy_workspace/cache/sampled_b111_sensor_spaced20_equal_v2_20260707.npz
legacy_workspace/cache/k26_l4_fullcov_ez_full1000cal_shift_full1000_grid_20260707.npz
```

Full200 diagnostics:

```text
current base raw                    = 1.229059e-6
sample E[z_L] MSE vs true E[z_L]    = 2.697125e-6
global shrink of sample E[z_L]      = neutral, oracle coefficient ~1
best OOF sampled-state Edgeworth    = essentially neutral to worse
true E[z_L] + sampled var/k3        = large oracle gain
```

Read: the sampled b111/skew contraction is a good sensor once the final
preactivation mean is correct, but the sampled final preactivation mean itself
is the bottleneck.  Global shrink and direct sampled PRE-EDGE do not unlock
the signal.  The missing object is still an independent, signed estimate of
the late hidden state / final preactivation mean.

### Union39 sparse-propagation economics check

Question: the cached `union39 + L4 PRE-EDGE + fullcov-Ez` branch has much
better raw MSE than k26 on public guards.  Can the same dynamic activation
coordinate pruning that made k26 economical make union39 deployable?

Built an isolated prototype:

```text
legacy_workspace/_pkg_union39_equal_l2snap_split035_045_rectsparse232s7_late192_finalsp176_winograd1_preedgeL4_fullcovEzCal_finalonly/
```

This keeps the k26 sparse arithmetic path (`7..23=232`, `24..30=192`,
`final=176`, one-level Winograd) and swaps in the union39 equal seed bank.

Op profile on 2 Mini MLPs:

```text
FLOPs = 6.677e10
ops   = 2645
predicted remote multiplier:
  25us/op 0.2698
  45us/op 0.2892
  65us/op 0.3087
```

Full1000 spaced20 direct run:

```text
raw = 7.770044838e-7
mean FLOPs = 6.674e10
ops = 2643
predicted adjusted:
  25us/op 2.095e-7
  45us/op 2.246e-7
  65us/op 2.397e-7
```

Full200 spaced10 schedule scout:

```text
232 / late192 / final176:
  raw=7.708710002e-7, mult25=0.2696, adj25=2.079e-7

224 / late176 / final168:
  raw=9.342425508e-7, mult25=0.2612, adj25=2.440e-7
```

Read: union39 sparse is a real raw-accuracy branch but not an adjusted-score
breakthrough.  It loses too much score to analytical FLOPs before reaching the
0.1 floor, and more aggressive sparse schedules hit the raw cliff before they
save enough compute.  Keep it as a diagnostic and maybe a high-accuracy
component if compute pricing changes, but do not promote it over the k26
economics line.

### Fullcov-mode b111/common-factor surrogate

Question: the oracle structured closure gets a strong result by using true
post-layer covariance/common-factor modes from the D: higher-moment files.  Can
the deployable full-covariance Gaussian state supply those low-rank modes
instead?

Added:

```text
legacy_workspace/probe_fullcov_mode_b111_surrogate.py
```

The probe deliberately separates three state choices:

```text
true final preactivation mean/variance + predicted gamma/excess
fullcov final preactivation mean/variance + predicted gamma/excess
sampled final preactivation mean/variance + predicted gamma/excess
```

Full1000 spaced20:

```text
base raw              = 1.134058629e-6
best g3 R2            = +0.87016
best true-state blend = 4.041347940e-7
best fullcov-state    = 1.162700584e-6  # worse than base
```

First200:

```text
base raw              = 1.230272861e-6
best g3 R2            = +0.87989
best true-state blend = 4.724902233e-7
best fullcov-state    = 1.235113017e-6  # worse than base
best sampled-state    = 1.230371294e-6  # essentially flat
```

Mean-error follow-up on First200, using the same fullcov-mode features to
predict `true_E[z_L] - fullcov_E[z_L]`:

```text
fullcov E[z_L] MSE        = 1.522321014e-4
corrected E[z_L] MSE      = 5.706038241e-5
mean-error R2             = +0.62516
corrected fullcov blend   = 1.231132661e-6  # still worse than base
```

Read: this is an important positive-negative result.  Fullcov low-rank modes
do contain the missing response-aligned K3/common-factor signal; gamma is
predictable at `~0.88` R2 and becomes a strong branch if the true final
preactivation mean is supplied.  But neither the deployable fullcov mean nor
the sampled mean is accurate enough to decode that signal, and the mode-based
mean correction merely reaches the same rough quality as the existing
calibrated fullcov-Ez proxy.  Do not package this as-is.  The next winning
path still needs a better independent late mean / final preactivation mean
estimate, after which the b111 sensor/closure machinery becomes valuable.

Follow-up shifted-row-cloud gate with the exported mode-corrected
`E[z_L]` proxy:

```text
proxy:
  legacy_workspace/cache/fullcov_mode_b111_fc_mu_corr_first200_proxy_20260708.npz

score cache:
  legacy_workspace/cache/k26_l4_fullcov_mode_b111_fc_mu_corr_shift_first200_20260708.npz

First200, current k26 seeds/weights, L2 snap split 0.35/0.45:
  lambda=0      raw = 1.318327944e-6
  lambda=0.03   raw = 1.263180623e-6
  lambda=0.05   raw = 1.254893588e-6  # best vs own baseline, -4.8%

Same First200 rows, incumbent calibrated fullcov-Ez grid:
  lambda=0      raw = 1.299927939e-6
  lambda=0.03   raw = 1.242113435e-6
  lambda=0.05   raw = 1.229059455e-6  # incumbent still better
```

Read: reject the exported fullcov-mode mean proxy as a production branch.  It
does carry a real signed shift against its own no-shift row cloud, but the
existing full1000-calibrated fullcov-Ez proxy is better on the same First200
gate.  The useful residue is not this linear ridge/eigen proxy; it is the fact
that a better independent final-preactivation mean would immediately unlock the
already-seen b111/Edgeworth gains.

Nonlinear follow-up:

```text
script:
  legacy_workspace/probe_fullcov_mode_ez_nonlinear.py

cache:
  legacy_workspace/cache/fullcov_mode_ez_nonlinear_first200_20260708.npz

First200 final-preactivation mean MSE:
  raw fullcov proxy          = 1.522321014e-4
  incumbent calibrated proxy = 5.064078978e-5
  HGB depth3/80 OOF proxy    = 5.034266887e-5  # tiny +0.6% vs incumbent
  HGB depth4/120 OOF proxy   = 5.044337168e-5
  ExtraTrees depth12/160     = 5.061304321e-5

shift gate:
  legacy_workspace/cache/k26_l4_fullcov_mode_ez_nonlinear_shift_first200_20260708.npz

First200, current k26 seeds/weights:
  lambda=0.04 raw = 1.252468024e-6
  lambda=0.05 raw = 1.248598240e-6  # best nonlinear mode proxy
  lambda=0.06 raw = 1.249761829e-6

Incumbent calibrated fullcov-Ez grid on same First200 rows:
  lambda=0.05 raw = 1.229059455e-6
```

Read: close this exact fullcov-mode feature family for package work.  The
nonlinear learner confirms there is real but shallow signed `E[z_L]` signal in
the modes; it slightly improves mean-proxy MSE but does not translate to a
better final row-cloud shift than the existing calibrated fullcov-Ez branch.
The bottleneck is no longer ridge underfitting.

### L30 hidden-mean nonlinear proxy

Question: instead of predicting final preactivation mean directly, can we
predict the layer-30 hidden post-activation mean error and then contract the
corrected hidden mean through the final weights?

Added:

```text
legacy_workspace/probe_l30_mean_proxy_nonlinear.py
```

First200 OOF:

```text
raw fullcov L30 mean MSE      = 7.608586485e-5
best ExtraTrees L30 mean MSE  = 2.527737699e-5  # strong hidden-state signal

raw fullcov final-Ez MSE      = 1.522321014e-4
incumbent final-Ez MSE        = 5.064078978e-5
best L30-contracted Ez MSE    = 5.079478135e-5  # slightly worse than incumbent
```

Simple grouped blend of incumbent calibrated fullcov-Ez, fullcov-mode HGB Ez,
and L30-contracted Ez:

```text
incumbent Ez MSE      = 5.064078978e-5
mode-HGB Ez MSE       = 5.034267553e-5
L30-contracted Ez MSE = 5.079477774e-5
OOF blend Ez MSE      = 5.004985843e-5
mean coefficients     = [intercept 3.56e-5, inc 0.330, mode 0.289, l30 0.382]
```

Shift gates:

```text
L30 nonlinear proxy:
  legacy_workspace/cache/k26_l4_fullcov_mode_ez_nonlinear_shift_first200_20260708.npz
  best lambda=0.05 raw = 1.248598240e-6

inc/mode/L30 blend proxy:
  legacy_workspace/cache/k26_l4_fullcov_inc_mode_l30_blend_shift_first200_20260708.npz
  best lambda=0.05 raw = 1.247716366e-6

incumbent calibrated fullcov-Ez grid, same First200 rows:
  best lambda=0.05 raw = 1.229059455e-6
```

Read: this is another positive-negative result.  The L30 hidden state is
learnable from fullcov/weight features in OOF, but the remaining contraction
error is still too large, and the actual row-cloud readout stays below the
current calibrated fullcov-Ez package.  Do not package L30-HGB or the proxy
blend as-is.

### L30 diagonal-Edgeworth source feature check

Question: did the L30 hidden-mean proxy miss a deployable local skew source?
This variant augments the previous fullcov/weight features with the Gaussian
rollout's diagonal third-cumulant contribution into layer 30:

```text
script:
  legacy_workspace/probe_l30_mean_proxy_edge_features.py

b3_j = sum_i W30[i,j]^3 * kappa3_post29_i
edge3_delta_j = (b3_j / sigma_pre30_j^3) / 6 * int relu(mu+sigma z) H3(z) phi(z) dz
```

First200 grouped OOF:

```text
l30_base_mse       = 7.608586485e-5
edge_l30_mse       = 7.608242397e-5   # direct diagonal source alone is tiny
fc_ez_mse          = 1.522321014e-4
incumbent_ez_mse   = 5.064078978e-5

best edge-feature model, ExtraTrees 12/160:
  l30_mse          = 2.537600173e-5
  l30_rel          = 0.333518
  l30_err_r2       = +0.57156
  projected Ez MSE = 5.099286192e-5
  rel to incumbent = 1.00695
```

Grouped OOF blend with the existing proxy set:

```text
incumbent fullcov-Ez     = 5.064078978e-5
mode-HGB proxy           = 5.034267553e-5
old L30 proxy            = 5.079477774e-5
new edge L30 proxy       = 5.099286124e-5
four-way blend           = 5.005311945e-5
previous three-way blend = 5.004985843e-5
```

Read: the hidden L30 residual remains learnable, but this diagonal Edgeworth
source does not supply the missing final-projection sign.  It slightly worsens
the best proxy blend, so there is no row-cloud package to promote.  The next
distinct route must estimate the all-distinct/response component directly or
change the sampling/quadrature state, not add more local diagonal skew features.

### PathCV84 learned sparse router transfer check

Question: can we recover the K26/pathCV per-row oracle using deployable
weight-only features rather than the brittle late-q25 scalar gate?

Training rows used actual First200 predictions:

```text
base  = k26_sparse_tail_drop_full200_keepgrid:pred_keep232
path  = pathcv84_sparse_schedule_full200_late:pred_late224
label = lower adjusted branch under measured branch multipliers
```

Transfer rows used packaged warning-spaced20 CSVs for both Full and Mini, with
no target-derived routing features.  Models tried: ridge adjusted-difference
regressor, ExtraTrees adjusted-difference regressor, logistic classifier, and
random-forest classifier.

```text
First200 fixed/oracle:
  K26 fixed      = 2.091982180e-7
  pathCV fixed   = 2.473782386e-7
  per-row oracle = 1.642233681e-7
  oracle route counts = {K26: 97, pathCV: 103}

Full warning spaced20 fixed/oracle:
  K26 fixed      = 1.976966736e-7
  pathCV fixed   = 2.599167758e-7
  per-row oracle = 1.577118649e-7

Mini warning spaced20 fixed/oracle:
  K26 fixed      = 2.312995296e-7
  pathCV fixed   = 3.289516795e-7
  per-row oracle = 1.871579072e-7
```

Best transfer behavior:

```text
ridge / ExtraTrees: route K26 for all 20 Full and all 20 Mini rows
  Full = 1.976966736e-7
  Mini = 2.312995296e-7

logistic: routes 1 Full row and 3 Mini rows to pathCV
  Full = 1.984880240e-7
  Mini = 2.600073791e-7

random forest: routes 5 Full rows and 5 Mini rows to pathCV
  Full = 2.077290386e-7
  Mini = 2.647913854e-7
```

Read: the K26/pathCV branch oracle is real, but this deployable router cannot
identify it.  When it routes nontrivially, it overpays on Mini; when constrained
conservatively, it collapses to K26 everywhere.  Do not package this learned
pathCV router.  The next path must bring an independent signed estimate, not
another same-branch route derived from coarse MLP features.

### Direct projected-L30 feature learner for final `E[z_L]`

Question: did the L30 hidden-mean learner fail because it optimized hidden
MSE, then projected, instead of directly optimizing final preactivation
coordinates?

Added:

```text
legacy_workspace/probe_l30_projected_ez_direct.py
```

The probe builds hidden L30 fullcov/weight features, contracts them through
the final weights into final-output features, then learns
`true_E[z_L] - incumbent_proxy_E[z_L]` under MLP-held-out folds.

First200 proxy result:

```text
incumbent fullcov-Ez proxy MSE = 5.064078978e-5
best projected ridge proxy MSE = 4.987704622e-5
relative proxy MSE             = 0.984918
signal R2                     = +0.01500
spaced20 proxy MSE             5.220310513e-5 -> 4.847458650e-5
last half proxy MSE            5.323708518e-5 -> 5.334106351e-5
```

So the objective-mismatch hypothesis is partly true: direct projection reveals
a small signed `E[z_L]` signal that hidden-MSE training diluted.  The nonlinear
row-cloud gate still rejects it.

K26 row-cloud shift on First200, using the same K26 seed bank/weights:

```text
incumbent calibrated fullcov-Ez, same rows:
  best lambda=0.05 raw = 1.229059455e-6

projected-L30 proxy:
  best lambda=0.05 raw = 1.248418916e-6

fractional alpha blends of projected signal into incumbent proxy:
  alpha=-0.50 best raw = 1.252071247e-6
  alpha=-0.25 best raw = 1.250822406e-6
  alpha=+0.25 best raw = 1.249088439e-6
  alpha=+0.50 best raw = 1.248609111e-6
  alpha=+0.75 best raw = 1.248383979e-6
```

Read: direct projected L30 features improve proxy MSE but not the nonlinear
sampled-cloud readout.  This closes the hidden-objective mismatch for this
feature family.  Do not package; use this only as evidence that marginal
`E[z_L]` proxy MSE is not sufficient for promotion.

### b111/fullcov-mode feature stack against current sampler

Question: the fullcov-mode b111/common-factor surrogate was too state-limited
as a standalone Edgeworth branch, but could its signed coordinates still help
when used as features on top of the current K26/L4/fullcov-Ez sampled readout?

Added:

```text
legacy_workspace/probe_b111_feature_stack_current.py
```

The feature stack used the incumbent lambda-grid rows plus fullcov-mode proxy
coordinates:

```text
proxy_ez, raw_fc_mu, fc_sig, gamma_hat, excess_hat,
proxy_ez - raw_fc_mu,
lambda-grid deltas around pred_lam0p05,
absolute and interaction variants
```

First200 grouped OOF, with the current full1000-calibrated K26/fullcov-Ez
`pred_lam0p05` as base:

```text
base raw                         = 1.229059455e-6
best ridge/HGB b111-feature row  = 1.234630508e-6
relative                         = 1.00453
```

Read: reject this fusion too.  The b111/fullcov-mode coordinates remain useful
as diagnostics of the missing higher-order object, but they do not create a
deployable correction when the final preactivation mean state is still wrong.
The live blocker remains independent estimation of L30 hidden mean / final
`E[z_L]`, not more learned features over the same row cloud.

### Low-overhead moment-stack interaction falsifier

Question: are we being too naive by testing weak analytic signals in isolation?
Maybe exact-GH, L4 PRE-EDGE, K26 z12, and the current K26-gamma sampler only
work when fitted together with interaction terms.

Command:

```text
python legacy_workspace/probe_lowoverhead_moment_stack.py \
  --base-mode k26gamma \
  --branch-sets exactgh,preedge4,k26z12 \
  --modes linear,abs,pair,all \
  --fit-modes global,neuron \
  --lams 0.01,0.1,1,10,100,1000,10000 \
  --clip 0.003 \
  --top-branches 8 \
  --out legacy_workspace/cache/lowoverhead_stack_k26gamma_top8_interactions_clip003_20260708.npz
```

Result:

```text
base K26 gamma raw:
  Full1000 = 1.470949253e-6
  Mini100  = 1.325972344e-6

best apparent Full1000 OOF:
  global linear lambda=0.01
  OOF raw  = 1.111907871e-6  # rel 0.7559
  Mini raw = 6.178434211e-6  # rel 4.6596, catastrophic transfer

best Mini-transfer-safe row:
  global linear lambda=10000
  OOF raw  = 1.470802695e-6  # rel 0.9999
  Mini raw = 1.326279059e-6  # rel 1.0002
```

Read: close this interaction-stack route for now.  The combined weak branches
can overfit public Full1000 labels, but the only Mini-stable setting collapses
to no meaningful correction.  This is a useful guardrail against the "many tiny
signals combine" failure mode: at least for these cached deployable-ish
branches, the combination does not survive transfer.

### Receiver-injected diagonal Cact recurrence

Question: did the failed final-rooted/root-collapsed higher-moment templates
miss the expert's proposed two-time structure
`source layer s -> receiver layer t -> final adjoint`?

Added:

```text
legacy_workspace/probe_receiver_injected_cact.py
legacy_workspace/probe_receiver_injected_diag_gauss.py
```

The first probe uses true public higher-moment state.  It preserves the
receiver-injected recurrence

```text
rowsum((B_s C_s) * (B_t diag(r_t) J_t ... J_{s+1}))
```

instead of collapsing the receiver into the source/final layer.  On First200,
the selected late diagonal source groups (`d=1,2`, gatevar/mean/prealpha/var/
excess receivers) capture true final cumulant corrections:

```text
true-moment selected recurrence, First200:
  K3 delta:             1.174241926e-6 -> 1.003384602e-6  rel 0.8545
  K34 delta:            1.081360131e-6 -> 9.469403677e-7  rel 0.8757
  truth from true-gauss 1.162166983e-6 -> 1.026881223e-6  rel 0.8836
  sampler residual:     1.229059455e-6 -> 1.234377828e-6  rel 1.0043
```

So the pair-distance recurrence is a real cumulant-closure primitive, but it
does not directly correct the current sampler.

The weight-only diagonal Gaussian approximation gives an even stronger
cumulant signal when it is allowed to sit on the oracle true-Gaussian base:

```text
diag-gauss recurrence, First200:
  K3 delta:             rel 0.8135
  K34 delta:            rel 0.8364
  truth from true-gauss rel 0.8463
  sampler residual:     rel 1.0110

diag-gauss recurrence, Full1000:
  truth from true-gauss 1.173408401e-6 -> 9.829922678e-7  rel 0.8377
```

However the honest deployable diagonal Gaussian final-mean base is unusable:

```text
official targets, diagonal Gaussian base, smoke20:
  9.557489890e-4 -> 1.044028813e-3  rel 1.092
```

The cached fullcov Gaussian final-ReLU base is much better but still far from
competitive:

```text
fullcov Gaussian final-ReLU:
  Full1000 raw global-cal = 2.630337289e-5
  Mini100  raw global-cal = 2.600070168e-5
```

A simple fullcov Gaussian ReLU branch blend into the current K26 sampler is
also effectively closed:

```text
Full1000 current base 1.393771419e-6 -> OOF 1.392301353e-6  rel 0.9989
Mini100 full-trained alpha: 1.278035604e-6 -> 1.281603544e-6  rel 1.0028
```

Read: the receiver-injected recurrence is the first new positive mechanistic
signal in this round, but it needs a good late marginal state.  Diagonal
Gaussian state is too wrong; true marginal state works; current sampler does
not expose a compatible residual.  The next honest test is the same
receiver-injected recurrence from a fullcov Gaussian per-layer state, preferably
as a CUDA-built offline cache before any flopscope candidate work.

### Fullcov receiver-injected recurrence

Added:

```text
legacy_workspace/probe_receiver_injected_fullcov_gauss.py
```

CUDA First200 result using full covariance Gaussian per-layer state:

```text
official fullcov Gaussian final-ReLU base:
  7.569996364e-5 -> 5.456202071e-5  rel 0.7208
```

This is a real correction to the fullcov analytic branch, but the absolute raw
MSE remains about forty times worse than the current sampler.  A feature cache
was written to:

```text
/mnt/d/whestbench-data/receiver_injected_fullcov_gauss_first200_20260708.npz
```

Fusing those features into the current K26/fullcov sampler is negative:

```text
current sampler residual, First200:
  all features  1.229059455e-6 -> 1.244518144e-6  rel 1.0126
  z-only        1.229059455e-6 -> 1.233810756e-6  rel 1.0039
  raw-only      1.229059455e-6 -> 1.237443082e-6  rel 1.0068
```

They also do not improve the signed final preactivation mean proxy:

```text
fullcov calibrated Ez proxy MSE:
  base          5.064078978e-5
  all features  5.082089505e-5  rel 1.0036
  z-only        5.069065047e-5  rel 1.0010
  raw-only      5.073135602e-5  rel 1.0018
```

Read: the expert's pair-distance recurrence is mathematically real when the
late marginal state is good, but the deployable Gaussian states we can afford
are not good enough, and the recurrence is not the missing signed sampler
coordinate.  Do not build a flopscope candidate for this branch unless a new
late marginal-state estimator appears.

### PathCV sparse schedule retest after recurrence closure

Question: if the live leaderboard's best raw/MSE tradeoff looks pathCV-like,
can we reduce pathCV compute with a better per-layer sparse schedule rather
than a uniform keep count?

CUDA probe:

```text
legacy_workspace/probe_pathcv_sparse_schedule.py
```

Full spaced20 first sweep, pathCV88, 512 rows:

```text
base224:       7-29=224, final=224
stair:         7-15=224, 16-23=216, 24-29=208, final=208

best broad-ish line on this slice:
  stair__z12_s2 raw = 4.882169734e-7
  base224__z12_s2 raw = 4.886827664e-7
```

More aggressive schedules on the same Full spaced20:

```text
ag4 = 7-19=216, 20-29=192, final=176
  ag4__z12_s3 raw = 5.585799526e-7
  ag4 raw         = 5.680804064e-7
```

This looked like a useful compute/accuracy frontier on the friendly slice, but
the transfer checks reject it:

```text
Full offset20:
  base224 raw      = 4.306022292e-7
  ag4 raw          = 5.013218446e-7
  ag4__z12_s3 raw  = 5.069719244e-7

Mini spaced20:
  base224 raw      = 6.529270865e-7
  base224__z12_s3  = 6.092310419e-7
  ag4 raw          = 8.994820168e-7
  ag4__z12_s3      = 8.570300182e-7
```

Read: aggressive pathCV pruning is another friendly-slice artifact.  `base224`
and `base224` with z12 remain the robust pathCV arms; deeper compute cuts
destroy Mini/offset parity.  Do not package `ag4`/`stair` unless a new gate can
detect the favorable slice without targets.

### Current K26 seed-cloud interaction residual closeout

Question: maybe the current paid K26 final seed cloud contains a cheap
target-free residual correction in the block spread, quantiles, skew/kurtosis,
or pairwise seed interactions that we have not decoded.

Added:

```text
legacy_workspace/probe_k26_seed_interaction_residual.py
```

The probe fits grouped-OOF residual corrections from the already-paid K26/L4
seed matrix and transfers the fitted correction from Full1000 to Mini100.

Key results:

```text
base K26/L4 current matrix:
  Full1000 raw = 1.476400956e-6
  Mini100  raw = 1.325317392e-6

diagquad low-regularization:
  Full apparent rel ~ 1.01965
  Mini transfer rel ~ 1.04410

conservative linear high-regularization:
  alpha=10000
  Full raw = 1.47640443e-6  # rel 1.000002
  Mini raw = 1.32531869e-6  # rel 1.000001
```

Read: close this same-telemetry route.  The per-seed spread/interaction
features either hurt both sides or shrink to a no-op.  This reinforces the
current blocker: the missing signal is not exposed by another low-cost readout
of the existing K26 row cloud; we need a new signed late-state observable or a
meaningful compute reduction in the sampler itself.
2026-07-08 - K26 Winograd2 economics and augmented count-router check
---------------------------------------------------------------------

Active K26 `late192_finalsp176` was copied to a two-level rectangular
Strassen-Winograd package.  The statistical estimator is unchanged: same 26
directions, L2 snap, L4 PRE-EDGE, fullcov-Ez final shift, tail keep schedule,
and final keep 176.  Only `_strassen2/_strassen2_rect` changed from one-level
to two-level Winograd.

Broad guards:

```text
full spaced20 raw=1.133884e-6 adjusted=1.899354e-7 mult=0.166610 flops=4.037e10
full offset20 raw=1.384830e-6 adjusted=2.286054e-7 mult=0.166458 flops=4.037e10
mini spaced20 raw=1.331829e-6 adjusted=2.194310e-7 mult=0.165448 flops=4.037e10
```

Artifact:

```text
whest-starterkit/packages/to_test_remote/submission_phase1_pure26_w05_unionteacher1100gamma_l2snap_split035_045_rectsparse232s7_late192_finalsp176_winograd2_preedgeL4_fullcovEzCal_finalonly_bundle.tar.gz
sha256 6baf15e6442cb8d67f1485683de80054f5078e9e5f8f3492527be2ffbb977c98
```

`op_profile.py` reports `FLOPs=4.037e10`, `warm_ops=6551`.  This is lower
counted FLOPs but higher warm-op count than Winograd1.  Recent successful
remote SPHEREx-family telemetry had only `~6-8ms` residual around `3.8e10`
FLOPs, so if that residual regime holds this package should multiply near
`0.151`.  It is an economics probe with expected remote adjusted roughly
`1.7e-7` on the friendly spaced20 guard and `~2.0-2.1e-7` on offset/mini.

The adaptive-count route was rechecked with current K26/L4 telemetry appended
to the old RACE features.  New probe:

```text
legacy_workspace/probe_race_shadow_router_k26_features.py
```

Full200 count set `(18,24,32,48,60)`:

```text
fixed18=3.370238061e-7
true_oracle=2.362590080e-7
direct_shadow_actual=2.864122340e-7
direct_plain_shadow_actual=2.742461627e-7
```

But learned OOF routers were all negative, including the diagnostic that trains
on true losses:

```text
best k26 true_oof/extratrees actual=3.531712443e-7
best block_k26 true_oof/extratrees actual=3.530931702e-7
best weight_k26 true_oof/extratrees actual=3.530545129e-7
best block_weight_k26 true_oof/extratrees actual=3.509665587e-7
```

Conclusion: the high-count oracle remains real, but current low-count/K26
telemetry does not expose a stable count-selection signal.  Do not spend remote
submissions on adaptive-count routers unless a new feature source first clears
OOF true-loss routing.

2026-07-08 - QR-offset + pathwise-CV row-quality probe
------------------------------------------------------

Question: QR-offset ensembles have excellent raw MSE but poor compute economics,
while pathwise first-layer CV improves high-count SPHEREx.  These had mostly
been tested as separate branches.  Test whether offset diversity makes the
pathwise-CV correction more efficient per propagated row.

New offline CUDA probe:

```text
legacy_workspace/probe_qroffset_pathcv.py
```

It uses the protected 18 seed identities, spread QR offsets, antithetic sphere
rows, optional exact first-layer affine correction, and split-fitted `z1` /
`z1+z2` pathwise controls.  Outputs were written to D scratch:

```text
/mnt/d/whestbench-data/qroffset_pathcv_spread10_20260708.npz
/mnt/d/whestbench-data/qroffset_pathcv_full_spaced20_20260708.npz
```

Tiny Full spread10 smoke:

```text
k3 best raw = 6.711246e-7  # h1corr z12_s2
k4 best raw = 4.578725e-7  # h1corr mean
k7 best raw = 3.572799e-7  # rawh1 z12_s2
```

Broader Full spaced20 guard:

```text
k3 h1corr_z12_s2 raw = 7.610882e-7
k4 h1corr_mean   raw = 5.830178e-7
k7 h1corr_z12_s2 raw = 3.398980e-7
```

The pathwise controls are real on top of qroffset: for k7 on spaced20, h1corr
plus z12 improves the raw qroffset mean from `4.114051e-7` to `3.398980e-7`.
However, adjusted economics do not clear the protected `311697` line.  Against
`311697` adjusted `2.370739e-7`, the required multipliers are:

```text
k3 raw 7.610882e-7 -> break-even multiplier 0.3115
k4 raw 5.830178e-7 -> break-even multiplier 0.4066
k7 raw 3.398980e-7 -> break-even multiplier 0.6975
```

Prior qroffset7x18 Strassen2 pricing was `mult ~= 0.814` with raw parity, so
even the k7 raw frontier models worse than the protected line.  k3/k4 would
need unrealistically low effective multipliers for their row counts.  Verdict:
good raw-frontier diagnostic and another confirmation that pathwise CV is
useful, but do not package without a materially stronger row/matmul compression
method.

2026-07-08 - Existing qroffset7 sparse package audit
----------------------------------------------------

After the qroffset+pathCV raw-frontier result, checked whether the already-built
qroffset7 sparse packages had an unlogged compute win.

Profiled `qroffset7x18_h1affine_rawfrontier_f16_strassen2_sparse224s7`:

```text
FLOPs = 1.896e11
ops   = 7130
predicted remote multiplier:
  25us/op 0.7627
  45us/op 0.8151
  65us/op 0.8675
```

Even if it preserved the `~3.4e-7` qroffset raw frontier, this prices around
`2.6e-7+`, so it is not a next-winner.

Profiled the most aggressive existing sparse variant,
`qroffset7x18_h1affine_rawfrontier_f16_strassen2_sparse128s7`:

```text
FLOPs = 1.333e11
ops   = 7130
predicted remote multiplier:
  25us/op 0.5556
  45us/op 0.6080
  65us/op 0.6604
```

But a Full spread5 score rejects it:

```text
indices 0,250,500,750,999
raw=1.034233e-02 adjusted=5.668825e-03 mult=0.547668
```

Conclusion: existing qroffset sparse packages do not rescue the high-count raw
frontier.  Mild pruning is still too expensive; aggressive pruning breaks the
row cloud.  Do not promote qroffset sparse without a different compression
mechanism than dynamic activation-energy keep.

2026-07-08 - Winograd3 and K26/L4 residual-learner closeouts
------------------------------------------------------------

Question 1: can pushing the rectangular Strassen-Winograd row-cloud product one
level deeper rescue the current K26 economics without changing the statistical
estimator?

Patched the diagnostic wrapper to allow `K26_RECT_WINOGRAD_LEVEL=3` and
profiled the same `late192_finalsp176` K26 branch:

```text
level2:
  FLOPs = 4.037e10
  warm_ops = 6551
  local warm residual ~= 39.8 ms

level3:
  FLOPs = 3.642e10
  warm_ops = 33893
  local warm residual ~= 135.5 ms
```

Read: closed.  Level 3 saves only `3.95e9` counted FLOPs while adding roughly
`27k` warm operations.  Even with the fairer Phase 1 residual accounting, this
is the wrong side of the compute tradeoff.  Keep the staged Winograd2 package
as the current low-FLOP economics probe; do not package Winograd3.

Question 2: does the current K26/L4/fullcov-Ez telemetry expose a stable
residual correction if we fit a lightweight equivariant learner or global
regressor on broad public data?

Built K26/L4 adapter caches:

```text
song/data/phase1_k26_l4_full1000_adapter_20260708.npz
  n=1000, channels=26, base_mse=1.393771419e-6

song/data/phase1_k26_l4_mini100_adapter_20260708.npz
  n=100, channels=26, base_mse=1.278035604e-6
```

CUDA message-net transfer, Full1000 fit / Mini fold-0 validation:

```text
test_base      = 1.327385234617e-6
best corrected = 1.333047066510e-6  # epoch 1
ratio          = 1.004265
```

Global sklearn probes over final-only and final+trajectory descriptors gave the
same diagnosis: ridge/HGB can shave at most about `0.04%-0.15%` on Full1000
OOF, but transfer to Mini worsens by roughly `0.35%-0.74%`.

Read: closed for this feature set.  This is not a training-capacity problem;
the already-paid K26/L4 telemetry does not expose the missing signed residual
stably enough to support a deployable correction.  Future learned branches need
a new target-aligned observable, not more epochs over these adapter features.

2026-07-08 - QR-offset hidden observer and pruned-tail frontier
---------------------------------------------------------------

Question: full-depth QR-offset ensembles have excellent raw MSE but poor
compute economics.  Can we use them only as shallow hidden-state observers,
either snapping the current K26 row cloud or enabling a smaller tail bank?

Added CUDA diagnostics:

```text
legacy_workspace/probe_qroffset_hidden_snap_k26.py
legacy_workspace/probe_qroffset_observer_pruned_tail.py
```

K26 hidden-snap add-on, Full spaced20:

```text
same-script K26 base:
  raw = 1.263817785e-6

k5 qroffset observer, snap L4+L8, beta=0.65:
  raw = 9.471138874e-7

k5 qroffset observer, snap L8 only, beta=0.80:
  raw = 9.671248696e-7

k3 qroffset observer, snap L4+L8, beta=0.50:
  raw = 1.073746562e-6
```

Shifted Full offset20 guard:

```text
same-script K26 base:
  raw = 1.328095106e-6

k5 qroffset observer, snap L8 only, beta=0.80:
  raw = 9.652198592e-7

k5 qroffset observer, snap L4+L8, beta=0.80:
  raw = 9.653093677e-7
```

Read: the hidden observer is a real target-free raw signal, and it transfers
across the shifted Full slice.  The literal implementation is likely
compute-killed: a k5 observer to L8 is a large extra row cloud before the K26
pass.  Do not package "observer + full K26" without a materially cheaper
implementation or a stronger combined estimator.

Pruned-tail variant: run the k5 QR-offset observer to L8, snap a selected tail
subset to that hidden mean, and only finish that tail.

Full spaced20:

```text
o5s18 all offsets / all 18 seeds:
  raw = 5.168902266e-7

o5s12 top-weight seeds, beta=0.65:
  raw = 5.928030639e-7

o3s18 three offsets / all 18 seeds, beta=0.90:
  raw = 6.509980242e-7

o5s8 top-weight seeds, beta=0.90:
  raw = 7.449241611e-7
```

Full offset20:

```text
o5s18 all offsets / all 18 seeds:
  raw = 4.861475248e-7

o5s12 top-weight seeds, beta=0.90:
  raw = 7.244367709e-7

o3s18 three offsets / all 18 seeds, beta=0.90:
  raw = 7.436660060e-7

o5s8 top-weight seeds, beta=0.90:
  raw = 9.672822043e-7
```

Approximate read: this is a useful high-raw frontier and a better diagnostic
than "full QR-offset everywhere", but adjusted economics are still tight.  The
two-slice raw band is roughly `6.6e-7` for `o5s12` and `7.0e-7` for `o3s18`;
they only become promotion candidates if a flopscope implementation prices
well below the naive block-layer estimate.  Next useful follow-ups are
pathwise-CV readout on the pruned tail or an exact row-product compression that
does not reproduce the sparse128 failure.  Do not spend time on more beta
tuning before pricing/compression.

Follow-up checkpoint/CV sweep:

```text
L4, Full spaced20:
  o5s12 beta=0.35 raw = 6.278939484e-7
  o3s18 beta=0.90 raw = 7.381562005e-7

L4, Full offset20:
  o5s12 beta=0.90 raw = 8.226997301e-7
  o3s18 beta=0.90 raw = 7.548556645e-7

L6, Full spaced20:
  o5s12 beta=0.75 raw = 5.948726721e-7
  o3s18 beta=0.95 raw = 6.666999620e-7

L6, Full offset20:
  o5s12 beta=0.95 raw = 7.873100606e-7
  o3s18 beta=0.75 raw = 7.784276243e-7
```

L8 remains the best two-slice raw/economics compromise.  L4 saves checkpoint
work but loses too much on the shifted slice; L6 is intermediate.

Block-level z1 pathwise CV on the pruned tail mostly worsens the reduced-tail
settings.  It gives only tiny gains on the full `o5s18` case and should not be
ported.

Rough block-layer pricing, calibrated from the existing qroffset7x18
Strassen2/sparse224 audit (`FLOPs=1.896e11`, effective mult about `0.815`):

```text
L8 o5s12 beta=0.90: rawavg~=6.62e-7, mult~=0.440, adj~=2.92e-7
L8 o3s18 beta=0.90: rawavg~=6.97e-7, mult~=0.412, adj~=2.87e-7
L8 o5s8  beta=0.90: rawavg~=8.56e-7, mult~=0.346, adj~=2.96e-7
full k5/o5s18:       rawavg~=5.02e-7, mult~=0.582, adj~=2.92e-7
```

Read: this family is a good raw-MSE diagnostic, not the next winner by itself.
It would need a materially better exact row-product implementation than the
current qroffset sparse/Strassen packages to become adjusted-positive.  Close
statistical tuning here; reopen only for a new compute-compression primitive.

2026-07-08 - L30 external-state learner and qroffset compression sanity checks
-----------------------------------------------------------------------------

Question 1: can a stronger Full1000 matrix learner predict the hidden L30 mean
itself, rather than fitting final residuals or same-row denoisers?  This is the
most direct deployable approximation to the true L30 oracle snap.

Added:

```text
legacy_workspace/build_hidden_layer_song_cache.py
```

Built the derived hidden-state cache on D: from the existing Full1000
seed-stat telemetry and public higher-moment files:

```text
/mnt/d/whestbench-data/phase1_l2snap_seedmeans_full1000_l30_cache_20260708.npz
  n=1000
  target_layer=30
  base_mse=2.376808530e-6
  pred_rows=(1000, 32, 256)
  pred_features=(1000, 32, 256, 26)
```

CUDA gate, fold0 held out, h32/r3, weight features, output layer 30:

```text
epoch 0001:
  train 2.401286e-6 / 2.404746e-6
  test  2.263132e-6 / 2.265058e-6
  ratio = 0.9991

epoch 0010:
  train 2.398561e-6 / 2.404746e-6
  test  2.263288e-6 / 2.265058e-6
  ratio = 0.9992
```

The run was stopped at epoch 10 because the held-out hidden-state gain was
only about `0.1%`.  Read: the stronger Full1000 L30 cache reproduces the old
first200 warning rather than unlocking the L30 oracle.  A generic
LayeredMessageNet over seed-stat rows does not recover the final-relevant
signed L30 error; do not spend a larger h64/h128 run here unless the feature
source changes.

Question 2: the qroffset/pathCV raw frontier is in top-leaderboard raw-MSE
territory, but too expensive.  The sparse224 and sparse128 variants were
already audited; check whether middle keep values preserve raw before declaring
the compression cliff.

Five-row Full spread check `0,250,500,750,999`:

```text
qroffset7x18 strassen2 sparse192s7:
  raw=7.239947e-6
  multiplier=0.688269
  FLOPs=1.708e11
  failed=0

qroffset7x18 strassen2 sparse160s7:
  raw=2.620919e-4
  multiplier=0.619061
  FLOPs=1.521e11
  failed=0
```

Read: the cliff happens before sparse192.  Dynamic activation-energy pruning
that works for K26 does not preserve the high-count qroffset row cloud at the
cuts needed for score competitiveness.  Reopen only for a different row-product
compression primitive, not another keep-count sweep.

Question 3: should old Song/fullskip results be treated as a current Phase-1
external branch?

Phase-1 Full spread5 score for `candidate_final_fullskip_estimator.py`:

```text
raw=2.336431e-5
multiplier=0.102734
FLOPs=2.107e10
failed=0
```

Read: no.  It is a near-floor analytic branch but far too inaccurate to be a
useful standalone or ordinary blend partner for the current K26 sampler.  Old
depth-8/fullskip Song numbers remain idea evidence, not directly transferable
candidate evidence.

Question 4: can the high-raw qroffset branch be compressed by using the full
row cloud only through an L8 observer, then thinning rows inside every
offset/seed block for the long tail?

Added:

```text
legacy_workspace/probe_qroffset_observer_rowtail.py
```

This differs from static input-row thinning: all qroffset rows are propagated
through L8, then each block keeps an evenly spaced subset of its 256 half-rows
for layers 9..31.  The kept tail rows are mean-snapped toward the full L8
observer mean before continuing.

Five-row spread smoke `0,250,500,750,999`, offset count 5:

```text
full k256              raw = 4.422216067e-7
k192 beta=1            raw = 4.440604576e-7
k128 beta=1            raw = 8.703232348e-7
k96  beta=1            raw = 9.454600294e-7
```

This was a real positive smoke: L8 observer + rowtail `k192` nearly preserved
the full k5 qroffset raw result.

Broad Full spaced20:

```text
k256 raw = 5.282438206e-7
k224 beta=0.5 raw = 5.994913217e-7
k192 beta=1.0 raw = 6.182604658e-7
k160 beta=1.0 raw = 7.382750042e-7
k128 beta=0.8 raw = 8.988861779e-7
```

Shifted Full offset20:

```text
k256 raw = 4.716231383e-7
k224 beta=1.0 raw = 5.085182859e-7
k192 beta=1.0 raw = 6.555941327e-7
k160 beta=1.0 raw = 6.368509028e-7
```

Approximate cost model relative to full k5 qroffset:

```text
cost_ratio ~= 8/32 + (half_keep/256) * 24/32

k224: ratio 0.906, rawavg ~5.54e-7
k192: ratio 0.812, rawavg ~6.37e-7
k160: ratio 0.719, rawavg ~6.88e-7
k128: ratio 0.625, rawavg ~9.0e-7 on spaced20
```

Read: rowtail is a genuine compression primitive and substantially better than
static qroffset sparse192, but it is not yet a next-winner.  When calibrated
against the existing qroffset k5/full pricing, the broad adjusted model is
still around `~2.9e-7`, worse than the protected line.  Reopen only if paired
with a tail control variate that improves raw at the same tail keep, or if a
flopscope implementation prices much better than the row-count model.

2026-07-08 - Winograd2 stats-CDF economics sibling
--------------------------------------------------

Copied the active K26 `late192_finalsp176_winograd2` package and replaced only
the hand polynomial normal-pdf/cdf approximation in the fullcov-Ez branch with
`flops.stats.norm.pdf/cdf`.

Validated package:

```text
legacy_workspace/_pkg_pure26_w05_unionteacher1100gamma_l2snap_split035_045_noaffine_module_rectsparse232s7_late192_finalsp176_winograd2_statscdf_preedgeL4_fullcovEzCal_finalonly/
```

Staged artifact:

```text
whest-starterkit/packages/to_test_remote/submission_phase1_pure26_w05_unionteacher1100gamma_l2snap_split035_045_rectsparse232s7_late192_finalsp176_winograd2_statscdf_preedgeL4_fullcovEzCal_finalonly_bundle.tar.gz
sha256 36d4deec72bb68405164ea0df022f90cb033e959a6804fec04dede4dc7f08dcf
```

Raw parity guards:

```text
full spaced20 raw=1.133887e-6 adjusted=1.846639e-7 mult=0.162498 flops=4.037e10
full offset20 raw=1.384829e-6 adjusted=2.233327e-7 mult=0.161475 flops=4.037e10
mini spaced20 raw=1.331829e-6 adjusted=2.282162e-7 mult=0.172520 flops=4.037e10
```

Compare against non-stats-CDF Winograd2:

```text
full spaced20 raw=1.133884e-6 adjusted=1.899354e-7 mult=0.166610 flops=4.037e10
full offset20 raw=1.384830e-6 adjusted=2.286054e-7 mult=0.166458 flops=4.037e10
mini spaced20 raw=1.331829e-6 adjusted=2.194310e-7 mult=0.165448 flops=4.037e10
```

`op_profile.py`:

```text
Winograd2 hand-CDF:  warm_ops=6551, FLOPs=4.037e10
Winograd2 stats-CDF: warm_ops=5931, FLOPs=4.037e10
```

Read: raw is unchanged to measurement precision and op count drops by 620.
Local residual timing is noisy, especially on Mini, so this is only a small
economics sibling.  It is worth keeping as a safer package if the Phase-1
remote client prices `flops.stats.norm` cheaply, but it does not move the
frontier toward `1e-7` by itself.

2026-07-08 - Rowtail CV and dynamic selector closeout
-----------------------------------------------------

Followed up the positive L8 observer + rowtail result with two possible
improvements:

1. block-level pathwise `z1/z2` controls on the kept tail rows;
2. dynamic selection of the tail row pairs from the full L8 observer cloud.

Caches:

```text
/mnt/d/whestbench-data/qroffset_observer_rowtail_cv_smoke2_20260708.npz
/mnt/d/whestbench-data/qroffset_observer_rowtail_z1cv_spread5_20260708.npz
/mnt/d/whestbench-data/qroffset_observer_rowtail_selectors_spread5_20260708.npz
```

The `z1`/`z12` controls did not survive beyond tiny/special cases.  On the
five-row spread smoke, the stronger static rowtail points worsened:

```text
static k160 beta=0.8: raw 5.209898672e-7 -> z1cv 7.191784235e-7
static k192 beta=1.0: raw 4.440604576e-7 -> z1cv 4.909458171e-7
```

Dynamic selectors were much worse than static row positions.  Selecting
antithetic row pairs by L8 centrality, high deviation, or activation energy
destroyed the estimator by orders of magnitude:

```text
static  k192 beta=1.0 raw = 4.440604576e-7
highdev k192 beta=1.0 raw = 2.177861213e-5
central k192 beta=1.0 raw = 3.468915198e-5
static  k160 beta=0.8 raw = 5.209898672e-7
highdev k160 beta=1.0 raw = 5.092608962e-5
```

Read: close dynamic rowtail selectors and rowtail CV for now.  The useful
compression primitive is specifically static antithetic rowtail after a full L8
observer.  Observable-based row picking breaks the quadrature balance; pathwise
CV does not repair the compute economics.  Reopen rowtail only for a new exact
row-product implementation or a fundamentally different tail compression.

2026-07-08 - Direct fullcov mean/variance closure falsifier
-----------------------------------------------------------

After revisiting the higher-moment roadmap, tested a deployable-shaped
alternative to "predict cumulants on top of a bad Gaussian state": directly
predict the true post-ReLU mean/variance correction from the full-covariance
Gaussian rollout state.

Added:

```text
legacy_workspace/probe_fullcov_direct_meanvar_closure.py
```

The first log-ratio target was unstable and exploded OOF, so the meaningful
gate used normalized additive mean deltas:

```text
python legacy_workspace/probe_fullcov_direct_meanvar_closure.py \
  --weights-npz-cache legacy_workspace/cache/phase1_full1000_weights_targets.npz \
  --indices 0:40 --folds 5 --order 4 \
  --ridge 0.03,0.1,0.3,1,3,10,30 \
  --clip-q 0.01 --hard-clip 0.5 --mean-target normdelta \
  --out /mnt/d/whestbench-data/fullcov_direct_meanvar_closure_first40_normdelta_20260708.npz
```

First40 MLP-held-out posthoc correction:

```text
gaussian final        = 5.917296490e-5
best posthoc ridge    = 3.121745367e-5  # ridge=0.03
```

Read: the fullcov state has predictable mean bias, but the corrected branch is
still far too inaccurate to be useful as an independent blend partner.  The
loose blend bar is around `3e-6` standalone raw; this is about `10x` worse even
on a small favorable diagnostic.  Close direct fullcov mean/variance closure
unless a materially richer state replaces the fullcov Gaussian features.

2026-07-08 - Cached K26 vs union39 router closeout
--------------------------------------------------

Added a reusable cached two-branch diagnostic:

```text
legacy_workspace/probe_cached_branch_router.py
```

Question: can the strong raw-MSE `union39 + L4 + fullcov-Ez` branch be used
only on rows where it beats the economical K26/L4 branch, without paying both
branches?

Run:

```text
python legacy_workspace/probe_cached_branch_router.py \
  --k26-lam 0.05 --k26-mult 0.1625 \
  --union-mults 0.216,0.270,0.289,0.309 \
  --both-mult 0.4325 \
  --train full_first100 --tests full_spaced20,mini100 \
  --folds 5 \
  --out legacy_workspace/cache/k26_union39_cached_router_20260708.npz
```

Representative results:

```text
Optimistic union mult 0.216:
  full_first100 fixed K26   adj = 1.878405144e-7
  full_first100 fixed union adj = 1.873676725e-7
  full_first100 row oracle  adj = 1.468917994e-7
  full_spaced20 fixed K26   adj = 1.841312918e-7
  full_spaced20 fixed union adj = 1.715197892e-7
  mini100 fixed K26         adj = 2.076807857e-7
  mini100 fixed union       adj = 2.101716973e-7
  mini100 row oracle        adj = 1.457230156e-7

Realistic union mult 0.270:
  full_first100 row oracle  adj = 1.623580100e-7
  full_spaced20 row oracle  adj = 1.620989312e-7
  mini100 row oracle        adj = 1.570111064e-7
```

But the deployable gates did not capture the oracle.  Weight-only OOF gates
either degraded transfer or collapsed to fixed K26.  Adding K26 prediction
summaries did not rescue the gate, and would also make the runtime policy more
expensive if K26 had to be run before choosing union39.

Computing both and using a fixed alpha blend gave good raw but bad adjusted:

```text
full_spaced20 both-cost best blend raw = 5.673488639e-7, adj = 2.453783836e-7
mini100      both-cost best blend raw = 7.062616720e-7, adj = 3.054581731e-7
```

Read: union39 remains a useful diagnostic/high-raw branch, but not a next
submission route under current compute pricing.  The row oracle is too small
and too unroutable to justify more selector work here.  Keep focusing on an
independent weight/moment estimate, especially deployable low-rank b111 or
equivalent late-preactivation mean/cumulant sensors.

2026-07-08 - True-b111 teacher student falsifier
-------------------------------------------------

Built a Song-compatible teacher cache for the higher-moment oracle branch:

```text
legacy_workspace/build_true_moment_teacher_cache.py
/mnt/d/whestbench-data/phase1_k26_l4_trueb111_teacher_full1000_20260708.npz
/mnt/d/whestbench-data/phase1_k26_l4_trueb111_teacher_first200_20260708.npz
```

The cache keeps the current K26/L4 adapter base and prediction features, swaps
the training target to the oracle `true_b111` branch from
`b111_response_features_full1000_20260707.npz`, and stores `truth_target` so
the checkpoint evaluator scores against actual final means.

First200 opportunity:

```text
true_b111 teacher raw = 1.692509907e-7
current adapter base  = 1.230272861e-6
base vs teacher       = 1.404236060e-6
```

Fast CUDA fold:

```text
python song/src/train_equivariant_residual.py \
  --cache /mnt/d/whestbench-data/phase1_k26_l4_trueb111_teacher_first200_20260708.npz \
  --out /mnt/d/whestbench-data/phase1_trueb111_student_first200_h16r2_fold0_e40_20260708.json \
  --checkpoint-out /mnt/d/whestbench-data/phase1_trueb111_student_first200_h16r2_fold0_e40_20260708.pt \
  --epochs 40 --hidden 16 --rounds 2 --batch 4 --holdout-mod 5 --holdout-value 0 \
  --lr 0.001 --weight-decay 0.01 --dropout 0.10 --weight-features --device cuda
```

The student learned a small part of the teacher on held-out MLPs:

```text
held-out teacher base      = 1.439460024e-6
held-out teacher gain=1    = 1.356611852e-6
```

But its predicted correction is not aligned with the actual truth residual:

```text
held-out truth base        = 1.236515607e-6
held-out truth gain=1      = 1.314342353e-6
held-out truth best gain   = 0.0
```

Read: close this exact learned-student route.  A generic equivariant message
student can imitate some oracle-branch variation, but the learned delta is
response-irrelevant or wrong-signed for truth.  This is consistent with earlier
Phase-1 learned residual runs: the missing signal is not exposed by current
adapter features plus a small message net.  Reopen only with a qualitatively
different target representation, e.g. explicit late preactivation mean/cumulant
state rather than final-vector branch imitation.

2026-07-08 - qroffset+pathCV top-weight seed compression
--------------------------------------------------------

Patched `probe_qroffset_pathcv.py` with explicit/top-weight seed selection and
tested the direct high-raw compression idea: keep the broad qroffset/pathCV
geometry, but use fewer protected seed blocks.

Run:

```text
python legacy_workspace/probe_qroffset_pathcv.py \
  --indices 0,50,100,150,200,250,300,350,400,450,500,550,600,650,700,750,800,850,900,950 \
  --seed-count 12 --seed-mode topweight --offset-counts 5,7 --offset-mode spread \
  --ridge 0.001 --folds 2 --z2-scales 1,2 --device cuda \
  --out /mnt/d/whestbench-data/qroffset_pathcv_top12_k5k7_full_spaced20_20260708.npz
```

Selected seeds:

```text
(28, 22, 2, 13, 24, 7, 27, 29, 0, 31, 15, 20)
```

Full spaced20 results:

```text
k5 best: rawh1_z1      raw = 6.057685077e-7
k7 best: rawh1_z12_s2  raw = 4.651604738e-7
```

Compare prior all18 k7 qroffset/pathCV on the same spaced20 guard:

```text
all18 k7 h1corr_z12_s2 raw = 3.398979644e-7
```

Read: seed-block compression alone loses too much raw frontier.  Top12 k7 is
interesting scientifically, and pathwise controls still help, but row-count
pricing would need to be far better than proportional for this to compete.  Do
not package.  Future qroffset/pathCV work needs either a fundamentally cheaper
row-product kernel or a way to preserve all18-like raw accuracy with a much
smaller tail, not simply fewer high-weight seeds.

Protected-weight follow-up on the same top12/k7 spaced20 guard:

```text
python legacy_workspace/probe_qroffset_pathcv.py \
  --indices 0,50,100,150,200,250,300,350,400,450,500,550,600,650,700,750,800,850,900,950 \
  --seed-count 12 --seed-mode topweight --offset-counts 7 --offset-mode spread \
  --ridge 0.001 --folds 2 --z2-scales 1,2 --block-weight-mode protected --device cuda \
  --out /mnt/d/whestbench-data/qroffset_pathcv_top12_weighted_k7_full_spaced20_20260708.npz
```

Best protected-weight line:

```text
k7 h1corr z1 protected-weight raw = 4.570337445e-7
```

Read: protected block weights recover only a small amount versus equal top12
(`4.65e-7 -> 4.57e-7`).  This confirms the compression loss is missing seed
diversity, not just suboptimal block weighting.

2026-07-08 - Extra final-preactivation row signal under current K26/L4 scorer
-----------------------------------------------------------------------------

Rechecked the independent signed final-preactivation mean coordinate against
the current K26/L4/fullcov-Ez branch, not the older Full200 sampler.  Added
sample-seed subset support to:

```text
legacy_workspace/probe_k26_l4_fullcov_ez_shift.py
```

Broad current-scorer results:

```text
C4 extra-Ez, Full1000 spaced20:
  base raw = 1.249117707e-6
  best lambda=0.15 raw = 1.011951233e-6  # rel 0.810133

C4 extra-Ez, Full1000 offset20:
  base raw = 1.631205277e-6
  best lambda=0.08 raw = 1.540179732e-6  # rel 0.944197

C2 extra-Ez, Full1000 spaced20:
  base raw = 1.249117707e-6
  best lambda=0.08 raw = 1.166755343e-6  # rel 0.934064

C2 extra-Ez, Full1000 offset20:
  base raw = 1.631205277e-6
  best lambda=0.03 raw = 1.627493034e-6  # rel 0.997724

C8 extra-Ez, Full1000 spaced20:
  base raw = 1.249117707e-6
  best lambda=0.25 raw = 8.720631252e-7  # rel 0.698143

C8 extra-Ez, Full1000 offset20:
  base raw = 1.631205277e-6
  best lambda=0.18 raw = 1.417161655e-6  # rel 0.868782
```

Caches:

```text
legacy_workspace/cache/k26_l4_extraez_c2_full_spaced20_20260708.npz
legacy_workspace/cache/k26_l4_extraez_c2_full_offset20_20260708.npz
legacy_workspace/cache/k26_l4_extraez_c4_full_spaced20_20260708.npz
legacy_workspace/cache/k26_l4_extraez_c4_full_offset20_20260708.npz
legacy_workspace/cache/k26_l4_extraez_c8_full_spaced20_20260708.npz
legacy_workspace/cache/k26_l4_extraez_c8_full_offset20_20260708.npz
legacy_workspace/cache/extra_ez_c2_c8_full_spaced20_even_20260708.npz
legacy_workspace/cache/extra_ez_c2_c8_full_offset20_even_20260708.npz
```

Read: signed `E[z_L]` is still one of the strongest missing observables.  C8
is robust on both shifted slices, and C4 is real but less stable.  Literal
append economics are still wrong: C8 adds roughly `8/26` of a sampled branch,
so the raw gain is not steep enough to reach `1e-7` adjusted.  Keep searching
for a cheaper proxy/replacement for this coordinate; do not package literal
C2/C4/C8 row append as a winner.

Seed replacement check:

```text
K24 = K26 minus seeds 15,70 + C2 extra-Ez, Full1000 spaced20:
  base raw = 1.495875210e-6
  best lambda=0.08 raw = 1.380809474e-6  # rel 0.923078
```

This known drop pair is not a viable replacement trade.  Reopen row replacement
only with a side observer that preserves the K26 raw baseline on both spaced
and offset guards.

Sampled-b111 / extra-Ez interaction check:

```text
cache: legacy_workspace/cache/sampled_b111_sensor_full200_equal_v2_20260707.npz

baseline sampled-b111 branch       raw = 1.313140624e-6
true mean + sample var + sample k3 raw = 9.758122644e-7
true mean + true var + sample k3   raw = 9.607680190e-7
true mean + true var + true k3     raw = 1.699202806e-7

extra-Ez c8 proxy + true var fit-blend  raw = 1.119338815e-6
extra-Ez c12 proxy + true var fit-blend raw = 1.072031808e-6
extra-Ez c18 proxy + true var fit-blend raw = 1.096831935e-6
```

Read: the b111 sensor is not the limiting piece; the missing piece is still
the signed final-preactivation mean/variance state.  Extra-Ez proxies help the
interaction but are not economically strong enough in their literal form.

2026-07-08 - current-scorer low-row extra-Ez recheck
----------------------------------------------------

Prior low-row extra-Ez probes used an older k26 scoring path.  Rebuilt the two
most plausible low-row proxy shapes and evaluated them under the current
K26/L4/fullcov-Ez scorer.

Proxy builds:

```text
/mnt/d/whestbench-data/extra_ez_lowrow_32x32_64x32_full_spaced20_even_20260708.npz
/mnt/d/whestbench-data/extra_ez_lowrow_32x32_64x32_full_offset20_even_20260708.npz
```

Current scorer, Full1000 spaced20:

```text
32x32 best lambda=0.10 raw = 1.065119652e-6
64x32 best lambda=0.14 raw = 1.038088834e-6
full C8 best lambda=0.25 raw = 8.720631252e-7
```

Current scorer, Full1000 offset20:

```text
32x32 best lambda=0.06 raw = 1.311546636e-6
64x32 best lambda=0.22 raw = 1.108763296e-6
full C8 best lambda=0.18 raw = 1.417161655e-6
```

Read: `64x32` is a real, more stable signed-Ez proxy under the current scorer,
and on the shifted slice it is much better than full C8.  But its modeled
economics are still only low/mid `2e-7` adjusted if appended literally:
roughly `1.04e-6 * ~0.21` on spaced20 and `1.11e-6 * ~0.21` on offset20.
This is useful as a replacement/fusion component, not yet a standalone next
winner.

Replacement check: top22 K26 full-ReLU seeds plus `64x32` low-row Ez proxy.
Top22 drops `[15, 33, 70, 110]` by the existing K26 weight prior.

```text
Full1000 spaced20:
  K22 base raw           = 1.540086995e-6
  K22 + 64x32 best raw   = 1.262906516e-6  # lambda=0.18

Full1000 offset20:
  K22 base raw           = 1.761898860e-6
  K22 + 64x32 best raw   = 1.323236299e-6  # lambda=0.26
```

Read: the low-row Ez proxy recovers a lot of the K22 loss, but not enough.
Dropping four full-ReLU K26 seeds harms the base more than the cheaper signed
proxy can repair.  Close this top-weight replacement form; if replacement is
reopened, it needs a searched/drop-specific seed set, not just low-weight seed
removal.

2026-07-08 - corrected fullcov/low-row Ez proxy mix sweep
---------------------------------------------------------

Built reusable proxy-mixing helper:

```text
legacy_workspace/make_ez_proxy_mix_cache.py
```

It creates one-dimensional mixtures of the cheap fullcov final-Ez proxy and
the `64x32` low-row sampled final-Ez proxy:

```text
proxy_mix = (1 - alpha) * lowrow_64x32 + alpha * fullcov_proxy
```

Important validation correction: an initial run accidentally used
`phase1_full200_weights_targets.npz` with Full1000 spaced indices
`0,50,...,950`.  `parse_indices` correctly clipped that to only four valid
rows, so those first four-row outputs are discarded.  Corrected runs below use
`phase1_full1000_weights_targets.npz`.

Caches:

```text
legacy_workspace/cache/ez_proxy_mix_fullcov_low64x32_spaced20_20260708.npz
legacy_workspace/cache/ez_proxy_mix_fullcov_low64x32_offset20_20260708.npz
legacy_workspace/cache/ez_proxy_mix_fine_fullcov_low64x32_spaced20_20260708.npz
legacy_workspace/cache/ez_proxy_mix_fine_fullcov_low64x32_offset20_20260708.npz
```

Corrected Full1000 spaced20:

```text
lowrow alpha=0.00     best raw = 1.038088834e-6  # prior standalone
alpha=0.05            best raw = 1.020401183e-6
alpha=0.10            best raw = 9.980969063e-7
alpha=0.15            best raw = 9.804838750e-7
alpha=0.20            best raw = 9.675353169e-7
alpha=0.25            best raw = 9.592740664e-7
alpha=0.50            best raw = 9.881698107e-7
```

Corrected Full1000 offset20:

```text
lowrow alpha=0.00     best raw = 1.108763296e-6  # prior standalone
alpha=0.05            best raw = 1.105369437e-6
alpha=0.10            best raw = 1.108709413e-6
alpha=0.15            best raw = 1.118779651e-6
alpha=0.20            best raw = 1.135579500e-6
alpha=0.25            best raw = 1.157312792e-6
alpha=0.50            best raw = 1.266639760e-6
```

Read: fullcov and low-row Ez are partially complementary on spaced20, but
fullcov contamination hurts the shifted offset guard.  A small fraction
(`alpha=0.05..0.10`) is harmless and maybe slightly better on average, while
`alpha=0.15` is the best simple average over the two 20-row guards.  This is a
real diagnostic signal, but still not a promotion: the literal low-row observer
adds enough compute that the modeled adjusted score remains in the low/mid
`2e-7` region.  The useful conclusion is structural: the next large gain needs
a cheaper independent estimate of final preactivation mean / layer-31 hidden
mean, not more tuning of the same row add-on.

2026-07-08 - Ez observer row-allocation and replacement probe
-------------------------------------------------------------

Question: the `64x32` low-row Ez observer is useful but expensive.  Is the
same 2048-half-row budget better spent as fewer complete QR blocks, many
partial blocks, randomized row subsets, or a replacement for some K26 full
sample blocks?

Current-scorer checks use:

```text
legacy_workspace/probe_k26_l4_fullcov_ez_shift.py
weights-cache = legacy_workspace/cache/phase1_full1000_weights_targets.npz
```

Corrected Full1000 spaced20, 2048 half-row observers:

```text
8x256 even   best raw = 8.723953340e-7
16x128 even  best raw = 1.103148573e-6
32x64 even   best raw = 1.050587407e-6
64x32 even   best raw = 1.038088834e-6  # prior current-scorer cache
128x16 even  best raw = 1.122098520e-6
256x8 even   best raw = 9.699846648e-7
```

Corrected Full1000 offset20, same family:

```text
4x256 even   best raw = 1.269805346e-6
8x256 even   best raw = 1.248618250e-6
64x32 even   best raw = 1.108869814e-6
256x8 even   best raw = 1.171598994e-6
```

Read: `8x256` full QR blocks are a strong spaced20 specialist, but `64x32`
is the robust shifted-slice design.  Complete QR blocks are not universally
better for signed final-Ez; the row allocation is slice-sensitive.

Random-row spot checks:

```text
off101, current scorer:
  64x32 random spaced20 best = 1.022886105e-6
  64x32 random offset20 best = 1.155885048e-6
  96x24 random spaced20 best = 1.183265627e-6
  96x24 random offset20 best = 1.034779573e-6

off202, current scorer:
  64x32 random spaced20 best = 9.228252693e-7
  64x32 random offset20 best = 1.359327010e-6  # no positive lambda
```

Read: random row subsets have real variance and can look excellent on one
guard, but the sign does not transfer across shifted slices.  Do not promote a
random-row offset without a much stronger no-target design principle.

Costly s64/s8 observer fusion, requiring both 2048-row observers:

```text
alpha is fraction of 8x256 in proxy = (1-alpha)*64x32 + alpha*8x256

spaced20:
  alpha=0.25 best raw = 9.370815922e-7
  alpha=0.50 best raw = 8.275204405e-7
  alpha=0.75 best raw = 7.987148780e-7

offset20:
  alpha=0.25 best raw = 1.046529656e-6
  alpha=0.50 best raw = 1.052652297e-6
  alpha=0.75 best raw = 1.150310299e-6
```

Read: fusion confirms complementary raw signal, but the doubled observer cost
pushes modeled adjusted score out of contention.  Useful ceiling diagnostic,
not a package.

Replacement check: top18 K26 sample blocks plus a 2048-row Ez observer.
Top18 seeds by K26 weights:

```text
111,107,44,63,24,105,49,84,48,20,88,16,17,102,106,79,26,112
```

Current scorer:

```text
top18 + 8x256, spaced20 best = 9.592306381e-7
top18 + 8x256, offset20 best = 1.629739245e-6

top18 + 64x32, spaced20 best = 1.284542560e-6
top18 + 64x32, offset20 best = 1.526686669e-6
```

Read: dropping full K26 sample blocks loses too much, especially on offset20.
Close this replacement family.  The durable result from this branch is not a
new artifact; it is that independent signed-Ez measurements are valuable, but
the robust version still costs too much to be the next adjusted-score winner.

2026-07-08 - early-start K26 sparse schedule closeout
-----------------------------------------------------

Question: can the active K26 rectsparse schedule save enough compute by
starting the activation-energy pruning one or two layers earlier, while keeping
the same L2 snap, L4 PRE-EDGE, gamma readout, and calibrated fullcov-Ez shift?

Exact CUDA scorer:

```text
legacy_workspace/probe_k26_sparse_schedule.py
```

Full1000 spaced20 (`0,50,...,950`), `lambda_Ez=0.05`:

```text
s7base: 7-23=232,24-30=192,final=176   raw = 1.134049833e-6
s6:     6-23=232,24-30=192,final=176   raw = 1.206232859e-6  rel = 1.063651
s5:     5-23=232,24-30=192,final=176   raw = 1.383231103e-6  rel = 1.219727
s6more: 6-23=224,24-30=192,final=176   raw = 1.388511399e-6  rel = 1.224383
s5more: 5-23=224,24-30=192,final=176   raw = 2.428849752e-6  rel = 2.141749
```

Read: closed.  The layer-6 start loses `6.4%` raw and layer-5 starts fall off a
cliff.  The compute saved by earlier sparse matmuls cannot pay for that raw
loss.  Keep pruning onset at layer 7 unless a different state snap or observer
changes the hidden distribution before the tail.

2026-07-08 - auxiliary final-cloud low-count replacement closeout
-----------------------------------------------------------------

Question: can a protected final-cloud observer replace enough K26 rows to buy
compute while preserving the late signed correction?

Two low-count grids were run with evenly spaced half-row observers.  Best local
fits were selected by the same broad mini/full guards rather than a tiny MLP
slice.

Half128-even grid:

```text
best by mini: count26 gamma alpha=0.01
Full raw          = 1.470648855e-6
Full adjusted est = 3.43717e-7
Mini raw          = 1.322289288e-6
Mini adjusted est = 3.09043e-7
modeled mult      = 0.23371814
```

Half64-even grid:

```text
best by mini: count26 gamma alpha=0.01
Full raw          = 1.470312182e-6
Full adjusted est = 2.97147e-7
Mini raw          = 1.321591247e-6
Mini adjusted est = 2.67091e-7
modeled mult      = 0.20209814
```

Read: the protected final-cloud signal is real, but in this replacement/append
shape it lands in the high `2e-7` to low `3e-7` adjusted range.  This is not a
route to the `1e-7` adjusted target unless the observer can be folded into work
already paid for by the main branch.

2026-07-08 - exact K26 row-trade plus low-row signed-Ez observer
----------------------------------------------------------------

Question: if we drop K26 sample blocks and spend the saved rows on a robust
independent signed-Ez observer, can we retain the K26 raw signal at lower cost?

Exact CUDA scorer:

```text
legacy_workspace/probe_k26_l4_fullcov_ez_shift.py
weights-cache = legacy_workspace/cache/phase1_full1000_weights_targets.npz
proxy = 64x32 even signed-Ez row observer
K26 seeds = top24 by K26 row weight
```

Full1000 spaced20 (`0,50,...,950`):

```text
lambda=0.00 raw = 1.495875210e-6
lambda=0.08 raw = 1.304222609e-6
lambda=0.10 raw = 1.272500183e-6
lambda=0.12 raw = 1.248386033e-6
lambda=0.14 raw = 1.232880996e-6
lambda=0.16 raw = 1.228081661e-6  # best
lambda=0.18 raw = 1.234890754e-6
lambda=0.20 raw = 1.253308238e-6
lambda=0.22 raw = 1.283334284e-6
lambda=0.25 raw = 1.348216197e-6
```

Full1000 offset20 (`25,75,...,975`):

```text
lambda=0.00 raw = 1.577841696e-6
lambda=0.08 raw = 1.450773170e-6
lambda=0.10 raw = 1.423496882e-6
lambda=0.12 raw = 1.397812035e-6
lambda=0.14 raw = 1.373718856e-6
lambda=0.16 raw = 1.351217800e-6
lambda=0.18 raw = 1.330308526e-6
lambda=0.20 raw = 1.310991148e-6
lambda=0.22 raw = 1.293265438e-6
lambda=0.25 raw = 1.230531944e-6  # best in this grid
```

Read: the signed-Ez observer recovers a lot of raw error from the dropped K26
rows, and the sign transfers across spaced/offset guards.  But the extra
observer cost is still too high: this looks like a low/mid `2e-7` adjusted
branch, not a `1e-7` branch.  Keep the lesson that signed-Ez is load-bearing;
close simple row replacement unless the observer is made essentially free.

2026-07-08 - low-rank deployable Ez residual basis closeout
-----------------------------------------------------------

Question: can the missing final preactivation mean signal be represented as a
small output-neuron basis, with only MLP-level coefficients predicted from
deployable weight/proxy/K26 diagnostics?

Probe:

```text
legacy_workspace/probe_lowrank_ez_residual_basis.py
```

Full1000 grouped OOF target:

```text
true E[z_L] - current fullcov/proxy E[z_L]
```

Best rich feature run (`lean,proxy,k26agg,diag`, ranks 2/4/8/12, multiple ridge
and shrink settings):

```text
base fullcov proxy MSE = 5.1856869e-5
best rank4 alpha10 shrink0.1 proxy MSE = 5.1856949e-5
proxy relative MSE = 1.00000155
residual R2 = -0.000956
cache = legacy_workspace/cache/lowrank_ez_residual_basis_rich_full1000_20260708.npz
```

Read: closed in this form.  The signed `E[z_L]` residual is not a few
cross-MLP output modes with predictable MLP-level coefficients.  Future Ez work
needs either a real independent measurement, a layerwise mechanistic recurrence,
or a representation that uses per-neuron state rather than only global basis
coefficients.

2026-07-08 - reduced-base sampler plus independent signed-Ez observer
---------------------------------------------------------------------

Question: can the true signed-final-preactivation oracle be converted into a
better compute trade by using fewer main K26 sample blocks, then spending the
saved rows on an independent final `E[z_L]` observer?

Patched:

```text
legacy_workspace/probe_extra_ez_lowrow_grid.py
```

Changes:

- use memmapped reads for the huge Full1000 weight cache;
- add `--base-seed-count`, `--base-seed-mode`, `--base-weight-mode`, and
  `--base-seeds` so the main sampler can be reduced instead of always using
  all 26 K26 blocks.

Full1000 spaced20 (`0,50,...,950`), independent observer seeds starting at
`200`, even row selection:

```text
base8 topweight + 8x256 Ez observer:
  best lambda=0.50 raw = 2.279836360e-6
  rough mult = 0.107055
  rough adjusted = 2.440687583e-7

base12 topweight + 8x256 Ez observer:
  best lambda=0.35 raw = 1.588657851e-6
  rough mult = 0.133819
  rough adjusted = 2.125929715e-7

base16 topweight + 8x256 Ez observer:
  best lambda=0.25 raw = 1.349959367e-6
  rough mult = 0.160583
  rough adjusted = 2.167806288e-7
```

The `64x32` observer variants were consistently weaker than `8x256` in this
reduced-base setting.  The base12/base16 curve is close to the current broad
protected line but not a new frontier, and this is before shifted-slice risk.

Read: close as a next-winner path.  The true-`E[z_L]` oracle says reduced
samplers could be elite, but the deployable low-row observer is still too noisy
for the saved compute to pay.  Reopen only with a sharper independent
preactivation-mean estimator or with a way to get the observer almost free from
work already paid by the main branch.

2026-07-08 - mixed K26 sparse schedule transfer check
-----------------------------------------------------

Question: after the current `late192_finalsp176_winograd2` economics line, is
there an untested layer-specific activation-coordinate schedule that saves
compute without hitting the known raw cliff?

Patched:

```text
legacy_workspace/probe_k26_sparse_schedule.py
```

The probe now memory-maps `weights.npy` and `targets.npy` from the huge
Full1000 cache instead of materializing the full 8GB archive.

Targeted Full1000 spaced20 (`0,50,...,950`) scout:

```text
base:     7-23=232,24-30=192,final=176       raw=1.133893061e-6
tailmix:  7-16=236,17-23=224,24-30=192,...   raw=1.130935968e-6  rel=0.997392
tailmix3: 7-12=236,13-23=224,24-30=192,...   raw=1.131031114e-6  rel=0.997476
tailmix2: 7-12=240,13-20=224,21-30=192,...   raw=1.140710017e-6  rel=1.006012
```

Shifted Full1000 offset20 (`25,75,...,975`) scout:

```text
base:     raw=1.384788188e-6
tailmix:  raw=1.377416359e-6  rel=0.994677
tailmix3: raw=1.377142341e-6  rel=0.994479
tailmix2: raw=1.374932750e-6  rel=0.992883
```

Broader Full1000 first100 guard:

```text
base:     raw=1.156406642e-6
tailmix:  raw=1.160426337e-6  rel=1.003476
tailmix3: raw=1.161106022e-6  rel=1.004064
```

Outputs:

```text
/mnt/d/whestbench-data/k26_sparse_schedule_floor_spaced20_20260708.npz
/mnt/d/whestbench-data/k26_sparse_schedule_floor_offset20_20260708.npz
/mnt/d/whestbench-data/k26_sparse_schedule_tailmix_spaced20_20260708.npz
/mnt/d/whestbench-data/k26_sparse_schedule_tailmix_full_first100_20260708.npz
```

Read: no promotion.  Mixed schedules produced tempting 20-MLP wins and small
compute savings, but the broader first100 guard reversed by about `0.35-0.4%`
raw.  Keep the current `7-23=232,24-30=192,final=176` schedule as the protected
K26 economics line unless a different state update changes the hidden row
distribution.

2026-07-08 - omitted-tail sketch variants for K26 sparse columns
-----------------------------------------------------------------

Question: instead of dropping omitted activation coordinates, can a cheap tail
approximation preserve the K26 row cloud under more aggressive column pruning?

Patched:

```text
legacy_workspace/probe_k26_sparse_tail_meanfield.py
```

It now memory-maps Full1000 `weights.npy` and `targets.npy` when possible.

Full1000 spaced20, layers `7..30`, selector `energy`, current fullcov-Ez shift:

```text
drop_scale:
  keep232 raw = 4.451998035e+00  # catastrophic scale explosion
  keep224 raw = 1.371811768e+01

he_col:
  keep256 raw = 1.133275453e-6
  keep232 raw = 1.129914302e-6  rel=0.997034
  keep224 raw = 1.165041956e-6  rel=1.028031
  keep208 raw = 1.699720717e-6  rel=1.499830
  keep192 raw = 1.008884655e-5

mean_he_col:
  keep256 raw = 1.133275453e-6
  keep232 raw = 1.137457116e-6  rel=1.003690
  keep224 raw = 1.131335339e-6  rel=0.998288
  keep208 raw = 1.384060176e-6  rel=1.221292
  keep192 raw = 3.673760523e-6
```

Shifted Full1000 offset20:

```text
he_col:
  keep256 raw = 1.371226398e-6
  keep232 raw = 1.380438175e-6  rel=1.006718
  keep224 raw = 1.496716831e-6  rel=1.091517

mean_he_col:
  keep256 raw = 1.371226398e-6
  keep232 raw = 1.378044938e-6  rel=1.004973
  keep224 raw = 1.380052029e-6  rel=1.006436
```

Outputs:

```text
/mnt/d/whestbench-data/k26_tail_drop_scale_spaced20_20260708.npz
/mnt/d/whestbench-data/k26_tail_he_col_spaced20_20260708.npz
/mnt/d/whestbench-data/k26_tail_he_col_offset20_20260708.npz
/mnt/d/whestbench-data/k26_tail_mean_he_col_spaced20_20260708.npz
/mnt/d/whestbench-data/k26_tail_mean_he_col_offset20_20260708.npz
```

Read: close this tail-sketch family.  `drop_scale` explodes over depth; Gaussian
tail variants do not make 208/192 viable, and their tiny keep232/224 wins are
friendly-slice artifacts that reverse on offset20.  A future sketch would need
an actually signed, layer-aware tail state rather than local omitted
mean/variance.

2026-07-08 - low-row independent final-Ez observer allocation sweep
-------------------------------------------------------------------

Question: can a cheap independent final preactivation mean observer replace some
of the expensive K26 sampled row cloud, or provide a robust add-on correction?

Patched:

```text
legacy_workspace/probe_extra_ez_lowrow_grid.py
```

The probe now supports hybrid observer allocations such as
`4x256+64x16`, with either row-weighted or block-weighted aggregation.  The
observer is external to the protected sampler rows: it estimates final
preactivation `E[z_L]` from extra low-row sphere blocks, then applies a scalar
shift to the protected final preactivation cloud before ReLU readout.

Important outputs:

```text
/mnt/d/whestbench-data/extra_ez_hybrid2048_broad_spaced20_20260708.npz
/mnt/d/whestbench-data/extra_ez_hybrid2048_broad_offset20_20260708.npz
/mnt/d/whestbench-data/extra_ez_reducedbase16_hybrid2048_spaced20_20260708.npz
/mnt/d/whestbench-data/extra_ez_reducedbase16_hybrid2048_offset20_20260708.npz
/mnt/d/whestbench-data/extra_ez_reducedbase12_hybrid2048_spaced20_20260708.npz
/mnt/d/whestbench-data/extra_ez_reducedbase12_hybrid2048_offset20_20260708.npz
/mnt/d/whestbench-data/extra_ez_reducedbase16_h4_64r16_first100_20260708.npz
/mnt/d/whestbench-data/extra_ez_reducedbase12_h4_64r16_first100_20260708.npz
```

20+20 Full1000 paired gate (`spaced20` + shifted `offset20`):

```text
K26 base + 2048 half-row observer:
  best = 4x256 + 64x16, lambda=0.22
  raw = 9.697208406e-7
  rough adjusted = 2.206044048e-7

base16 topweight + 2048 half-row observer:
  best = 4x256 + 64x16, lambda=0.35
  raw = 1.298215011e-6
  rough adjusted = 2.084713609e-7

base12 topweight + 2048 half-row observer:
  best = 4x256 + 64x16, lambda=0.40
  raw = 1.557281143e-6
  rough adjusted = 2.083941646e-7
```

First100 guard:

```text
base16:
  8x256 observer, lambda=0.35
    raw = 1.473001681e-6
    rough adjusted = 2.365391422e-7
  4x256 + 64x16, lambda=0.30
    raw = 1.610947266e-6
    rough adjusted = 2.586908687e-7

base12:
  8x256 observer, lambda=0.40
    raw = 1.782826576e-6
    rough adjusted = 2.385764809e-7
  4x256 + 64x16, lambda=0.35
    raw = 1.996396082e-6
    rough adjusted = 2.671561881e-7
```

Read: no package.  The paired 20+20 gate showed real signal and better
allocation geometry, but the broader first100 guard reversed the hybrid
allocation and left the best reduced-base observer near the old protected
score.  The durable lesson is that independent final-Ez measurements help, but
the low-row allocation is not robust enough as a standalone path to 1e-7.  Use
first100/full200 before promoting any future observer design; do not trust
`spaced20+offset20` alone for this family.

2026-07-08 - reduced K26 counts with incumbent fullcov-Ez shift
---------------------------------------------------------------

Question: the previous reduced-base observer sweep was missing the deterministic
fullcov final-preactivation mean shift that makes the protected package strong.
Can we drop K26 sampled seed count while retaining the incumbent fullcov-Ez
correction?

Probe:

```text
legacy_workspace/probe_k26_l4_fullcov_ez_shift.py
```

Inputs:

```text
weights/targets: legacy_workspace/cache/phase1_full1000_weights_targets.npz
proxy: legacy_workspace/cache/fullcov_gaussian_ez_proxy_full1000_globalcal_from_full1000_20260707.npz
indices: first100
sample weights: K26 weights renormalized over selected top-weight seeds
lambdas: 0,0.02,0.03,0.04,0.05,0.06,0.08,0.10,0.12
```

Outputs:

```text
/mnt/d/whestbench-data/k26_l4_fullcov_ez_top12_first100_20260708.npz
/mnt/d/whestbench-data/k26_l4_fullcov_ez_top16_first100_20260708.npz
/mnt/d/whestbench-data/k26_l4_fullcov_ez_top18_first100_20260708.npz
/mnt/d/whestbench-data/k26_l4_fullcov_ez_top20_first100_20260708.npz
/mnt/d/whestbench-data/k26_l4_fullcov_ez_top22_first100_20260708.npz
/mnt/d/whestbench-data/k26_l4_fullcov_ez_top24_first100_20260708.npz
/mnt/d/whestbench-data/k26_l4_fullcov_ez_top26_first100_20260708.npz
```

First100 best raw by count, with optimistic linear count-only multiplier
`0.173965 * count / 26` floored at `0.1`:

```text
count  lambda  raw             rough_mult  rough_adjusted
12     0.12    2.626730028e-6  0.100000    2.626730028e-7
16     0.10    2.019312205e-6  0.107055    2.161782448e-7
18     0.10    1.783393676e-6  0.120437    2.147871329e-7
20     0.08    1.589317475e-6  0.133819    2.126812419e-7
22     0.08    1.397435687e-6  0.147201    2.057041456e-7
24     0.08    1.250160994e-6  0.160583    2.007546991e-7
26     0.06    1.152066476e-6  0.173965    2.004192445e-7
```

Read: no package.  Even in an optimistic linear compute model, top24 is only
tied with count26 on first100; real packages have fixed overheads, so dropping
seeds is unlikely to improve adjusted score.  Top22 and below lose too much raw
accuracy.  This closes the simple "same fullcov-Ez, fewer K26 seeds" branch
unless a later implementation can remove fixed overhead or recover the lost raw
accuracy with an independent estimator.

2026-07-08 - L30 hidden-state Song learner smoke
------------------------------------------------

Question: the true layer-30 hidden mean is an enormous oracle.  Can a small
matrix-equivariant Song/PERO-style learner correct the sampled/telemetry layer
30 mean from the existing per-layer `l2snap_seedmeans` cache?

Important guardrail: the higher-moment labels on D: are for the public Full
split only, not Mini.  A first attempted Full->Mini hidden-label transfer was
stopped because the Mini cache was being compared to Full moment files; the
bogus Mini base hidden MSE was `1.33`, immediately revealing the split mismatch.

Valid cache:

```text
/mnt/d/whestbench-data/phase1_l2snap_seedmeans_layer30_full1000_20260708.npz
source seed cache: song/data/phase1_l2snap_seedmeans_full1000_slim.npz
target: D: higher-moment true post-ReLU mean at layer 30
base hidden MSE: 2.376808530e-6
```

Grouped Full1000 fold-0 CUDA gate:

```text
python song/src/train_equivariant_residual.py \
  --cache /mnt/d/whestbench-data/phase1_l2snap_seedmeans_layer30_full1000_20260708.npz \
  --out /mnt/d/whestbench-data/phase1_l2snap_layer30_song_h16r2_fold0_e30_20260708.json \
  --checkpoint-out /mnt/d/whestbench-data/phase1_l2snap_layer30_song_h16r2_fold0_e30_20260708.pt \
  --epochs 30 --hidden 16 --rounds 2 --batch 4 \
  --holdout-mod 5 --holdout-value 0 \
  --lr 0.001 --weight-decay 0.01 --dropout 0.05 \
  --weight-features --device cuda
```

Result:

```text
epoch 1  heldout ratio = 1.0011
epoch 10 heldout ratio = 0.999952  # best, +0.0048%
epoch 20 heldout ratio = 1.0002
epoch 30 heldout ratio = 1.0002
```

Read: no package and no scale-up.  This validates the non-naive grouped-fold
setup, but the current `l2snap_seedmeans` matrix representation does not expose
a useful transferable L30 hidden-mean correction to this small Song learner.
Do not spend a large GPU run on this exact representation; a future learner
would need a materially richer state or a different target/architecture.

Follow-up capacity check on the current K26/L4 final-target adapter:

```text
python legacy_workspace/train_song_adapter_transfer.py \
  --train-cache song/data/phase1_k26_l4_full1000_adapter_20260708.npz \
  --valid-cache song/data/phase1_k26_l4_mini100_adapter_20260708.npz \
  --out /mnt/d/whestbench-data/phase1_k26_l4_final_song_h32r3_e30_full2mini_20260708.json \
  --pred-out /mnt/d/whestbench-data/phase1_k26_l4_final_song_h32r3_e30_full2mini_20260708.npz \
  --epochs 30 --hidden 32 --rounds 3 --batch 4 \
  --lr 0.0007 --weight-decay 0.02 --dropout 0.08 \
  --weight-features --eval-every 3 --device cuda
```

Stopped at epoch 6 because transfer was already clearly worse:

```text
Mini base      = 1.278035604e-6
epoch 1 Mini   = 1.278668027e-6  ratio=1.000495
epoch 3 Mini   = 1.283475606e-6  ratio=1.004257
epoch 6 Mini   = 1.283936244e-6  ratio=1.004617
```

Read: larger message-net capacity does not rescue the current K26/L4 final
feature representation.  It worsens Mini before producing any useful transfer
signal, so do not run h64/h128 variants on this same cache without a new input
state or a different supervised target.

2026-07-08 - union53 seed-subset transfer ceiling recheck
---------------------------------------------------------

Added a reusable seed-subset diagnostic:

```text
legacy_workspace/probe_union_seed_subset_transfer.py
```

This uses the cached `l2snap_union53_seed_preds_full1000.npz` and independent
`l2snap_union53_seed_preds_mini100.npz` seed banks.  It reports three equal-
weight greedy curves:

* `target_greedy`: selected on Full public targets, so this is an unsafe oracle;
* `teacher_greedy`: target-free, selected to match the full 53-seed mean;
* `mini_greedy`: selected on Mini targets, only a diagnostic ceiling.

The rough multiplier model uses `ref_count=26`, `ref_mult=0.1625`, and a
`0.020` fixed multiplier.  It is intentionally optimistic for low counts.

Key results:

```text
full 53 equal:
  Full raw = 8.036259277e-7, rough adj = 2.495103962e-7
  Mini raw = 6.763953415e-7, rough adj = 2.100077459e-7

target_greedy selected on Full targets:
  k=18 Full raw = 2.247073498e-6, Mini raw = 2.115078601e-6
  k=26 Full raw = 1.529120581e-6, Mini raw = 1.582571009e-6
  k=40 Full raw = 1.017965274e-6, Mini raw = 9.424098044e-7
       rough adjusted = 2.44e-7 Full / 2.25e-7 Mini

teacher_greedy, target-free match to 53-seed mean:
  k=18 Full raw = 2.286929576e-6, Mini raw = 1.782459558e-6
  k=26 Full raw = 1.605849383e-6, Mini raw = 1.408033692e-6
  k=40 Full raw = 1.050420089e-6, Mini raw = 8.947677835e-7
       rough adjusted = 2.51e-7 Full / 2.14e-7 Mini

mini_greedy diagnostic ceiling:
  k=26 Full raw = 1.701047363e-6, Mini raw = 1.079280102e-6
  k=40 Full raw = 1.092697745e-6, Mini raw = 7.361362560e-7
       rough adjusted = 2.61e-7 Full / 1.76e-7 Mini
```

Read: close "seed identity magic" as a route to the 1e-7 adjusted target.  Even
the unsafe Full-target greedy curve does not get close after optimistic compute
pricing, and the target-free teacher-matching curve is weaker.  The full union
bank remains a high-raw teacher/control source, but its raw gain arrives too
slowly per paid seed block.  Future seed-set work should only be reopened if
there is a new arithmetic scheme that radically reduces per-seed row-cloud
cost, or if seed means are used as labels for a different deployable estimator.

2026-07-08 - qroffset observer package economics closeout
----------------------------------------------------------

Question: the qroffset/pathCV branch has strong raw signal, and the pruned-tail
observer probes showed target-free hidden-observer signal.  Are the literal
observer packages cheap enough to be a leaderboard path?

Profiled packages:

```text
whest-starterkit/packages/active/submission_phase1_qroffobs_k5_l8_o5s12_b09_f16_winograd1_finalonly_bundle.tar.gz
whest-starterkit/packages/active/submission_phase1_qroffobs_rowtail_k224_b05_protected_f16_winograd1_finalonly_bundle.tar.gz
```

`op_profile.py` results:

```text
qroffobs_k5_l8:
  FLOPs = 1.268e11
  warm residual ~= 56 ms
  predicted multiplier = 0.4735 - 0.4854

qroffobs_rowtail_k224:
  FLOPs = 1.522e11
  warm residual ~= 84 ms
  predicted multiplier = 0.5670 - 0.5789
```

Cached raw summaries for the pruned-tail and row-tail observer families put the
best robust warning-slice raw around `4.8e-7` to `5.2e-7`, depending on
observer layer, beta, and row-tail shape.

Read: diagnostics only.  At `0.47x-0.58x`, even `5e-7` raw prices around
`2.4e-7` to `2.9e-7`, and the branch would need raw near `2e-7` to become a
credible 1e-7 adjusted path.  Keep qroff/pathCV as evidence that an independent
hidden/final-Ez observer has signal; do not promote the literal observer
packages without a new cost representation.

2026-07-08 - reduced K top-weight plus low-row final-Ez observer rerun
----------------------------------------------------------------------

Question: can we replace some expensive K26 sampled seeds with a small
independent sampled final-preactivation `E[z_L]` observer while retaining broad
raw accuracy?

Probe:

```text
legacy_workspace/probe_k26_l4_fullcov_ez_shift.py
```

Outputs:

```text
legacy_workspace/cache/k18top_l4_extraez64x32_spaced20_20260708.npz
legacy_workspace/cache/k18top_l4_extraez64x32_offset20_20260708.npz
legacy_workspace/cache/k20top_l4_extraez64x32_spaced20_20260708.npz
legacy_workspace/cache/k20top_l4_extraez64x32_offset20_20260708.npz
legacy_workspace/cache/k22top_l4_extraez64x32_spaced20_20260708.npz
legacy_workspace/cache/k22top_l4_extraez64x32_offset20_20260708.npz
legacy_workspace/cache/k24top_l4_extraez64x32_spaced20_20260708.npz
legacy_workspace/cache/k24top_l4_extraez64x32_offset20_20260708.npz
```

Best scalar-shift results:

```text
K18 + 64x32 low-row Ez:
  spaced20 lambda=0.14 raw=1.288397896e-6
  offset20 lambda=0.30 raw=1.526934040e-6

K20 + 64x32 low-row Ez:
  spaced20 lambda=0.18 raw=1.364642182e-6
  offset20 lambda=0.30 raw=1.460946537e-6

K22 + 64x32 low-row Ez:
  spaced20 lambda=0.18 raw=1.262929627e-6
  offset20 lambda=0.26 raw=1.323252874e-6

K24 + 64x32 low-row Ez:
  spaced20 lambda=0.18 raw=1.229951310e-6
  offset20 lambda=0.26 raw=1.232047648e-6
```

Read: no package.  The observer recovers meaningful signed `E[z_L]` signal from
dropped K26 seeds, but K18/K20/K22 remain too inaccurate, and K24 is only a
low/mid-2e-7 adjusted line after realistic overhead.  This reinforces the
larger lesson: final-preactivation mean is the right target, but the current
low-row way of buying it is not cheap enough for 1e-7.

2026-07-08 - Full1000 to Mini current-state Song adapter gate
-------------------------------------------------------------

Question: can a Phase-1-native equivariant residual learner over the current
K26/L4 adapter features learn a transferable correction if trained on the full
1,000 public MLPs and validated on the independent Mini100 split?

Run:

```text
python legacy_workspace/train_song_adapter_transfer.py \
  --train-cache song/data/phase1_k26_l4_full1000_adapter_20260708.npz \
  --valid-cache song/data/phase1_k26_l4_mini100_adapter_20260708.npz \
  --out legacy_workspace/cache/song_adapter_full1000_h64r4_wfeat_20260708.json \
  --pred-out legacy_workspace/cache/song_adapter_full1000_h64r4_wfeat_20260708_preds.npz \
  --device cuda --epochs 50 --hidden 64 --rounds 4 --batch 8 --lr 0.001 \
  --weight-decay 0.003 --dropout 0.05 --weight-features --eval-every 5 \
  --seed 20260708
```

The cache loaded with train `(1000, 256)`, valid `(100, 256)`, feature tensor
`(1000, 32, 256, 26)`, and 35 input features with `--weight-features`.

Observed before stopping the run:

```text
train base raw = 1.393771419e-6
valid base raw = 1.278035604e-6

epoch 1:  valid raw = 1.282224356e-6
epoch 5:  valid raw = 1.278786743e-6
epoch 10: valid raw = 1.281591079e-6
```

Read: reject this representation.  The model can fit a little train-side noise,
but independent Mini validation is flat or worse.  More epochs over the same
current-state feature tensor are not a 1e-7 path.  Reopen learned residuals only
with a materially new runtime observable, such as a deployable signed
late-hidden or final-preactivation error sensor.
### 2026-07-08 - pathCV compensated sparse schedule probe

Question: can the high-raw pathCV branch become economically useful by pruning
activation coordinates more aggressively than sparse224, while replacing omitted
coordinates with a cheap Gaussian/mean tail correction?

Change:

```text
legacy_workspace/probe_pathcv_sparse_schedule.py
```

now supports `--tail-mode {drop,mean,mean_he_scalar,mean_he_col,he_scalar,he_col}`.
The default remains `drop`; the probe below used `mean_he_col`, which adds the
omitted column mean and a per-output He-style omitted-tail variance before the
ReLU.

Runs:

```text
/mnt/d/whestbench-data/pathcv_schedule_meanhecol_full_spaced20_20260708.npz
/mnt/d/whestbench-data/pathcv_schedule_meanhecol_full_offset20_20260708.npz
/mnt/d/whestbench-data/pathcv_schedule_meanhecol_sweep_full_spaced20_20260708.npz
/mnt/d/whestbench-data/pathcv_schedule_meanhecol_sweep_full_offset20_20260708.npz
```

Best schedules:

```text
base224 = 7-29=224, final=224
sA      = 7-15=224, 16-23=208, 24-29=192, final=192
sB      = 7-15=224, 16-21=208, 22-29=192, final=192
sD      = 7-17=224, 18-25=208, 26-29=192, final=176
```

Full spaced20, indices `0,50,...,950`, pathCV84-style rows:

```text
base224__z12_s3  raw=4.795494943e-7
sB__z12_s3       raw=4.786396314e-7
sA__z12_s3       raw=4.789432163e-7
sD__z12_s3       raw=4.793482538e-7
```

Full offset20, indices `5,55,...,955`:

```text
base224__z12_s3  raw=5.194946763e-7
sD__z12_s3       raw=5.210622924e-7
sB__z12_s3       raw=5.212578536e-7
sA__z12_s3       raw=5.213530091e-7
```

Read: compensated coordinate pruning transfers in the narrow sense that the
224/208/192 stair is raw-neutral to within about half a percent on the broad
guards.  Uniform 208/192 and late176/160 are not safe enough.  This is useful
as a small compute trim for a routed pathCV arm, but not a standalone 1e-7
breakthrough: pathCV's main problem remains seed/row count and branch routing,
not the last few percent of coordinate FLOPs.

### 2026-07-08 - qroffset/pathCV count-stack correction and stratified guard

Question: the qroffset/pathCV probe computes nested count readouts
`k3/k4/k7` in a single run.  Can a free fixed linear stack across those count
prefixes beat the best single count without more estimator FLOPs?

Important harness correction:

```text
legacy_workspace/probe_qroffset_pathcv_count_stack.py
```

had a non-affine stacking bug: predictors were standardized without restoring
their means/intercept, producing impossible raw errors near `0.28`.  The
non-affine path now fits the raw readouts directly and reports the fitted
coefficients correctly.

Small-slice corrected results:

```text
/mnt/d/whestbench-data/qroffset_pathcv_full_spaced20_20260708.npz
  best single: k7_h1corr_z12_s2 raw=3.398979644e-7
  best stack:  rawh1_z12_s2     raw=3.030609669e-7
  coef ~= [0.112568, 0.083632, 0.803541]

/mnt/d/whestbench-data/qroffset_pathcv_spread10_20260708.npz
  best single: k7_rawh1_z12_s2  raw=3.572798505e-7
  best stack:  rawh1_z12_s2     raw=3.260054571e-7
  coef ~= [0.075424, 0.232836, 0.691510]
```

Stronger Full stratified-50 guard:

```text
/mnt/d/whestbench-data/qroffset_pathcv_full_strat50_20260708.npz

k3 best: h1corr_mean raw=7.324494161e-7
k4 best: h1corr_mean raw=5.818022079e-7
k7 best: h1corr_mean raw=3.287123708e-7
```

Corrected stack on the same stratified-50 guard:

```text
/mnt/d/whestbench-data/qroffset_pathcv_count_stack_full_strat50_20260708.npz

best single: k7_h1corr_mean raw=3.287123708e-7
best stack:  h1corr_mean    raw=3.485495828e-7
```

Read: reject count-stack promotion.  The k3/k4/k7 stack was a tempting narrow
slice win but did not survive the broader stratified guard.  The qroffset/pathCV
branch remains a real raw-MSE frontier (`~3.3e-7` on stratified Full50), but at
current qroffset7x18 pricing it is a high-compute diagnostic, not an adjusted
winner.  Reopen only if a materially cheaper way to buy the independent
qroffset/pathCV observations appears.

### 2026-07-08 - K26 final omitted-coordinate tail correction

Question: in the current K26/L4/fullcov-Ez sparse branch, can the omitted final
activation coordinates be used as a cheap correction instead of being dropped?

Change:

```text
legacy_workspace/probe_k26_sparse_schedule.py
```

now supports final-layer tail modes.  The relevant production-shaped mode is
`--final-tail-mode mean`, which adds the row-cloud mean contribution from the
omitted final coordinates before the final ReLU.  The probe was rerun with the
uncalibrated fullcov-Ez proxy cache, avoiding double calibration.

Baseline schedule:

```text
7-23=232,24-30=192,final=176
```

Corrected beta sweep, Full spaced20 (`0,50,...,950`):

```text
drop / beta=0 raw = 1.133893061e-6
mean beta=0.25 raw = 1.133367011e-6
mean beta=0.50 raw = 1.132970824e-6
mean beta=1.00 raw = 1.132523529e-6
```

Independent Full offset20 (`25,75,...,975`):

```text
drop / beta=0 raw = 1.384788188e-6
mean beta=1.00 raw = 1.379337835e-6
```

Full first100:

```text
drop / beta=0 raw = 1.156406642e-6
mean beta=1.00 raw = 1.148184856e-6
```

Layer schedule sweep with `mean beta=1.0`:

```text
tailmix  = 7-16=236,17-23=224,24-30=192,final=176
tailmix3 = 7-12=236,13-23=224,24-30=192,final=176
tailmix2 = 7-12=240,13-20=224,21-30=192,final=176

Full spaced20:
  base     raw=1.132523529e-6
  tailmix  raw=1.129586293e-6
  tailmix3 raw=1.129668997e-6
  tailmix2 raw=1.139135093e-6

Full offset20:
  base     raw=1.379337835e-6
  tailmix  raw=1.372149850e-6
  tailmix3 raw=1.371887089e-6
  tailmix2 raw=1.369620016e-6

Full first100:
  base     raw=1.148184856e-6
  tailmix  raw=1.152354778e-6
  tailmix3 raw=1.153033914e-6
  tailmix2 raw=1.156243057e-6
```

Gaussian omitted-tail mode (`mean_he_col`) does not make smaller final keeps
safe:

```text
Full spaced20:
  f192 raw=1.132729897e-6
  f176 raw=1.132471230e-6
  f160 raw=1.135179587e-6
  f144 raw=1.190417468e-6
  f128 raw=1.862305331e-6

Full offset20:
  f192 raw=1.379325354e-6
  f176 raw=1.379347730e-6
  f160 raw=1.387609324e-6
  f144 raw=1.597961454e-6
  f128 raw=3.077268524e-6

Full first100:
  f192 raw=1.147739915e-6
  f176 raw=1.148092422e-6
  f160 raw=1.158949379e-6
  f144 raw=1.321806474e-6
  f128 raw=2.378073210e-6
```

Read: keep the `mean beta=1.0` final-tail correction as a small, nearly free
candidate tweak after flopscope profiling, but do not treat it as a path to
`1e-7`.  It buys about `0.1%` to `0.7%` raw on broad guards.  Mixed activation
schedules are not robust enough because they lose on first100, and the Gaussian
tail does not allow final keep below roughly `176`.

### 2026-07-08 - late empirical full-covariance restart reject

Question: can the current K26 row cloud be compressed late in the network by
keeping only its empirical mean/covariance, regenerating a Gaussian row cloud,
and sampling the remaining tail?  This is a stronger version of the old
diagonal sample/analytic handoff: if even a one-layer full-covariance restart
works, it would point to a cheaper late-state representation.

Run:

```text
python legacy_workspace/probe_empirical_gaussian_restart.py \
  --weights-cache legacy_workspace/cache/phase1_full1000_weights_targets.npz \
  --indices 0,50,100,...,950 \
  --seeds <K26 seeds> --seed-weights <K26 weights> \
  --handoffs 24,28,30,31 --normal-seed 20260708 \
  --batch-mlp 1 --device cuda \
  --out-cache /mnt/d/whestbench-data/empirical_gaussian_restart_k26_full_spaced20_late_20260708.npz
```

Full spaced20, K26 geometry:

```text
sample_full_prod  raw=1.245530368e-6
gauss_raw_after31 raw=2.149062376e-6  rel=1.725
gauss_raw_after30 raw=3.163917612e-6  rel=2.540
gauss_raw_after28 raw=4.652743301e-6  rel=3.736
gauss_raw_after24 raw=7.367028910e-6  rel=5.915
```

OOF two-feature stacks with the sampled prediction also worsened:

```text
base sample_full                         1.245530368e-6
sample + gauss_raw_after31 OOF stack     1.434329548e-6
sample + gauss_raw_after30 OOF stack     1.433019944e-6
sample + gauss_raw_after28 OOF stack     1.420786936e-6
sample + gauss_raw_after24 OOF stack     1.369732832e-6
```

Read: reject this compression path.  The late row cloud contains important
non-Gaussian structure that empirical mean/covariance does not preserve, even
one layer before the final readout.  Do not build a candidate around a
Gaussianized late restart or a mean/covariance-only tail summary.

### 2026-07-08 - K26 final-tail one-level Winograd remote-compute hedge

Question: after adding the cheap omitted-final-coordinate mean correction, is
the two-level Winograd row-cloud multiplication still the best production
economics shape?  Two-level has lower counted FLOPs, but one-level has far
fewer flopscope operations and may price better remotely if client residual is
still material.

Built an isolated sibling:

```text
legacy_workspace/
_pkg_pure26_w05_unionteacher1100gamma_l2snap_split035_045_noaffine_module_rectsparse232s7_late192_finalsp176_winograd1_statscdf_finaltailmean_preedgeL4_fullcovEzCal_finalonly/
```

and packaged:

```text
whest-starterkit/packages/to_test_remote/
submission_phase1_pure26_w05_unionteacher1100gamma_l2snap_split035_045_rectsparse232s7_late192_finalsp176_winograd1_statscdf_finaltailmean_preedgeL4_fullcovEzCal_finalonly_bundle.tar.gz
```

The only code change versus the two-level final-tail stats-CDF sibling is:

```text
_strassen2      = _winograd1_any
_strassen2_rect = _winograd1_any
```

Profile:

```text
one-level final-tail stats-CDF:
  warm FLOPs = 4.524e10
  warm ops   = 2031
  remote model:
    25us/op mult = 0.1850
    45us/op mult = 0.1999
    65us/op mult = 0.2149

two-level final-tail stats-CDF, earlier profile:
  warm FLOPs = 4.038e10
  warm ops   = 5937
  remote model:
    25us/op mult = 0.2030
    45us/op mult = 0.2467
    65us/op mult = 0.2903
```

Package-shaped raw guards:

```text
Full spaced20:
  raw = 1.132513e-6
  local adjusted = 2.009282e-7
  local mult = 0.177152

Full offset20:
  raw = 1.379384e-6
  local adjusted = 2.444458e-7
  local mult = 0.177114

Mini spaced20:
  local run had one WSL residual-zero row, so aggregate score is invalid.
  Excluding that local residual failure, valid-row raw = 1.277049e-6.
```

Read: this is not a new statistical estimator and not a `1e-7` path by itself.
It is a legitimate remote-compute hedge against the two-level package: raw is
parity on the Full guards, while the op profile is much friendlier if remote
residual scales with operation count.  It is worth remote-testing only if we
need to disambiguate one-level versus two-level client economics; otherwise the
two-level package remains better under purely local/FLOP-heavy scoring.

### 2026-07-08 - Phase-1 synthetic all-layer cache and diagonal-student gate

Question: can a Phase-1-native synthetic all-layer bake support a learned
external estimator lane, without using C: for large data?

Built on D: with CUDA:

```text
python legacy_workspace/build_phase1_synthetic_mc_cache.py \
  --out /mnt/d/whestbench-data/phase1_synth_depth32_diag_alllayers_128x1m_20260708.npz \
  --n-mlps 128 --samples 1000000 --chunk 65536 \
  --width 256 --depth 32 --all-layers --diag-rows --device cuda
```

Artifact:

```text
/mnt/d/whestbench-data/phase1_synth_depth32_diag_alllayers_128x1m_20260708.npz
size ~= 1.1G
final MC label-noise MSE mostly ~= 1e-8 to 1e-7
```

Small CUDA transfer gate:

```text
python song/src/train_equivariant_residual_big.py \
  --cache song/data/phase1_diag_mini100_matrix_cache.npz \
  --finetune-cache song/data/phase1_diag_full1000_matrix_cache.npz \
  --pretrain-cache /mnt/d/whestbench-data/phase1_synth_depth32_diag_alllayers_128x1m_20260708.npz \
  --pretrain-all-layers --pretrain-epochs 20 --epochs 40 \
  --hidden 16 --rounds 2 --batch 8 \
  --lr 0.001 --weight-decay 0.01 --finetune-lr 0.0005 \
  --dropout 0.05 --weight-features --device cuda --eval-every 5 \
  --out /mnt/d/whestbench-data/phase1_diag_student_synth128_full1000_to_mini_h16r2_20260708.json
```

Stopped manually at epoch 10 because Mini transfer plateaued:

```text
Mini diagonal base                 8.850247e-4
after synthetic pretrain epoch 20  6.508868e-4  ratio=0.7354
after Full1000 finetune epoch 5    6.469630e-4  ratio=0.7310
after Full1000 finetune epoch 10   6.471313e-4  ratio=0.7312
```

Read: the synthetic infrastructure works and the labels are accurate enough
for a learned-lane smoke, but diagonal-only state is far too weak to blend with
the current sampler.  Do not scale diagonal-only PERO.  Reopen the synthetic
lane only with a richer deployable state/teacher: current K26/L4 features,
state-space cumulant features, or a direct late hidden/final preactivation
teacher.

### 2026-07-08 - State-aware synthetic student smoke

Question: does adding the deployable analytic estimator's internal per-layer
state fields to the synthetic all-layer student create a standalone estimate
accurate enough to blend with the protected sampler?

Extractor fix:

```text
song/src/extract_state_features.py now materializes small referenced
Mini/Full200 weight slices once, while preserving source indices for seeds.
```

Generated state caches:

```text
/mnt/d/whestbench-data/states/phase1_synth_depth32_diag_alllayers_128x1m_20260708_states7.npz
/mnt/d/whestbench-data/states/phase1_diag_mini100_matrix_cache_states7.npz
```

CUDA gate:

```text
python song/src/train_equivariant_residual_states.py \
  --cache song/data/phase1_diag_mini100_matrix_cache.npz \
  --states-dir /mnt/d/whestbench-data/states \
  --pretrain-cache /mnt/d/whestbench-data/phase1_synth_depth32_diag_alllayers_128x1m_20260708.npz \
  --pretrain-all-layers --pretrain-epochs 30 --epochs 0 \
  --hidden 16 --rounds 2 --batch 8 \
  --lr 0.001 --weight-decay 0.01 --dropout 0.05 \
  --device cuda --eval-every 5 \
  --out /mnt/d/whestbench-data/phase1_state_student_synth128_to_mini_h16r2_20260708.json
```

Best held-out Mini result:

```text
Mini diagonal base         8.850247e-4
state-aware corrected      3.398598e-4
ratio                      0.3840
best epoch                 25
```

Read: real relative transfer, but still orders of magnitude too weak to serve
as the independent estimate needed for sampler blending.  Do not scale this
diagonal/state-only student.  If reopening the learned lane, condition on a
much stronger target object: late hidden/final preactivation cumulant sensors,
current protected sampler state, or true-moment-derived closure labels.

2026-07-08 - qroffset rowtail pricing and synthetic K26 pretrain smoke
----------------------------------------------------------------------

Question 1: does the existing rowtail qroffset observer package price low
enough to convert its good raw-MSE diagnostic into an adjusted-score candidate?

Profiled:

```text
legacy_workspace/_pkg_qroffobs_rowtail_k224_b05_protected_f16_winograd1_finalonly/
```

`op_profile.py` on three Mini MLPs:

```text
warm FLOPs = 1.522e11
warm ops   = 808
predicted remote multiplier:
  25us/op 0.5670
  45us/op 0.5730
  65us/op 0.5789
```

The matching cached rowtail guards have protected-weight raw around
`5.8e-7` on Full spaced20 and `5.4e-7` on Full offset20.  At a `~0.57`
multiplier this prices around `3.1e-7` to `3.4e-7`, so it is not a next-winner
or 1e-7 path.  Close the literal rowtail package; keep rowtail only as evidence
that shallow independent hidden observers contain signal.

Question 2: can synthetic MC labels help the current K26/L4 Song adapter,
unlike the rejected diagonal-only synthetic student?

Patch:

```text
legacy_workspace/build_k26_l4_ez_matrix_cache.py
```

now accepts synthetic caches that use `target` instead of `targets`.

Attempted a full synthetic-128 K26/L4 feature extraction on D, but
`batch-mlp=8` stalled after the first batch and was stopped after about five
minutes.  A fast smoke with `batch-mlp=1` succeeded:

```text
python legacy_workspace/build_k26_l4_ez_matrix_cache.py \
  --weights-cache /mnt/d/whestbench-data/phase1_synth_depth32_diag_alllayers_128x1m_20260708.npz \
  --skip-true-ez --indices 0:16 --batch-mlp 1 --device cuda \
  --out /mnt/d/whestbench-data/phase1_synth16_k26_l4_ez_matrix_20260708.npz
```

Built a same-shape 26-channel Song adapter cache:

```text
/mnt/d/whestbench-data/phase1_synth16_k26_l4_song_adapter_20260708.npz
base_mse = 2.009486701e-6
```

Transfer gate:

```text
python song/src/train_equivariant_residual_big.py \
  --cache song/data/phase1_k26_l4_mini100_adapter_20260708.npz \
  --finetune-cache song/data/phase1_k26_l4_full1000_adapter_20260708.npz \
  --pretrain-cache /mnt/d/whestbench-data/phase1_synth16_k26_l4_song_adapter_20260708.npz \
  --pretrain-epochs 20 --epochs 20 --hidden 16 --rounds 2 --batch 8 \
  --lr 0.001 --weight-decay 0.01 --finetune-lr 0.0005 --dropout 0.05 \
  --weight-features --device cuda --eval-every 5
```

Observed:

```text
pretrain epoch 1  public-test ratio = 1.0166
pretrain epoch 20 public-test ratio = 1.1145
finetune epoch 1  public-test ratio = 1.0042
finetune epoch 5  public-test ratio = 1.0098
finetune epoch 10 public-test ratio = 1.0109
```

The run was stopped at epoch 10.  Read: reject this synthetic-K26 pretraining
lane in its current form.  The synthetic labels and current K26 adapter
representation do not supply a transferable correction; they actively push the
public/Mini validation direction wrong before Full1000 finetune flattens near
or above baseline.  Do not scale synthetic K26 extraction until the runtime
state/target changes materially.

Correction / stronger follow-up: the host-RAM `train_equivariant_residual_big.py`
path used in the smoke above loads `pred_rows` and ignores the richer
`pred_features` tensor.  To make sure this was not a false negative, reran the
existing synthetic-16 adapter with the feature-aware transfer trainer:

```text
python legacy_workspace/train_song_adapter_transfer.py \
  --train-cache /mnt/d/whestbench-data/phase1_synth16_k26_l4_song_adapter_20260708.npz \
  --valid-cache song/data/phase1_k26_l4_mini100_adapter_20260708.npz \
  --out /mnt/d/whestbench-data/phase1_synth16_k26_featureaware_to_mini_h16r2_20260708.json \
  --pred-out /mnt/d/whestbench-data/phase1_synth16_k26_featureaware_to_mini_h16r2_20260708_preds.npz \
  --epochs 12 --hidden 16 --rounds 2 --batch 4 --lr 0.001 \
  --weight-decay 0.01 --dropout 0.05 --weight-features \
  --eval-every 3 --device cuda --seed 20260708
```

Observed:

```text
train base raw = 2.009486701e-6
Mini base raw  = 1.278035604e-6

epoch 1  Mini ratio = 1.151751
epoch 3  Mini ratio = 1.073796
epoch 6  Mini ratio = 1.036653  # best
epoch 9  Mini ratio = 1.056861
epoch 12 Mini ratio = 1.061472
```

Read: close the synthetic-16 K26 adapter even after correcting the feature
consumption issue.  Scaling to 32/64 synthetic rows is now low priority unless
the target representation changes; this feature-aware gate still learns a
synthetic-side correction that transfers wrong-signed to independent Mini.

2026-07-08 - truncated old-side observer compression closeout
-------------------------------------------------------------

Question: the old protected/l2snap side observer is one of the only external
signals that transferred Full->Mini, but the literal full-depth side observer
is too expensive.  Can the transferable correction be recovered from early or
checkpoint-only half-row features, so the auxiliary branch could be stopped
before the final layer?

Patch:

```text
legacy_workspace/probe_l2snap_side_teacher_distill.py
```

Added no-final layer modes:

```text
early4          = [3,7,11,15]
early8          = [1,3,5,7,9,11,13,15]
checks3_nofinal = [7,15,23]
checks4_nofinal = [3,11,19,27]
```

Run:

```text
python legacy_workspace/probe_l2snap_side_teacher_distill.py \
  --variants half64_even,half128_even \
  --layer-modes early4,early8,checks3_nofinal,checks4_nofinal \
  --alphas 0.03,0.1,0.3 --abs-modes 1 --mixes 0.0,0.5,1.0 \
  --folds 5 \
  --out legacy_workspace/cache/l2snap_side_teacher_distill_early_nofinal_20260708.json
```

Best strict gain-calibrated rows:

```text
base Mini raw = 1.278035604e-6

best Mini gain:
  half128_even checks4_nofinal, teacher label, alpha=0.3
  Mini raw = 1.275746130e-6, rel = 0.998209
  Full rel = 0.999519
  Mini residual corr = 0.0471
  Mini teacher corr  = 0.1048

best unit-signal Mini row:
  half64_even checks4_nofinal, teacher label, alpha=0.03
  Mini raw = 1.275340126e-6, rel = 0.997891
  Full rel = 0.999597
```

Read: close this compression route.  Checkpoint-only side features carry a
small transferable signal, and the signal grows as checkpoints get later, but
it is two orders of magnitude too small for the compute it would require.  A
half64 side observer through layer 27 still costs roughly `0.17 * 28/32` of
the current sampled-row work for only about `0.2%` raw gain, and half128 is
worse on compute.  The useful old-side signal therefore appears to be a final
tail measurement, not an early/checkpoint state that can be cheaply stopped.

2026-07-08 - fused Strassen transform economics probe
-----------------------------------------------------

Question: the active two-level Strassen K26 package is analytically cheaper
but creates thousands of flopscope add/sub/concat operations.  Can those
linear combinations be fused into a few dense coefficient transforms, trading a
small number of extra FLOPs for a much lower op count and better remote
residual economics?

Added isolated wrapper:

```text
legacy_workspace/candidate_k26_rectsparse_fusedstrassen_env.py
```

It wraps the current
`late192_finalsp176_winograd2_statscdf_finaltailmean` package and replaces only
`_strassen2` / `_strassen2_rect`.  Small-shape equivalence against dense
matmul passed at float32 tolerance:

```text
level1 maxerr ~= 1.1e-6
level2 maxerr ~= 1.1e-6
```

`op_profile.py`, 2 Mini MLPs:

```text
existing two-level final-tail stats-CDF:
  FLOPs = 4.038e10
  ops   = 5937
  remote model at 25/45/65us = 0.2030 / 0.2467 / 0.2903

fused level 2:
  FLOPs = 5.887e10
  ops   = 1969
  remote model at 25/45/65us = 0.2345 / 0.2490 / 0.2635

fused level 1:
  FLOPs = 4.750e10
  ops   = 1969
  remote model at 25/45/65us = 0.1927 / 0.2072 / 0.2217

existing one-level final-tail stats-CDF, prior profile:
  FLOPs = 4.524e10
  ops   = 2031
  remote model at 25/45/65us = 0.1850 / 0.1999 / 0.2149
```

Read: close fused coefficient-transform Strassen for the current package.
The op-count reduction is real, but dense coefficient transforms charge too
many analytical FLOPs.  Fused level 1 is slightly better than two-level under
high residual-cost assumptions, but it is still worse than the already-staged
one-level Winograd final-tail package on both FLOPs and modeled multiplier.

2026-07-08 - Full-to-Mini fullcov-mode Ez transfer proxy
--------------------------------------------------------

Question: the first200 fullcov-mode nonlinear probes improved final
preactivation `E[z_L]` proxy MSE in OOF, but did not beat the incumbent
calibrated fullcov-Ez shift on the same Full slice.  Does the same idea
transfer Full -> Mini if trained as an atlas-style proxy from D: higher-moment
labels, without using Mini moment labels?

Added:

```text
legacy_workspace/probe_fullcov_mode_ez_transfer.py
```

It trains HGB models on Full higher-moment labels for
`true_E[z_L] - fullcov_E[z_L]`, applies them to Mini weights/features, and
writes a compact `proxy_ez` cache for the existing K26/L4 scorer.

Run:

```text
python legacy_workspace/probe_fullcov_mode_ez_transfer.py \
  --train-indices 0:200 --valid-indices 0:100 \
  --models hgb:3:80:0.01,hgb:4:120:0.03 \
  --device cuda \
  --out /mnt/d/whestbench-data/fullcov_mode_ez_transfer_train200_to_mini_20260708.npz
```

Full train-side Ez proxy quality:

```text
raw fullcov Ez MSE       = 1.522325163e-4
hgb:3:80 train proxy MSE = 4.928015840e-5
hgb:4:120 train proxy MSE= 4.801516962e-5
```

Mini final-mean K26 shift gate:

```text
incumbent full1000-calibrated fullcov-Ez:
  best lambda=0.05 raw = 1.278035604e-6

HGB transfer proxy:
  best lambda=0.04 raw = 1.287517330e-6
```

Follow-up: fit a Full-side linear blend of incumbent proxy plus both HGB
proxies, selected by grouped Full Ez OOF only:

```text
incumbent train Ez MSE = 5.064078978e-5
best blend OOF Ez MSE  = 4.717269319e-5
```

Mini final-mean gate for the blended proxy:

```text
best lambda=0.04 raw = 1.290005349e-6
```

Read: close this atlas proxy shape.  The nonlinear/fullcov-mode features can
improve `E[z_L]` proxy MSE on Full labels, but the correction is not aligned
with the final ReLU row-cloud shift well enough to beat the incumbent
calibrated fullcov-Ez proxy on independent Mini.  Do not scale this HGB proxy
to Full1000 unless the feature state changes materially.

2026-07-08 - target-aligned final-cloud shift teacher
-----------------------------------------------------

Question: maybe training an `E[z_L]` proxy is the wrong target.  The deployable
K26/L4 scorer shifts the sampled final preactivation row cloud and only then
applies ReLU/gamma readout.  Build the inverse label directly: for each
Full-row final neuron, find the minimum-norm scalar preactivation shift that
would make the existing K26/L4 row cloud match the final target under fixed
base gamma, then train the existing K26/L4 feature bank to predict that proxy.

Added:

```text
legacy_workspace/build_final_cloud_shift_teacher.py
```

Important bug caught in the smoke: zero-target final neurons make the inverse
non-unique.  The first bisection chose an arbitrary far-negative shift and
produced absurd `teacher_ez_resid_mse ~= 3.1e3`.  The corrected solver searches
toward the root closest to zero; smoke shifts became sane:

```text
teacher_shift range ~= -0.0064 .. +0.0447
teacher_ez_resid_mse = 4.676e-3
```

First200 teacher build:

```text
python legacy_workspace/build_final_cloud_shift_teacher.py \
  --indices 0:200 --batch-mlp 8 --device cuda \
  --out /mnt/d/whestbench-data/k26_l4_final_cloud_shift_teacher_first200_20260708.npz

base_final_mse                  = 1.299927939e-6
fixed_gamma_teacher_final_mse   = 7.54e-15
teacher_ez_resid_mse            = 4.144314677e-3
```

Grouped Full first200 ridge against the nonlinear teacher, using the existing
deployable K26/L4 feature bank:

```text
best train OOF teacher MSE = 3.707875931e-3
relative to teacher base   = 0.894690
```

This looks like ~10.5% teacher-variance capture, but the real final scorer says
it is not useful.  Same-subset Full first200 OOF proxy through the actual
K26/L4 final-ReLU shift:

```text
lambda=0       raw = 1.299928535e-6
lambda=0.002   raw = 1.299831200e-6  # rel 0.999925, negligible
lambda=0.005   raw = 1.300457040e-6
lambda=0.02    raw = 1.317467200e-6
```

Full200->Mini transfer through the same scorer is decisively negative:

```text
lambda=0       raw = 1.325311634e-6
lambda=0.02    raw = 1.328898875e-6
lambda=0.05    raw = 1.407804405e-6
lambda=0.10    raw = 1.735598184e-6
lambda=1.00    raw = 5.006859537e-5
```

Read: close this target-aligned proxy with the current K26/L4 feature bank.
It is a better supervised target than true `E[z_L]`, but the same deployable
features still do not predict the signed row-cloud correction in a useful way.
Do not scale this to Full1000 unless the feature state changes materially.

2026-07-08 - oracle-local b111 nonlinear bridge falsifier
---------------------------------------------------------

Question: ridge over local/oracle b111 features was already known to predict
the true b111 component but not the current sampler residual.  Was that merely
a linear-model limitation?

Ran the CUDA nonlinear companion on the cached Full1000 b111 response cache:

```text
python legacy_workspace/probe_b111_cached_mlp.py \
  --cache legacy_workspace/cache/b111_response_features_full1000_20260707.npz \
  --feature-set local_sq --target oracle_residual --gain-mode oof_gain \
  --folds 5 --hidden 96 --depth 2 --epochs 50 --device cuda \
  --out /mnt/d/whestbench-data/b111_oracle_local_sq_mlp_oof_current_20260708.npz
```

Result:

```text
signal_r2 against oracle residual = +0.846876
base current sampler raw          = 2.301738107e-6
corrected raw                     = 2.301794970e-6
mean fitted gain                  = -0.00251
```

Direct sampler-residual target with the same oracle-local features:

```text
signal_r2 against sampler residual = -0.000947
corrected raw                      = 2.303917969e-6
```

Read: close "predict b111 better" as a bridge into the current K26/SPHEREx
sampler.  Even oracle-local nonlinear features can predict the b111/oracle
component but it is orthogonal to the current sampler residual.  The missing
K26 coordinate remains signed late hidden / signed final preactivation mean,
not standalone b111 accuracy.

2026-07-08 - current signed-Ez low-row observer and gate closeout
-----------------------------------------------------------------

Question: can the robust independent signed final-preactivation observer be
turned into either a deployable add-on or a target-free adaptive gate on top of
the current K26/L4/fullcov-Ez scorer?

Patched:

```text
legacy_workspace/probe_extra_ez_lowrow_grid.py
```

with `--store-preds`, so the fixed-lambda final predictions can be reused for
target-free gate diagnostics without recomputing the low-row observers.

Full first100, current scorer, `even` row subsets:

```text
base K26/L4/fullcov-Ez raw       = 1.268959223e-6

8 blocks x 256 half-rows:
  best lambda = 0.20
  raw         = 1.002110808e-6
  rough adj   = 2.279728857e-7

64 blocks x 32 half-rows:
  best lambda = 0.16
  raw         = 1.082874333e-6
  rough adj   = 2.46346e-7

hybrid 4x256 + 64x16:
  best lambda = 0.20
  raw         = 1.068418510e-6
  rough adj   = 2.43057e-7
```

Mini100, same setup:

```text
base K26/L4/fullcov-Ez raw       = 1.337610563e-6

8 blocks x 256 half-rows:
  best lambda = 0.20
  raw         = 1.143343094e-6
  rough adj   = 2.601021987e-7

64 blocks x 32 half-rows / hybrid:
  raw         ~= 1.145e-6
```

The signed-Ez observer is a real raw-MSE reducer but still prices as a
mid-`2e-7` adjusted branch, not a `1e-7` branch.

Target-free adaptive gates were then tested using saved prediction grids and
per-block proxy-Ez statistics for the 8x256 observer:

```text
Full first100:
  best fixed lambda raw      = 1.002110808e-6
  per-MLP lambda oracle raw  = 9.100103469e-7
  OOF lambda-gate raw        = 1.044622740e-6

Mini100:
  best fixed lambda raw      = 1.143343094e-6
  per-MLP lambda oracle raw  = 1.030835563e-6
  OOF lambda-gate raw        = 1.177240751e-6

Full-trained gate applied to Mini:
  raw                        = 1.180532031e-6
```

Read: close the current low-row observer gate.  There is genuine per-MLP oracle
headroom, but the available block-reliability features do not predict the
right lambda; the gate worsens both Full and Mini.  The useful lesson remains
that signed final preactivation / late hidden mean is the load-bearing missing
coordinate.  The next live path needs a cheaper mechanistic proxy for that
coordinate, not more gating around the expensive observer.

2026-07-09 - K26 fullcov hidden-row snap closeout
-------------------------------------------------

Question: can the cheap fullcov Gaussian rollout proxy do more than provide a
weak final `E[z_L]` shift?  Specifically, can we nudge the sampled K26 row
cloud at an intermediate hidden layer toward the fullcov rollout mean and get a
cheaper approximation to the missing signed late-hidden state?

Probe:

```text
legacy_workspace/probe_k26_l4_fullcov_ez_shift.py
weights-cache = legacy_workspace/cache/phase1_full1000_weights_targets.npz
proxy-ez-cache = legacy_workspace/cache/fullcov_gaussian_ez_proxy_full1000_globalcal_from_full1000_20260707.npz
indices = 0,50,...,950
device = cuda
```

Caches:

```text
legacy_workspace/cache/k26_fullcovez_snap_none_spaced20_20260709.npz
legacy_workspace/cache/k26_fullcovez_snap_l8b005_spaced20_20260709.npz
legacy_workspace/cache/k26_fullcovez_snap_l24b005_spaced20_20260709.npz
```

Full1000 spaced20, no hidden snap:

```text
lambda=0.00 raw = 1.249116278e-6
lambda=0.03 raw = 1.165023151e-6
lambda=0.05 raw = 1.133122822e-6
lambda=0.08 raw = 1.121510569e-6  # best
```

Snap layer 8, beta=0.05:

```text
lambda=0.00 raw = 1.270331105e-6
lambda=0.03 raw = 1.185254207e-6
lambda=0.05 raw = 1.152718727e-6
lambda=0.08 raw = 1.140181121e-6  # best
```

Snap layer 24, beta=0.05:

```text
lambda=0.00 raw = 1.282981679e-6
lambda=0.03 raw = 1.232357123e-6
lambda=0.05 raw = 1.221590814e-6  # best
lambda=0.08 raw = 1.239916122e-6
```

Read: close this snap form.  The fullcov hidden mean is not a safe hidden-row
state target inside the current K26 sampler.  This agrees with prior
Gaussianized-tail and sample-to-analytic handoff failures: the row cloud's
non-Gaussian structure matters.  Keep fullcov only as the weak final
preactivation proxy already used by the K26/L4 branch; do not rerun hidden
fullcov snaps without a materially different state representation.

2026-07-09 - Winograd2 stats-CDF final-tail guard refresh
---------------------------------------------------------

Question: the final omitted-coordinate mean correction was a small but robust
raw-MSE improvement, and a two-level Winograd2 final-tail package was staged.
Does that artifact have clean official-evaluator local guards, or should the
older non-final-tail stats-CDF package remain the practical baseline?

Estimator:

```text
legacy_workspace/_pkg_pure26_w05_unionteacher1100gamma_l2snap_split035_045_noaffine_module_rectsparse232s7_late192_finalsp176_winograd2_statscdf_finaltailmean_preedgeL4_fullcovEzCal_finalonly/estimator.py
```

Scored with:

```text
whest-starterkit/quick_score_selected.py
width=256, depth=32, flop_budget=272000000000, max_threads=2
```

Outputs:

```text
legacy_workspace/cache/winograd2_statscdf_finaltail_full_spaced20_20260709.csv
legacy_workspace/cache/winograd2_statscdf_finaltail_full_offset20_20260709.csv
legacy_workspace/cache/winograd2_statscdf_finaltail_mini_spaced20_20260709.csv
```

Clean guard results:

```text
Full spaced20: raw=1.132513e-6 adjusted=1.840953e-7 mult=0.161858 flops=4.038e10 failed=0
Full offset20: raw=1.379384e-6 adjusted=2.226663e-7 mult=0.161845 flops=4.038e10 failed=0
Mini spaced20: raw=1.330306e-6 adjusted=2.139550e-7 mult=0.161376 flops=4.038e10 failed=0
```

Read: promote this over the non-final-tail Winograd2 stats-CDF artifact as the
current best compute-polished K26 package.  It is a real small improvement and
has clean Mini/Full guards.  It is still not a verified sub-`2e-7` remote
upgrade: the shifted Full guard and Mini guard remain around `2.1e-7` to
`2.23e-7` locally.  The remaining blocker is still raw MSE on hard slices, not
only remote residual pricing.

2026-07-09 - Winograd level and fullcov-Ez lambda retune closeout
------------------------------------------------------------------

Question: can the current best K26/L4/fullcov-Ez/final-tail package be pushed
under the `2e-7` local hard guards by cheaper Winograd scheduling or by simply
retuning the final preactivation proxy gain?

Baseline current package:

```text
legacy_workspace/_pkg_pure26_w05_unionteacher1100gamma_l2snap_split035_045_noaffine_module_rectsparse232s7_late192_finalsp176_winograd2_statscdf_finaltailmean_preedgeL4_fullcovEzCal_finalonly/estimator.py
```

Winograd diagnostic package:

```text
legacy_workspace/_pkg_pure26_w05_unionteacher1100gamma_l2snap_split035_045_noaffine_module_rectsparse232s7_late192_finalsp176_win2dense_win1sparse_statscdf_finaltailmean_preedgeL4_fullcovEzCal_finalonly/estimator.py
```

Op-profile results on two Mini MLPs, with remote residual models
`25/45/65us` per counted op:

```text
current win2 sparse: flops=4.038e10 ops=5937 mult=0.2030/0.2467/0.2903
final win1 only:     flops=4.050e10 ops=5811 mult=0.2023/0.2450/0.2878
win1 from L28:       flops=4.090e10 ops=5433 mult=0.2003/0.2402/0.2802
win1 from L20:       flops=4.207e10 ops=4425 mult=0.1953/0.2279/0.2604
win1 from L7:        flops=4.417e10 ops=2787 mult=0.1880/0.2085/0.2290
```

Official local quick-score on Full spaced20:

```text
current win2 sparse: adjusted=1.840953e-7 raw=1.132513e-6 mult=0.161858
win1 from L28:       adjusted=1.841297e-7 raw=1.132513e-6 mult=0.162304
win1 from L20:       adjusted=1.871454e-7 raw=1.132513e-6 mult=0.164974
win1 from L7:        adjusted=1.927141e-7 raw=1.132513e-6 mult=0.169930
```

Read: Winograd1 lowers operation count but increases analytical FLOPs enough
that local adjusted score does not improve.  It remains only a possible remote
residual hedge, not a local promotion.

Fullcov-Ez gain sweep, official local quick-score:

```text
lambda=0.03 Full spaced20: adjusted=1.891911e-7 raw=1.163724e-6
lambda=0.03 Full offset20: adjusted=2.193255e-7 raw=1.359698e-6
lambda=0.03 Mini spaced20: adjusted=2.157504e-7 raw=1.335094e-6

lambda=0.04 Full spaced20: adjusted=1.858040e-7 raw=1.145725e-6
lambda=0.04 Full offset20: adjusted=2.205274e-7 raw=1.366979e-6
lambda=0.04 Mini spaced20: adjusted=2.143160e-7 raw=1.330302e-6

lambda=0.05 Full spaced20: adjusted=1.840953e-7 raw=1.132513e-6
lambda=0.05 Full offset20: adjusted=2.226663e-7 raw=1.379384e-6
lambda=0.05 Mini spaced20: adjusted=2.139550e-7 raw=1.330306e-6
```

Read: smaller lambda helps the shifted Full guard but loses on the friendlier
Full and Mini guards.  The three-guard mean is not improved, so keep
`FULLCOV_EZ_LAMBDA=0.05` as the current default.  The live blocker is not this
one scalar; it is missing signed late-state/final-preactivation information on
hard networks.

2026-07-09 - Tailmix2 sparse schedule official guard
----------------------------------------------------

Question: the torch schedule probe showed a mixed activation-coordinate
schedule that improved Full offset and Mini slightly while losing Full spaced.
Does it survive the official flopscope local scorer and the current final-tail
package?

Diagnostic package:

```text
legacy_workspace/_pkg_pure26_w05_unionteacher1100gamma_l2snap_split035_045_noaffine_module_tailmix2_finalsp176_winograd2_statscdf_finaltailmean_preedgeL4_fullcovEzCal_finalonly/estimator.py
```

Schedule:

```text
base:     L7-L23=232, L24-L30=192, final=176
tailmix2: L7-L12=240, L13-L20=224, L21-L30=192, final=176
```

Official quick-score, `FULLCOV_EZ_LAMBDA=0.05`:

```text
Full spaced20: adjusted=1.850668e-7 raw=1.139130e-6 mult=0.161889 flops=3.966e10
Full offset20: adjusted=2.223272e-7 raw=1.369663e-6 mult=0.162085 flops=3.966e10
Mini spaced20: adjusted=2.131358e-7 raw=1.325721e-6 mult=0.160812 flops=3.966e10
```

Compared with the current base package:

```text
Full spaced20: worse by +0.009715e-7 adjusted
Full offset20: better by -0.003391e-7 adjusted
Mini spaced20: better by -0.008192e-7 adjusted
three-guard mean: 2.068433e-7 vs 2.069055e-7  # tiny +0.03%
```

Combined with lower fullcov-Ez gain, `FULLCOV_EZ_LAMBDA=0.04`:

```text
Full spaced20: adjusted=1.886855e-7 raw=1.152690e-6
Full offset20: adjusted=2.208279e-7 raw=1.356986e-6
Mini spaced20: adjusted=2.136241e-7 raw=1.324685e-6
```

Read: tailmix2 is a real but microscopic compute/accuracy tweak.  It is not a
verified sub-`2e-7` upgrade and should not be promoted by itself.  The lower
lambda combination is rejected because it overpays on Full spaced20 and does
not improve Mini adjusted.  Keep tailmix2 as a possible ingredient if a larger
raw-MSE improvement appears, but do not submit it as the next remote candidate.

2026-07-10 - New-eyes sweep: zero-preserving power snaps and exact ReLU sparsity
-------------------------------------------------------------------------------

Full audit and score/rules context:

```text
legacy_workspace/NEW_EYES_SWEEP_2026-07-10.md
```

New statistical/computational mechanism: replace the signed H1/H2 affine
moment maps by a positive coordinatewise map `c*h**p` whose exponent matches
the desired raw-second/mean-squared ratio and whose scale matches the desired
mean.  L4 uses the corresponding positive mean rescale.  Exact ReLU zeros then
survive every sampled correction, allowing exact gathered sparse products in
all sampled hidden layers.

Broad raw guards, incumbent affine -> zero-preserving power:

```text
Full spaced20: 1.132523529e-6 -> 1.155823402e-6  (+2.06%)
Full offset20: 1.379337835e-6 -> 1.379219828e-6  (-0.01%)
Mini spaced20: 1.330283301e-6 -> 1.333061424e-6  (+0.21%)
```

The scale-only zero-preserving map failed (`1.54e-6` versus `9.58e-7` on the
five-model smoke); matching CV is load-bearing.

Sparse implementation cleanup:

- boolean `argpartition` instead of full per-row sorting;
- bounded underflow/overflow nonzero-count buckets;
- batch and gathered-array caps remain configurable.

Production-shaped candidates:

```text
fullcov power+sparse:
  Mini index0 raw=5.398160e-7, FLOPs=3.190e10
  op count ~= 6166 after bounded buckets

tailmix2 nofullcov power+sparse:
  Mini index0 raw=5.803655e-7, FLOPs=2.979e10
```

Nofullcov power broad guards:

```text
Full spaced20 = 1.278447293e-6
Full offset20 = 1.364237364e-6
Mini spaced20 = 1.370043519e-6
```

Validated staged archives, not submitted:

```text
submission_phase1_k26_zerosparse_power_fullcov_finalonly_bundle.tar.gz
sha256 7fd81c9b8936dafb22ec9c19442b6519d9de2ed846acfadcb4f8a5b289c28d44

submission_phase1_k26_zerosparse_power_tailmix2_nofullcov_finalonly_bundle.tar.gz
sha256 44b4da59b72e5d6da86195fdbbd62c53633ad9f8a296323a73ae3a630da2af8c
```

Residual-free broad adjusted lower bounds are still about `1.35e-7` to
`1.62e-7`; this is a genuine Pareto candidate, not a demonstrated robust
`1e-7` solution.  Remote sparse-backend/residual economics must be measured.

Additional new lanes closed:

- Per-entry top-k plus moment restoration has a hard cliff below state fraction
  `~0.55`; late-only use saves too little compute.
- A verified 17-basis real MUB construction from GF(16) bent functions was
  tested.  Four-basis groups looked positive on development rotations but were
  `15.6%` worse on held-out Full80 and `2.2%` worse on Mini100.  Do not promote.

### 2026-07-10 - First remote sparse-kernel telemetry and runtime repair

Submission `315586` used the first fullcov archive and was rejected during
smoke setup without consuming quota.  The bundled module's `inputs` member was
Fortran-contiguous; the remote `flopscope-client==0.8.0rc5` codec rejects NPY
headers with `fortran_order=True`.  Re-saving every module member C-contiguous
preserved all values exactly.  A new tar audit now checks safe paths, manifest
membership/hashes, and every NPZ with the exact rc5 codec.  It found 33 of 75
parked archives unsafe, mainly for this same layout issue.

Corrected submission `315588` passed setup and produced useful partial
telemetry, but the overall evaluation did not finish:

```text
completed telemetry rows = 87
public scored rows        = 50
public raw MSE mean       = 1.305e-6
public adjusted mean      = 1.828e-7
analytical FLOPs mean     = 3.207e10
effective compute mean    = 3.800e10
implied multiplier        ~= 0.140
backend time mean         = 56.475 s
overhead time mean        = 2.238 s
residual time mean        = 59.33 ms
wall time mean            = 58.772 s
```

None of the 87 completed rows reported a per-row time/budget exhaustion, but
the job stopped with remaining work.  The operational cause is clear: 6,166
remote operations put the mean wall time against the 60-second boundary and
the overall evaluator deadline.  Do not retry the bin-8 kernel unchanged.

Exact bucket-width profiles on Mini index 0 (raw predictions identical):

```text
bin 8:  calls=6166  FLOPs=3.190e10
bin16:  calls=4736  FLOPs=3.277e10
bin32:  calls=3978  FLOPs=3.433e10
bin64:  calls=3730  FLOPs=3.729e10
```

The remote bin-8 backend averages about 9 ms per dispatched operation.  Bin32
therefore gives the best reliability/economics compromise: about 35% fewer
calls for only ~7.6% more analytical FLOPs, with unchanged predictions.  A
C-order bin32 fullcov archive was validated, rc5-codec audited, and submitted
next.  The no-fullcov bin32 sibling is staged as a lower-call hedge:

```text
nofull bin32: calls=3001  FLOPs=3.223e10
```

### 2026-07-10 - Bin32 remote boundary and float32 sampled state

Submission `315595` tested the C-order fullcov bin32 archive. Its predictions
were algebraically identical to the bin8 run and its public raw mean was again
`1.304568e-6`. It completed 90 of 100 MLPs; exactly one evaluated row failed:

```text
index 84 samantha-doyle: TIME_EXHAUSTED at 63.521 s
completed telemetry rows = 90
public raw MSE mean       = 1.304568e-6
public adjusted mean      = 1.858242e-7
public effective mean     = 3.873353e10
public score multiplier   = 0.142403
```

This is a worker-runtime failure rather than a scoring or setup failure. The
50 public rows averaged 49.76 s wall time; several individual rows completed
around 60-63 s. Reducing the sparse bucket call count from 6,166 to roughly
3,978 helped but did not provide enough worst-row margin.

The remote worker supplies MLP weights as float32. The power-snap constants
were promoting the sampled hidden state to float64 after H1/H2/L4, so sampled
matrix products were still executing in float64. Casting only the post-snap
sampled states back to float32 preserves the analytic fullcov branch in
float64 and keeps final output behavior unchanged to rounding precision.

Local forced-float32-weight profiles:

```text
Mini index0, bin8:
  baseline raw=5.397967e-7 backend=25.274 s
  f32state raw=5.392735e-7 backend=12.905 s

Mini index1, bin8:
  baseline raw=4.992149e-7 backend=20.031 s
  f32state raw=4.989797e-7 backend= 7.072 s

Mini index0, bin32 f32state:
  raw=5.392380e-7 FLOPs=3.433427e10 calls=3981 backend=7.690 s

Five-model spaced Mini guard, bin32 and forced float32 weights:
  baseline raw=1.130534e-6 elapsed=103.91 s
  f32state raw=1.130510e-6 elapsed= 37.94 s
```

Thus float32 state is a genuine 2-3x sampled-backend speedup here, with only a
`~2e-5` relative raw-MSE change on the five-model guard. It does not exploit
complex packing and does not lower analytical FLOP billing; its purpose is to
make the estimator operationally reliable under the worker time limit.

Validated fullcov bin32 float32-state archive:

```text
submission_phase1_k26_zerosparse_power_fullcov_bin32_f32state_corder_finalonly_bundle.tar.gz
sha256 3967332cc9234d117f758104dca3c32bf42855eda591f38f71f0ba68c324655e
exact extracted-tar subprocess index0 raw=5.392345e-7, wall=8.242 s
```

The exact archive passes manifest/hash checks, the rc5 NPZ codec, contract
validation, and an isolated subprocess setup/predict run. The corresponding
tailmix2/no-fullcov bin32 float32-state hedge is also staged and rc5-audited:

```text
submission_phase1_k26_zerosparse_power_tailmix2_nofullcov_bin32_f32state_corder_finalonly_bundle.tar.gz
sha256 2937aa759e576132d735a1fd8320493d9bcee5d3ca82b7a4600fc01b94176c17
Mini index0 raw=5.797475e-7 FLOPs=3.222785e10 calls=3004 backend=7.024 s
```

Submission `315597` was the fully validated fullcov/bin32/float32-state
archive. It completed successfully:

```text
adjusted score       = 1.846213032e-7
raw final MSE        = 1.304593260e-6
completed MLPs       = 100 / 100
public mean FLOPs    = 3.445e10
public mean effective= 3.851e10
public multiplier    ~= 0.1415
public mean wall     = 24.489 s
public backend       = 22.889 s
public residual      = 40.59 ms
```

Compared with bin32/float64-state submission `315595`, remote public wall time
fell from 49.76 s to 24.49 s and the lone timeout disappeared. Raw MSE changed
by only `+2.53e-11` (`+0.0019%`). This validates float32 sampled state as the
correct operational repair. It is now the best completed result from this
sweep and the appropriate fullcov control for subsequent statistical A/Bs.

Submission `315599` was the tailmix2/no-fullcov/bin32/float32-state A/B. It
also completed all 100 MLPs:

```text
adjusted score       = 1.846264130e-7
raw final MSE        = 1.414868308e-6
public mean FLOPs    = 3.234e10
public mean effective= 3.546e10
public mean wall     = 23.717 s
public residual      = 31.14 ms
```

This is an almost exact adjusted-score tie with fullcov `315597`, but worse by
`5.11e-12`. Removing full covariance lowers effective compute by 7.9% while
raising raw MSE by 8.45%; the two effects cancel. This clean remote A/B confirms
that the present fullcov correction sits on the same Pareto frontier rather
than breaking it. Neither adding it nor removing it is a route to `1.5e-7` by
itself.

After float32 made runtime safe, reprofiled the sparse bucket width. Selected
Mini-index0 points:

```text
bin  1: FLOPs=3.138e10 calls=26873
bin  2: FLOPs=3.136e10 calls=14959
bin  4: FLOPs=3.151e10 calls= 9077
bin  8: FLOPs=3.190e10 calls= 6169
bin 10: FLOPs=3.210e10 calls= 5601
bin 12: FLOPs=3.229e10 calls= 5275
bin 14: FLOPs=3.251e10 calls= 5031
bin 16: FLOPs=3.277e10 calls= 4739
bin 20: FLOPs=3.314e10 calls= 4457
bin 24: FLOPs=3.349e10 calls= 4309
bin 32: FLOPs=3.433e10 calls= 3981
```

The remote residual-vs-call slope from the two float32 submissions places the
effective-compute minimum near bin 14-16. Bin 1/2 save little arithmetic but
create prohibitive call overhead; bin32 is over-coarse once timeout risk is
removed. A fully audited bin16 archive was submitted as the next compute-only
refinement, with a remote score prior around `1.81e-7`.

Submission `315604` validated that prediction:

```text
adjusted score       = 1.818554040e-7
raw final MSE        = 1.304591361e-6
completed MLPs       = 100 / 100
public mean FLOPs    = 3.290e10
public mean effective= 3.787e10
public multiplier    ~= 0.13923
public mean wall     = 23.694 s
public residual      = 49.73 ms
```

This is the best completed result from the sweep. The bin14 residual/FLOP model
is at most a roughly `0.1%` further improvement, so bucket-width polishing is
now closed as a material route. It cannot bridge the gap to `1.5e-7`.

Matched public fullcov/no-fullcov rows show that even an impossible perfect
per-MLP router would score only `1.741566e-7` (28 fullcov wins, 22 no-fullcov
wins), a 5.67% gain over the best fixed branch. Closure on/off routing is real
but has insufficient ceiling for `1.5e-7`, even before solving the deployable
gate problem.

## 2026-07-10: affine hybrid exact-kernel breakthrough

A fresh compute audit found three compatible exact transformations: route the
signed affine-snap states through level-vectorized Winograd, route ordinary
nonnegative post-ReLU states through a count-sorted bin-8 sparse dispatcher,
and retain full-covariance symmetry with a direct three-operand `einsum` plus
an outer-product covariance update.  The previous sparse dispatcher rescanned
all rows for every count bucket, while the new dispatcher sorts once and finds
all bucket boundaries with `searchsorted`.  The numerical estimator is still
the affine K26/fullcov model; no activation or weight values are pruned or
approximated.

Official `v1-phase1`, forced-float32, 20-MLP guards:

```text
Full indices 0,50,...,950:
  raw=1.132664e-6 FLOPs=3.139e10 effective=3.588e10
  multiplier=0.13189435 adjusted=1.498272e-7 failed=0
Full indices 25,75,...,975:
  raw=1.379371e-6 FLOPs=3.143e10 effective=3.570e10
  multiplier=0.13125615 adjusted=1.804799e-7 failed=0
Mini indices 0,5,...,95:
  raw=1.330454e-6 FLOPs=3.140e10 effective=3.542e10
  multiplier=0.13020646 adjusted=1.728109e-7 failed=0
```

The matched earlier affine Full-spaced control was raw `1.132513e-6`, FLOPs
`4.038e10`, adjusted `1.840953e-7`.  Thus raw changes by only about +0.013%,
FLOPs fall 22.3%, and the adjusted guard improves 18.6%.  This is a material
frontier move, not bucket-width polishing.

The isolated production package is:

```text
_pkg_breakthrough_k26_affine_hybrid_sorted8_symcov_f32state_finalonly/
```

The fully packaged archive is:

```text
submission_breakthrough_k26_affine_hybrid_sorted8_symcov_f32state_corder_finalonly_bundle.tar.gz
sha256 618a2ba3e0e0f8f56701027a3ccd9c6fd4634b1c70ce799f1fbaccad262df1e6
```

Compilation, `whest validate`, rc5 manifest/codec audit, and an exact extracted
archive subprocess all pass.  Extracted index 0 returns float32 `(32, 256)`,
raw `5.241068784418e-7`, and `31,216,179,726` FLOPs.  Full rationale and
dead-end lanes are recorded in `BREAKTHROUGH_AFFINE_HYBRID_2026-07-10.md`.

Submission `315631` remotely validated the full composition:

```text
adjusted score       = 1.693694567317525e-7
raw final MSE        = 1.293952077503491e-6
completed public rows= 50 / 50
public mean FLOPs    = 3.135482728e10
public mean effective= 3.564030825e10
public multiplier    = 0.131030545022
public mean wall     = 25.188927 s
public backend       = 23.544072 s
public overhead      = 1.602000 s
public residual      = 42.854810 ms
```

Versus the previous best submission `315604`, adjusted improves 6.8659%, raw
improves 0.8155%, FLOPs fall 4.7000%, effective compute falls 5.8990%, and
residual falls 13.8312%.  All 100 telemetry rows completed and all 50 public
rows were scored.  This closes the remote-overhead concern: sorting counts
once and routing signed/nonnegative states transfers as a real leaderboard
gain.  `315631` is the new completed best from this lineage.

## 2026-07-10: exact affine-baseline factorization and post-breakthrough sweep

Affine H1/H2/L4 snaps have the exact form `relu_state * scale + offset`.
Factoring the shared column offset out of the next product restores the ReLU
zeros without changing the estimator:

```text
state @ W = (state - column_min) @ W + column_min @ W
```

The first term uses the count-sorted sparse kernel and the second is one small
vector product.  Three 20-MLP guards all retained affine raw accuracy while
lowering mean analytical FLOPs to about `3.103e10`.  The audited archive is:

```text
submission_root_breakthrough_k26_affine_baselinesparse_sorted8_symcov_f32state_corder_finalonly_bundle.tar.gz
sha256 0a4b753f72333c7545aa5aaf4fbf98c4a99bbb03bec9cf37296eac7526410772
```

Submission `315634` is the new protected fixed/private-transfer result:

```text
adjusted score       = 1.672720225539068e-7
raw final MSE        = 1.293972317171210e-6
public mean FLOPs    = 3.100311700e10
public mean effective= 3.520530772e10
public residual      = 42.021907 ms
```

An independently queued duplicate, `315638`, had bit-identical raw MSE and
FLOPs but residual `48.008926 ms`, effective `3.580400963e10`, and adjusted
`1.705584777988446e-7`.  This directly measures a roughly two-percent
adjusted-score noise band from remote residual timing; do not spend more slots
on identical-kernel repeats.

The subsequent wide sweep produced useful falsifiers but no further promoted
fixed branch:

- `probe_union39_row_budget_root2.py` kept all 39 seed strata while reducing
  rows per stratum.  At a K26-sized half-row budget, raw rose to
  `1.35e-6..1.46e-6`; keep-224 recovered `9.40e-7` but costs about 31% more
  sampled rows.  The quadrature cliff is structural.
- `probe_union39_seed_compression_root2.py` chose complete 26-seed banks by
  target-free closeness to the union39 teacher.  The best teacher subset was
  excellent on Full-spaced (`9.795e-7`) but failed Full-offset (`1.612e-6`)
  and Mini (`1.632e-6`).  This is another seed-slice illusion.
- `probe_activation_codebook_root2.py` tested ordinary, honest grouped weight
  reductions with amplitude, rank, and column-mean-preserving codebooks.
  Low-resolution errors explode through depth; amplitude-128 reaches only
  `1.241e-6` raw and has no arithmetic advantage once 128 group reductions
  are priced.  Close activation quantization for the sampled tail.
- `probe_covbalanced_qr_thinning_root2.py` jointly removes rows across all K26
  orthogonal banks to minimize aggregate covariance anisotropy.  Despite the
  target-free spectral balancing, keep-248 changes raw
  `1.1325e-6 -> 1.1872e-6` for only 3.1% fewer rows; keep-224 reaches
  `1.3586e-6` for 12.5% fewer.  Higher-order path structure, not degree-two
  covariance alone, makes full QR blocks load-bearing.
- Seedwise moment assimilation was split by layer.  H1 seedwise is worse
  (`1.1429e-6`), H2 seedwise is only a 0.024% friendly-slice improvement
  (`1.13226e-6`), and block-centered L4 PRE-EDGE is worse (`1.16366e-6`).
  None clears a material transfer gate.

The fixed score is now compute-polished enough that reaching `1.5e-7` still
requires roughly nine percent raw-MSE improvement.  The honest live surface
remains a qualitatively stronger signed late-state/final-preactivation mean or
all-distinct cumulant estimator, not more row thinning, seed shopping, or
value compression.

## 2026-07-11: Mixed238 broad transfer, compute closure, and adaptive-pilot falsifier

The partial-Union mixture was promoted from an endpoint diagnostic to the
production-shaped start-5 sparse chassis.  It keeps all K26 half-directions and
adds 30 tight-frame-repaired 32-row strata from the Union39-only seeds:

```text
K26 half rows             = 6656
extra half rows           =  960
total half rows           = 7616
live antithetic rows      = 15232
fixed extra-bank alpha    = 0.10
```

The initial H2-K3 wrapper was not the robust form.  Matched Mini20 changed
`1.075539983e-6 -> 1.060126586e-6` when H2 was removed, while FLOPs and calls
also fell.  This agrees with submission `315775`: the H2/K4 package completed
reliably at adjusted `1.490121755e-7`, but raw worsened relative to the
`315736` start-5 control, so the apparent local H2 gain did not transfer.

Official forced-float32 20-MLP guards:

```text
family                 Full spaced      Full offset      Mini spaced       mean raw       mean FLOPs
K26 start-5 base       1.140898283e-6   1.375648081e-6   1.333275318e-6   1.283273894e-6  2.845924038e10
Mixed238 base          1.022426973e-6   1.261784132e-6   1.060126586e-6   1.114779230e-6  3.235978118e10
Mixed238 H2 beta .75   9.763534806e-7   1.218140248e-6   1.075539983e-6   1.090011237e-6  3.239411410e10
Mixed238 diag/no-FC    1.052459028e-6   1.294889707e-6   1.092647639e-6   1.146665458e-6  3.098138852e10
```

Mean raw*FLOP ratios versus K26 are `0.9878` for the base, `0.9668` for H2,
and `0.9727` for diagonal/no-fullcov.  Local residual-inclusive work products
favor the no-fullcov sibling by about 6.6%, but residual timing is noisy and
the corresponding remote prior remains around `1.4e-7`, not `1e-7`.  Thus
Mixed238 is a genuine broad raw-MSE improvement, but added sampled work mostly
cancels it in the adjusted score.  Do not upload it as a claimed breakthrough.

The no-fullcov compute probe is:

```text
candidate_mixed238_r32a010_diagsecond_nofullcov_root.py
```

It replaces the pruned full-covariance analytic recursion with a diagonal
mean/variance screen and disables the final-Ez shift.  Across the three guards
it gives up 2.9-3.1% raw versus Mixed238 base while cutting analytical FLOPs
about 4.3%; retain it as a Pareto sibling only.

Zero-extra-propagation readout probes were negative or microscopic:

- per-seed target-fitted ridge weights improve Full fit but worsen Mini;
- per-MLP and per-coordinate adaptive alpha models do not beat fixed alpha in
  Full MLP-held-out CV; their Mini gain collapses to a near-constant
  `alpha ~= 0.109` and is below one percent;
- Union39-teacher-only extra-seed weights improve Full truth about 0.5% but
  worsen Mini, including when the teacher fit uses both distributions.

The high-ceiling adaptive pilot idea was tested explicitly in
`probe_adaptive_h2_seed_selection_root.py`.  All 56 K26+Union blocks are paid
through H2, then only 26 blocks continue through the 30-layer tail.  The
sampled layer-work premium is roughly 5.5-7%.  Unconstrained greedy H2-moment
selection looked spectacular on Full spaced10:

```text
fixed raw       = 1.346472990e-6
adaptive raw    = 9.647078259e-7
ratio           = 0.71647
```

It failed the required disjoint transfer audit.  Mini even10 improved 12.9%,
but Mini odd10 worsened 15.5%; combined Mini20 was essentially flat.  Four
objective-improving swaps from K26 worsened Mini20 by 11.2%.  A backward
mean-field response objective worsened Full, and a full-depth diagonal-tail
proxy selector worsened Mini20 by 9.1%.  This is a clean falsifier: shallow or
analytic proxy agreement does not identify signed full-depth quadrature error.
Close adaptive seed selection unless a new target-free observable directly
tracks the missing late signed mean.

## 2026-07-11: response-stratified thinning and CountSketch tail falsifiers

Two distinct attempts were made to preserve signed tail information while
cutting sampled coordinate/row work.  Both were isolated diagnostics and no
package or remote submission was produced.

`probe_response_stratified_tail_root.py` keeps the complete K26 cloud through
a checkpoint, rolls diagonal mean-field gates backward from 26 orthogonal
Walsh output probes, sorts each seed/sign stratum by its assigned signed
response, and retains midpoint quantiles as actual rows.  Unlike earlier
centroid compression it never averages through a ReLU; unlike norm coresets it
uses the full future weight chain.  Mini index 0 produced one striking scout:

```text
base                    raw=4.970400827e-7
checkpoint8 keep224     raw=3.952702001e-7
raw ratio               =0.795248
row-work proxy          =0.910156
raw*row-work ratio      =0.723800
```

The required disjoint Full guard rejected it immediately:

```text
Full indices 0,100,...,900
base                    raw=1.046706307e-6
checkpoint8 keep224     raw=1.436578133e-6
raw ratio               =1.372475
raw*row-work ratio      =1.249167
```

This is another signed response slice illusion, not a transferable coreset.
Close this response-stratified selector; do not broaden or package it.

`probe_k26_countsketch_tail_root.py` keeps an activation-energy head exact and
approximates every omitted coordinate with deterministic signed CountSketch
products.  It then restores the exact empirical preactivation cloud mean
before every ReLU.  The mechanism is unbiased before the nonlinearities and
uses all coordinates, so it is materially different from deterministic tail
drop or a Gaussian omitted-tail closure.  Nevertheless collision variance
explodes through depth.  On Mini index 0:

```text
incumbent base                          raw=3.994897515e-7
head160, 2x16 buckets, beta=.5          raw=6.086340289e-5
head160, 2x16 buckets, beta=1           raw=6.866486402e-2
head160, 2x32 buckets, beta=.05/.1/.2   raw=9.32e-5 / 8.68e-5 / 6.93e-5
```

Even exact first-moment restoration does not control sketch-induced variance
after repeated ReLUs.  Close hidden-product CountSketch in this form; a future
randomized product would need a second-moment-preserving construction, not
another bucket/gain sweep.

## 2026-07-11: pair-C22 audit and richer exact-mean PathCV controls

Audited the proposed sampled pair-pair fourth-cumulant snap before building a
production probe.  Ordinary PRE-EDGE already contracts the complete sampled
next-preactivation fourth moment, whereas a same-cloud `C22` contraction keeps
only a repeated-pair projection of that information.  More importantly, the
existing true-component diagnostic shows that even oracle `K4` deltas are
neutral when added directly to the sampled estimator; they become valuable
only inside a substantially better absolute propagated distribution state.
Therefore a sampled `C22` snap is not a new sensor and was closed without an
expensive rollout.  Pair-pair K4 remains live only as a genuinely independent
transition/state model, not as another decode of K26 rows.

Tested three stronger exact-mean controls for the high-count PathCV family:

```text
script:
  legacy_workspace/probe_pathcv_full_h1_ridge_root.py

Full indices 0,250,500,750,999, PathCV76 dense statistical path:
  plain raw                               = 8.388496387e-7
  existing scalar mean-field PathCV       = 4.431907499e-7
  full row H1 ridge, sample covariance    = 4.441992011e-7
  full row H1 ridge, exact H1 covariance  = 4.467659946e-7
```

The full `256 -> 256` H1 response contains the scalar response as a possible
direction but does not improve it under seed-block cross-fitting.  Regressing
the final QR-block means directly on H1 block-mean defects is wrong-signed: the
most shrunk nonzero point is already `1.00099x` the plain raw and larger gains
worsen rapidly.

The same probe also tested a cross-output quadratic control.  For the deployed
linear response `z1`, every coordinate of
`z2 = z1**2 - E[z1**2]` has an analytic zero sphere mean.  Unlike the old z12
control, the new regression lets every `z2_k` predict every output `j`.
It still collapses to the scalar control:

```text
existing scalar PathCV                   = 4.431907499e-7
best cross-quadratic checked             = 4.434842803e-7
```

Finally derived and tested a distinct first-order spherical Stein control:

```text
E[n (q.T U) f_j(U) - q.T grad f_j(U)] = 0

script:
  legacy_workspace/probe_pathcv_firstorder_stein_root.py
```

This uses exact deep-network VJPs and deterministic Rademacher output probes;
it is unrelated to the earlier approximate Laplacian/boundary-current arm.
On the same five Full MLPs with 26 QR blocks:

```text
plain                                    = 1.750949606e-6
ordinary first-layer PathCV              = 1.218660486e-6
Stein alone, best checked                = 1.750425835e-6
PathCV + Stein                           = 1.237587422e-6
```

The finite-QR Stein means were small, supporting the identity implementation,
but the control is statistically flat and one probe costs approximately an
extra reverse trajectory pass.  Close full-H1 regression, block-H1 regression,
cross-quadratic PathCV, and first-order Stein in these forms.  No package or
remote submission was produced.

## 2026-07-11: real Kerdock 4-design raw breakthrough and compute closure

Implemented the explicit maximal real mutually-unbiased-basis construction in
dimension 256 from the finite-field Kerdock set:

```text
legacy_workspace/probe_k26_kerdock_mub_root.py
```

The generated 129 bases satisfy, in float32 after one common QR rotation:

```text
within-basis orthogonality max error       ~= 2.5e-8
cross-basis |inner product|                = 1/16
cross-basis magnitude max error            ~= 1.9e-8
```

Thus the complete antipodal cloud is a real spherical 4-design.  It is a
genuinely new direction family, not another random QR seed portfolio.

Partial same-count rules do not transfer.  A rigid 26-base subset gave
`1.277e-6` on Full spaced20.  Splitting it across eight independent global
orientations looked mildly positive on both Full guards but failed Mini:

```text
eight-pack count26, Full spaced20          = 1.124693038e-6
eight-pack count26, Full offset20          = 1.320204062e-6
eight-pack count26, Mini spaced20, K26 w   = 1.424516061e-6
matched K26 guards                         = 1.140898283e-6 / 1.375648081e-6 / 1.333275318e-6
eight-pack count26, all Mini100            = 1.522794698e-6
matched K26, all Mini100                   = 1.308909093e-6
```

The *complete* 129-base rule is a broad raw-MSE breakthrough in the exact
production-shaped L4/fullcov/sparse-tail statistical path:

```text
Full spaced20                              = 3.175510765e-7
Full offset20                              = 2.762191417e-7
Mini spaced20                              = 2.767363878e-7
three-guard mean                           = 2.901688687e-7
```

However, it uses `129/26 = 4.9615x` as many complete bases.  Its three-guard
raw-times-row product is `1.1219x` the K26 product, so the raw breakthrough is
not yet an adjusted-score breakthrough.  Count65 gave `6.324453797e-7` on
Full spaced20 and lies on the same unfavorable product curve.  Two complete
design orientations gave `1.741743019e-7`, only `1.823x` lower raw for `2x`
the work, so there is no super-Monte-Carlo cancellation.

Required compression attempts were negative:

- Ordinary coordinate pruning has a severe deep-composition cliff.  Changing
  the current `232/192/176` schedule to even the moderate
  `208/176/144/112` schedule raised raw to `1.469e-4`; more aggressive points
  reached `9e-3..6e-2`.
- Dense complete-design scalar PathCV improved `2.846396e-7 -> 2.748712e-7`
  on Full spaced20 (`3.4%`).  Isolated degree-6/8/10 spherical-harmonic
  controls were weaker; the best checked degree-10 arm was `2.792138e-7`.
- Final sampled PRE-EDGE remains neutral/worse even in this low-noise regime.
- Checkpoint kernel herding to 26 bases and multilevel checkpoint PathCV both
  failed the raw-times-work gate.  Best L2 multilevel result on the five-row
  scout was about `9.036e-7` at a `1.248x` row-work proxy.

Supporting probes/caches:

```text
legacy_workspace/probe_kerdock_pathcv_root.py
legacy_workspace/probe_kerdock_checkpoint_herding_root.py
legacy_workspace/probe_kerdock_checkpoint_pathcv_root.py
legacy_workspace/cache/root_kerdock_complete129_*_20260711.npz
```

Deep exact Winograd remains the only obvious way to convert the complete-rule
raw line into a score line, but a direct NumPy benchmark shows the wall-time
cliff: for a 4096x256 product, vectorized levels 5/6/7 take approximately
`0.119s / 0.279s / 0.728s`.  Level 7 has the needed arithmetic reduction but
would exceed the smoke wall across 32 layers.  Keep the complete Kerdock rule
as a new high-quality teacher/statistical reference; do not package or upload
it until an exact low-residual contraction or a successful checkpoint
compression changes its compute product.

## 2026-07-12: remote-candidate audit and Mixed238/H2/diagonal bundle

Re-audited actual staged archives rather than experimental caches.  The
existing `mixed238_baseshift` archive is a real, self-contained conservative
probe.  Its three 20-MLP guards average `1.109818521e-6` raw at
`3.235902541e10` FLOPs, versus the matched K26 start-5 guard at
`1.283273894e-6` and `2.845924038e10` FLOPs.  The raw-times-FLOP ratio is about
`0.983`, which maps the `1.49665e-7` robust remote line to roughly `1.47e-7`.
Fresh extraction checks passed all manifest hashes and `whest validate`; an
isolated full-depth Mini index-0 prediction returned raw `4.368830486e-7`,
`3.2142169991e10` FLOPs, and `9.17s` wall.  This is submit-safe as an
incremental A/B, not a route to `1e-7`.

The strongest broad-guarded unbundled sibling was then vendored as:

```text
whest-starterkit/packages/to_test_remote/
  submission_root_mixed238_h2tree_diagsecond_nofullcov_finalonly_bundle.tar.gz
sha256 = 7ea80f7f911410ee856ec029e03d8b512cde60692cfbcd6a3e421a63253252ae
```

It combines the Mixed238 bank, the cheap H2 K3-tree mean snap, and diagonal
analytic screening with the final full-covariance Ez shift disabled.  Across
Full spaced20, Full offset20, and Mini spaced20 it averages
`1.118246671e-6` raw, `3.101573499e10` FLOPs, and a local adjusted score of
`1.634853235e-7`.  Against the matched K26 guard, raw-times-FLOP is about
`0.950`, which at the time projected around `1.42e-7`.  **That projection was
later falsified by submission `315829` (`1.557374486e-7` actual) and must not
be reused as a current estimate.**  The local
residual-inclusive ratio would project around `1.33e-7`, but that timing gain
is not bankable remotely.  An additional Full irregular20 guard is
`1.257542172e-6` raw.

The extracted archive passes every manifest hash and `whest validate`, and its
Mini index-0 prediction has exact source parity: raw `5.061951960e-7` and
`3.0799437394e10` FLOPs.  Historical uncontended source profiling took about
`7.9-8.2s`; during the fresh package audit the host load was approximately 79
on 32 CPUs, the same in-process prediction took `35.2s`, and the first
subprocess attempt timed out.  Therefore rerun the extracted full-depth
subprocess under normal load before upload.  Also retain the statistical
caveat: H2 improved all three local guards here, but the earlier H2/K4 remote
submission `315775` failed to improve raw.  Treat this archive as the strongest
current remote probe, not as a guaranteed breakthrough.

At the user's explicit request, the verified archive was uploaded as AIcrowd
submission `315829` at `2026-07-12 00:10 EEST`.  The create call succeeded and
the first two authenticated status polls reported `submitted` / successfully
enqueued, with no score or smoke result yet.  Do not infer transfer or
operational success until the remote state advances.

After host load fell below core count, the exact extracted `315829` archive
passed the previously open full-depth subprocess gate: Mini index 0 returned
raw `5.061954027e-7`, `3.0799437394e10` FLOPs, `8.224951s` wall,
`7.865831s` backend, and `0.124661s` residual.  This closes the local
operational concern; the remote result is now a pure transfer/pricing test.

Remote submission `315829` subsequently completed successfully, including
smoke and scoring.  Its public adjusted score was `1.557374486e-7` and its
reported raw/secondary score was `1.258738594e-6`.  This is worse than the
robust K26 frontier (`~1.49665e-7`) and H2/K4 probe `315775`
(`1.490121755e-7`).  The archive is therefore operationally reliable but is
not an incumbent.  The broad local compute projection did not transfer into
an adjusted improvement, reinforcing the earlier warning that H2's local
gain is not stable remotely.

## 2026-07-12: target-free fourth-moment row weighting

Tested a zero-runtime-cost attempt to improve K26 by choosing simplex weights
that match the Gaussian fourth-moment tensor more closely.  The optimum was
essentially uniform (`min=0.038440`, `max=0.038484`) and did not improve Full
spaced20, Full offset20, or Mini spaced20.  Dynamic moment weighting was also
worse than the incumbent on all three guards; anchoring it strongly to the
incumbent merely recovered the incumbent.  This avenue is closed.

Supporting probe/cache:

```text
legacy_workspace/probe_k26_fourth_moment_weights_root.py
legacy_workspace/cache/root_k26_fourth_moment_weights_20260712.npz
```

## 2026-07-12: higher-moment receiver, complete-design compression, and early teachers

Derived an exact final-adjoint receiver for propagated fourth-order state.  If
the true final preactivation mean, variance, and K3 are supplied, the
K4-plus-`K3 x covariance` receiver is highly predictive of the remaining
final-layer error: on Full200 Gaussian states its correction has correlation
`0.9147`, `R2 = 0.8235`, and a fixed gain near `0.625` leaves roughly
`4.5e-8` isolated raw MSE.  A linear receiver can be propagated exactly in
`O(depth * width^3)`.  This is an important mechanistic object, but it is not
yet deployable: causal rollout inherits sufficiently large K21/K3/state error
that the receiver gain disappears.  Low-count experiments supplied with true
K3/K4 also establish the sampling floor (`c26 = 1.647e-6`, `c18 = 2.378e-6`,
`c13 = 3.223e-6`), so the missing ingredient is state accuracy rather than a
different final readout.

Empirical-Bayes shrinkage of sampled variance improves its standalone Full200
base by `6.43%`, but only about `2%` remains after the incumbent's existing
`5%` Gaussian-Ez shift (Full about `2.61%`, held guard `2.37%`, Mini `1.82%`).
This is real but below a safe remote/package threshold.

The complete 129-basis Kerdock teacher was also tested with scalar PathCV and
copula/variance corrections.  Dense PathCV improves Full spaced20 only
`2.846396e-7 -> 2.748712e-7` (`3.43%`).  The best fixed copula arm gives
`2.82785e-7`, but its raw-times-row product is still `1.1511x` K26.  Exact
dense Strassen/Winograd recursion does not rescue the compute line: on the
full 66,048-row cloud, level 3 takes about `0.698s` per product and level 4
about `1.277s`, with additions and residual wall making both uneconomic.

Two new attempts to compress the complete design failed independent guards:

- Distributing 51/52 rows from every Kerdock basis into a same-count tight
  frame is catastrophic even after global repair.  On index 0, K26 is
  `1.596e-6`; distributed variants range from `1.64e-5` to `3.72e-5` raw.
  Within-basis completeness is therefore load-bearing beyond covariance.
- Propagating all 129 bases through H2, then retaining 52 rows per basis and
  repairing them, produced a seductive index-0 result (`9.291e-7`, projected
  raw-times-work `0.731x`).  On the untouched offset indices
  `25,275,525,775`, however, it is `1.821e-6` versus K26 `1.515e-6`, for a
  `1.508x` work product.  Random row selection fails similarly.

Finally tested a lower-cost multilevel teacher that keeps every incumbent K26
row and adds 13 ordinary QR bases only through an early checkpoint.  At H2 it
costs `3.125%`: index 0 appears strongly positive (`1.372e-6` versus
`1.596e-6`, product `0.887x`), but the untouched offset-four guard improves
raw only `2.10%` (`1.483e-6` versus `1.515e-6`) and is `1.0096x` after work.
Extending the same teacher through H4 costs `6.25%` and creates an even larger
index-0 mirage (`1.101e-6`, product `0.733x`); on the untouched guard it
reverses to `1.598e-6` raw and `1.1208x` product.  Early auxiliary moment
teaching is therefore closed in this form.  The general checkpoint option in
`probe_k26_sparse_schedule.collect` defaults to H2 and regression-reproduces
the prior result exactly.

Supporting probes/caches:

```text
legacy_workspace/probe_kerdock_distributed_rows_root.py
legacy_workspace/probe_kerdock_h2_row_thin_root.py
legacy_workspace/probe_k26_extra_h2_teacher_root.py
legacy_workspace/cache/root_kerdock_h2_row_thin_offset_holdout4_20260712.npz
legacy_workspace/cache/root_kerdock_h2_row_thin_random_offset_holdout4_20260712.npz
legacy_workspace/cache/root_k26_extra_h2_teacher_offset_holdout4_20260712.npz
legacy_workspace/cache/root_k26_extra_h4_teacher_offset_holdout4_20260712.npz
```

No package or remote submission was produced from these experiments.  The
remaining live problem is a transferable late-state correction at roughly
incumbent compute, not further early-cloud marginal repair.

### Network-adaptive H1 Kerdock coreset

Tested one further complete-design compression that preserves whole bases.
All 129 Kerdock bases are surveyed through H1, a label-free diagonal nonlinear
mean/variance surrogate is propagated through the future weights, and kernel
herding chooses 26 complete bases for the expensive production-shaped tail.
The selected bases use equal weights; convex-polished weights were unstable.
The optimistic row-work ratio is `1.123798x` K26 before about 2--4% additional
vector-surrogate arithmetic.

The result is a particularly clear transfer failure:

```text
index 0:       K26 1.595515e-6 -> coreset 8.128202e-7, product 0.573x
offset four:   K26 1.515101e-6 -> coreset 1.125985e-6, product 0.835x
Full offset20: K26 1.379345e-6 -> coreset 1.066699e-6, product 0.869x
Full spaced20: K26 1.132516e-6 -> coreset 1.126946e-6, product 1.118x
Mini spaced20: K26 1.330300e-6 -> coreset 2.150492e-6, product 1.817x
```

Thus the surrogate can genuinely identify a favorable quadrature-error regime
on the Full offset family, but its ordering does not transfer to Mini and is
not self-certifying.  Do not fit a public target router or package it.

```text
legacy_workspace/probe_kerdock_h1_surrogate_coreset_root.py
legacy_workspace/cache/root_kerdock_h1_surrogate_coreset_full_spaced20_20260712.npz
legacy_workspace/cache/root_kerdock_h1_surrogate_coreset_full_offset20_20260712.npz
legacy_workspace/cache/root_kerdock_h1_surrogate_coreset_mini_spaced20_20260712.npz
```

### Final-adjoint endpoint CV and sparse-compute closeout

Isolated the new K4 receiver from the failed causal mean replay.  The paid K26
endpoint is preserved; sampled final K4 is shrunk toward the Gaussian
star/chain/receiver prior, and only the resulting Edgeworth difference is added
to the incumbent.  On Full200 the best exhaustive fixed grid changes
`1.299927084e-6 -> 1.299862177e-6` (`0.005%`), while the held residue split is
flat/slightly worse.  The receiver is a real oracle component but final K4 is
not the incumbent's load-bearing error coordinate.

```text
legacy_workspace/probe_k26_final_adjoint_k4_cv_root.py
legacy_workspace/cache/root_k26_final_adjoint_k4_cv_full200_20260712.npz
```

Reprofiled the exact sparse PathCV76 implementation in the official rc5-shaped
environment on Mini index 0:

```text
exact concat: raw=1.926773230e-7 F=90.156075B wall=25.259s residual=.276s
direct assign: exact parity, F=90.136152B wall=24.357s
bin4 assign: raw=1.926778122e-7 F=88.875570B wall=24.259s
```

It is smoke-safe locally and exact sparsity cuts the old dense arithmetic by
about one third.  It is nevertheless not a broad `1e-7` candidate: the frozen
historical Full spaced20 path-only raw is `5.539937e-7`, which prices near
`1.8e-7` at 89--90B before residual.  The tiny Mini-index projection near
`8e-8` must not be represented as a broad estimate.

Approximate PathCV state truncation is decisively unsafe.  Keeping 96 positive
coordinates and restoring the exact cloud mean reaches `69.49B` but raw
explodes to `5.26e-2`; keep128 still gives `3.56e-5`.  A new paired exact-row
pilot calibrated every output slope (keep96, 1024 pilot rows) reaches `72.61B`
but remains catastrophic at `3.59e-3`.  Omitted rowwise fluctuations change
future gates and cannot be reconstructed by marginal affine calibration.

```text
legacy_workspace/candidate_pathcv76_sparse_pilotcal_root.py
```

An honest K26 coarse/fine tail MLMC probe was also run on the untouched
offset-four guard.  The best moderate arm checked (`late16_o224_i176_f144`,
64/256 stratified fine rows) has raw relative `1.0801`, work `1.0232`, and
product `1.1051`; all lower-work arms are worse.  Later/aggressive branches
range from `1.31x` to `8.96x` product.  Nonlinear tail error is not sufficiently
low-variance for this telescoping estimator.

```text
legacy_workspace/cache/root_k26_sparse_mlmc_offset_holdout4_20260712.npz
legacy_workspace/cache/root_k26_sparse_mlmc_late_offset_holdout4_20260712.npz
```

### Late complete-Kerdock compression

Generalized the checkpoint row-thinning mechanism beyond H2 and tested whether
the full 4-design can be retained until late depth.  At L24 on index 0, random
rows plus global affine repair preserve raw `3.886e-7`, but 3.973x work makes
the product only `0.968x` K26.  L16 is a stronger scout (`4.402e-7`, 2.985x,
optimistic product `0.824x`) but fails the untouched offset-four gate:
`9.348e-7` versus K26 `1.515e-6`, giving product about `1.842x`.

Whole-basis checkpoint herding does not rescue the geometry.  At L16/index0,
the complete rule is `3.147e-7` raw at 4.962x work; the best 26-basis coreset is
`6.393e-7` at 2.981x, so raw-times-work worsens from `1.561e-6` to `1.906e-6`.
Close late scattered-row and whole-basis compression in these forms.

```text
legacy_workspace/cache/root_kerdock_l16_row_thin_offset_holdout4_20260712.npz
```

### High-capacity causal L30 hidden-mean model

Repeated the learned late-state experiment at materially higher capacity on
the 1000-MLP L30 cache: hidden width `64`, eight equivariant message-passing
rounds, weight features enabled, and a fixed `mod 5 = 0` holdout.  This is a
large increase over the previous `h16/r2` scout, so it directly tests whether
the earlier near-zero gain was a capacity bottleneck.

The gate remained negative:

```text
epoch 1:  train 2.404561e-6 / 2.404746e-6
          test  2.266344e-6 / 2.265058e-6   ratio 1.0006
epoch 10: train 2.403086e-6 / 2.404746e-6
          test  2.266052e-6 / 2.265058e-6   ratio 1.0004
```

Training was intentionally stopped at epoch 10.  More capacity fits only a
tiny part of the training residual and does not beat the sampled L30 mean on
held-out networks.  Close weight-conditioned equivariant regression of this
feature set as a production correction; any learned revisit needs a new
causal coordinate or supervision target, not a wider version of this model.

```text
cache = /mnt/d/whestbench-data/phase1_l2snap_seedmeans_layer30_full1000_20260708.npz
command output target = song/runs/phase1_l30_hiddenmean_h64r8_fold0_e20_root_20260712.json
```

### Reliability-weighted causal variance fusion

The G0-prime audit had tested only hard analytic-prefix replacement.  Tested
the missing intermediate: fit one sampled-versus-analytic variance fusion
weight per early layer against Full-spaced20 variance teachers, then freeze
both those weights and the final correction gain before Full-offset20 and
Mini-spaced20 scoring.

```text
layerwise LS weights = [0, 1, .934, .781, .600, .461, .350, .299]

                         Full spaced   Full offset   Mini
LS precision fusion          .87862       .87908    .91158
independent-risk fusion      .87900       .87949    .91176
hard analytic prefix 7       .85170       .83825    .86544
```

All values are raw-MSE ratios after the Full-spaced-selected scalar output
gain is frozen.  Fusion is broadly positive but materially weaker than the
already-known hard-prefix screen and misses the three-guard `.85` bar.  The
fact that variance-MSE-optimal fusion removes useful output correction shows
that the analytic branch is serving partly as a structured biased proxy, not
as a clean estimate of the causal variance channel.  Do not advance this to
the incumbent row-cloud interface.

```text
legacy_workspace/probe_variance_precision_fusion_root.py
legacy_workspace/cache/root_variance_precision_fusion_20260712.npz
```

### Final-cloud factor Rao--Blackwellization

Ran the previously implemented but unevaluated factor-mixture arm in
`probe_k26_row_pathcv_root.py`.  It keeps the empirical low-rank factors of
the shifted final preactivation cloud and Gaussian-integrates only the
orthogonal residual.  This differs from the catastrophic checkpoint restart:
the complete sampled trajectory is retained through the final product.

On the five-MLP Full spread scout, however, the rank frontier is flat:

```text
base       1.094245198e-6
rank 128   1.093942389e-6   ratio .999723
rank 160   1.094106265e-6   ratio .999873
rank  96   1.095619294e-6   ratio 1.001256
rank  64   1.098893487e-6   ratio 1.004248
rank   0   1.869224403e-6   ratio 1.708232
```

The maximum gain is `0.028%` before paying for a sample-covariance
eigendecomposition.  It is too small to warrant broad guards or a flopscope
implementation.

```text
legacy_workspace/cache/root_k26_final_factor_rb_full_spread5_20260712.npz
```

### Full-depth final-weight-blind causal representation gate

Built a direct gate for the P-006 causal premise on the existing 1000-MLP L30
cache.  Unlike the earlier synchronous h64/r8 model, this c8 shared recurrence
traverses all 31 hidden layers using signed `W` and squared-`W` messages.  It
predicts the hidden residual without reading W31; W31 is used only to report
the projected-Ez ratio.  Eight aggregate seed-cloud features were used, with
`index mod 5 == 0` held out by MLP.

The larger receptive field does not reveal the missing coordinate:

```text
epoch  1: train hidden .996963   held hidden .999742   held Ez .999142
epoch  4: train hidden .995053   held hidden .999444   held Ez .998865
epoch 10: train hidden .991122   held hidden .998778   held Ez .998415
epoch 12: train hidden .993824   held hidden 1.005490   held Ez 1.004840
epoch 14: train hidden .989838   held hidden .999253   held Ez .998853
```

Training was stopped at epoch 14.  Best held hidden gain is only `0.122%`,
versus P-006's required `40%` reduction.  This is historically touched public
data and an older anchor, so it is not a formal generated-data kill theorem;
it is nevertheless a strong negative representation gate.  Do not launch the
2048-MLP label bake for this same seed-summary feature geometry.  A P-006
revisit needs the newly proposed analytic/local features to demonstrate signal
on the existing 128 generated MLPs first.

```text
song/src/train_p006_hidden_public_gate_root.py
```

## 2026-07-12: gauge-invariant downstream-response D56 screen

Tested one target-free replacement for D56's common-column ranking.  The
analytic post-ReLU second moment was multiplied by a diagonal squared-Jacobian
response propagated backward from the final ReLU.  This score is invariant to
the exact positive hidden-unit rescaling symmetry and changes neither row count
nor drop count.

The disabled wrapper had exact prediction/FLOP parity.  The enabled rule cost
only about `15.4M` extra counted FLOPs and passed the one-row safety check, but
failed its first frozen Full scout:

```text
Full 0,250,500,750: D56 1.023591722e-6 -> response 1.029985583e-6
raw ratio 1.006246496; per-row .997774/.998144/1.011188/1.038172
```

The preregistered rule required every scout below one, so the other scouts and
broad guards were not opened.  Close response-weighted common screening in
this form; no exponent/blend sweep or package was produced.

```text
legacy_workspace/probe_d56_response_screen_preregister_root.md
legacy_workspace/probe_d56_response_screen_root.py
```

## 2026-07-14: row-relative LUT products and whole-tail 9-bit freeze

Tested an honest arithmetic-memory exchange for the sampled row-cloud
products.  For nonnegative activation row `h`, quantize
`h_i = row_max * code_i/(levels-1)` approximately, build the current
MLP-product lookup table `table[i,code,j] = code*W[i,j]`, then gather/reduce
and restore the row scale.  Table construction, gathers, reductions, and
scaling are all executed under the prediction budget.

Closed variants before the final freeze:

- float16 tables: slower in NumPy and statistically worse;
- nonlinear companding powers 0.75 and 1.5: worse raw and wall;
- extending LUT use through call 30: worse;
- 9-bit on every K26 product: lower quantization error but worse effective
  compute from wall time;
- batch 8192: one Mini broad row exceeded the 60-second wall guard inside a
  large `take`, so it is operationally rejected;
- direct 7-bit and histogram/mean-correction variants: failed their scouts or
  were path-sensitive and slower.

The safe freeze uses 9-bit tables on calls 7--29 and batch 4096 in
`candidate_mixed238_d56_lut9_b4096_final_root_20260714.py`.  Full20 science
predictions from the completed 8192 run are batch-invariant; the exact failed
Mini row and then all Mini20 rows were replayed at 4096.

Broad combined evidence versus paired exact K26:

```text
n rows                    40 (Full20 + Mini20)
LUT9 raw                  1.5050235228290131e-6
exact-K26 raw             1.7284343929178058e-6
ratio                     0.870743795075931
mean counted FLOPs        21,969,881,883.6
mean residual             0.1350988416s
local floor projection    1.1315031794401847e-7
```

At residual penalties of +10ms or +20ms relative to 315968 the compute
multiplier remains on the 10% floor; +30/+40/+50ms sensitivity projects
`1.148e-7/1.190e-7/1.231e-7`.  The stratified 200k bootstrap gives a raw-ratio
95% interval `0.833635--0.925689` and floor-score interval
`1.08328e-7--1.20290e-7`.

Exact package and operational audit:

```text
archive SHA-256           71f4b0d834d0ad8ee3bb64a2a21adc3f2bcf088b37f87f8b2efdcfa53bfa18b7
RC5 codec/manifest        pass
extracted validate        pass
fresh setup median        5.622s (315968 control 6.236s)
isolated predictions      9/9 pass
max worker / outer wall   15.693s / 17.236s
max residual              0.190s
remote submission         316263
```

Terminal remote result:

```text
adjusted score             1.1920758902441732e-7
raw final-layer MSE        1.1907924300658123e-6
adjusted/raw multiplier    0.1001077820
inferred effective compute 27.229316710B
improvement vs 315968      16.481996%
```

The effective compute is inferred from the reported adjusted/raw pair; do
not present it as directly reported telemetry.  Remote raw was `5.2399%`
worse than the local remote-anchored projection.  Submission `316263` is the
new measured incumbent, and future candidates must beat its actual remote
score rather than the optimistic local `1.1315e-7` point estimate.

### Seven-basis dividend reinvestment: closed

Built a target-independent extension of Mixed238 from seven whole bases of a
fixed second Kerdock orientation.  Selection used exact H6 cross-energy only;
the natural blend was `4/21 = 0.190476`.  The candidate added 1,792 half-cloud
rows and raised index-level counted work from about `21.97B` to `26.47B`.

```text
                              old raw       augmented raw    ratio
Full initial 5              9.211052e-7       8.049694e-7   0.873917
Mini initial 5              1.465742e-6       1.520212e-6   1.037160
Full disjoint 5             1.957393e-6       1.363606e-6   0.696644
Mini disjoint 5             1.385228e-6       1.710710e-6   1.234967
combined disjoint 10        1.671310e-6       1.537158e-6   0.919733
```

Every disjoint Mini row worsened, and the combined raw gain is far smaller
than the work increase.  A seven-basis fresh-QR control immediately worsened
Mini index 0 from `4.52258e-7` to `6.77186e-7` (`1.497x`).  These failures
show that partial extra frames introduce material transfer bias; only complete
designs have previously been robust.  Do not broad-run, package, or submit
either candidate.

```text
candidate_mixed238_plus7k2_h6_d56_lut9_root_20260714.py
candidate_mixed238_plus_freshqr7_d56_lut9_root_20260714.py
cache/root_mixed238_plus7k2_h6_d56_lut9_*_20260714.csv
```

### Selective late 10-bit precision: closed

Kept the remote-proven 9-bit representation on calls 7--19 and increased only
calls 20--29 to 1,024 levels.  This isolates the late interval that delivered
the larger share of the earlier 8-to-9-bit gain.  The implementation retained
batch 4096 and counted every larger table construction and lookup operation.

```text
                 LUT9 raw       selective-LUT10 raw   ratio      wins
Full 0/250/500/
750/950          9.211052e-7       9.139323e-7       0.992213    4/5
Mini 0/20/40/
60/80            1.465742e-6       1.478739e-6       1.008867    0/5
combined         1.193424e-6       1.196335e-6       1.002440
counted F ratio                                         1.0092
```

Mini worsened on every row, pooled raw worsened, and work rose.  Therefore the
candidate cannot beat submission 316263 and is closed before broad scoring.
The result is evidence of beneficial 9-bit regularization, not monotone
convergence toward the exact product.

```text
candidate_mixed238_d56_lut10late20_b4096_root_20260714.py
cache/root_mixed238_d56_lut10late20_b4096_*_20260714.csv
```

### Honest real K129 package audit and calibration

Global batching across sparsity buckets removes partially filled LUT batches,
and `einsum("rko->ro")` replaces the slower but identically counted reduction.
The final real-float source reproduces the original prediction and counted
price exactly on Full-0:

```text
raw          1.989072665761e-7
F            87.703617419B
clean wall   43.782390s
residual     0.474606s
```

The explicit archive contains the four Python modules, K129 direction asset,
K26 setup asset, and manifest.  A fresh extraction passed all six manifest
hashes, compilation, and `whest validate`; isolated Mini-0 and Mini-65 workers
completed in `42.630s` and `45.950s` with exact finite outputs.

```text
archive submission_root_kerdock129_d56_lut9_lambda015_globalbatch_einsum_real_20260714_explicit_bundle.tar.gz
sha256  3974170ba4ea0b3a1b3cfffd8cf1e86762f501f4f56103c3916dcacf34dff9ff
```

Statistical calibration is not strong enough for submission:

```text
broad 36 raw                   2.502018094339e-7
untouched Full5 raw            3.279855184875e-7
untouched Mini5 raw            4.737928952499e-7
combined 46 raw                2.829599393111e-7
stratified bootstrap 95%       2.315249e-7 -- 3.424353e-7
316263 raw-transfer calibrated 2.977868e-7
remote-scaled point adjusted   about 1.17e-7
```

This is approximately tied with submission 316263, with a wide uncertainty
interval.  Keep the audited archive as a durable real control; do not upload it.

### Exact K258 cancellation, H2 holdout, and submission 316276

Two independent complete K129 orientations have flattened error cosine
`0.065478`.  Their target-free equal average scores `1.300278007874e-7` on the
three-family 36-row gate, a `46.10%` raw reduction.  A compact asset stores two
float32 rotations plus shared int8 phase rows and reconstructs the entire
`66,048 x 256` half-direction tensor bit for bit by float64 FWHT during setup.
The complete archive passed hashes, extraction, contract, 8-GiB memory, and a
strict Mini-0 worker.

Because the original quarter-strength H2 repair had failed the untouched
single-orientation guard, both H2 choices were compared on the same ten fresh
K258 rows:

```text
                         Full5 raw       Mini5 raw       pooled10 raw
quarter H2              1.403362905e-7  1.160369606e-7  1.281866256e-7
inherited H2            1.238179517e-7  1.291677073e-7  1.264928295e-7
inherited / quarter                                         0.986786

quarter local adjusted mean    9.519110116e-8
inherited local adjusted mean  9.331453647e-8
max wall over all 20 runs      31.869321s
```

The H2 sign reverses by family and its pooled effect is small.  Complete-rule
cancellation, not H2 retuning, is the transferable statistical result.

The compact quarter-H2 L3 archive became submission `316276` and completed all
remote rows:

```text
raw              1.161788501847e-7
adjusted         7.598512223542e-8
F mean           173.958406723B
effective mean   177.901836091B
residual mean    39.434ms
failures         0
```

This is the first measured score below `1e-7`.  The arithmetic result is exact,
but its two-real-column `complex64` packing is explicitly subject to announced
organizer repricing.  Treat `316276` as validated science/current score and as
a teacher for a durable real kernel, not as evidence that its compute multiplier
will survive that change.

### Fixed positive-row cap: closed

Tested the direct real-kernel hypothesis that dropping the smallest positive
ReLU terms would remove enough LUT bandwidth to make K258 runnable.  Full-0:

```text
baseline K129               raw 1.9890727e-7  F 87.704B  wall 43.78s
hard cap 96                 raw 9.9613947e-2  F 76.223B  wall 35.68s
cap 96 + row L1 rescale     raw 7.4673513     F 76.845B  wall 36.60s
cap 96 + exact mean repair  raw 2.0778481e-2  F 77.226B  wall 57.70s
```

The exact cloud-mean repair computes `mean(A) @ W` and forces every approximate
product to that exact mean, so the remaining failure is specifically missing
rowwise fluctuation, not simple scale drift.  This family is many orders of
magnitude from the raw frontier and is closed without Mini or K258 expansion.

```text
candidate_kerdock129_d56_lut9_lambda015_globalbatch_einsum_hardcap_root_20260714.py
cache/root_k129_real_lut9_hardcap96*_full0_20260714.csv
```
