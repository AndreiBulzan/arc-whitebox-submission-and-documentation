# Preregistration: R91 vectorized fourth-B7 dense cubes

Date: 2026-08-08

## Scope

Compose the sealed R90 estimator with one exact association-changing rewrite:
every argument-free float32 `(256,256) @ (256,256)` product in the production
prediction is evaluated by four vectorized B7 levels.  The sealed R78 owner
census proves that exactly five such calls exist: two structured rotations,
one repaired-H2 middle product, and two analytic H2-target products.

The statistic, Kerdock rows, weights, repairs, endpoint scalar, readout, and
all nonmatching products remain unchanged.  No target is opened before the
physical gate.

## Implementation boundary

The B7 tree is represented as a bank at every level.  Each forward and reverse
level uses whole-bank add/sub operations, avoiding the thousands of Python
leaf objects implied by a literal rank-2401 recursion.  The sole leaf matmul
has shape `(2401,16,16) @ (2401,16,16)`.

## Gates

1. Deterministic package build and exact-source hash checks must pass.
2. One initialized plus one steady exact-package row must observe exactly five
   replacements, finite outputs, and fewer steady counted FLOPs than R90's
   `123,391,852,159`.
3. Only if gate 2 passes, run a quiet R90/R91/R91/R90 ABBA.  Promote only if
   median effective compute also decreases.
4. Only if ABBA passes materially, run the full target-free five-lane Mini100
   capture and open targets post-seal.  Public and holdout raw-MSE ratios must
   each be at most `1.001` versus R90.

Any counted or effective regression kills R91 and freezes R90.  A failure does
not invalidate B7 algebra; it means this tiny five-product plan is uneconomic
under real transport and data-movement pricing.

No upload, remote run, or submission is authorized.

