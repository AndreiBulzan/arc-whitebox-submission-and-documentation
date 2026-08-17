# Covariance-diagonal polarization K4 R1 preregistration

Date: 2026-07-29

Evidence scope: offline **component** falsifier on the already-captured K146
layer-30 state.  No FlopScope session, physical/timed row, benchmark lane,
package, upload, submission, or remote action.

## Distinct hypothesis

The exact-teacher PTCC experiment showed that a rank-eight symmetric
fourth-order core in the covariance eigenspace recovers the fully mixed
preactivation K4 query (`0.9943` correlation), but its unrestricted core has
`C(11,4)=330` degrees of freedom.  The same fit on the production K146 cloud
worsened both families.

R1 tests whether that transfer failure is overfitting of an unrestricted
empirical core.  Constrain the core to the eight-dimensional
orthogonally-decomposable form

```text
K4(i,j,k,l) = sum_s lambda_s P[i,s] P[j,s] P[k,s] P[l,s],
```

where `P` is the top-eight covariance eigenspace of the same arm.  Fit only
the eight amplitudes `lambda_s`, jointly and without targets, from both
empirical repeated connected views:

```text
c31(i,j) ~= sum_s lambda_s P[i,s]^3 P[j,s]
c22(i,j) ~= sum_s lambda_s P[i,s]^2 P[j,s]^2.
```

The two blocks are divided by their own RMS before one fixed ridge solve
(`damp = 1e-3`).  For each realised final-weight column `w_o`, query

```text
kappa4[o] = sum_s lambda_s (P[:,s]^T w_o)^4.
```

This is not the killed literal pair contraction, independent SVD-triplet
interpretation, or unrestricted PTCC core.  It is a jointly fitted,
globally symmetric eight-parameter latent-factor model.

The candidate changes only the fourth-order part of the existing arm
readout:

```text
current_arm
  + bounded_Edgeworth(empirical_K3, covdiag_K4)
  - bounded_Edgeworth(empirical_K3, direct_empirical_K4),
```

then enforces the existing armwise Jensen/second-moment interval and uses the
literal `129:17` arm weights.  Coefficient one follows from the replacement
identity; there is no target-fitted scalar.

## Fixed screen

Reuse the frozen production-state capture:

```text
Full       0, 1, 100, 101
Generated  0, 1, 64, 65
```

The method is useful for the immediate checkpoint only if the identical
spelling satisfies all of:

- Full pooled raw-MSE ratio `<= 0.84`;
- Generated noise-corrected pooled ratio `<= 0.84`;
- every observed row ratio `<= 1.50`;
- finite outputs and exact armwise ReLU bounds;
- on the four Full rows, exact final-preactivation K4 query correlation
  `>= 0.80` and relative RMSE `<= 0.60`.

The `0.84` accuracy threshold is economic.  Adding the three required
width-176 Grams to the remote `170.3B` anchor projects about `184.2B`
effective work.  At that price, adjusted `1.2e-7` requires raw MSE
approximately `<=1.77e-7`, or about `0.85` of the current `2.09e-7`.

Failing either family kills this exact constrained-core spelling.  No rank,
basis, damping, blend, or residual-pair sweep follows.

