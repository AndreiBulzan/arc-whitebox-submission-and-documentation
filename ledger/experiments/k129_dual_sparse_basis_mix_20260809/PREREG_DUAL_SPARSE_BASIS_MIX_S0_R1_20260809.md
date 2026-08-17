# Preregistration: dual sparse-basis frame mix S0

Date: 2026-08-09

Evidence: **broad statistical** accuracy capacity from the already sealed exact
per-basis endpoint archives on Full100 and Generated128; all compute and score
figures are **projections**.  This stage performs no prediction run, physical
FlopScope session, Mini100 access, upload, submission, or remote action.

## Question

Can a fixed total population of Kerdock bases be divided between the ordinary
q0 frame and the independently weight-conditioned polar frame, so that frame
decorrelation beats the loss from not completing either 129-basis frame?

This differs from the killed sparse-polar arm, which retained the complete
129-basis q0 estimator and appended 32--104 polar bases.  Here the total number
of evaluated bases is fixed and both arms may be incomplete.

## Frozen grid

- Total basis counts: 32, 48, 64, 80, 96, 112, 128, 129.
- Equal split: floor(K/2) q0 bases and ceil(K/2) polar bases.
- Support rules:
  - `complementary`: one frozen permutation; q0 and polar receive disjoint
    segments of that permutation;
  - `independent`: independently frozen q0 and polar permutations.
- Seeds: 64 deterministic seeds derived only from the integer 20260809.
- Readout: the uniform mean over all selected basis endpoints.  The equal-frame
  mean is also reported as a diagnostic.

The endpoint archives were formed inside complete-frame computations.  This
is therefore a capacity screen, not a lawful low-K implementation receipt:
the complete-frame endpoint repair may leak unavailable full-frame statistics.
Any passing point must be rerun literally with its own low-K repair.

## Gates

Promote at most one structural cell to a literal target-free capture only if:

1. its median raw ratio to complete q0 is at most 0.70 on both Full100 and
   Generated128;
2. its tenth-percentile seed has ratio at most 0.80 on both families;
3. its conservative projected adjusted score is at most 1.05e-7 on both
   families; and
4. at least 70% of rows improve in both families for the selected frozen seed.

Mini100 remains unopened in S0.

