# K129 R56 joint continuation support — R62 preregistration

## Prior-art boundary

Prior work separately selected the alternate support, separately compressed
the q0 anchor, tried smaller alternate supports with the q0 anchor fixed, and
tried merging repaired-H2 trajectories before D6.  It did not jointly select
the complete late-continuation linear combination.  This distinction matters:
an alternate trajectory costs much more than a q0 contrast, and separate
selection cannot exchange one for the other.

## Exact representation

Relative to the already-paid primary q0 endpoint, form 177 target-free atoms:

- the fixed 48 R56 alternate endpoint trajectories minus q0; and
- all 129 q0-basis endpoint trajectories minus q0.

R56 is exactly representable in this dictionary with 112 nonzeros: its 48
alternate coefficients and 64 q0-anchor coefficients.  No new statistic is
introduced by the dictionary.

## Fixed screen

For support sizes `{56,64,72,80,96,112}` and relative ridges
`{0.125,0.25,0.5,1.0}`, recompute OMP inside four network folds with equal
Full/Generated family weight.  Fit each all-row support target-free and price
it using the existing measured/projected per-trajectory slopes:

- alternate trajectory: `0.2053952B` effective;
- q0 late-continuation trajectory: `0.007253886B` effective.

Select by worst-family cross-validated reconstruction ratio times projected
effective-compute ratio versus R56.

## Gate

Seal for post-target scoring only if the selected candidate has product ratio
strictly below one in both families.  Scored targets remain unopened in this
screen.  Compute is a **projection** until an exact FlopScope implementation;
the screen is **component** evidence only.

