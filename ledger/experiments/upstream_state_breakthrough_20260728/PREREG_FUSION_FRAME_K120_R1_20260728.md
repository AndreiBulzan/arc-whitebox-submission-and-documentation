# Preregistration: two-orientation fusion-frame K120 r1

Date: 2026-07-28

Evidence target: **component**.  Economics are a **projection**.  No
FlopScope session, physical receipt, package, upload, submission, or remote
action is authorized.

## Hypothesis

The complete-basis unit used by the current estimator couples two desirable
properties that need not have the same price:

1. all 256 axes of one orthonormal basis give exact covariance;
2. many distinct Kerdock bases expose different high-order gate cells.

Dropping complete bases preserves (1) but loses (2).  This experiment keeps
all 258 production bases across both orientations, but only 119 antipodal
axes per basis.  A deterministic fusion-frame coordinate descent chooses
the axes jointly so their aggregate covariance remains close to identity.
The cloud has 61,404 signed rows, versus 61,440 for conventional K120, hence
essentially identical dense propagation work.

This is a new state geometry.  It is not another basis-support fit,
checkpoint resampling, marginal closure, endpoint head, or shallow
response-matching recombination.

## Immutable construction

- Use the exact two hash-pinned production orientation banks, shape
  `(2,129,256,256)`.
- Flatten orientation/basis to 258 blocks.
- In every block select exactly 119 axes.
- Initialize with
  `default_rng(20260728).choice(256,119,replace=False)`, independently in
  flattened block order.
- Let `C` be the sum of selected rank-one projectors and
  `T=(258*119/256) I`.
- Perform exactly eight block-coordinate sweeps in block order `0..257`.
  For block `b`, remove its old projector, form `E=C-T`, score every row
  `v` by `v' E v`, and replace the block by the 119 lowest-score rows.
  Ties follow NumPy's first integer ordering after sorting selected IDs.
- Use equal mass on every retained signed row.

Control: the conventional balanced K120 cloud, exactly the first 60 complete
bases from each production orientation.  Both arms:

- use the same exact H1 global mean/second-moment affine match;
- then propagate the literal cloud through all remaining affine/ReLU layers;
- report the equal-row post-ReLU mean at every layer.

Fixed rows:

```text
Full       2, 102
Generated  2, 66
```

Capture opens weights only and freezes both prediction arms.  The separate
scorer then opens the named all-layer diagnostic means.

## Gates

Target-free geometry:

1. candidate signed rows exactly `61,404`; control exactly `61,440`;
2. every block has 119 unique valid axes;
3. normalized aggregate frame Frobenius error `<=0.025`;
4. maximum absolute frame eigenvalue error `<=0.05`.

Post-seal, separately by family:

5. pooled final-layer MSE candidate/control `<=0.80`;
6. pooled all-layer MSE candidate/control `<=0.90`;
7. both individual final-layer row ratios `<=1.25`;
8. at least one of two rows improves.

All gates must pass.  Failure kills this exact seed/rank/sweep construction;
do not change the rank, seed, sweep count, or row mask on these rows.

## Promotion and economics

The experiment is deliberately a same-node-count mechanism test under a
simple literal cloud.  Passing does not claim production K120 raw MSE.
Passing authorizes a production-statistic replay with the exact frozen masks
and existing H1/H2/L4/late repairs on a disjoint two-family bank.

Row-count scaling from remote K146 is `61,404 / 74,752 = 0.82144`.
Holding all non-cloud work proportional would project `171.0B -> 140.47B`.
At `140.47B`, the raw ceilings are approximately `2.324e-7` for a
`1.2e-7` adjusted checkpoint and `1.936e-7` for `1.0e-7`.  These are
arithmetic targets, not receipts.

