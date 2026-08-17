# Preregistration: R90 plus Haar final-output reliability blend R10E

R10D found a target-free 16-basis Haar final-output blend with 12.0% raw
reduction against a plain K129 diagnostic baseline.  R10E reconciles that
survivor with the actual statistical incumbent: K129 O0 with the live
`lambda=0.0075` endpoint repair and output scale.

This remains a replay on the already-open R9B/R10 development bank.  First
capture the exact repaired q0 prediction using the same frozen adapter and
endpoint readout that own R90's numerical prediction.  Do not read benchmark
targets.  Require target-free source and row association with the sealed R9B
and R10D inputs.

Scale the two exact Haar final-output replicas by the live output scale.  For
each cumulative query count `1,2,4,8,16`, define independent corrections

`d_r = haar_final_r - r90_q0`.

Estimate one global reliability coefficient from the cross-replica moment

`beta_single = clip(2 <d0,d1> / (||d0||^2+||d1||^2), 0, 2)`.

For the average of both replicas use

`beta_average = clip(<d0,d1> / ||(d0+d1)/2||^2, 0, 2)`.

Seal q0 plus the corresponding correction for replica 0, replica 1, and their
average at every count.  Only after hashing those predictions may the
already-open development targets be read.

Report corrected MSE ratios and projected economics at `1.08B` per complete
Haar basis.  Freeze a spelling for a new disjoint bank only if it improves
both families and its projected adjusted score is below R90's projected
`1.1307520123575174e-7` reference.  A failure kills this blend against the
actual incumbent but does not retroactively invalidate R10D's diagnostic.

No package, upload, submission, remote action, physical FlopScope row, or
Mini100 row is authorized.
