# K146 Full-training residual R1 preregistration

Date: 2026-07-29

Evidence scope: offline `component` / `broad statistical` only. No physical
FlopScope row, package, upload, submission, or remote action is licensed.

## Hypothesis

The previous shared K146 endpoint corrector had only 47 training MLPs during
model selection and 73 during its final fit. Its in-fit raw-MSE ratio was
`0.8695`, but its 26-network Full holdout and Generated transfer reversed.
This experiment tests whether that was small-network estimation error by
expanding the same lawful, output-permutation-equivariant observable to the
unused official training population.

No frozen lower-K confirmation index may be captured or scored. The existing
100-row endpoint capture is merged with target-free captures from dataset
indices below 400, excluding both existing rows and the exact frozen
confirmation set. This yields 375 distinct non-confirmation MLPs.

## Frozen split and model ladder

Network split is by dataset index modulo five:

- train: residues 0, 1, 2;
- development: residue 3;
- held: residue 4.

Only Full train targets fit models. Only Full development targets choose
among the four already-defined feature subsets and ridge penalties
`0.1, 1, 10, 100, 1000, 10000`. No nonlinear estimator is admitted in R1.
The selected ridge is refit on train plus development, then Full-held and
Generated predictions are serialized and hashed before either score is
opened.

## Gates

Continue from development to post-seal scoring only if:

```text
Full development pooled raw ratio  <= 0.92
Full development row-ratio p95     <= 1.35
```

Promote the mechanism only if both post-seal families satisfy:

```text
Full held pooled raw ratio          <= 0.88
Generated pooled raw ratio          <= 0.88
each family row-ratio p95           <= 1.25
incremental estimator count         <= 2B (static projection)
```

Failure stops this feature family. Passing licenses estimator integration,
not a remote submission.
