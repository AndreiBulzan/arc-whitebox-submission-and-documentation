# Verdict: BF-XCORR and final-only LMCBR E1/E2 R1

Date: 2026-08-09

## Outcome

The endpoint-calibration investigation produced one strong structural result
and two production kills.

- **BF-XCORR: killed.** The best public-selected cell (`J=2`,
  `lambda=0.015`) transferred with only `0.9457%` raw reduction on the
  official holdout half and `1.0606%` pooled. No cell reached the
  preregistered `20%` target-aware-capacity gate.
- **LMCBR E1 target-aware capacity: passed strongly.** One shared,
  nonnegative 129-basis weight vector per row, constrained by a fixed
  max-weight cap and effective-sample-size floor, reduced pooled raw MSE by
  `97.13096%` in the best accepted cell. Even the tight `2/129` cap retained
  `95.63845%` pooled reduction, improved all 100 rows, and had realized ESS
  at least `68.88`.
- **LMCBR E2 final-only target-free bridge: killed.** The best frozen
  public-selected analytic-proxy cell gave only `1.11497%` pooled reduction
  and improved `45/100` rows. Its mean output-correction cosine with the
  needed correction was `0.07523`; its mean basis-weight-delta cosine with a
  target-aware solution was only `0.01798`.
- **E1 held-output cross-fit: passed strongly.** With weights fitted on one
  frozen half of the output neurons and evaluated on the unseen half, the
  tight `2/129` cap achieved `81.82218%` pooled raw reduction, improved all
  100 rows, and reached `82.57864%` / `81.22466%` on the public / holdout
  halves. Thus E1 is not merely in-sample 129-parameter interpolation.

## Interpretation

The 129 realized late endpoints contain a remarkably stable, low-risk
correction direction which transfers across unseen sibling outputs. The failure is observability: the already-computed
analytic final-preactivation proxy points almost orthogonally to the stable
target-aware basis reweighting. This closes the final-only analytic bridge;
it does not erase the demonstrated endpoint-space capacity.

Per preregistration, checkpoint constraints are not piled onto a final-only
bridge that transferred at only about one percent. A future restart requires
a genuinely new target-free late observable whose held-output alignment with
the E1 direction is demonstrated before a production implementation.

## Evidence boundary

- target-free array capture: **component**
- BF-XCORR, E1 capacity, and E2 scoring: **broad statistical** on official
  Mini100 public+holdout
- no FlopScope session, physical row, package, upload, submission, or remote
  action

## Sealed artifacts

- target-free capture NPZ:
  `runtime/artifacts/r90_bf_xcorr_lmcbr_final_mini100_r1_targetfree_20260809.npz`
  (`a5d54eedb25144f6305f5450318f69551ec6dd360bd6b7c6a289f21c9c5b4a41`)
- BF-XCORR score:
  `runtime/artifacts/r90_bf_xcorr_mini100_r1_postseal_20260809.json`
- E1 arrays:
  `runtime/artifacts/r90_lmcbr_e1_capacity_mini100_r1_postseal_20260809.npz`
  (`f3ad506206932282ba5c52e16f4dd5e83b5c001c53e940bd635e6120b86b2253`)
- E1 score:
  `runtime/artifacts/r90_lmcbr_e1_capacity_mini100_r1_postseal_20260809.json`
- E2 target-free candidate NPZ:
  `runtime/artifacts/r90_lmcbr_e2_final_candidates_r1_targetfree_20260809.npz`
  (`6b4e207777676b30c7049ddef1c43f3dddda17b9f107098a9e5272b2278d0291`)
- E2 score:
  `runtime/artifacts/r90_lmcbr_e2_final_mini100_r1_postseal_20260809.json`
- E1 held-output cross-fit:
  `runtime/artifacts/r90_lmcbr_e1_output_crossfit_mini100_r1_postseal_20260809.json`
