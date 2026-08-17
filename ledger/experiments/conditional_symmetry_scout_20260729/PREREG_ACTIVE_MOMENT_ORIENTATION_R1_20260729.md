# Active-moment orientation R1 preregistration

Date: 2026-07-29

Evidence scope: target-free **component** capture followed by a separate
post-seal signed-final-preactivation score.  No FlopScope session, physical
row, GPU run, package, upload, submission, or remote action.

## New mechanism

The Gaussian input law is orthogonally invariant, but a finite Kerdock rule is
not.  R1 uses the supplied weights to orient the existing deterministic
quadrature toward the network's active input subspace without changing its
row count.

For each MLP:

1. A weights-only diagonal-Gaussian gate rollout forms the mean-gated
   input-to-final-signed Jacobian.
2. Its top eight left singular vectors define the active input subspace.
3. For each of the eight already-frozen Kerdock orientations, measure the
   fourth spherical moments of both its complete 129-basis rule and its fixed
   production S17 subset in that active subspace.
4. Among the 56 ordered distinct orientation pairs, choose the literal
   `129:17` pair whose combined diagonal and pairwise fourth moments are
   closest to the exact uniform-sphere values:

   ```text
   E[u_i^4]       = 3 / (d(d+2))
   E[u_i^2 u_j^2] = 1 / (d(d+2)), i != j.
   ```

5. Read the already-frozen per-basis signed endpoints only after the pair is
   fixed and form their literal `129:17` mean.

There is no target-fitted coefficient, pilot propagation, score-based
selection, row-count change, or accounting device.  This differs from the
killed first-layer QR rule, gate-feature herding, and pilot-consensus
orientation router: it matches an exact integration identity in the
weight-conditioned active subspace.

## Fixed rows and gate

```text
Full       0, 1, 50, 51, 100, 101, 150, 151
Generated  0, 1, 32, 33, 64, 65, 96, 97
rank       8
primary    any complete orientation
secondary any distinct orientation, fixed S17
```

Freeze selected pairs and signed predictions before opening
`target_final_premean`.

Promote only if all of the following hold on both families:

- pooled signed MSE ratio to the fixed O0/S17-O1 control `<= 0.50`;
- row-ratio p95 `<= 1.50`;
- at least `75%` of rows improve.

Otherwise kill this active-moment orientation rule without a broader capture
or runtime build.

## Deployment boundary

The target-free selector's dense arithmetic is projected below `6B` per MLP
(mean-gated Jacobian plus eight rank-eight node projections), so it would fit
under the `272B` cap when added to the current K146 graph.  This is a
projection, not a FlopScope receipt; accuracy is controlling at this stage.
