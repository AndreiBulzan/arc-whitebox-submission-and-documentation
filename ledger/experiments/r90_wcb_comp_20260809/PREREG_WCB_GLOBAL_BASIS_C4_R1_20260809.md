# Preregistration: R90 WCB whole two-level basis synthesis C4-R1

Date: 2026-08-09

## Scope

This is an offline, target-free, exact-algebra gate. It opens no MLP, target,
FlopScope session, physical row, Mini100 row, package, upload, submission, or
remote action. Evidence is a **projection**, not a physical receipt.

The C3 receipt established an exact 3,042,135,996-operation projected saving
from the public alternative-basis spelling of the live two-level R40 map. This
gate asks whether evaluating each of the two basis levels as one linear map
removes at least the remaining arithmetic needed to cross the 3.6B threshold.

## Frozen inputs

- Public alternative-basis schedules at commit
  `8bb354d63061504d1a712efafc8d06a0e8fa3f07`:
  `FMMa633_CoBL.m`, `FMMa633_CoBR.m`, and `FMMa633_ICoB.m`.
- The live production R40 schedule already pinned by the capsule.
- PLinOpt commit `656c3167a025518ea0451e44ada56fa5f8663051`,
  compiled privately in `/tmp`; its binary and source hashes must be recorded.
- Geometry: 66,096 padded rows; five width-234 and eight width-216 products.

The script must prove that the one-level compositions
`sparse_A o CoBL`, `sparse_B o CoBR`, and `ICoB o sparse_C` equal the live
production U, V, and W factors (including its stored-state sign gauge). It
must also build each two-level basis map by executing the exact recursive
schedule on coordinate vectors; a bare Kronecker expression is not accepted
without the ordering check.

## Baselines and accounting

Expanded to uniform depth-two leaf blocks, the recursive basis schedules cost:

- left CoBL: 2,664 additions;
- right CoBR: 378 additions;
- inverse ICoB: 2,592 additions.

One saved left or inverse node is worth 591,192 operations over the complete
R90 geometry. One saved right node is worth 7,988 operations. The existing
alternative-basis projection is 3,042,135,996 operations.

The official PLinOpt optimizer is run on each exact two-level map. Returned
SLPs must pass `SLPchecker -M` against their source matrices. Unary signs are
not charged on either side; additions and subtractions are counted alike.

## Gates

Let `C_L`, `C_R`, and `C_I` be the best independently verified whole-map
addition counts. The additional projected saving is

`(2664-C_L)*591192 + (378-C_R)*7988 + (2592-C_I)*591192`.

- **First pass:** total WCB saving at least 3.6B.
- **Implementation-headroom pass:** total WCB saving at least 4.5B.
- A result below 3.6B kills only basis-only whole-map synthesis. It does not
  kill fusion of the bases with the sparse core or a direct full U/V/W
  compiler.

No estimator implementation or benchmark run is authorized by this gate.

