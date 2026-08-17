# Preregistration: complete-frame polar barycenter R1

Date: 2026-08-04

Evidence sought: **component** on the old Full16/Generated16 pilot and
**broad statistical** transfer evidence on official Mini100.  This experiment
does not use FlopScope, build a package, or perform a remote action.

## Prior-art preflight

The capsule was searched for `geodesic`, `Cayley`, `polar interpolation`,
`frame midpoint`, `frame barycenter`, `matrix log/exp`, `blend rotation`, and
the equivalent operation “replace an average of complete rotated cubatures by
one complete cubature in an averaged orthogonal frame.”  The nearest results
are:

- complete right-, left-, depth-2-, and depth-2-right frames;
- fixed partial-frame mixtures and static endpoint coresets;
- adaptive orientation selectors; and
- cheap H2/D4/D6 predictors of complete-frame differences.

None constructs one orthogonal frame as the polar factor of a sum of complete
frames.  This attempt is therefore **novel in the capsule**.  It does not
reopen a killed partial-support rule: every candidate propagates one complete
K129 spherical design and preserves its exact low-order cubature identities.

## Fixed construction

For each MLP let `Q0`, `Qr`, `Qd2`, and `Qd2r` be the already-defined complete
q0, right-Gram, depth-2 `J J^T`, and depth-2 `J^T J` orthogonal frames.  For a
fixed nonnegative coefficient vector `a`, define

```text
M(a) = sum_i a_i Q_i
Qbar(a) = polar(M(a)) = U V^T,  M(a) = U S V^T.
```

The following candidates are frozen before any target is read:

```text
q0                    ordinary q0 control
polar_q0_right        a = (1/2, 1/2, 0, 0)
polar_q0_d2right      a = (1/2, 0, 0, 1/2)
polar_q0_right_d2     a = (1/3, 1/3, 1/3, 0)
polar_four_equal      a = (1/4, 1/4, 1/4, 1/4)
polar_four_transfer   a = (0.26894451080027754,
                           0.24397240258269645,
                           0.22259094800742266,
                           0.26449213860960330)
```

The last weights were trained previously on Full100 and Generated128 complete
frame endpoints and transferred unchanged to Mini100.  They are not refitted
here.

All six predictions are sealed first on Full16, Generated16, and official
Mini100.  A single candidate is selected using only the worst Full16/
Generated16 raw-MSE ratio.  Mini100 is then scored unchanged.

## Target ceiling and decision

Because a polar candidate runs one complete K129 trajectory, its propagation
cost is the same class as q0/R31; orthogonal-frame construction is a small
per-MLP side cost.  At equal effective compute, reaching `1e-7` from remote
R31 (`1.157747573e-7`) requires raw ratio at most `0.86375`.

Promote only if:

1. the selected candidate has raw ratio `<=0.90` on both pilot families;
2. official Mini100 raw ratio is `<=0.86375`;
3. at least 60% of Mini100 rows improve; and
4. the paired-bootstrap Mini100 ratio upper 95% endpoint is `<1.0`.

Otherwise kill the exact polar-barycenter class without tuning coefficients
on Mini100.

