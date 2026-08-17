# Preregistration: frozen Haar16 arc confirmation R9B

R9A used only the already-open four-row R6 development bank.  It froze two
Haar16 layer-31 mean-query arc estimators before this confirmation:

- primary accuracy cell: 512 probes, mean-defect shrink `0.75`, relative
  ridge `0.003`;
- economic secondary: 128 probes, mean-defect shrink `1.0`, relative ridge
  `0.01`.

R9B captures Full rows 676--691 and Generated rows 72--87.  These 32 rows are
disjoint from R1--R6 and from the R8C/R8D packet frame selection banks.  During
capture, each network supplies:

1. the exact canonical K129 layer-31 population and actual final endpoints;
2. its exact K129 layer-31 mean and full covariance;
3. two independent Haar16 layer-31 mean estimates;
4. 512 frozen random synthetic final-row features.

The target mean is

`k129_mean + shrink * (mean(two Haar16 replicas) - k129_mean)`.

The Gaussian feature moment uses the exact K129 full covariance.  One shared
129-basis constrained-ridge weight vector is fit in synthetic-feature space,
sums to one, and is applied to the 256 actual final endpoints.  No benchmark
target, label, ID-dependent fitted coefficient, family coefficient, clipping,
or row-wise selection is available during capture or fitting.

After sealing, score both frozen cells.  A cell promotes only if:

- pooled corrected-MSE ratio is at most `0.90`;
- each family ratio is at most `0.95`;
- at least 22 of 32 rows improve; and
- a paired row bootstrap gives `P(ratio < 1) >= 0.995`, tightened for the two
  frozen cells.

This is a broad-statistical accuracy gate, not a FlopScope or Mini100 receipt.
No physical run, package, upload, submission, or remote action is authorized.
