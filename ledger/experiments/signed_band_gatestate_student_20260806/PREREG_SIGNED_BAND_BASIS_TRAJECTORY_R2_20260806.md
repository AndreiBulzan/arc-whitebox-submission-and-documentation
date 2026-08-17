# Preregistration: signed low-band basis-trajectory student R2

Date frozen: 2026-08-06

## Question and prior-work boundary

R1 established that trajectory summaries pooled over all 129 Kerdock bases
do not predict the sealed degree-6--20 correction across Full and Generated
families.  R2 asks the immediate stronger question: does retaining the basis
identity at the already archived final, layer-4 and layer-8 response-projected
checkpoints recover the correction?

The same sequences previously failed to predict an all-eight-orientation
premean teacher and the total true premean error.  They are reopened only
because the new Poisson teacher isolates a stable signed low-frequency defect
instead of the entire residual.  Failure here closes this exact per-basis-
mean/checkpoint state for the new teacher; it may not be retried with more
epochs or another generic decoder.  A further reopen would require within-
basis direction state, explicit high-order contractions, or derivative/
boundary information.

## Frozen data and label

Use the named archived provenance roots consumed by
`trajectory_premean_20260804`:

- Full-A `root_k258_8orientation_full200_20260715_work`;
- Generated `root_k258_8orientation_generated128_20260715_work`.

Select only the 20 Full rows `7,17,...,197` and Generated rows `0,...,19`,
matching the sealed R1 low-band teacher.  Only orientation-zero features are
read:

- 129 final signed-preactivation basis endpoints;
- 129 layer-4 response-projected basis values;
- 129 layer-8 response-projected basis values;
- their three pointwise products and the frozen 15 scalar summaries.

The feature construction is unchanged from
`screen_basis_trajectory_student_r2_20260804.py`.  The label is the sealed R1
target-free degree-6--20 correction.  No challenge target is opened.

## Frozen screen and gate

Use the same two shared per-neuron model spellings as R1:

1. ridge alpha 30;
2. histogram gradient boosting: 300 iterations, depth 6, learning rate 0.07,
   L2 regularization 1.0, seed 20260806.

Train Full20 and predict Generated20, then train Generated20 and predict
Full20.  Standardization uses the training family only.  Select the model
with the lower worst reciprocal-family teacher-space MSE ratio; ties select
ridge.

All conditions are required in both directions:

- teacher-space MSE ratio versus zero correction `<= 0.50`;
- Pearson correlation `>= 0.65`;
- sign agreement on above-median-absolute teacher entries `>= 0.65`;
- maximum absolute prediction `<= 0.01`.

Passing authorizes only a post-seal development diagnostic and a fresh
disjoint capture/cost audit.  It does not authorize an estimator, package,
upload, adjusted-score claim, or remote submission.

