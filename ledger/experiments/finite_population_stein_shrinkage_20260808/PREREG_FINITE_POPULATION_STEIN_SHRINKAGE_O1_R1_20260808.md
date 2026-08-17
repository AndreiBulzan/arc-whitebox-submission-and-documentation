# Finite-population Stein shrinkage O1/R1 preregistration

Date: 2026-08-08

Evidence boundary: **component** replay on the already sealed Full640--641
and Generated88--89 complete-basis packet-response arrays.  This run opens
the already-used post-seal targets, but performs no new network propagation,
Mini100 access, physical benchmark, packaging, upload, or remote action.

## Prior-art preflight

Queries covered `shrink`, `Stein`, `James--Stein`, `basis source`,
`complete-basis audit`, `projected total contraction`, `weighted basis`, and
the equivalent scalar-control operation.

Nearest controlled negatives:

- grouped complete-basis sampling rejected the unshrunk (`alpha=1`) 4--8
  basis estimator because its variance was enormous;
- projected total contraction rejected near-unbiased projected auditing,
  including impossible complement-trained controls, against the much tighter
  `8.68e-9` variance allowance;
- weighted complete-basis fitting retained unavailable-output `S=16`
  capacity but supplied no lawful target contraction.

This is a **materially new statistical objective**: deliberately bias a
random complete-basis estimate toward the safe zero correction and require
only a score-positive 18--30% raw gain.  It is not a renamed near-unbiased
audit.

## Mandatory normalization correction

The stored atom columns `a_b` already contain `1/129`, and their sum is only
the packet *source* term.  That source has a large mean which cancels a known
radial term `d`.  Shrinking `sum a_b` toward zero while retaining `d` would
not return to R87 at `alpha=0` and would test the wrong estimator.

Define the complete full-correction population instead as

```text
g_b = d + 129 * a_b
t_full = mean_b(g_b) = d + sum_b(a_b).
```

For a random support `A` of size `k`,

```text
Y_A = mean_{b in A}(g_b)
prediction(alpha) = q0 + alpha * Y_A.
```

Then `alpha=0` is exactly the canonical/R87-compatible `q0` endpoint and
`E_A[Y_A]=t_full`.  The run must verify

```text
q0 + t_full == exact stored packet population mean
```

to numerical tolerance before scoring.

## O1 capacity test

- Input capture:
  `runtime/artifacts/grouped_gate_source_basis_g0_r1_targetfree_20260808.npz`.
- Use the mean of all 32 stored packet replicates for each basis.  This removes
  packet-treatment noise and is deliberately optimistic for a production
  estimator based on one treatment.
- Draw `20,000` frozen, target-blind simple-random-without-replacement
  supports at each
  `k in {4,6,8,10,12,16,20,24}` with seed `2026080827`.
- Reuse every support on all four development rows.
- For each row and support, grant the unavailable target-dependent best
  scalar

  ```text
  alpha_oracle = clip(-<e_q0,Y_A> / ||Y_A||^2, 0, 1).
  ```

- Primary statistic: the median across supports of the pooled four-row raw
  reduction when every row receives its own oracle scalar.  Also record Full,
  Generated, shared-scalar, alpha=1, and quantile diagnostics.

## Gate

Proceed to target-free SURE/James--Stein/replicate rules only if at least one
tested score-compatible `k` has:

```text
median pooled per-row-oracle raw reduction >= 35%
```

and neither family has negative median reduction.  If this fails, reject the
whole scalar Stein-shrinkage family: SURE and empirical Bayes cannot exceed a
target-dependent per-row best scalar on the same correctly centred
observation.

The 1.075B "half-basis" production cost is not established by the current
capture: each stored treatment averaged four branch rows per Kerdock line and
projects to about 2.15B per complete basis.  Report score arithmetic at both
1.075B (speculative branch-pair half) and 2.15B (captured full treatment),
labelled **projection** only.

