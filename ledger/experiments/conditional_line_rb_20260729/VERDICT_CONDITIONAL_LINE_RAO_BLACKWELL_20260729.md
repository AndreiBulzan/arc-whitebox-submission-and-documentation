# One-coordinate conditional-line Rao--Blackwell verdict

Date: 2026-07-29  
Evidence label: **projection**  
Decision: **kill before empirical execution**

## Idea

Fix a unit vector `u` and a point `v` in its 255-dimensional orthogonal
complement.  Along

```text
x(t) = v + t u,     t ~ N(0,1),
```

a bias-free ReLU MLP is a continuous one-dimensional piecewise-linear
function.  If all breakpoints were propagated exactly, its Gaussian line
integral could be evaluated analytically and would Rao--Blackwellize one
input coordinate.

The mathematics is valid.  It is not remotely compatible with the requested
`<=5B` incremental budget at the K26 operating point.

## Breakpoint bound

Let `I_l` be the number of affine intervals after ReLU layer `l`.  On each
incoming interval, each of 256 preactivations is affine and may introduce
one zero.  Therefore

```text
I_0 <= 257
I_l <= 257 I_(l-1)
I_31 <= 257^32 = 1.3117768e77.
```

That is a worst-case bound, not a claim about typical random networks.
However, no exponential growth is needed for the cost rejection.

For a generic line, layer 0 has 256 distinct roots.  At a carried knot, a
layer-1 preactivation is positive in about half of the 256 output channels.
Consequently the probability that a knot disappears from the union of all
output activation breakpoints is approximately `2^-256`; essentially all
256 inherited knots survive even if no new zero is created.

An optimized hinge sweep can process the first set of sparse layer-0 events
in `O(width^2)`.  From the next layer onward, the slope-change vector at an
inherited knot is dense because different downstream gates are active.
Propagating one 256-event slope-change bank through one dense `256 x 256`
weight therefore costs at least

```text
256 * 256 * (2*256 - 1) = 33,488,896
```

ordinary multiply/add operations.  Holding the event count artificially
fixed at 256 for all remaining 30 transforms gives an extremely favorable
zero-growth floor of

```text
1,004,666,880 operations per conditional line.
```

This excludes sorting, root tests, Gaussian integration, new roots,
intercepts, result movement, residual time, and every existing estimator
operation.  Exact fast matrix multiplication could reduce the constant, but
not the factor of hundreds to thousands required below.

## K26 budget contradiction

The preserved K26 measure contains exactly `6,656 x 256` half-direction
rows, followed by its antithetic expansion.  A conditional construction has
one 255-dimensional base point per line; applying the line integral across
that existing half measure therefore entails 6,656 distinct breakpoint
problems.  Breakpoints and gate masks depend on the base point and cannot be
shared across those lines.

Even under the impossible zero-growth floor:

```text
all 6,656 base lines       6.687e12 operations
20% of the base lines      1.338e12 operations
available increment        5.000e9  operations
```

Five billion operations can cover at most four full-depth lines under the
dense floor, only `0.060%` of the 6,656-line measure.  Even a generous
best-known fast-matmul factor would cover only a few more lines, not the
approximately 1,332 equal-weight lines needed for a 20% variance ceiling.

For completeness, even the unrealistically favorable interpretation of
“K26” as only 26 conditional lines does not rescue the proposal: removing
20% of equal line variance requires six lines, whose zero-growth dense floor
is `6.028B`, already above the entire increment.  The actual estimator has
6,656 base directions, not 26.

The variance ceiling is also generous: it assumes conditional integration
removes *all* variance contributed by every treated line and that treated
line contributions can be selected perfectly.  Real one-coordinate
Rao--Blackwellization removes only the conditional variance in that
coordinate.

## Decision

The proposal is killed on an optimistic arithmetic lower envelope before a
breakpoint-growth probe.  An empirical probe cannot reverse the conclusion:
the rejection assumes **zero new breakpoints after the first layer**, while
a real probe can only retain or add work.  Running one would therefore add
measurement detail without changing the `<=5B` feasibility decision.

This does not close approximate conditional integration with a fixed tiny
quadrature rule, nor a low-rank analytic closure that avoids explicit
breakpoint propagation.  It closes exact one-dimensional breakpoint
propagation as a K26-scale, 20%-variance, 5B-increment mechanism.

No target, estimator prediction, GPU, FlopScope session, physical row,
package, upload, submission, or remote action was used.

