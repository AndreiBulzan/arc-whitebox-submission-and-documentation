# Preregistration: exact-H1-moment LMCBR H1 R1

Date: 2026-08-09

## Question

Can analytically exact first-layer spherical moment totals identify the stable
129-basis endpoint reweighting capacity already measured on official
Mini100, without using targets or propagating a second frame?

The frozen R90 endpoint atlas contains one final 256-vector per Kerdock basis.
For the actual first-layer weight columns `a_j` and a uniform unit-sphere
direction `u`, the exact identity

`E |u^T a_j|^p = Gamma(d/2) Gamma((p+1)/2) / (sqrt(pi) Gamma((d+p)/2)) * ||a_j||^p`

provides target-free totals for every positive moment. The signed basis pair
provides the corresponding per-basis estimate `0.5 * mean |B_b W0|^p`.
The complete Kerdock design is essentially exact at degree four but is not
exact at degrees six and above; those higher-moment defects are the proposed
basis-quality observable.

A second exact block uses the full first-layer ReLU covariance under the
sphere and contracts it through 32 frozen actual columns of `W1`. This gives
exact second moments of 32 layer-two preactivation directions.

## Evidence boundary

- Build candidates from all 100 official Mini100 **weights only** and the
  already sealed target-free R90 endpoint atlas.
- The builder must not map or read `targets.npy`.
- Seal all candidate predictions, hyperparameters, sources, and input hashes.
- Only the separate scorer may open Mini100 targets.
- Select one cell on official Mini100 public rows 0--49 and transfer it
  unchanged to official Mini100 holdout rows 50--99.
- Full/Generated data do not select, screen, or score this experiment.

## Frozen feature families

Moment powers are computed from the raw unit-sphere first-layer projections.

1. `p1`
2. `p3`
3. `p5`
4. `p6`
5. `p8`
6. `p10`
7. `p12`
8. `p6_8`
9. `p6_8_10_12`
10. `odd_1_3_5_7`
11. `all_1_3_5_6_7_8_10_12`
12. `nextdiag32`
13. `p6_8_10_12_nextdiag32`

`nextdiag32` uses output indices `0,8,...,248` of the actual second-layer
matrix. The first-layer artificial repair used by R90 is reproduced exactly;
the per-basis repaired states are contracted through those 32 directions.
Their target is obtained from the exact spherical first-layer ReLU covariance
and the same 32 directions.

Each family is standardized either by per-coordinate between-basis standard
deviation or by its exact-total magnitude. The frozen ridge grid is

`1e-4, 3e-4, 1e-3, 3e-3, 1e-2, 3e-2, 1e-1, 3e-1, 1, 3, 10`.

Weights lie on the target-free calibration ray from uniform and are clipped
by each frozen pair of constraints:

- ESS minimum: 32, 64, 96;
- maximum weight: 2/129, 4/129, 8/129;
- nonnegative weights summing to one.

One shared weight vector is applied to all 256 final outputs.

## Gates

Promote only if the public-selected cell, unchanged on holdout, achieves:

- at least 15% raw reduction on public;
- at least 15% raw reduction on holdout;
- at least 20% pooled raw reduction;
- no half regresses;
- at least 60/100 rows improve.

This is broad statistical accuracy evidence only. A pass still requires a
capsule-native FlopScope implementation and official Mini100 physical
accounting before it can be banked.

