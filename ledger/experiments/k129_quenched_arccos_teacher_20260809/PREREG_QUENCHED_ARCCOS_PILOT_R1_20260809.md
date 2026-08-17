# Preregistration: quenched last-layer arc-cosine pilot teacher R1

Date: 2026-08-09

## Hypothesis

The stable target-aware 129-basis calibration oracle has large capacity, but
direct vector audits are too noisy.  Replace those 256 realized-output
contractions by a one-layer population objective.

All first 31 supplied layers are evaluated exactly.  Conditional on their
states, averaging squared final-output error over Gaussian final rows is the
first-order arc-cosine-kernel MMD exactly.  A generic pilot input supplies
target features for hundreds of synthetic final rows simultaneously.  This
may estimate basis weights with much lower variance than a 256-vector final
output audit.

This is not KLPQ or an infinite-depth kernel.  There is no continuation
through layers 1--31.

## R1 capture

- two Full and two Generated development weights;
- all 129 canonical complete bases, with the exact global H1 moment match;
- candidate states evaluated literally through layer 31;
- 512 fixed synthetic Gaussian final rows;
- two independent generic antithetic pilot batches at cumulative sizes
  `128,256,512,1024,2048,4096`;
- an independent 65,536-row dense teacher batch;
- actual final-row candidate endpoints and dense-MC endpoints retained for
  post-seal diagnostics.

The capture is target-free: it opens weights and lawful network evaluations,
not benchmark expectation targets.  It opens no FlopScope session, physical
row, Mini100 row, package, upload, submission, or remote action.

## Gates

1. Dense synthetic-row teacher weights must retain at least 80% of the
   post-seal stable target-aware basis-weight gain.
2. A target-free two-replicate rule using at most 4,096 pilot rows must retain
   at least 70% of dense-teacher gain and at least 20% raw reduction on both
   component families.
3. If dense target features fail, kill the population last-layer teacher.
   If dense passes but 4,096 pilots fail, the representation survives but is
   not production-compatible at the present cost.
4. No Mini100 or physical work until the component gates pass and the entire
   rule is frozen on a broader two-family bank.

