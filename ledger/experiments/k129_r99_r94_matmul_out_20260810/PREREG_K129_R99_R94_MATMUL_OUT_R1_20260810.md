# Preregistration: R99 R94 `matmul(out=)` integration

Date: 2026-08-10

## Hypothesis

FlopScope 0.10 permits two prediction-preserving execution improvements that
were unavailable to the original R94 package:

1. reuse destination storage for the two live late-B7 product geometries and
   remove the fringe `copyto`; and
2. replace the dominant destination-backed R40 `vecmat` spelling with the
   lower-wall destination-backed `matmul` spelling.

The repaired-H1 product-bank reuse is excluded because its component
projection is only `0.0116362B` while it requires roughly `519 MiB` of extra
persistent storage.

## Frozen parent and candidate

- Parent: exact packaged R94 archive, SHA-256
  `fbb9e28232c46c5fde2bde57bb2805bdf758aba3f2626485da7e7c6183917e77`.
- Candidate label: R99.
- Estimator statistic, Kerdock support, readout parameters, output scale and
  endpoint lambda are unchanged.
- The late-B7 rewrite preserves operation association and is byte-exact in
  its component gate.
- The R40 primitive changes float32 matmul association; its four exact-shape
  component relative RMSE values are `8.0042e-8`--`8.0247e-8`.
- No target-dependent or network-specific parameter is introduced.

## Static ledger

- R94 initialized FLOPs: `125835566424`.
- R94 steady FLOPs: `123391852159`.
- Late-B7 exact count removal: `19822656` per prediction.
- Expected R99 initialized FLOPs: `125815743768`.
- Expected R99 steady FLOPs: `123372029503`.
- Component-projected effective saving from late-B7 plus R40:
  `0.292133331B`, or approximately `0.209%` of remote R94/R92-scale compute.
- Additional persistent late-B7 storage: `243535488` bytes.

## Execution sequence

1. Build the deterministic package from the exact R94 archive and run the
   package audit and WhestBench 0.14 validation.
2. Run a separately hash-pinned capsule-native initialized-plus-steady whole
   gate before any broad execution.
3. If the whole gate passes, run all official Mini100 rows in the established
   five-lane, twenty-row-per-lane layout. Targets remain unopened.
4. Seal the target-free capture, then score once against the official targets
   and the exact R94 capture.

## Gates

The candidate is banked only if all of the following hold:

- zero failures and finite `(32,256)` outputs;
- exact initialized and steady FLOP counts above;
- all 100 official Mini rows complete;
- pooled raw-MSE ratio to R94 at most `1.001`; neither public nor holdout raw
  ratio may exceed `1.002`;
- measured Mini100 adjusted-score ratio to R94 at most `0.998` (at least
  `0.2%` improvement);
- maximum wall remains below the established safe envelope;
- package and source hashes are pinned in the final receipt.

If the adjusted improvement is below `0.2%`, retain the component code but do
not promote R99 over R94. Five-lane timing is broad physical evidence, not a
paired residual-time identity; an ambiguous result requires a quiet paired
multirow control rather than optimistic wording.

No upload, remote action or submission is authorized by this preregistration.
