# Preregistration — query-adapted marginal K3/K4 transport

Evidence label: **component**. This is an offline teacher-forced transition
study. It is not a whole estimator, FlopScope receipt, package, upload, or
remote action.

## Question

Given the exact inference-time *kind* of state

- post-ReLU mean and covariance;
- repeated connected views
  \(C_{21,ij}=\kappa(h_i,h_i,h_j)\),
  \(C_{22,ij}=\kappa(h_i,h_i,h_j,h_j)\), and
  \(C_{31,ij}=\kappa(h_i,h_i,h_i,h_j)\);
- the next realized weight columns;

can the next-layer marginal preactivation K3/K4 be recovered accurately
without constructing either \(W^{\otimes 3}\), \(W^{\otimes 4}\), or a
universal four-index tensor?

The exact pair-supported contractions are frozen as

```text
k3_pair =
    3 (w^2)^T C21 w - 2 sum_i C21_ii w_i^3

k4_pair =
    4 (w^3)^T C31 w
  + 3 (w^2)^T C22 (w^2)
  - 6 sum_i C22_ii w_i^4
```

They include every term involving at most two distinct source indices and
omit the all-distinct and `iijk` terms.

The sole spectral arm interprets low-rank repeated views as observations of
a symmetric CP tensor. For a singular triplet `(u, s, v)` of `C21` or
`C31`, order `p=3` or `4`, it uses

```text
alpha = s * <u, v^(p-1)> / ||v^(p-1)||^2
query = alpha * (<v, w>)^p
```

summed through ranks `1, 2, 4, 8, 16`. This identity is exact for a single
symmetric rank-one cumulant component. It is only a hypothesis for a
mixture.

## Frozen rows and layers

Whole-MLP splits:

```text
train       Full rows   0..63
diagnostic  Full rows  64..95
sealed eval Full rows 128..191
```

The earlier exploratory work touched only rows `0..47`, which are entirely
inside the training split. Diagnostic and evaluation predictions must be
frozen together before either split's `pre_*` cumulant labels or
`official_alm` members are opened.

Milestone target layers are `1, 3, 7, 15, 23, 31`; each uses the exact
post-moment views at the preceding layer and the actual next weight matrix.
Layer zero is outside this transition question.

All Gaussian-radius moments are exactly de-mixed before connected moments
are formed. The final predicted ReLU mean is multiplied by
`q1 = E[R/sqrt(256)]` to return to the challenge's Gaussian input law.

## One bounded learned correction

After the literal pair and rank-16 spectral arms are measured, one shared
ridge map is permitted. It is trained only on rows `0..63`. Inputs are the
pair components, cumulative spectral queries, query mean/variance and
weight invariants, analytic Edgeworth sensitivity products, and a quadratic
depth coordinate. It has three outputs:

1. standardized marginal K3;
2. standardized marginal K4;
3. the standardized combined ReLU correction.

This is one linear shared map, not a per-network or per-neuron fit. Ridge
strength is frozen at `1.0` after feature standardization. The direct
correction head is reported separately from the forced K3/K4 Edgeworth
head. Every output is clipped to exact Jensen/second-moment bounds.

## Gates and interpretation

- Report marginal K3/K4 RMSE, correlation, and Edgeworth-sensitivity-weighted
  error by depth.
- Report one-step teacher-forced ReLU-mean MSE by depth, especially layer 31.
- Compare with the exact marginal oracle, Gaussian, literal pair, and
  spectral arms.
- The current first-place raw requirement is compute-conditioned:
  approximately `1.24e-7` at the `27.2B` multiplier floor, `1.05e-7` at
  `32B`, `5.27e-8` at `64B`, and `3.12e-8` at `108B`.
- The `3e-8` level is a stretch/oracle bar, not an unconditional kill.
- Independent Test500 has marginal moments but no repeated connected views,
  so it is not a compatible transfer bank for this transition.
- Even a pass is only local transition capacity. A closed recurrence that
  produces the next post repeated views, a free rollout, participant-lawful
  implementation, and a physical price remain open.

