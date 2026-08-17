# K129 R56 frame restriction — R61 preregistration

## Prior-art boundary

The July-31 branch already tested separately selected `right+d2` and
`right+d2right` estimators.  The August-3 all-387 scout also allowed every
frame at once and failed nested transfer.  Neither result answers the narrow
R56 question: whether its fixed-size, ridge-regularised 48-atom correction
can retain target-free reconstruction after deleting one complete frame
family, in particular the independently constructed right-Gram frame.

## Fixed experiment

Use the sealed target-free four-frame D6 proxy and q0 anchor.  For each of
the six nonempty proper alternate-frame pools (`right`, `d2`, `d2right`, and
the three pairs), recompute a size-48 OMP support inside every one of four
network folds.  Search only the previously used ridge grid
`{0.125, 0.25, 0.5, 1.0}`.  Select by the worst Full/Generated normalised
cross-validation ratio.  Do not open scored targets.

## Gate

A pool promotes only if its nested target-free reconstruction MSE is no
worse than R56 in **both** Full100 and Generated128.  The principal target is
`d2+d2right`, because it permits deletion of the independent right-Gram
eigenframe.  Any adjusted-score implication remains a **projection** until
the selected statistic has an exact physical implementation and whole-row
receipt.

