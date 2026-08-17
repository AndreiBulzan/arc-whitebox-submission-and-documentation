# K146 conditional-Schur fold R1

Date: 2026-07-28  
Evidence sought: target-free capture plus `component` development score;
all compute numbers are `projection`.

## Fixed question

The live K146 graph keeps `S` from each analytic 232-wide screen at layers
12--20 and currently replaces omitted `O` only by its analytic mean. R1
tests the target-free conditional expectation

```text
h_O ~= mu_O / sphere + (h_S - mu_S / sphere) B
B = solve(Cov_SS + ridge I, Cov_SO)
W_eff = W_S + B W_O
offset = (mu_O / sphere - (mu_S / sphere) B) W_O
z = h_S W_eff + offset
```

`Cov` is the symmetrized analytic post-ReLU covariance already propagated by
the screen closure. The fixed ridge is
`1e-4 * mean(diag(Cov_SS))`; it is target-independent. No coefficient is
fitted.

## Fixed rows

Targets remain unopened until all predictions and their hashes are sealed.
These are development component rows, not a confirmation bank.

```text
Full       241, 251, 257, 263
Generated   28,  31,  34,  39
```

## Fixed candidates

The control is the associated current mean-only width-216 graph.

```text
mean_energy_w216        current mean fold, energy subset
schur_energy_w216       conditional fold, energy subset
schur_energy_w192       conditional fold, energy subset
schur_obsvar_w192       conditional fold, variance*observability subset
schur_greedy_w192       conditional fold, affine-residual-greedy subset
schur_greedy_w184       same nested greedy order, width 184
schur_greedy_w176       same nested greedy order, width 176
```

The observability is the existing target-free backward squared-weight/gate
recursion. The covariance-aware greedy order minimizes the next-affine
conditional residual at each source layer. Starting from residual covariance
`R=Cov`, aligned next right `W`, and target-free downstream diagonal `Q`, it
selects

```text
p = argmax_i || Q^(1/2) W^T R[:, i] ||^2 / R[i, i]
R <- R - R[:, p] R[p, :] / R[p, p]
```

and uses the first `176`, `184`, or `192` pivots. The next-layer output
screen and analytic downstream observability are fixed before the candidate
cloud is run. No target, row score, or candidate prediction enters selection.

Everything outside layers 12--21 remains the K146/m17 lambda-zero graph:
support17, H2 repair, L4 snap, screens, late armwise width 192, final width
176 plus mean restoration, gamma readout, and literal `129:17` blend.

## Association and advance gates

Before the grid runs, `mean_energy_w216` on Full0 must associate with the
sealed R17 GPU/physical baseline at relative RMSE `<=2e-6` and maximum
absolute error `<=3.2e-5`.

The Schur mechanism is viable only if `schur_energy_w216`, relative to the
mean-only control:

- has pooled MSE ratio `<=1.02` in each family;
- has no row ratio above `1.35`.

A narrowed candidate advances only if, in each family:

- pooled MSE ratio to control is `<=1.05`;
- at least two of four rows improve or tie;
- no row ratio exceeds `1.50`.

All candidates and both families are reported regardless of gates. There is
no post-target coefficient fit, row routing, or silent candidate deletion.

## Lawful cost projection

The changed sampled graph uses FlopScope's released matmul rule
`M*N*(2*K-1)`. The existing direct-operation projection for a layer-12
middle gives steady counted work:

```text
width 216   144.013215420B
width 192   130.737337812B
width 184   126.618225372B
width 176   122.652202980B
```

For every source layer with retained `s`, omitted `o=232-s`, and next width
`t`, R1 additionally prices:

```text
solve:        2*s^3/3 + 2*s^2*o
B @ W_O:      s*t*(2*o-1)
mu_S @ B:     o*(2*s-1)
W_eff add:    s*t
offset sub:   o
```

The final offset product replaces the incumbent mean-fold product and is not
double-counted. Covariance slicing is priced by its moved element count.
The greedy selector receives a separate conservative pointwise/matmul
projection; it is not called free. These are projections, not a physical
FlopScope receipt, and they do not price a wall saving.

## Boundaries

The shared benchmark lock is mandatory for CUDA capture. Capture opens only
weights and seeds. A separate scorer validates the exact prediction seal
before opening targets. No physical row, FlopScope execution, package,
network, remote action, upload, submission, or `STATUS.json` mutation is
authorized.
