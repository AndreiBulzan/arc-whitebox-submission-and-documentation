# Evidence and promotion rules

## Evidence labels

Use these exact labels in notes and candidate cards:

| label | meaning |
|---|---|
| remote | completed AIcrowd evaluation |
| broad statistical | frozen literal predictions scored on a named bank |
| measured whole | complete estimator setup and prediction |
| component | only a subgraph was measured |
| projection | arithmetic assembled from separate measurements |

Never convert a projection into a receipt by prose.

## Fixed local banks

Do not conflate the two official downloadable Phase-1 splits with this
project's legacy bank names:

| surface | rows | relationship |
|---|---:|---|
| official `mini` | 100 | fixed, downloadable ARC/AIcrowd development split |
| official `full` | 1,000 | fixed, downloadable split independent of `mini` |
| project `Full100` | 100 | selected rows of official `full`; **not** official `mini` |
| remote evaluation | 100 | separate grader suite; 50 public-score plus 50 gated rows |

The remote public-score rows expose results and telemetry after submission,
not downloadable weights or targets. Accuracy-changing candidates must not
claim independent-family coverage merely from project `Full100`. The official
`mini` split is a blocking separate transfer check before packaging or
submission; tuning against `mini` does not then make it a virgin holdout.

### Exact packaged Mini100 gate (blocking)

For every accuracy-changing candidate, **“Mini100 validated” means that the
exact, hash-pinned submission archive's numerical implementation has run on
all 100 official `mini` networks** through the released participant
client/server and official runner lifecycle.  Capture those 100 exact outputs
target-free, seal them, and only then open the corresponding targets and score
them.

An offline NumPy/SciPy/Torch/CUDA reconstruction over Mini100 is `broad
statistical` evidence only.  It does not satisfy this gate, even when the
offline rule uses the intended formula and one or a few physical rows associate
within tolerance.  A one-row archive association is only a `measured whole`
diagnostic for that row; it cannot establish data-dependent eigensolver,
ordering, branching, dtype, or floating-point equivalence over Mini100.

Do not call an accuracy-changing candidate `Mini100 validated`, submission
ready, or give it a remote score projection until this exact-archive all-100
gate passes.  A prediction-preserving engineering descendant may inherit an
existing exact Mini100 score only when its output identity is proved for the
complete changed execution path; otherwise rerun the all-100 gate.

The comparison must be exact-package candidate versus exact-package baseline
on the same 100 rows.  A favorable central mean is not a promotion result.
Use paired row errors and require the prespecified uncertainty bound to clear
both the incumbent and the claimed adjusted-score threshold.  Retrospective
R85 evidence is the controlling example: exact R85 versus exact R31 had a
central Mini100 projection of `1.08044e-7`, but its paired 95% interval was
`9.3939e-8 .. 1.24734e-7`, only 54/100 rows improved, and the bootstrap
probability of adjusted score at most `1.10e-7` was only `0.592`.  The exact
all-row mean looked good; the uncertainty gate correctly blocks promotion.

### Fast iteration ladder

Keep accuracy throughput separate from wall/compute measurement:

1. Use cached target-free sufficient statistics or the frozen GPU/offline
   implementation to reject hypotheses quickly over the largest available
   fixed and generated banks.  This is `broad statistical` evidence only.
2. Freeze the candidate before scoring.  Use paired row deltas and stop bad
   candidates early in prespecified batches; early stopping can kill a path,
   but can never promote one.
3. For a finalist, run the exact candidate archive and exact incumbent archive
   over all official Mini100 rows.  The incumbent capture may be reused by
   hash.  Five independent persistent 20-row lanes are permitted for this
   numerical/lifecycle gate.
4. Price wall, residual, and effective compute only from a separate isolated
   initialized/steady receipt.  Concurrent Mini100 timing is contention data
   and must not be used to claim a compute saving or a remote wall projection.

On the 32-logical-CPU development host, the five-lane exact archive controller
completed R85 in `925.765s` and R31 in `932.863s`; the corresponding serial
rate was about 51 minutes per 100 rows.  Thus the exact numerical gate is now
about `3.3x` faster without changing the estimator or moving work out of
FlopScope.  Counted FLOPs, output hashes, crashes, and persistent-worker state
remain admissible; concurrent residual/effective timing does not.

These rows have already been used and are regression banks, not virgin data:

| bank | rows | use |
|---|---|---|
| Full sentinels | `0,184,686,699` | count, cap, protocol diversity |
| Full24 | `0,50,...,550` and `5,55,...,555` | cheap two-block screen |
| Full100 (legacy project name) | `7,17,...,997` | broad fixed statistic drawn from official `full` |
| Full1000 | all official rows | final local population |
| Generated128 | all generated rows | process-separated family guard |

These names describe standard project banks. A sealed experiment-specific
index manifest overrides the shorthand. In particular, the current K238
“Full100” is the explicit 100-index subset stored in its broad-score JSON,
not the canonical `7,17,...,997` canary sequence.

For accuracy-changing work, freeze source, constants, rows, and serialized
predictions before opening targets. Report pooled loss, halves, fixed strata,
row losses, bootstrap intervals, and hashes. A three-row win is a scout only.

