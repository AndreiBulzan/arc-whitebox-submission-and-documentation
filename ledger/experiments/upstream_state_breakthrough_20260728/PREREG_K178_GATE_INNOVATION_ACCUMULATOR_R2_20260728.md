# K178 analytic gate-innovation accumulator R2

Date: 2026-07-28

Evidence: target-free fixed 4+4 capture, then one post-seal component score.
No physical row, package, upload, submission, or remote action.

## Structural repair

R1 injected the complete S109-minus-S49 pilot difference at every layer,
double-counting the part already propagated from earlier layers. R2 keeps
the same 13 positive plus 13 antithetic rows and `<=8B` cost boundary, but
injects only the newborn gate innovation.

Let `c_(l-1)` be the total desired post-ReLU mean correction carried from
the previous layer. For weight `W_l`:

```text
p_l = c_(l-1) W_l
z0   = z_coupled - p_l
d_l  = mean(ReLU(z0)_S109) - mean(ReLU(z0)_S49)
g_l  = mean(ReLU(z0_S49 + p_l) - ReLU(z0_S49))
u_l  = d_l - g_l
```

After the ordinary ReLU, add the shared coordinate vector `u_l` to both the
paid S49 cloud and missing-basis pilot, then set `c_l=d_l`.

The pilot-derived `z0` removes the propagated prior uniform correction.
`g_l` is the analytic empirical gate response of that prior correction.
Therefore `u_l` contains only the new support-specific ReLU birth.

## Required algebra checks before weights

1. If `c_(l-1)=0` and `d_l=0`, then `u_l=0`.
2. For an identity activation, if `d_l=c_(l-1)W_l`, then `g_l=d_l` and
   `u_l=0`; a linearly propagated difference is never injected twice.
3. For literal ReLU, `g_l + u_l = d_l` to floating tolerance by
   construction.

The capture must run and record these checks before opening any MLP weights.

## Everything else frozen

- Full rows `0,1,2,3`; Generated rows `2,4,5,6`.
- S49/S109 and their 47-basis overlap are unchanged.
- Positive row IDs are `floor(j*256/13)`, `j=0,...,12`.
- Coefficient is one; no fit, clipping, checkpoint choice, or support route.
- The production attachment remains:

```text
q1_hat = q1_K178 + (q1_coupled_literal - q1_baseline_literal)
candidate = (129*q0_K178 + 109*q1_hat)/238.
```

## Hard stop

After sealing, stop this lane unless candidate/K178 final target MSE is
`<=1.10` on both Full4 and Generated4, every value is finite, and no row
ratio exceeds `2.0`. The stronger `<=0.82` checkpoint threshold is reported
but is not required merely to keep the mechanism alive.

## Cost projection

R1 projected `7.034077184B`. R2 adds one length-256 vecmat per layer and
pilot-sized subtraction/ReLU response arithmetic. Conservative incremental
projection:

```text
R1                                           7.0341 B
32 correction vecmats                          0.0042 B
pilot reconstruction and gate response         0.0410 B
------------------------------------------------------
R2                                            7.0793 B
```

Movement, calls, residual, wall, and integration remain unmeasured.
