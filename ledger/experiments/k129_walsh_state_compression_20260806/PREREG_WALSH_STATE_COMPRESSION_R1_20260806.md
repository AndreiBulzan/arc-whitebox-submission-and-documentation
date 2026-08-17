# K129 within-basis Walsh-state compression R1 preregistration

Evidence target: **component**.

## Question

Can the 256 direction values inside every signed Kerdock basis be represented
by substantially fewer Walsh coefficients between nonlinear layers, while
retaining enough final accuracy to improve raw-MSE times compute?

This is not prior direction thinning: all 256 direction values are
reconstructed before every ReLU.  It is not the seven-bit lane: values remain
float32 and no operands are packed.  It is not a Walsh/basis decoder: the
Walsh coefficients are the propagated numerical state, not post-hoc features.

## Frozen pilot

- Sources: exact K129 O0 front, H2 repair, L4 snap, screening, late selection,
  final mean restoration, gamma readout, and output scale inherited from the
  current numerical family.
- Rows: Full indices `{0,17}` and Generated indices `{0,17}`.
- Ranks: `{32,64,96,128}` of 256 direction characters.
- Two target-free selectors:
  - `low_degree`: masks ordered by Boolean Hamming weight, then mask number;
  - `global_energy`: the common masks with largest state energy, pooled over
    signs, bases, and active neurons independently at every layer.
- The same retained mask set is used by every sign/basis at a layer.  DC is
  always retained.
- A rank-256 replay on Full row 0 is an implementation identity check.
- No coefficient, rank, selector, layer window, or output blend is fitted to
  targets.

At each propagated layer the retained coefficient vectors are multiplied by
the actual network matrix, scattered into a 256-character spectrum, inverted
to all 256 direction values, passed through the unchanged nonlinearity, and
transformed again.  Thus the only approximation is the discarded Walsh state.

## Kill/promote rule

This four-row screen is only a fast component falsifier.  A spelling survives
to a disjoint 8+8 screen only if it satisfies its rank-specific pooled raw-MSE
ceiling on **both** families:

| retained rank | maximum raw-MSE ratio to exact K129 |
|---:|---:|
| 32 | 1.60 |
| 64 | 1.35 |
| 96 | 1.20 |
| 128 | 1.10 |

These are conservative feasibility gates, not compute receipts.  Count,
movement, request, residual, wall, and effective-compute pricing is deferred
unless a spelling survives both families.  Failure kills the tested ranks and
selectors, not all possible spectral bases or nonlinear spectral closures.

## Boundaries

Capture opens weights only and seals all predictions first.  A separate
post-seal scorer may then open targets.  There is no FlopScope run, physical
receipt, package, upload, submission, or remote action.

