# Preregistration: sparse matched Poisson probe R3

Date frozen: 2026-08-06

## Question

Can the passed 832-complete-cloud Poisson teacher be estimated directly from
one small, structured probe population rather than distilled from insufficient
q0 summaries?

R1 and R2 showed that neither pooled gate/margin state nor archived per-basis
final/L4/L8 means contain the signed low-band correction across families.  R3
adds a new observable: matched off-design function values.  It is a direct
sparse approximation of the successful semigroup oracle, not a learned
residual and not another reweighting of existing Kerdock values.

## Frozen sparse rule

For each of 64 diagnostic replicate seeds:

1. draw one Haar orthogonal frame with base seed `2026080641 + 1000003*r`;
2. draw `j` uniformly from `0..127` after the frame and Latin shift;
3. retain frame rows `j` and `j+128`;
4. retain the corresponding two shifted-Latin quantiles, separated by one
   half-cycle;
5. for every one of the 129 Kerdock bases, perturb those two canonical lines
   with the exact spherical Poisson kernel;
6. include both antipodes.

At each of the 13 frozen R2 radii this is `2*129*2 = 516` trajectories.  One
two-direction probe therefore adds `13*516 = 6,708` trajectories, 10.156% of
the 66,048-row q0 population.  The matching 516 canonical-center values are
assumed to be reused from the already paid q0 pass; the oracle capture may
recompute them only to form an exact offline matched difference.

The primary spelling is diagnostic replicate 0, frozen before capture.  The
other 63 replicates estimate seed robustness and may not be used to select a
better seed.  The one-direction halves are reported as cost/variance
diagnostics only; the primary candidate averages both orthogonal directions.

## Frozen inversion and label

For every network, output neuron and sparse replicate, fit

```text
mean_sparse[f(P_rho u) - f(u)]
    = sum_g c_g (b_g(rho) - 1)
```

with exactly the R2 four band shapes, ridge 1.0 and the per-radius weights
sealed by the 40-network complete-cloud teacher.  The degree-6--20 coefficient
is the sparse correction.  No target is involved.

Compare it with the already sealed complete-cloud low-band correction on the
same Full20 and Generated20 rows.

## Target-free gates

The fixed replicate-0 two-direction correction must satisfy, independently
on Full and Generated:

- teacher-space MSE ratio `<= 0.50`;
- Pearson correlation `>= 0.65`;
- sign agreement on above-median-absolute teacher entries `>= 0.65`;
- maximum absolute correction `<= 0.01`.

Robustness gates:

- median two-direction replicate teacher-space MSE ratio `<= 0.65` in each
  family;
- at least 75% of the 64 replicate corrections have pooled MSE ratio `<=0.80`.

Passing establishes only target-free component capacity.  The 10.156% row
ratio is not a counted-compute receipt: a separately pinned R87-compatible
transport and FlopScope audit must establish whether the probe fits the 15B
incremental ceiling.  No estimator, package, upload, score claim, or remote
submission is authorized here.

