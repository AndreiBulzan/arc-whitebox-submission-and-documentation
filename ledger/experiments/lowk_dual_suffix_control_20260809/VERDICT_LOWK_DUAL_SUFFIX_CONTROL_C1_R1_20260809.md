# Verdict: kill nonlinear basis-mean suffix control

Date: 2026-08-09

## Outcome

Reject the tested layer-5 nonlinear basis-mean suffix as a multifidelity
control for complete ordinary/polar basis tails.

This is **component development diagnostic** evidence for accuracy and a
**projection** for compute.  It used the fixed sixteen-row public pilot,
sealed every target-free CUDA output before opening the stored targets, and
ran no FlopScope session, physical row, Mini100 row, package, upload, or
submission.

## Controlling numbers

The surrogate propagated the positive and negative layer-5 mean state of
each of twenty ordinary and twenty polar bases through the actual nonlinear
suffix.  Its mean per-basis coarse/fine correlations were only:

```text
ordinary  0.239393
polar     0.230896
```

At the maximum preregistered floor-compatible support of fourteen exact
tails (seven per arm):

```text
herded residual-control raw MSE   7.39263e-6
same-support direct raw MSE       3.32548e-6
control/direct ratio              2.22303x
optimistic count projection       20.1440B
```

The result is not a near miss.  The control worsened the same selected fine
tails, every row was worse than the forty-tail shared-support population,
and even twenty exact tails produced `6.17642e-6` raw MSE.

The all-coarse population itself had raw MSE `1.83803e-2`.  Sampling a few
large fine-minus-coarse residuals cannot recover its missing common mode.
This reproduces, with a richer nonlinear coarse map, the controlling failure
of the earlier layer-8 affine orientation multifidelity experiment.

## Scope of the kill

This closes:

- two-row positive/negative basis-mean suffixes at layer 5;
- coefficient-one residual correction from that surrogate;
- the frozen first/diagonal-second checkpoint herding rule on that residual.

It does **not** close the independently reported direct joint checkpoint
coreset.  The component's shared coarse-derived late support also worsened
the complete fine population (`1.75083e-6`) relative to the independently
captured exact K20+K20 control (`8.21881e-7`), so it is not admissible evidence
against a direct selector using each selected cloud's actual late supports.

The next lawful question is therefore a clean capacity separation: capture
accurate per-basis endpoints under the literal K20 ordinary and K20 polar
tails, apply the already-sealed layer-5 selection order, and measure both
target-free direct selection and target-aware best-support/weight capacity
at 8--14 total tails.  If even target-aware stable weights lack `<=1e-6`
capacity, no smarter selector should be built.

## Evidence

- preregistration:
  `PREREG_LOWK_DUAL_SUFFIX_CONTROL_C1_R1_20260809.md`
- target-free capture:
  `runtime/artifacts/lowk_dual_suffix_control_c1_r1_targetfree_20260809.npz`
- capture receipt:
  `runtime/artifacts/lowk_dual_suffix_control_c1_r1_targetfree_20260809.json`
- post-seal receipt:
  `runtime/artifacts/lowk_dual_suffix_control_c1_r1_postseal_20260809.json`

