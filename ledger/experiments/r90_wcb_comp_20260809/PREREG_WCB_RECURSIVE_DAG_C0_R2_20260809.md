# WCB recursive-DAG cross-boundary duplicate audit C0 R2

Date: 2026-08-09

Evidence class: exact symbolic component/projection. No MLP, target, FlopScope
session, package, upload, or submission.

## Motivation

R1 attempted to synthesize the fully expanded mixed R40 x B7 transforms from
scratch. One W trial showed that this destroys the excellent recursive circuit
structure and would require roughly 35 hours for the preregistered search. It
was stopped before any receipt.

R2 keeps the live recursive circuits exactly and asks the cheapest genuinely
cross-boundary question: do internal signed linear forms recur across B7 copies,
R40 copies, or the B7/R40 boundary and therefore admit exact global reuse?

## Frozen object

Audit the exact inner tensor product

`<6,3,3:40> x <2,2,2:7> = <12,6,6:280>`

for U, V, and W. Use:

- the frozen R87/Tichavsky R40 A/B/C schedules (74/53/142 nodes);
- the live B7 factors and explicit live-minimal circuits (4/4/7 nodes);
- current separable baselines U=590, V=407, W=848 nodes.

Expand every *existing recursive internal node* to its signed coefficient form
over the original inner-composite inputs. Identify a form with its negative,
but do not synthesize any new form. Count nodes whose form already exists as an
input or earlier recursive node. Verify that recursively produced outputs equal
the exact Kronecker U/V/W factors and that the multiplication tensor is exact.

## Projection and gate

Report optimistic exact-arithmetic savings from duplicate elimination under the
live R90 shapes, separately for U and W, using the same R1 projection factors.
This is still a projection because liveness, cap-safe scheduling, requests, and
floating association are not implemented.

Promote only if optimistic combined savings are at least 1.5B operations.
Otherwise kill *duplicate-only cross-boundary reuse*, not WCB alternative-basis,
serendipitous-rank, or more general circuit rewriting.
