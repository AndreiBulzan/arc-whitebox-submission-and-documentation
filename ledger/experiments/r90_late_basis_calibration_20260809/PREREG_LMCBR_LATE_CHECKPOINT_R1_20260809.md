# Preregistration: R90 late-checkpoint LMCBR R1

Date: 2026-08-09

## Evidence boundary

This is a target-free ordinary-CUDA component capture followed by a sealed
candidate build and a post-seal score on the official Mini100.  The capture
may map only `weights.npy`; it must never map or read the target member.
No FlopScope physical row, package, upload, submission, or remote action is
authorized.

The experiment is reopened after the preregistered E1 output-coordinate
cross-fit retained 81.82% pooled raw reduction with the tight 2/129 weight
cap.  That receipt establishes a real basis-quality direction shared by
unseen output coordinates.  E2 killed only the final-preactivation analytic
proxy as its target-free teacher; it did not observe exact per-basis deep
checkpoint means.

## Frozen capture

On each official Mini100 weight row, run the same target-free standard O0
CUDA trajectory that is already associated to the sealed q0 endpoint atlas.
Hook the eight late `energy_indices` calls and record the incoming O0 state
at calls 1--8, corresponding to post-ReLU layers 23--30.  For every layer,
record per-basis antipodal means, multiplied by the exact chi radial mean.
Also record per-basis radial second moments and the float32 analytic-closure
screened mean and screened second-moment targets at the identical coordinate
indices.

The capture must require:

- 129 O0 bases, 66,048 rows, width 256;
- eight late calls, each with 200 active coordinates;
- analytic and CUDA screen IDs agree exactly;
- the final q0 prediction agrees with the existing target-free Mini100 seal
  to at most 1e-10;
- no target access.

## Frozen target-free calibration grid

Build one shared 129-vector of basis weights independently for each network.
For each feature set below, center the basis checkpoint matrix at the uniform
weight vector and solve a spectral ridge ray toward the analytic checkpoint
total.  Scale coordinates by either identity or a floored across-basis
standard deviation.  Apply the largest ray fraction satisfying nonnegative
weights, the registered ESS floor, and the registered max-weight cap.

Feature modes:

- checkpoint means only;
- checkpoint second moments only;
- checkpoint means and second moments concatenated.

Layer sets (fixed before targets):

- each singleton 23, 24, 25, 26, 27, 28, 29, 30;
- (30, 28);
- (30, 28, 24);
- (30, 28, 24, 23);
- (30, 29, 28);
- (30, 29, 28, 27, 26, 25, 24, 23).

Ridge multipliers:

`1e-4, 3e-4, 1e-3, 3e-3, 1e-2, 3e-2, 1e-1, 3e-1, 1, 3, 10`.

ESS floors: 32, 64, 96.  Weight caps: 2/129, 4/129, 8/129.

Every target-free candidate prediction is the existing final endpoint-basis
matrix multiplied by the sealed checkpoint-derived weights.  The complete
grid must be hash-sealed before any target member is opened.

## Post-seal gates

Score first 50 and last 50 Mini100 rows separately as the two frozen evidence
lanes.  Select a single cell on the first lane only and report its unchanged
transfer to the second lane and pooled set.

- Promote: at least 15% raw reduction on each lane, at least 20% pooled, no
  material row-tail instability, and constraints satisfied on every row.
- Weak lead: 5--15% pooled with the same sign on both lanes; retain only as a
  possible observable/control and continue to earlier checkpoints.
- Kill this late-checkpoint teacher family: below 5% pooled or sign reversal
  after the full preregistered grid.  This does not kill earlier exact
  checkpoints, stochastic audits, or a different target-free teacher.

The E1 target-aware weights may be opened only after candidate sealing, for
diagnostic cosine/alignment reporting.  They may not influence candidate
construction or selection.
