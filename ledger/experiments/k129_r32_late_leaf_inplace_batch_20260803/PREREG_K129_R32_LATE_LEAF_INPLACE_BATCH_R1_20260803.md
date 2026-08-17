# K129 R32 in-place late-leaf batch preregistration

Date: 2026-08-03

## Prior-art block

Searched the capsule for `late`, `B7`, `repack`, `in-place`, `contiguous`,
`deep_left`, `deep_right`, `overwrite`, `reuse`, and `stack`.  Existing work
covers direct late-ReLU destinations and R31's explicit four-leaf stack, but
not reuse of the dead deepest Winograd quadrant as the fourth leaf carrier.
The requested observable is therefore new.

## Exact lifetime identity

After the deepest left transform, `a21` is dead once `la4`, `la5`, and `la6`
have been formed.  Compute `la2 = a12 - la5` into that dead `a21` slot.  The
existing contiguous bank is then exactly `(a11, a12, la2, a22)`.

After the prepared-right deepest transform, `b12`, `b21`, and `b22` can be
repacked in place as `(b21, b22, rb3)` because all four right transform ranks
have already been formed.  The existing right bank is then exactly
`(b11, b21, b22, rb3)`.  It is prepared once and consumed twice.

The quadrant axes are exposed once, during initialized prediction, through
persistent reshape/transpose views of the existing left/right workspace.
No view construction recurs on the steady row.  This removes R31's two
`stack` operations per product and adds three small `copyto` operations per
right preparation.

## Frozen ceiling

- Removed stack count: `14 * 19,535,712 = 273,499,968`.
- Added right-repack count: `7 * 3 * 29,400 = 617,400`.
- Net counted saving versus R31: `272,882,568`.
- Expected initialized count: `126,210,393,459`.
- Expected steady count: `123,966,341,434`.

The initialized count above excludes the one-time view-creation charge and is
therefore a lower bound until the first physical receipt.  The steady count is
exact.

Promotion requires the exact ledger, R31's fetched prediction SHA-256,
100-MiB cap compliance, and lower steady effective compute.  The first whole
run is a measured-whole diagnostic, not broad or remote evidence.  No upload
or submission is authorized.
