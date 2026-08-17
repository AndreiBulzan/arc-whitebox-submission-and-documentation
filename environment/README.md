# Environment

The final estimator was developed and validated against **FlopScope 0.10.0**
and **WhestBench 0.14.0**. Their project metadata and complete `uv` lockfiles
are included here. The remote evaluator used Python 3.10; local development
also used NumPy 2.2.6.

The archive's internal `manifest.json` retains the older `0.9.1` / `0.13.0`
packager fields inherited from its seed archive. Those fields are provenance,
not the final evaluation environment. The included whole-run receipts and the
remote grading receipt come from the current 0.10.0 / 0.14.0 stack.

The challenge datasets, service configuration, and evaluator secrets are not
part of this release.
