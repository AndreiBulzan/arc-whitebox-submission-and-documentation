# Schur moment-descent complete frame R4

Date: 2026-08-05

Evidence sought: **component**.  This is a target-free, ordinary-CUDA
accuracy falsifier.  It authorizes no FlopScope run, package, upload, or
submission.

## Distinction from prior negative results

Equal-row multiframe splitting, sparse alternate-frame supports, endpoint
herding, and the signed four-frame quartic design all choose nodes from
several frames and lose the complete-frame structure.  The earlier Schur R3
instead produced a promising single complete orthogonal frame by alternating
the invariant rotation planes of `polar @ q0.T`.

R4 keeps that single-frame construction.  It changes only the binary choice
inside each invariant Schur plane, while retaining an orthogonal 256 by 256
frame and therefore exact signed first and second moments for every one of
the 129 structured bases.

## Frozen target-free rule

For each MLP:

1. construct `q0`, the fixed `polar_q0_right_d2` frame, and the real Schur
   decomposition of `polar @ q0.T` exactly as in R3;
2. define the target feature as the mean of the q0 and polar first-layer
   absolute/standardised-fourth moment vectors used by R3;
3. start separately from the two alternating Schur-plane masks;
4. in ascending Schur-angle order, toggle one complete plane iff doing so
   strictly reduces the target-feature squared error; make two deterministic
   passes; and
5. retain the better of the two resulting masks and project the resulting
   frame to the nearest orthogonal matrix once.

No challenge target, endpoint, final prediction, fitted coefficient, random
restart, or MLP identity is used by the rule.

## Fixed falsifier

- Full rows: `300,303,306,309`.
- Generated rows: `32,35,38,41`.
- Controls: q0 and the unchanged alternating `angle_b` rule.
- Candidate: the frozen moment-descent rule above.

Promote to a wider target-free capture only if the candidate is finite and:

- candidate/q0 pooled raw-MSE ratio is at most `0.80` in each family;
- candidate/angle_b pooled raw-MSE ratio is at most `0.90` in each family;
- at least five of eight rows improve over q0; and
- its target-free first-layer moment loss is below angle_b on every row.

Failure kills this exact moment-descent spelling; it does not alter the
already measured fixed-angle_b broad result.
