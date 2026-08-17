# Preregistration: direct-mean Haar count curve R10C

R10's stable direct layer-31 mean calibration reached ratio `0.83644` with
two Haar16 replicas on the 32-row development bank.  A post-seal diagnostic
using either replica alone gave ratios `0.92269` and `0.88751`.  R10C asks how
few complete Haar bases preserve enough gain to pay for their propagation.

On the same already-open R9B/R10 development bank, capture cumulative means at
`1,2,4,8,16` Haar bases for each of the two already-defined deterministic seed
replicas.  This capture remains structurally target-free but is a development
curve, not validation.

For every count, replay exactly one fixed direct-mean estimator:

- shrink `0.1` from the K129 mean toward the query mean;
- relative ridge `1e-6` toward uniform basis weights;
- equality-constrained raw weights and the preregistered `[0,4/129]`
  ray-bounded spelling;
- replica 0, replica 1, and their average.

Do not scan new shrink or ridge values.  Report corrected raw ratios and a
**projection** using `1.08B` incremental effective operations per queried
complete basis, relative to the `139.365B` remote R87 price.  Any survivor
must subsequently be frozen and confirmed on a new bank before physical or
Mini100 work.

No package, upload, submission, remote action, physical FlopScope row, or
Mini100 row is authorized.
