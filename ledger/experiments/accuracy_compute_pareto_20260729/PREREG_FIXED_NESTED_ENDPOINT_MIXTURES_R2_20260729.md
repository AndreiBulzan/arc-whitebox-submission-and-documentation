# Fixed K162/K226 endpoint-mixture screen

Date: 2026-07-29

Evidence scope: offline **broad statistical** development screen with
**projection** economics. No estimator, physical row, package, network,
remote action, upload, or submission.

## Question

Can two already sealed, target-free endpoint estimators (`m=33` and `m=97`)
be combined by a coefficient fixed from support arithmetic, without fitting
challenge targets, such that both Full100 and Generated64 improve enough to
offset the cost of propagating the union of their supports?

The two frozen supports overlap in 31 bases. Their union has 99 bases, so a
runtime able to expose both endpoints would propagate total `K=129+99=228`.
This is a support-union projection, not an implemented graph.

## Frozen candidates

Let `P33` and `P97` be the already sealed lambda-zero final predictions, and
let `P(w)=(1-w)P33+wP97`. The following weights are frozen before computing
any mixture score:

1. `equal`: `w=1/2`;
2. `sqrt_support`: `w=sqrt(97)/(sqrt(33)+sqrt(97))`;
3. `total_support_inverse_variance`: `w=226/(162+226)`;
4. `o1_support_inverse_variance`: `w=97/(33+97)`;
5. `first_order_richardson`: `w=97/(97-33)` (diagnostic; cancels an assumed
   leading `1/m` bias and uses a negative `P33` coefficient).

No coefficient may be altered after scores are computed. No oracle optimum
is a candidate. Scoring uses the exact identity

`MSE(P(w),T)=(1-w)MSE(P33,T)+w MSE(P97,T)-w(1-w)MSE(P33,P97)`,

so no target-bearing member is opened by this screen.

## Price projections

The conservative K228 price extrapolates two bases from the old K226 whole
at the observed K214-to-K226 slope. A second descriptive projection subtracts
the fixed effective-work reduction measured by the K162 persistent-operands
component. Neither is a whole receipt.

Promotion requires:

- adjusted central `<=1.2e-7` in both families at the conservative price;
- bootstrap-p95 adjusted `<=1.2e-7` in both families;
- no family central regression relative to the better of P33/P97 at the
  same K228 price.

Failure closes fixed two-endpoint arithmetic mixtures; it does not close a
new inference observable or a learned target-free selector.
