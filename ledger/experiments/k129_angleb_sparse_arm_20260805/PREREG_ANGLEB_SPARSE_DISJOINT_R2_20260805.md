# Angle-B sparse correction disjoint transfer R2

Date: 2026-08-05

The R1 pilot promoted a sparse correction from the fixed complete Schur
`angle_b` frame.  This R2 test freezes its target-free OMP supports and runs
literal combined q0+sparse-angle-B trajectories on disjoint Full84,
Generated112, and all official Mini100 networks.

The candidate grid is frozen to sizes 8, 16, 24, and 32.  Output weights
0.25, 0.50, and 0.75 are reconstructed from the captured sparse endpoints.
One size/weight is selected using only disjoint Full84 and Generated112.
Only after that selection is written may official Mini100 targets be opened.

The cost model is deliberately conservative: R78 at 140.1303116625B,
0.999B per added basis, plus 5B for frame construction.  Promotion requires
both disjoint-family projected scores <= `1.08e-7`, both raw ratios <= 0.78,
and at least 55% of rows improved.  The unchanged selected rule then requires
official Mini100 projected adjusted <= `1.10e-7`, raw ratio <= 0.82, and at
least 60/100 rows improved.

This licenses accuracy promotion only.  A passing rule still needs a
current-meter physical graph and wall receipt before packaging.

