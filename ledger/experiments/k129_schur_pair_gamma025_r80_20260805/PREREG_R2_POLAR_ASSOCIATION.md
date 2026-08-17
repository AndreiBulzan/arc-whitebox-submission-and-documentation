# R80 R2: restore the frozen final polar projection

The first physical gate measured a 2.394124428B constructor and passed
determinism, maximum-error, compute, and wall gates, but its final prediction
association RMSE was 2.3418e-5 against the preregistered 2.0e-5 threshold.
That version deliberately omitted the offline construction's final polar
projection.

R2 restores the exact `_project_orthogonal` operation with a full float64 SVD.
No selector, frame metric, propagation graph, or statistical parameter changes.
The original 6.269B constructor ceiling and association thresholds remain
unchanged.

