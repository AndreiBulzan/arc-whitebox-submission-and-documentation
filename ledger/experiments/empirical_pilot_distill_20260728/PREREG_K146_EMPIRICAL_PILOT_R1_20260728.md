# K146 empirical hidden-pilot distillation R1

Date: 2026-07-28  
Evidence sought: target-free capture plus `component` development score;
all compute numbers remain `projection`.

## Fixed question

The width-compression cliff may be caused by a mismatched analytic
post-ReLU covariance teacher, rather than by irreducible hidden dimension.
R1 carries one exact full-width Kerdock basis from each quadrature arm
(`512 + 512` signed rows) alongside the compressed cloud through layers
12--21.

At each transition after layer 12, the pilot supplies the exact
full-path next preactivation `Y`.  From the current compressed pilot state
`X`, R1 fits the target-free weighted affine map

```text
xbar = weighted_mean(X)
ybar = weighted_mean(Y)
A = solve(Xc.T @ D @ Xc + ridge I, Xc.T @ D @ Yc)
b = ybar - xbar @ A
z_main = h_main @ A + b
```

where the two pilot bases receive their literal outer-arm masses
`129/146` and `17/146`.  The ridge is fixed at
`1e-4 * trace(Xc.T @ D @ Xc) / width`.  No target, target-derived
coefficient, row score, or prior prediction enters the fit.

The high pilot is propagated independently through the exact analytic
232-wide screens.  The low support at layers 12--20 remains the current
analytic second-moment energy support.  Layer 21 rejoins the existing
200-wide screen, after which the incumbent late graph and literal
`129:17` readout are unchanged.

## Fixed rows and candidates

Targets remain unopened until every prediction and hash is sealed.
These rows are disjoint from the conditional-Schur R1 development rows.

```text
Full       331, 337, 347, 353
Generated   73,  79,  83,  89
```

```text
mean_energy_w216       associated incumbent control
mean_energy_w192       mean-fold compression comparator
pilot_energy_w216      empirical-pilot mechanism control
pilot_energy_w192      checkpoint candidate
pilot_energy_w176      deeper compression candidate
```

The pilot rows are fixed:

```text
O0 local basis 0: global rows     0..511
O1 local basis 0: global rows 66048..66559
```

## Association and advance gates

Before the grid runs, the shared `mean_energy_w216` implementation on
Full0 must be bit-associated to the sealed incumbent reference under the
existing relative-RMSE `2e-6` and maximum-absolute `3.2e-5` bounds.

The pilot mechanism passes only if `pilot_energy_w216`, against
`mean_energy_w216`, has in each family:

- pooled MSE ratio `<= 1.01`;
- no row ratio above `1.25`.

The narrowed candidates are both reported.  `pilot_energy_w192` advances
only if both family pooled ratios are `<=1.02` and no row ratio is above
`1.40`.  `pilot_energy_w176` advances only if both family pooled ratios
are `<=1.03` and no row ratio is above `1.40`.  These limits target the
approximately `1.2e-7` adjusted checkpoint after lawful pilot overhead,
not merely parity with the uncompressed raw error.

## Lawful operation projection

The ordinary width-dependent graph starts from the released direct-op
projection.  R1 additionally prices, for pilot size `p=1024`, each exact
high-pilot transition, weighted centering, normal equations, ridge solve,
intercept, and main offset addition using released elementwise and matmul
rules.  It also prices the pilot gathers.  Nothing is assigned zero cost.

The projection is deliberately conservative: it retains all incumbent
mean-fold setup work even where the empirical affine fit replaces it.
No wall saving is inferred from the operation count, and no projection is
called a physical receipt.

## Boundaries

The shared benchmark lock is mandatory for CUDA capture. Capture opens
only weights and seeds. A separate scorer validates the exact prediction
seal before opening targets. No physical row, FlopScope execution,
package, network, remote action, upload, submission, or `STATUS.json`
mutation is authorized.
