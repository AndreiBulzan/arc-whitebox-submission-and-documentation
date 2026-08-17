# Angle-B sparse correction pilot R1

Date: 2026-08-05

Evidence sought: target-free **component** endpoint capture on fixed
Full16/Generated16 rows, followed by a post-seal economic screen.  This is
not a physical, package, Mini100, or remote claim.

## Prior-work boundary

The capsule already tested sparse corrections from right-Gram and polar
frames.  It has not tested a sparse correction from the fixed Schur
`angle_b` frame.  That distinction matters because the complete `angle_b`
frame transferred on Full100, Generated128, and official Mini100, and its
error is nearly uncorrelated with q0.

## Frozen screen

- rows: first 16 rows from each sealed broad angle-B family;
- supports: pooled target-free OMP reconstruction of the complete
  `angle_b - q0` endpoint direction;
- sizes: 8, 16, 24, 32, 48, 64;
- output weights: 0.25, 0.50, 0.75;
- conservative cost: R78 effective anchor 140.13B, 0.999B per additional
  basis, and 5B frame-construction allowance.

Promote to a disjoint broad literal test only if one rule has projected
adjusted score at most `1.08e-7` on both pilot families, raw ratio at most
0.78 on both, and improves at least 10/16 rows in both.  The stricter pilot
threshold leaves room for transfer loss on official Mini100.

