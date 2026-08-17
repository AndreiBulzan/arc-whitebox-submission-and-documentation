# Low-K dual complete-K2 checkpoint coreset C3 R1

Date: 2026-08-09

Evidence sought: target-free CUDA component capture on the fixed public16
development rows, followed by post-seal scoring against the already sealed C2
literal per-basis endpoints. No Mini100, physical, package, upload, or remote
action is authorized by this experiment.

## Question

C2 established that 12--14 literal ordinary/polar tails have ample stable
target-aware capacity, while independent first/diagonal-second-moment herding
does not discover the support. Test whether the missing observable is the
complete degree-two checkpoint algebra and joint arm selection.

For every one of the 20 ordinary and 20 polar complete bases, capture at layer
5 the basis mean, diagonal raw second moment, and off-diagonal raw second-moment
matrix. Propagate all prefix rows through one additional actual supplied-weight
ReLU layer and capture the same blocks for both its preactivation and activation,
plus the preactivation/activation cross moment. Store only the resulting 40x40
feature Gram matrices. Targets and suffix endpoints remain unopened.

Post-seal, construct a nested balanced coreset by kernel herding over ordinary/
polar pairs. Each selected arm has total weight 1/2. Refit nonnegative weights
on each frozen support with ridge `1e-7` and a maximum selected weight of twice
the within-arm uniform weight. Compare, in order:

1. layer-5 mean plus diagonal second moment;
2. layer-5 complete second moment;
3. layer-5 complete plus layer-6 activation mean/diagonal moment;
4. layer-5 complete plus layer-6 complete activation moment;
5. the preceding blocks plus layer-6 preactivation and cross-moment algebra.

Every block Gram is normalized by its centered trace before combination. The
old layer-5 diagonal objective is reported as a guard for every support.

Primary gate: a target-free construction with at most seven bases per arm must
reach raw MSE at most `1.0e-6`; `8.0e-7` is the preferred promotion threshold.
It must not depend on targets for features, support, weights, tuning, or tie
breaking. Passing C3 authorizes a frozen broader transfer screen, not a score
claim. Failure kills this exact complete-K2/joint-herding spelling but not a
learned checkpoint-to-support map or the external friend's implementation.

