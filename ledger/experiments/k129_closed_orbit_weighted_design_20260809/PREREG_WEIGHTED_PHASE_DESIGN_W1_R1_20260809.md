# Weighted closed-orbit phase design W1 R1

Date: 2026-08-09

Evidence sought: target-free algebraic **component** gate on the already
target-free-only Full472 and Generated80 closed-orbit rows.  No neural
propagation, expectation target, FlopScope session, physical row, Mini100 row,
package, upload, submission, or remote action is authorized by this gate.

## New distinction

The R3/R4 phase-interlace experiments forced equal weights on 129 bases drawn
from four decorrelated closed-orbit phases.  Their mixed support violated the
degree-four spherical identity, and equal-weight phase assignment could not
repair it.  W1 keeps the same 129 evaluated bases but allows one target-free,
nonnegative weight per complete basis.

For basis fourth-moment tensors `T_i`, the exact objective is

`||sum_i w_i T_i - T_sphere||_F^2`.

Its Gram entries are available without trajectories:

`<T_i,T_j> = sum_ab <b_ia,b_jb>^4`.

The weights satisfy:

- `sum_i w_i = 1`;
- exactly `1/4` total weight on each of the four phases;
- `0 <= w_i <= c/129`, for `c in {2,4,8}`;
- a no-cap diagnostic is retained but cannot promote production work.

This preserves phase diversity explicitly; the solver cannot return the
canonical all-phase-zero design.  Test both the frozen cyclic support and the
previous exact-potential optimized support.  The quadratic program is solved
in float64 and checked against the exact Kerdock/Welch lower bound.  The
canonical K129 rule must reproduce zero relative excess to `1e-6`.

## Gate

Promote one literal weighted propagation scout only if, on both rows, a
solution with `c <= 4` has:

- relative fourth-tensor excess <= `0.05`;
- phase-mass error <= `1e-8`;
- finite nonnegative weights and ESS >= 32;
- a successful converged convex solve.

Otherwise close nonnegative per-basis weighting on these fixed four-phase
supports.  A failure does not prove that a different orbit contains an exact
mixed MUB design or that signed weights are impossible; signed weights would
need a separate stability argument and are not licensed here.
