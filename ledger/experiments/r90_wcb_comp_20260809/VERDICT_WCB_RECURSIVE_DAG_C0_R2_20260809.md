# WCB recursive-DAG C0 R2 verdict

Date: 2026-08-09

Evidence label: **component plus projection**. No MLP row, target, FlopScope
session, physical run, package, upload, submission, or remote action was used.

## Verdict

Kill only the free signed-form duplicate-reuse shortcut across the existing
R40 x B7 recursion boundary.

The audit reconstructed the live mixed factors exactly and verified the
complete tensor identity. The existing recursive circuits contain:

- U: 590 arithmetic nodes, zero duplicate signed forms;
- V: 407 arithmetic nodes, zero duplicate signed forms;
- W: 848 arithmetic nodes, zero duplicate signed forms.

Consequently, literal cross-boundary reuse of forms already computed by the
current schedules projects to exactly zero saved operations, versus the
preregistered 1.5B promotion gate.

This does **not** reject whole-composite alternative bases, synthesis of new
linear forms, lower-rank algorithms, meta-flips, or serendipitous products.
It establishes that any such gain must change the algebra rather than merely
deduplicate the live recursive DAG.

Artifacts:

- preregistration: `PREREG_WCB_RECURSIVE_DAG_C0_R2_20260809.md`
- source: `audit_wcb_recursive_dag_c0_r2_20260809.py`
- receipt: `runtime/artifacts/r90_wcb_recursive_dag_c0_r2_projection_20260809.json`

