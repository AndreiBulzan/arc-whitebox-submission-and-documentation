# Preregistration: regularized D6 scout-16 pair on Mini100

Date: 2026-08-04

R1's pre-existing ridge-0.01 rule failed official Mini100 with raw ratio
`1.5136`.  That directly identifies the invalid assumption: its development
coefficients (`5.274`, `3.297`) were not stable across populations.

R2 tests the already sealed, pre-existing ridge-`1.0` member of the same
candidate family (record 26).  It is not a coefficient refit: before either
R1 or R2 Mini target was opened, record 26 was fixed at

```text
right coefficient       2.7961965428005953
d2 coefficient          1.9802766966338847
Full100 raw ratio        0.8370004004
Generated128 raw ratio   0.8524343676
```

Everything else—two 16-basis supports, proxy definition, q0 anchor, and
depth six—is identical to R1.  R2 advances only under the unchanged blocking
gates: raw ratio `<=0.88`, at least 60/100 improved rows, and paired-bootstrap
upper 95% ratio below one.  No FlopScope, package, upload, or remote action is
part of this test.

