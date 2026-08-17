# Public structure-aware transfer to K146: R1 verdict

Date: 2026-07-29

## Verdict

**Kill this transfer for the current checkpoint.**  The public method has
two ideas that are genuinely distinct from K146—final-two-layer always-on
folding and firing-rate-sorted cold-prefix multiplication—but neither can
provide the roughly `12--13B` cold-count reduction needed to move the
remote K146 anchor from about `171B` to the `~156B` checkpoint after the
separate `2--3B` persistent-destination port.

There is also no public evidence for a `>=10%` raw-MSE improvement.  The
authors report raw final-layer MSE `2.18e-7`, versus K146 submission 320262's
remote `2.090467e-7`; the public result is about `4.3%` worse raw.  This is a
comparison of two remote estimators, not an ablation or a prediction of
what a hybrid would score.

## What the public estimator does

The sources are the authors' [AIcrowd forum
post](https://discourse.aicrowd.com/t/phase-i-submission-structure-aware-estimation-for-random-relu-mlps/18106)
and [attached
write-up](https://discourse.aicrowd.com/uploads/short-url/70eZEVfgieVHDVbbrmSc1Aesugy.pdf).
For submission 319341 they report adjusted score `1.551e-7` and raw MSE
`2.18e-7`.  Under the challenge adjustment rule this implies approximately
`193.5B` effective compute; that compute figure is a projection, not a
reported receipt.

Their main mechanisms are:

1. a diagonal-Gaussian moment pass and the classification
   `dead: alpha < -3`, `on: alpha > 3`, `kink: otherwise`;
2. pilot refinement of neurons close to those boundaries;
3. omission of dead sampled channels;
4. a final-two-layer fold in which the on channels use
   `W31_on @ W32`, dead channels vanish, and only kink channels remain
   sampled; and
5. a firing-rate ordering whose cold prefix has expected support about
   three, followed by `(0,1,2,4,8,remainder)` row buckets and an ordinary
   dense hot block.

K146 already applies a stronger estimator-specific compression chassis:
analytic energy screens, width `216` in its middle, width `192` at each of
layers 24--30, width `176` at the final sampled product, specialized
M248/M267 bilinear circuits, and exact omitted-mean restoration.  This does
not make the classifiers identical, but it removes much of the structure
that made the public method economical at width 256.

## Final-two-layer fold

The current K146 tail spends:

```text
last M248 late layer        4.1389B
M267 final product          4.4280B
zero-cost deletion ceiling  8.5669B
```

Thus even deleting both products for free would save only `8.567B`, below
the `12B` material gate.  A lawful public-style replacement necessarily
adds sampled kink products plus the folded `W31_on @ W32` product and
therefore saves less.

The existing target-free late-ReLU capture also shows that the seam is not
nearly linear: on the same two Full and two Generated rows used below,
the layer-30 armwise exact-sign classification retains `86--134` kink
channels out of 200.  The conservative seven-sigma classifier in that
capture marks all 200 as kink.  Since the zero-cost ceiling already fails,
no target association or accuracy screen was run.

Evidence labels: the count ceiling is a **projection**; the regime capture
is a target-free **component** result.

## Cold-prefix multiplication

A preregistered target-free GPU census replayed the K146 graph on:

```text
Full        409, 419
Generated     7,  11
```

At all seven width-192 late products and the width-176 final product, it
formed exactly the public cold prefix: largest firing-rate-sorted prefix
with summed rate at most three and a hot-block floor of 96.

The structure exists—late prefix widths were `40--71`, and final prefix
widths were `23--43`—but the economics do not transfer:

| projection | four-row range |
|---|---:|
| ordinary dense hot block, all bookkeeping gifted | `-4.891B .. -3.304B` |
| ordinary dense hot block, implementation-aware | `-6.020B .. -4.441B` |
| same-efficiency variable FMM, every auxiliary cost gifted | `+5.548B .. +6.670B` |

The first two rows mean that a faithful port is slower in counted work than
the incumbent specialized M248/M267 graph.  The third is an intentionally
unattainable optimistic bound: it gifts an unimplemented variable-shape
FMM that scales perfectly at K146's current per-inner-width efficiency, as
well as support discovery, sorting, permutation, restoration, and
combination.  Even that bound is below `12B` on every row.

Evidence labels: the four prediction replays and firing-rate census are
target-free **component** evidence; all operation-count comparisons are
**projections**.

## Accuracy comparison

| estimator | evidence | adjusted | raw MSE | effective compute |
|---|---|---:|---:|---:|
| public submission 319341 | authors' public remote report | `1.551e-7` | `2.18e-7` | `~193.5B` inferred |
| K146 submission 320262 | remote | `1.315388e-7` | `2.090467e-7` | `~171B` |

The public write-up identifies substantial error in always-on outputs, but
does not publish a correction that can be transferred independently of its
sampler.  Its classification and folding are compute devices, not evidence
of a raw-error mechanism that clears the `>=10%` gate.

## Execution boundary and reopen condition

This lane made four offline GPU predictions.  It opened zero targets,
computed zero scores, ran zero FlopScope or physical rows, built zero
packages, and performed zero uploads or remote actions.

Reopen only if a code release supplies one of:

- a variable-shape hot-block circuit materially more efficient than
  K146's incumbent M248/M267 circuits;
- a construction that removes the row-specific cold-weight gathering
  rather than merely bucketing it; or
- a target-independent classifier/correction with evidence for at least
  `10%` raw-MSE reduction on both families.

## Local artifacts

- `PREREG_K146_COLD_PREFIX_CENSUS_R1_20260729.md`
- `census_k146_cold_prefix_r1_20260729.py`
- `k146_cold_prefix_census_r1_20260729.json`
- `analyze_k146_cold_prefix_economics_r2_20260729.py`
- `k146_cold_prefix_economics_r2_20260729.json`
- `../late_relu_regime_20260728/k146_late_relu_regime_r1_targetfree_20260728.json`
