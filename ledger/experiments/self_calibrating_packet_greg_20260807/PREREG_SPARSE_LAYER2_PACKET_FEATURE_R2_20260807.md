# Sparse exact layer-two packet feature R2 preregistration

Date: 2026-08-07

## Question

Do a small number of exact, weight-selected layer-two neuron responses expose
the 8--16 low-dimensional *quenched* candidate-output directions that the R1
universal and exact-first-layer NNGP feature spans missed?

R1 established target-free that the best old span explained only `7.257%` of
within-line output variance against a `70%` gate, while the actual candidate
outputs place `73.69%` of their energy in eight output modes and `85.20%` in
sixteen.  This licenses a materially new actual-weight/gate observable, not a
larger NNGP rank sweep.

## Prior-art and reopen boundary

Capsule searches covered partial/sparse layer-two propagation, probe neurons,
neuron leverage, spectral pruning, shadow networks, absolute-preactivation
features, and branch-swap features.  No controlled attempt computes exact
candidate-specific responses of a frozen subset of second-layer neurons for
the Gaussian packet pool.  Prior H1/H2 moment repairs and pooled gate-state
students summarize the existing propagated frame; they do not observe the
off-frame packet candidates.  This round is `novel in capsule` and uses a
materially new observable.

## Fixed population

Use the same `m=8`, 33,024-line, independent-noise packet construction,
normalization, Full `640..647`, and Generated `88..95` rows as packet GREG R1.
Recreate complete candidate outputs only in memory.  Associate finite-pool
means with the sealed output-oracle receipt to maximum absolute error `2e-6`.

## Probe selection

Network propagation uses row vectors `h @ W`.  Define a target-free
downstream squared-sensitivity score for every layer-two activation coordinate:

```text
s = ones(256) / 256
for layer = 31 down to 2:
    s = 0.5 * (W[layer]**2) @ s
```

The factor `0.5` is the isotropic ReLU gate probability.  Select the 32
largest coordinates of the resulting `s`, using coordinate index as the
deterministic tie-break.  The 8- and 16-probe sets are nested prefixes.

For every candidate preactivation `z = y @ W[0]`, form both first-layer
branches `h+ = ReLU(z)` and `h- = ReLU(-z)`.  For selected layer-two neuron
`j`, compute exactly:

```text
q+_j = ReLU(h+ @ W[1][:,j])
q-_j = ReLU(h- @ W[1][:,j]).
```

No NNGP or downstream approximation enters these features.

## Fixed feature spans

Each raw feature is centred within its line and scaled by its pooled RMS.
If a selected raw feature has RMS at most `1e-14`, retain that column as
identically zero, record the degeneracy, and do not replace or reselect its
probe neuron.  This preserves the frozen nested support while allowing the
minimum-norm Gram solve to expose the reduced effective rank.
The base feature is the complete branch-symmetric first preactivation:

```text
H1A = abs(z)                         # 256 features
```

For each probe neuron add three branch-swap-invariant features:

```text
qsum   = q+ + q-
qdiff2 = (q+ - q-)**2
qprod  = q+ * q-.
```

Test exactly four nested spans:

- `H1A`: `r=256`;
- `H1A_L2P8`: `r=280`;
- `H1A_L2P16`: `r=304`;
- `H1A_L2P32`: `r=352`.

This deliberately tests a generous feature ceiling.  A capacity failure
kills cheaper projections of the same raw features.  Maximum absolute
within-line feature-sum drift must be at most `2e-10`.

## Oracle and gates

Use the exact `B_star` calculation and conditional independent-selection
variance from packet GREG R1:

```text
delta_random = sum ||V||^2 / (256 * M^2 * 8)
delta_oracle = sum ||V - X B_star||^2 / (256 * M^2 * 8)
R2_oracle = 1 - delta_oracle / delta_random.
```

A span passes only if all hold:

- pooled `delta_oracle <= 4.0e-8`;
- Full and Generated `delta_oracle <= 4.8402381038e-8` separately;
- pooled `R2_oracle >= 0.70`;
- at least 12/16 networks have `R2_oracle >= 0.60`;
- all association, centring, finite-value, and Gram-rank checks pass.

Only an oracle-capacity pass licenses a separate one-observation-per-line
cross-fit.  R2 itself does not simulate cross-fitting and opens no targets.

If `H1A_L2P32` fails, kill exact first-layer plus up-to-32 sparse second-layer
probe features.  The next admissible route must obtain later/deeper actual
network behavior: a separately gated width-8/16 actual-weight shadow-tail
oracle or complete progressive propagation.  Do not add more layer-two probe
neurons by routine sweep.

## Evidence boundary

This is target-free **component** evidence on a reused research bank.  No
FlopScope session, physical row, estimator implementation, production compute
claim, package, upload, or submission is authorized.
