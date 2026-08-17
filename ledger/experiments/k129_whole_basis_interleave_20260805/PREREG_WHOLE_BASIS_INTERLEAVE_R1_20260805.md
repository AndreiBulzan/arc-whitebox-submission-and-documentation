# K129 whole-basis frame interleave R1

## Distinct prior-work boundary

The earlier equal-row multiframe experiment partitioned the 256 direction
ordinals *inside every basis*.  The frozen static coreset used fitted affine
weights and support.  This experiment does neither: every retained atom is a
complete 256-direction basis, all 129 basis means have uniform weight, and
only the frame assigned to each complete basis changes.

## Fixed candidates

Using the already sealed q0, right-Gram, depth-2 `J J^T`, and depth-2
`J^T J` per-basis endpoints, seal these rules before opening labels:

1. `q0_control`: all 129 bases from q0;
2. `alternating2`: basis `b` uses q0 for even `b`, right-Gram for odd `b`;
3. `block2`: bases `0..64` use q0 and `65..128` use right-Gram;
4. `cyclic4`: basis `b` uses frame `b mod 4` in the order above.

Every prediction is the uniform mean of exactly 129 complete-basis endpoints.
No support, coefficient, frame, or rule may be changed after capture.

## Fast gates

Score first on the existing Full100 and Generated128 banks.  Promote exactly
one fixed rule to official Mini100 only if its raw-MSE ratio to q0 is below
`0.97` on both families and it improves at least half the rows in both.  A
Mini100 successor requires ratio at most `0.97`, at least 55/100 improved
rows, and a bootstrap ratio upper endpoint below `1.0`.

The experiment is offline component/broad-statistical evidence.  It performs
no FlopScope run, package, upload, or remote action.
