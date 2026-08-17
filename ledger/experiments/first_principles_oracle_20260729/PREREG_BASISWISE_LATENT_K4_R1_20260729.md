# Basiswise latent-K4 R1 preregistration

Date: 2026-07-29

Evidence scope: target-free CUDA capture followed by a separate post-seal
two-Full/two-Generated component score.  No FlopScope session, physical row,
package, upload, submission, or remote action.

## New observable

The killed production PTCC experiment pooled every particle in an arm into
one empirical law.  That is not neutral for a structured Kerdock quadrature:
the current estimator averages complete 512-row basis estimates, and its
accuracy depends on cancellations between those basis estimates.

R1 preserves that quadrature boundary:

1. At the existing layer-30-hidden/final-weight product, form one shared
   rank-eight covariance eigenspace per arm from the complete arm cloud.
2. Inside each complete Kerdock basis separately, center its 512 hidden
   particles, project them to that shared rank-eight space, and contract the
   latent particles through the actual final weight matrix.
3. Compute the scalar third/fourth cumulants and bounded marginal Edgeworth
   correction separately for every basis and final output.
4. Subtract the same basis's direct empirical Edgeworth readout, add the
   latent correction to the literal basis post-ReLU mean, enforce the exact
   per-basis Jensen/second-moment bounds, and only then average bases.
5. Blend the two arms by their literal `129:17` support weights.

Two fixed readouts are frozen:

- `k4_only`: retain the basis's direct empirical K3 and replace only K4 by
  the shared-subspace latent K4;
- `k3_k4`: replace both basis K3 and K4 by their latent values.

There is no fitted coefficient, target-dependent choice, row routing, or
family-specific spelling.

## Rows and hard gate

```text
Full       0, 100
Generated  0, 64
```

Kill a readout immediately if either family's pooled observed final-layer
MSE exceeds the unchanged K146 control.  Promote to a 16+16 falsifier only
if one identical readout has:

- pooled observed ratio `<=0.75` in both families;
- every row ratio `<=1.50`;
- finite outputs and every basis/arm correction inside the exact bounds.

## Deployment arithmetic projection

The operation graph reuses the already-computed hidden arm and final
preactivation arrays.  It adds, per arm: one centered hidden Gram, one
rank-eight basis extraction, hidden-to-rank projection, rank-to-final
projection in output segments, and scalar moment reductions grouped by
complete basis.  The capture receipt records the exact ordinary-real
multiply/add/subtract subtotal for this declared graph and separately names
the classical symmetric-eigensolver allowance.  It is a projection, not a
FlopScope receipt.

No new ordinary result need exceed one `(signed_rows,64)` float64 segment;
the receipt records that byte ceiling explicitly.