### Lower-K post-selection confirmation

The lower-K support search and its development Full100 used only Full indices
`0..199`. A separate 100-row confirmation bank has been frozen with one row
from every consecutive eight-row block over `200..999`. Its complete index
list and deterministic generation rule are pinned in
`codex_2/K238_LOWERK_ALLSIX_PHYSICAL_SCREEN_AMENDMENT_20260726.md`.

The SHA-256 of the little-endian signed-int64 index array is:

```text
80e307124b091f6cd07b9f26287433d24004b914ee232a3597fefd9e45c4a313
```

Those targets remain unopened for lower-K confirmation. Do not capture or
score them until one exact source identity and physical price has passed the
runtime promotion lane. Freeze its predictions, seeds, source, and price
before opening targets; the confirmation result may not select another size.

## Complete candidate ladder

A sturdy candidate must pass all of:

1. depth 32, width 256, exact FlopScope `0.9.1+np2.2.6`;
2. frozen source and dependency hashes;
3. literal official Mini100, project Full100 or larger, and Generated-family
   scoring for every accuracy-changing candidate;
4. central adjusted score and the prespecified uncertainty bound below the
   claimed threshold;
5. one complete whole with `C_m <=272B`, not an additive projection;
6. every ordinary result `<=104,857,600` bytes and finite `(32,256)` output;
7. the exact archive through byte-pinned official WhestBench `0.13.0`
   runner code and released client/server transport, setup, prediction,
   fetch, and teardown;
8. numerical association with the frozen statistical prediction;
9. isolated wall receipts and a multi-process `5x20` suite stress test;
10. package import, file/size limits, participant dependency closure, and
    clean-process validation through production setup, manifest calls, at
    least one prediction, and teardown.

Remote submission remains a separate user decision.

For item 10, participant code is limited to the standard library,
`whestbench`, and `flopscope`/`flopscope.numpy`. Audit the complete reachable
closure under a participant-frame allowlist: evaluator-internal NumPy does
not make raw `numpy` importable by submission code. Local `python -I`,
contract validation, archive validation, or import-only smoke is not a
substitute for exercising production setup and prediction. Pin and authorize
the exact archive SHA only after this gate passes.

### Mandatory official-runner archive gate

WhestBench calls `Estimator.setup(context)` outside every
`BudgetContext`. Production setup must therefore use Python-only state and
emit **zero FlopScope transport requests**. Numerical runtime construction
must occur inside the first grader-owned prediction context and its FLOPs,
residual, and wall count against that first row. Do not open a private setup
budget to make a local test pass.

Drive the safely extracted, exact-SHA archive through byte-pinned official
WhestBench `0.13.0` runner code over the released byte-faithful pipe
transport. Require bare production-geometry setup, then both the first
initialized and a steady prediction. Each row must match its pinned ledger
and prediction identity, pass the challenge caps, and pass the predeclared
effective-compute safety ceiling; record that ceiling in the receipt.

`whest validate`, `validate-package`, generic-width smoke tests, import-only
checks, and setup tests wrapped in a `BudgetContext` are necessary at their
respective scopes but do not cover this gate. A setup-only pass is
**component** evidence. A completed production setup plus prediction is a
**measured whole**; it is not `remote` or `broad statistical` evidence.

## Wall evidence

Wall, residual, effective work, and peak memory are admissible only when:

- the shared benchmark lock was held for the full interval;
- CPU preflight was quiet;
- no known historical controller lock or visible competing FlopScope/audit
  process overlapped;
- no independent assistant artifact tree changed during the run;
- the isolation report and estimator receipt cover the same interval.

If contention occurred, count, parity, and result-cap evidence can survive.
Timing and effective work cannot.

Use `tools/run_isolated_benchmark.py` rather than an uncoordinated shell run.

## Kill and reopen discipline

### Prior-art preflight (blocking)

Before writing code, proving a new identity, or running a component for any
new mechanism, search the capsule for prior work on all of:

- the proposed mechanism and ordinary synonyms;
- the equivalent algebraic operation (for example gauge, adjoint,
  transposition, tensor rank, leaf recursion, CSE, or control variate); and
- the exact production call site or tensor shape that would change.

Record the queries, the nearest prior artifacts, and one of these outcomes in
the attempt's preregistration or verdict: `novel in capsule`, `materially new
observable`, or `skip: controlled negative already applies`. A renamed
implementation is not a new mechanism. If reopening killed work, identify the
specific assumption invalidated by the new observable before doing work.

Also perform a target-ceiling calculation first. If the best possible saving
or accuracy gain from the affected operations cannot reach the attempt's
declared promotion threshold, stop at the static calculation; do not build or
run the estimator. This preflight is a blocking rung, not optional
housekeeping.

Kill unchanged work when count alone exceeds the raw-conditioned ceiling,
a broad family reverses beyond uncertainty, a hard cap/protocol gate fails
without a lawful repair, or a theoretical lower bound misses the target.

Reopen a killed class only with a new observable, a new execution surface,
or a concrete mechanism that invalidates its controlling negative result.
