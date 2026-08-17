# K146 empirical hidden-pilot distillation R1b

Date: 2026-07-28  
Evidence sought: target-free capture plus `component` development score;
all compute numbers remain `projection`.

## Correction and fixed mechanism

R1b is the identity-corrected execution of R1. R1 accidentally selected
two positive half-bases per arm because `exact_front_m17` stores all
positive half-bases before all negative half-bases. R1b independently
hash-pins that layout source and selects exactly one complete signed
Kerdock basis from each arm:

```text
O0 basis 0: positive global rows     0..255
            negative global rows 33024..33279
O1 basis 0: positive global rows 66048..66303
            negative global rows 70400..70655
```

The resulting `512 + 512` pilot rows have the same lawful cost as R1.
They receive literal outer-arm masses `129/146` and `17/146`.

At layers 13--21, the current compressed pilot `X` is affinely regressed
onto the exact full-width pilot's next preactivation `Y`:

```text
A = solve(Xc.T @ D @ Xc + ridge I, Xc.T @ D @ Yc)
b = weighted_mean(Y) - weighted_mean(X) @ A
z_main = h_main @ A + b
```

The ridge remains fixed at
`1e-4 * trace(Xc.T @ D @ Xc) / width`. Energy supports, the exact high
pilot, the late graph, and the literal `129:17` readout are unchanged.
No target or target-derived value enters the fit.

## Fresh fixed rows and candidates

Targets remain unopened until every R1b prediction and hash is sealed.

```text
Full       359, 367, 373, 379
Generated   97, 101, 103, 107
```

```text
mean_energy_w216
mean_energy_w192
pilot_energy_w216
pilot_energy_w192
pilot_energy_w176
```

## Fixed gates

Full0 incumbent association must pass relative RMSE `<=2e-6` and maximum
absolute error `<=3.2e-5`.

`pilot_energy_w216` passes the mechanism gate only if, in both families,
its pooled MSE ratio to `mean_energy_w216` is `<=1.01` and its maximum row
ratio is `<=1.25`.

`pilot_energy_w192` advances at pooled ratios `<=1.02` and maximum row
ratios `<=1.40`; `pilot_energy_w176` advances at pooled ratios `<=1.03`
and maximum row ratios `<=1.40`, separately in both families.

All candidates are reported. There is no post-target fit, routing, or
candidate deletion.

## Cost and boundaries

R1b reuses R1's conservative released-rule projection, including exact
high-pilot propagation, gathers, weighted centering, normal equations,
solve, intercept, and main offset additions. No operation is assigned
zero cost and no wall claim is made.

The benchmark lock is mandatory. Capture opens only weights and seeds; a
separate seal-verifying scorer opens targets afterward. No physical row,
FlopScope execution, package, network, remote action, upload, submission,
or `STATUS.json` mutation is authorized.
