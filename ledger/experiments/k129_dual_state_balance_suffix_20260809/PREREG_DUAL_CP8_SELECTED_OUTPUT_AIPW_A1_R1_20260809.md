# Preregistration: checkpoint-8 selected-output AIPW oracle A1

Date: 2026-08-09

Evidence: target-free capture and unavailable-output capacity oracle on one
Full and one Generated network, followed by post-seal component scoring. No
physical FlopScope run, Mini100 access, upload, submission, or remote action.

## Purpose

Test the expert's distinct post-suffix mechanism: after one q/p assignment has
been propagated, use only its observed per-line outputs to fit separate q and p
response models and estimate the missing arm by four-fold cross-fitted AIPW.

## Frozen geometry

- Rows: Full `11`, Generated `127`.
- Checkpoint: 8.
- Assignment: frozen D3 32-probe whole-pair signs, with the sign or its exact
  complement chosen by the low bit of SHA-256 of the supplied weights.
- Features known for both arms: pair means and pair second moments of the 32
  exact layer-9 probe preactivations and ReLUs (128 values), globally
  standardized and mixed by a fixed signed Walsh-Hadamard transform.
- Feature ranks: 16, 32, 64.
- Ridge fractions: `1e-4`, `1e-3`, `1e-2` times training count.
- Four cross-fit folds: Kerdock basis index modulo four.

Every nuisance fit sees only q outcomes on selected-q training lines and p
outcomes on selected-p training lines. Unselected outputs are used only after
the estimate is frozen to measure oracle error.

## Global-support interference control

Late production supports depend on the whole population. For each assignment,
derive layers 24--31 supports and the endpoint shift from the actually selected
population. Define both q/p counterfactual tails under that same support path
and shift. The selected counterfactual rows must reproduce the directly
propagated selected output. Record separately:

1. AIPW error relative to this common-path dual teacher;
2. common-path teacher drift relative to the actual complete q0/polar teacher.

## Costs and gate

Projected increments above the checkpoint-8 D3 graph are `0.3B`, `0.6B`, and
`1.4B` for ranks 16, 32, and 64. These are projections only.

Promote only if some rank at most 64:

- projects to at most `1.10e-7` on both rows after sealing;
- retains at least 80% of complete-dual target gain on both rows;
- has target-free squared discrepancy from the actual dual teacher at most
  `2.5e-8` on both rows.

A passing cell is a necessary-condition result only and must be frozen on a
larger disjoint block before production work.
