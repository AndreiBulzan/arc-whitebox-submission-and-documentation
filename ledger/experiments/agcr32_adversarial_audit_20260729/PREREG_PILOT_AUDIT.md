# AGCR-32 availability-consistent pilot audit preregistration

Frozen before any pilot-rule endpoint score was computed.

## Scope

This is an adversarial, target-free, cache-only **component** audit.  It tests
whether the very large `agcr32_20260729` cache result survives when the
predictor is restricted to information a lawful estimator could possess.
It is not a whole estimator, a raw challenge score, a current-FlopScope
receipt, or permission for a physical or remote run.

The already observed post-result fact that the original residual is nearly
row-global is allowed motivation.  No outcome for any exact-pilot rule below
has been inspected.  The rules, pilot counts, aggregations, and gates in this
document are fixed before those endpoints are scored.

## Frozen inputs

Use the same first 12 complete rows from each of:

- `root_k258_8orientation_full200_20260715_work`;
- `root_k258_8orientation_full200_offset200_20260715_work`;
- `root_k258_8orientation_generated128_20260715_work`.

Read only `completion.npy`, `indices.npy`, `observer_basis_postmean.npy`,
`response_matrix.npy`, and `final_basis_premean.npy`.  Use checkpoint axis 1
(L8).  `final_basis_premean` is a target-free internal model endpoint, not a
challenge target.  It is unavailable to the predictor except at the exact
pilot atoms declared below.

For every row define

```text
H[o,b,:] = observer_basis_postmean[o,1,b,:]
R        = response_matrix[1,:,:]
Y[o,b,:] = final_basis_premean[o,b,:]
E        = Y - H @ R
```

with eight orientations, 129 bases, and width 256.  Recompute the response
sign cells exactly as in the sealed AGCR screen: center flattened `H`, form
`Z = centered(H) @ R`, take the first two right singular vectors, canonicalize
each sign by its largest-absolute coordinate, and use the two score signs as
four cells.

## Fixed exact-pilot rules

Pilot counts are `p in {1,2,4,8}` **total atoms per MLP**, not per
orientation.  The rules are nested.

### Fixed spread

Flattened `(orientation, basis)` pilots are prefixes of:

```text
(0,0), (4,128), (2,64), (6,32),
(1,96), (5,16), (3,80), (7,48)
```

This is a bit-reversal spread over orientations paired with the historical
spread basis schedule.  It uses no endpoint residual.

### Response-cell balanced

Use fixed cell order `(0,3,1,2)`.  Within each cell, rank atoms by increasing
squared Euclidean distance of their two response scores to that cell's score
centroid, breaking ties by flattened atom index.  The nested order takes the
nearest atom from each cell in fixed cell order, then the second-nearest atom
from each cell in the same order.  Prefixes of lengths 1, 2, 4, and 8 are the
pilots.  All four cells must be nonempty and contain at least two atoms.

Thus `p=4` buys exactly one endpoint in every response cell and `p=8` buys
exactly two.  Selection consumes only `H` and `R`, never `Y` or `E`.

## Fixed predictors

After selecting pilots, and only then, reveal their exact residual vectors
`E[pilot]`.

1. `global`: predict every nonpilot atom by the arithmetic mean of all pilot
   residual vectors.
2. `cell`: predict a nonpilot atom by the arithmetic mean of pilot residuals
   in its response cell; if that cell has no pilot, fall back to the global
   pilot mean.

Pilot atoms themselves are copied exactly.  There is no fitted coefficient,
shrinkage, weighting, family training, row selection, clipping, or
post-result choice.

## Fixed metrics

For each rule, count, and predictor report independently on pooled Full
(24 rows) and Generated (12 rows):

- `SSE_remaining / SSE_zero_remaining`, excluding exact pilots;
- `SSE_all / SSE_zero_all`, with exact pilots included at zero error;
- row-ratio median, p95, and maximum;
- endpoint correlation on remaining atoms.

Because the challenge consumes the mean endpoint rather than 1,032 separate
basis endpoints, also report the stricter aggregate diagnostic:

- per row, reconstruct every endpoint as `H @ R + E_hat`, average over all
  orientations and bases, and compare that 256-vector with the exact cached
  all-eight endpoint mean;
- pool its MSE/RMS across each family;
- divide that MSE by the target-free action energy between the exact O0/O1
  endpoint mean and the exact all-eight endpoint mean.

This aggregate metric prevents a tiny per-atom ratio against the very large
common residual from hiding a pilot-noise vector that survives averaging
unchanged.

Also independently reproduce:

- the original leave-one-orientation-out four-cell ratio;
- its leave-one-orientation-out global-centroid ratio;
- `SSE_cell / SSE_global` and the cell-increment correlation;
- exact row-global residual-energy fraction and complementary atom-centered
  fraction.

## Fixed decision rule and economics boundary

A pilot configuration is mechanism-plausible only if, on both Full and
Generated:

- remaining-atom residual ratio is at most `0.50`;
- row-ratio p95 is at most `1.0`;
- the same predictor (`global` or `cell`) wins without family-specific
  selection.

For a plausible score path it must additionally have, on both families,
aggregate endpoint-mean RMS at most `1e-3` and aggregate MSE no more than
`0.25` of the exact O0/O1-to-all-eight action energy.  The `1e-3` threshold is
deliberately generous relative to the roughly `4.6e-4` output RMS of the
current remote raw score; passing it is necessary, not sufficient.

These gates are intentionally only necessary component gates.  A pass does
not establish a current-0.9.1 score path.  Economics must be reported
separately as a projection using a named current-meter chassis and an explicit
tail-cost bound; old FlopScope 0.8 counts may not be promoted.  If no
current-meter tail-cost receipt exists, the conclusion must say that compute
viability is unproved.

Historical comparisons are fixed to
`ROOT_K129_ADAPTIVE_TAIL_SELECTOR_VERDICT_20260713.md` and
`K258_MICROTAIL_ORIENTATION_CV_AND_OFFSET_BINDING_VERDICT_ROOT_20260715.md`.
No physical, timed, FlopScope, package, challenge-target, or remote work is
authorized.
