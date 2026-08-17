# R65 preregistration — target-free R56 teacher compression

Evidence scope: component only. No scored targets, FlopScope session, package,
upload, or submission may be used by this screen.

Question: can the sealed R56 48-state late correction be reconstructed from a
strictly smaller subset of the already captured 64-state literal bank?  This
is different from R50: R50 refit the target-free ideal.  R65 treats R56's own
correction as the teacher, so it asks only whether the deployed statistic is
physically redundant.

The support and coefficients are selected on Full100 prediction tensors only.
Generated128 is an untouched transfer family.  Candidate sizes are 24, 28,
32, 36, 40, 44, 46, 47, and 48.  Relative ridges are 0, 1e-10, 1e-8, 1e-6,
and 1e-4.  The target-free proxy assumes that teacher error and compression
error are uncorrelated:

`(R56 broad raw MSE + teacher-drift MSE) * candidate price / R56 price`.

Promote to a single post-seal broad scoring pass only if one candidate is
strictly smaller than 48 and its proxy product ratio is below 0.995 on both
Full and Generated.  Select by worst-family ratio, then lower price, then
lower drift.  If the gate fails, kill this exact teacher-compression spelling.

