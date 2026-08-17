# Low-K TRB T1/R1 post-seal verdict

Status: `kill`

Evidence: **component** on the reused public256 development bank; no Mini100 or physical receipt.

Direct held raw MSE: `3.15352662099e-06`.
Unavailable-teacher train-fitted blend held raw MSE: `7.81372480928e-07`.
Selected transform: `signed_trb_l1` with beta `0.0461383679403`.
Selected frozen held raw MSE: `3.15315202917e-06`.
Selected correction cosine: `0.50992921087`.
Best target-aware gain retention: `0.000225573545932` from `signed_trb_l4`.

## Gates

- `best_capacity_retention_at_least_0_70`: `False`
- `selected_correction_cosine_at_least_0_25`: `True`
- `selected_bank_disagreement_at_most_half_energy`: `True`
- `selected_frozen_held_raw_at_most_9e_7`: `False`
- `breakthrough_held_raw_at_most_8e_7`: `False`
- `pass`: `False`

The result is only a mechanism gate. A pass authorizes a production-shaped K12/K24 implementation and exact official Mini100 evaluation; it is not itself a benchmark score.
