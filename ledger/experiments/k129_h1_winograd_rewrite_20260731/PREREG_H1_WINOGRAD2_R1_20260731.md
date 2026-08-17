# K129 repaired-H1 two-level Winograd preregistration

Evidence target: `component`, followed by at most one `measured whole` if the
component passes. No package, upload, remote action, or submission is
authorized.

The R26 product census identifies the repaired-H1 product
`(33024, 256) @ (256, 256)` as the largest direct dense leaf. Its direct
FlopScope count is `4,320,067,584`. Replace only this product by the standard
two-level rectangular Strassen-Winograd identity. Preserve the construction
of `repaired_sum`, every other operation, and the final output scale.

Predeclared gates:

- production geometry is exactly `(33024, 256) @ (256, 256)`;
- the 49-way leaf result is at most the 100-MiB hard cap;
- component counted FLOPs are lower than the direct product;
- component wall is not more than `0.25 s` slower than direct in a paired,
  released FlopScope 0.9.1 pipe run;
- all outputs are finite;
- component relative RMSE versus direct is at most `2e-5`;
- no raw-array computation occurs inside the candidate implementation.

If any gate fails, kill this realization. If all pass, permit one isolated
Full0 whole only after the successor and its runner are independently
hash-pinned. A component pass is not a whole-estimator runtime or accuracy
claim.
