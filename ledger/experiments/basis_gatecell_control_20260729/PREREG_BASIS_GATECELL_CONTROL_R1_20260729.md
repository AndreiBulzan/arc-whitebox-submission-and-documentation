# Basis-level nonlinear gate-cell control R1

Date: 2026-07-29.

Scope: target-free CUDA component capture and separate post-seal 2+2
development score.  No FlopScope, physical row, package, upload,
submission, or remote action.

## Distinct estimator

The previous two control scouts fit nodewise covariance.  Kerdock
quadrature error is the error of complete-basis replicate means, so R1
changes the statistical object:

1. compute the same 256 actual nonlinear-pilot input gradients;
2. take the top four input directions of their weights-only SVD;
3. evaluate four exact-mean ReLU ridge controls;
4. average output and controls inside each complete 512-point basis;
5. fit the output/control map across even basis replicates and apply it
   only to odd replicates, then swap.

The coefficient is therefore optimized for integration error rather than
pointwise function approximation.  Four features leave four or more
training bases even at K16.  Ridge is fixed to `1e-3` times the mean
basis-Gram diagonal.

## Gate and price

Rows are Full `{4,104}` and Generated `{4,68}` at K `{16,32}`.  Promote
only at pooled ratio `<=0.50` in both families with every row `<=1.25`.
Otherwise close the control-functional seam.

The pilot adjoint, four feature projections, basis reductions, and small
fits project below `2B` additional ordinary-real operations at K146.  This
is not a FlopScope receipt.
