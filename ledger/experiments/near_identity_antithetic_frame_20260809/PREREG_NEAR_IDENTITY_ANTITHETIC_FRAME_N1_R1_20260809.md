# Preregistration: near-identity antithetic complete-frame curvature N1/R1

Date: 2026-08-09

Evidence scope: target-free ordinary-CUDA component capture followed by a
post-seal 16-Full/16-Generated score.  No FlopScope session, physical row,
package, Mini100 run, upload, submission, or remote action is authorized.

## Question

Can two opposite, weight-adapted rotations of the complete K129 design create
a low-variance curvature correction that can be estimated from a few matched
antipodal lines per basis?

For the standard frame `F` and a weight-conditioned orthogonal map

```text
Q(theta) = V B(theta) V^T,
```

where `V` is the stable descending eigenbasis of `W0^T W0` and `B` rotates
all adjacent eigenvector pairs by the same angle, evaluate the two complete
frames

```text
F_plus  = F Q(theta)
F_minus = F Q(theta)^T.
```

The complete correction is

```text
d(theta) = 0.5 * (mu_plus + mu_minus) - mu_q0.
```

For a matched antipodal line, use the same expression before averaging over
lines.  The opposite rotations cancel the first angular term pointwise.  The
finite-population variance being sampled is therefore a second-order
curvature observable, not the first-order far-frame difference killed by M1
and M2A.

## Prior-art preflight

Capsule searches covered `near-identity`, `small rotation`, `symmetric
rotation`, `antithetic rotation`, `angular Killing`, `matched frame`,
`finite-population`, `right-Gram`, and the complete-frame capture sites.

Nearest controlled work:

- `angular_killing_control_20260801` propagated exact infinitesimal angular
  derivatives through K16/K32 and failed.  It did not evaluate opposite
  finite rotations, a complete K129 pair, or cancellation before line
  sampling.
- `fullframe_weight_rotation_20260731` evaluated single far rotations and
  sparse complete bases.  It did not evaluate a geodesically paired
  near-identity curvature measure.
- matched-frame M1 sampled whole basis differences from one far frame; M2A
  sampled antipodal lines from that same first-order difference.  Both had
  excessive variance.  They do not bound the variance after pointwise
  `+theta/-theta` cancellation.
- four-frame and polar mixtures establish that complete-orientation
  cancellation has real capacity, but their literal sparse descendants do
  not implement this curvature observable.

Outcome: **materially new observable in the capsule**.

## Frozen screen

- Rows: the first 16 sealed Full rows and first 16 sealed Generated rows from
  `matched_right_stratified_lines_m2a_r1_targetfree_20260809.npz`.
- Angles: `pi * (1/64, 1/32, 1/16, 1/8, 1/4, 1/2)`.
- Endpoint rule: R94 constants, lambda `0.019` and output scale
  `1.0000140859542768`.
- Line counts: `1,2,4,8,12,16,24,32,48,64,96,128,192,256`.
- One shared positive shrinkage coefficient is optimized only after the
  target-free arrays are sealed.  It is constrained to `[0,1]`.
- Generated label-noise MSE is subtracted exactly as in prior broad screens.

## Exact risk and cost projection

For row error `e`, complete correction `d`, and exact finite-population
sampling variance `v_l`, the conditional expected MSE is

```text
MSE(alpha,l) = ||e||^2 + 2 alpha <e,d>
               + alpha^2 (||d||^2 + v_l).
```

Sampling `l` antipodal lines in every basis for both rotations costs

```text
2 * 129 * l / 256
```

complete-basis equivalents.  The screen uses the conservative projection

```text
extra effective B = 5 + 0.999 * equivalents.
```

The 5B intercept includes the eigenframe and irregular-row handling.  This is
a projection, not a physical receipt.

## Target ceiling and gates

At R94's projected score `1.11463864845e-7` and effective reference
`137.8297337635B`, a 16-line paired arm adds about 21.1B and needs a raw ratio
near `0.625` to reach `8e-8`.  A 32-line arm adds about 37.2B and needs a raw
ratio near `0.565`.  Small improvements cannot justify implementation.

Promote to a broad target-free capture only if at least one fixed angle:

1. gives complete-pair raw ratio at most `0.75` separately on Full and
   Generated;
2. gives a line-sampled expected raw ratio at most `0.70` on both families at
   no more than 40 complete-basis equivalents;
3. projects to at most `9.0e-8` on both families on this small screen; and
4. improves at least 11/16 rows in each family in conditional expected risk.

The `9e-8` scout gate deliberately leaves margin before the required broad
and exact-Mini100 `8e-8` gate.  Failure closes this fixed adjacent-eigenplane
pair, not every possible antithetic rotation generator.

