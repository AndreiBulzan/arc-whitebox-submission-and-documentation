# Schur pair-stratified sensitivity exponent R4

Date: 2026-08-05

R1 established complementary transfer behavior at the endpoints of one
target-free metric family.  Pairwise selection by unweighted first-layer
energy (`gamma=0`) scored Full/Generated ratios 0.931247/0.962494, while
weighting by the full diagonal downstream sensitivity (`gamma=1`) scored
0.948454/0.930991.  The former subsequently scored 0.914523 on official
Mini100; the latter scored 0.950697.

This preregisters the geometric interpolation

`metric(block) = sum(coordinate^2 * sensitivity^gamma)`

for `gamma in {0.25, 0.50, 0.75}`.  Everything else—including one block per
adjacent absolute-angle bin—is unchanged.  Selection uses only Full100 and
Generated128, minimizing the worst family raw ratio.  Promotion requires
both ratios <=0.94, pooled <=0.93, and at least half the rows improved in
both families.  Only the unchanged winner may then be scored on Mini100.

