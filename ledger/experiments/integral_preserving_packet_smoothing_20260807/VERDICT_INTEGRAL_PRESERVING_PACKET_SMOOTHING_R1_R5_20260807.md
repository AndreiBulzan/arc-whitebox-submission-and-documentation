# Integral-preserving packet smoothing: R1--R6 verdict

Date: 2026-08-07

## Controlling conclusion

Integral-preserving smoothing is a real, large accuracy mechanism.  A fresh
Gaussian-packet oracle reduced pooled raw MSE by `53.574%` across disjoint
Full8 and Generated8 families and improved `16/16` rows.  This is **broad
statistical** component evidence for the target, not a contest estimator.

The immediate production bridge is unresolved.  Exact full-covariance
Gaussian moment propagation reproduced the local packet correction with
`0.997800` fidelity through layer 32 on a deliberately small four-network,
sixteen-centre micro-oracle.  In contrast, every production-shaped state
tested so far failed:

- diagonal and block-isotropic variances (`b=1..256`) lost the correction at
  layer 2 and were catastrophic by layer 32;
- stochastic covariance-factor states (`r=1,2,4,8`) improved `0` rows and
  remained worse than canonical K129 even at `r=8`;
- one shared full-rank covariance with the exact all-centre gate Gram retained
  `0.972178` correction fidelity at layer 1, only `0.226120` at layer 2,
  and fell to `-65.9023` by layer 32;
- prior sparse Poisson and mixed-frame probes already showed that a few local
  samples do not estimate the collective smoothed average.

Therefore no estimator, package, remote-score projection, upload, or
submission is authorized by this work.  The live mathematical problem is to
compute the *centre-averaged full-covariance correction collectively* without
materializing one `256 x 256` covariance for every Kerdock centre.

## Evidence table

| Round | Evidence | Result | Decision |
|---|---|---|---|
| R1 ideal semigroup smoothing | broad statistical, post-seal reanalysis of an existing target-free capture | pooled ratio `0.451357`, reduction `54.864%`; Full `59.462%`, Generated `52.038%`; `16/16` improved | pass capacity only |
| R2 true Gaussian packets | broad statistical, fresh target-free Full8/Generated8 capture followed by post-seal scoring | pooled ratio `0.464256`, reduction `53.574%`; Full `61.932%`, Generated `48.320%`; bootstrap 95% ratio `0.4017..0.5385`; `16/16` improved | pass target only |
| R3 block/diagonal closure | broad statistical | layer-1 fidelity about `0.972`; layer-2 fidelity about `-13`; final raw MSE about `7e-5` | kill entire block/diagonal family |
| R4 full covariance | component, four networks and sixteen centres | correction fidelity `0.999667` at L1, `0.999630` at L8 and `0.997800` at L32; final cosine `0.999021` | pass mathematical closure; not production |
| R5 stochastic covariance factors | broad statistical exploratory target association | `r=1` median raw reduction `-7.214`; `r=8` reduction `-0.807`; `0` improving rows | kill ordinary low-rank stochastic factors |
| R6 one shared dense gate-Gram covariance | target-free component | pooled correction fidelity `0.972178` at L1, `0.226120` at L2 and `-65.9023` at L32; final cosine `0.113030` | kill `K=1` shared dense covariance |

R1 used `rho=0.975`.  R2--R6 used the preregistered moderate packet radius
from the packet oracle.  No FlopScope session, physical row, upload, remote
action, or submission occurred in R1--R6.

## What this says about the external follow-up

The follow-up's strategic retraction is accepted: retire fixed-channel
Design-Defect Tangent ARC and its `8--20B`/score forecasts.  Its distinction
between a compact projector formula and an ordinary low-rank tensor state is
consistent with R3 and R5.  The exact catalecticant constants and general
theorems supplied externally have not all been independently re-proved in
this capsule, so they remain external mathematical claims rather than local
receipts.

The replacement hypothesis survived its decisive accuracy oracle.  It did
not survive its proposed cheap block closure.  Full covariance shows that the
failure is representational rather than a failure of integral-preserving
smoothing itself.

R6 also closes the literal one-shared-covariance spelling.  Its collapse by
layer 2 shows that the centre--covariance/gate correlation cannot be replaced
by a single gate Gram.  This does not by itself kill `K>1` dense prototypes,
but those are admissible only after a teacher receipt demonstrates low
distortion for `K<=16`; arbitrary cluster sweeps are not licensed.

## Next admissible research

Do not build a contest graph yet.  The next oracle must attack one of these
collective representations:

1. an implicit centre-averaged covariance operator exploiting Kerdock/MUB
   transforms and the nearly exact linearized full-covariance recurrence;
2. a moment-preserving joint cubature over `(centre, packet noise)` that keeps
   the global K129 cancellations while reducing conditional variance at a
   one-frame-size budget;
3. a structurally labelled, equivariant compression student trained against
   full-covariance layerwise corrections, accepted only after unchanged
   cross-family transfer.

Any survivor must retain at least `70%` of the true-packet gain, show at least
`20%` frozen cross-family raw reduction, and have a measured effective cost
compatible with the remote score before estimator implementation.

## Receipts

- `runtime/artifacts/ideal_integral_preserving_smoothing_oracle_r1_postseal_20260807.json`
- `runtime/artifacts/true_gaussian_packet_oracle_r2_postseal_20260807.json`
- `runtime/artifacts/block_packet_closure_r3_postseal_20260807.json`
- `runtime/artifacts/fullcov_packet_micro_oracle_r4_targetfree_20260807.json`
- `runtime/artifacts/stochastic_covfactor_packet_r5_postseal_20260807.json`
- `runtime/artifacts/shared_dense_gategram_packet_r6_targetfree_20260807.json`
