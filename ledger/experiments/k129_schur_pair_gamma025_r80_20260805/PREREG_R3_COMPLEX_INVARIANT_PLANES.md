# R80 R3: direct complex invariant-plane recovery

R2 proved that the remaining association gap was not caused by omitting the
final polar projection.  The symmetric-part eigenspaces recover exact planes
only for an exactly orthogonal relative map; the float32 input frames leave a
small non-normal perturbation that is amplified through 32 layers.

R3 replaces only that plane-recovery step with the eigenvectors of the relative
map itself.  Conjugate eigenpairs are converted to real two-dimensional bases;
the +1/-1 singleton handling, fixed gamma=0.25 metric, pair selection, final
polar projection, and propagated R78 graph are unchanged.  An offline
five-row check reduced frame discrepancy on every inspected row.  The original
association, 6.269B constructor, determinism, and wall gates remain unchanged.

