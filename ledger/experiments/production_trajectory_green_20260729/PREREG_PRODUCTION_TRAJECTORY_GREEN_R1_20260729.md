# Production trajectory Green-function R1 preregistration

Date: 2026-07-29

Evidence sought: **component** first, with a whole-MLP held split and an
independent Generated transfer guard.  This authorizes only an ordinary-CUDA
target-free capture and separate offline scoring.  It does not authorize
FlopScope, a physical row, package work, upload, submission, or remote action.

## New observable

The current K146/m17 replay carries 129 complete Kerdock bases in orientation
0 and 17 bases in orientation 1 through every repaired/compressed layer, but
discards their trajectory structure at readout.  R1 retains nine zero-mass
replicate contrasts at checkpoints after weight layers
`4,8,12,16,20,24,28,30`:

- the O1-minus-O0 arm mean;
- four balanced Walsh contrasts of the 128 nonzero O0 basis means;
- four zero-mean unit-RMS DCT contrasts of the 17 O1 basis means.

Each contrast is transported to the signed final preactivation through the
realized downstream weights and the already-computed analytic-closure gate
probabilities.  Thus every feature is a covariant, target-free estimate of a
layer-local quadrature innovation in the coordinate system that affects the
scored output.  This is not a final endpoint head, orientation-population
proxy, local scalar moment cell, or K32 literal-direction recurrence.

The production numerical trajectory, support17, H1/H2 repairs, layer-4 snap,
screen schedule, late energy selections, and literal `129:17` arm blend are
unchanged.

## Frozen rows and fitting

- Full train: indices `0..79`.
- Full development: `80..99`.
- Full held: `100..139`.
- Generated transfer: `0..63`.

The capture opens weights only and seals the baseline signed premean plus 72
transported feature vectors for every row.

One shared output-permutation-equivariant linear correction is then fitted on
flattened Full-train MLP/output records.  Features are divided by their
Full-train RMS; there is no intercept.  Ridge is selected on Full development
from the fixed grid:

```text
0, 1e-6, 1e-4, 1e-2, 1, 100
```

After selection, held Full and Generated predictions are serialized before a
separate scorer opens their signed targets.  No Generated target participates
in fitting or selection.

## Hard gate

Continue only if all hold for exact signed-final-preactivation MSE:

1. corrected/control pooled ratio `<=0.75` on Full held;
2. corrected/control pooled ratio `<=0.75` on Generated;
3. row-ratio p95 `<=1.10` on both;
4. at least 60% of rows improve on both;
5. all arrays are finite and the target-free/held seals match.

A pass is still not a final-ReLU estimator receipt.  It earns a direct
production final-ReLU replay and arithmetic projection.  Failure closes this
exact nine-mode/eight-checkpoint Green-function spelling.

