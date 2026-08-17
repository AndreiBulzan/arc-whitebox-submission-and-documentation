# Root workstream progress — 2026-07-10

## Completed remote results

| Submission | Branch | Adjusted | Raw final MSE | Mean FLOPs | Mean effective |
|---|---|---:|---:|---:|---:|
| `315621` | affine K26, dynamic exact-zero q4 | `1.784251752e-7` | `1.293972961e-6` | `3.270e10` | `3.740e10` |
| `315630` | affine K26, sorted8/Winograd/symmetric fullcov | `1.695586977e-7` | `1.293953238e-6` | `3.135e10` | `3.562e10` |
| `315631` | parallel duplicate of the same hybrid | `1.693694567e-7` | `1.293952078e-6` | `3.135e10` | `3.564e10` |
| `315632` | affine K26, sorted8 no-fullcov tailmix2 | `1.732102476e-7` | `1.401151843e-6` | `2.977e10` | `3.358e10` |
| `315634` | affine K26, exact baseline-sparse + symmetric fullcov | `1.672720226e-7` | `1.293972317e-6` | `3.100e10` | `3.521e10` |
| `315645` | affine K26, vectorized stops + known-sign/final-only cleanup | `1.644828082e-7` | `1.293969012e-6` | `3.072e10` | `3.453e10` |

`315630/315631` have the same statistical estimator and differ only at normal
remote residual/runtime-noise scale.  `315645` is the current best completed
fixed/private-transfer branch.  It used `3.072e10` mean analytical FLOPs,
`3.453332061e10` effective compute, and `38.10 ms` mean residual.

The exact submitted archive was:

```text
whest-starterkit/packages/to_test_remote/
submission_root_k26_affine_baselinesparse_vecstops_knownsign_corder_finalonly_bundle.tar.gz
sha256 e79c6de8493630a9a5b2bb8bab297d7a3dc1a19eac90546acaaca3284f8788f3
```

It passed a clean-extraction compile, exact rc5 C-order codec/manifest audit,
contract validation, and subprocess prediction smoke before upload.

## Public-suite 1.5e-7 route

The exact per-name minimum over graded submissions `315630`, `315632`, and
`315611` is:

```text
mean adjusted = 1.4827146665475675e-7
fullcov 315630 wins = 23 / 50
nofull  315632 wins = 16 / 50
pathCV  315611 wins = 11 / 50
```

The route table is saved in:

```text
cache/root_public_oracle_315630_315632_315611_20260710.csv
```

A deterministic public-name router was packaged and submitted.  Unknown names
default to the robust fullcov branch, so this is explicitly a public-suite
specialization rather than a claim of transfer to the fresh private suite.

```text
package:
whest-starterkit/packages/to_test_remote/
submission_root_public_name_router_315630_315632_315611_corder_bundle.tar.gz

sha256:
f6d988ae59689dee8c5f5cf5c678565129039cb6606ca5332c645769551fc53b
```

The 33.93 MB archive is below the 50 MiB limit.  Contract validation, exact rc5
manifest/C-order codec audit of all three nested direction stores, exact route
set comparison, and a clean-extraction mixed-branch smoke all pass.

After `315634` completed, the updated oracle over `315634`, `315632`, and
`315611` became `1.474889882590631e-7` with the same `23/16/11` branch-count
split.  A fully audited fallback router archive using `315634` as the default
branch is staged but was not submitted while the first router is grading:

```text
submission_root_public_name_router_315634_315632_315611_corder_bundle.tar.gz
sha256 d4f8bc6c7ad884330a39efee0a57d7757fdc53864022b9722c84aeafa73294c1
```

## Next fixed-branch refinement

Affine snapped states have the exact form `ReLU cloud * scale + column offset`.
The next product can therefore be evaluated as:

```text
(state - column_min) @ weight + column_min @ weight
```

The first term restores exact ReLU zeros and uses the sorted sparse kernel; the
second is one small bias product.  This is exact real arithmetic, not packing,
quantization, or value dropping.

Independent 20-model guards:

| Guard | Raw | FLOPs | Effective | Adjusted |
|---|---:|---:|---:|---:|
| Full spaced | `1.132593e-6` | `3.103e10` | `3.451e10` | `1.440044e-7` |
| Full offset | `1.379468e-6` | `3.105e10` | `3.475e10` | `1.757715e-7` |
| Mini spaced | `1.330407e-6` | `3.103e10` | `3.507e10` | `1.711895e-7` |

The standalone C-order archive passed compile, contract, codec/manifest, and
clean-extraction Phase-1 prediction checks and has been submitted for a remote
A/B.

```text
package:
whest-starterkit/packages/to_test_remote/
submission_root_breakthrough_k26_affine_baselinesparse_sorted8_symcov_f32state_corder_finalonly_bundle.tar.gz

sha256:
0a4b753f72333c7545aa5aaf4fbf98c4a99bbb03bec9cf37296eac7526410772
```

Remote submission `315634` completed successfully at `1.6727202255390684e-7`
adjusted and `1.2939723171712102e-6` raw, with mean FLOPs `3.100e10`, mean
effective compute `3.521e10`, and mean residual `42.02 ms`.

## 2026-07-10 local pruning sweep after `315645`

The strongest new statistical mechanism is target-free ReLU output screening:

1. reuse the paid fullcov preactivation mean to remove a small common set of
   outputs;
2. use the 96 largest live inputs as a per-row sign pilot;
3. skip only the most-negative pilot outputs on the exact positive tail.

No MLP names, public IDs, complex values, quantization, or bitpacking are used.
The rule is a permutation-equivariant function of the supplied weights and
live sampled state.

The current transfer-guarded point is common drop 20, row screen 64, with a
small `0.01 * fullcov_premean` term in the row score:

```text
candidate:
candidate_k26_affine_global20_fullcovmean_b001_screen64_fast_ragged_f32_root.py

offline raw/work ratios:
Full1000 spread100   raw=0.999171  affected_mult=0.8598
Full spaced20        raw=0.996578  affected_mult=0.8601
Full offset20        raw=1.002191  affected_mult=0.8600
Mini spaced20        raw=1.003642  affected_mult=0.8603
```

Production-shaped three-Mini A/B against the `315645` source:

```text
candidate raw=9.296271e-7  FLOPs=2.821e10  local adjusted=1.487876e-7
incumbent raw=9.261823e-7  FLOPs=3.080e10  local adjusted=1.577533e-7
```

Thus the real implementation gives up only `0.37%` raw on that guard, cuts
analytical FLOPs by `8.4%`, and improves local adjusted score by `5.7%`.
Single-model subprocess telemetry is also numerically sound:

```text
candidate raw=5.012143e-7  FLOPs=2.8356e10  residual=188.4 ms
incumbent raw=5.016835e-7  FLOPs=3.0967e10  residual=189.7 ms
```

The blocker is backend portability, not smoke correctness.  The ragged
gather/prefix kernel takes about `30-31 s` locally versus `7-8 s` for the
incumbent on the same quick-score path.  Earlier sparse-kernel remote runs
showed materially slower backend scaling, so this form is not yet a safe
upload under the 60-second per-MLP limit.  Its central score projection is
roughly `1.51-1.53e-7`, not a certain sub-`1.5e-7` result.

Related findings:

- A float32 ragged prefix changes checked raw MSE by only `0.003-0.030%` and
  reduces backend time by about `23-27%` versus float64.  This uses the native
  float32 data path; it is not a FLOP-accounting trick.
- A risk-aware 64/96 hierarchical pilot reached `1.0011x` raw and `0.8167x`
  affected work offline, but its production form measured `2.746e10` FLOPs,
  `3456` calls, and `46 s` local backend.  The second ragged gather erases the
  statistical advantage operationally.
- Common-only global pruning is backend-safe but projects around
  `1.53-1.56e-7`; it lacks enough arithmetic saving by itself.
- More aggressive input keeps (`228/188/final172`) cost `2.4%` raw on the
  100-network guard; starting screening at layer 5/6 did not transfer; pilot-
  only Gaussian tail marginalization was catastrophic.
- Paid fullcov-mean global drops 21-22 have a tiny work-product edge, but the
  production/raw margin is too small to displace the robust drop-20 point.

No further remote submission was made: the user requested local-first testing
until a candidate is a clear winner or a remote A/B is needed to resolve real
overfit uncertainty.

## Remaining bottleneck and honest path

At current remote raw `~1.294e-6`, the absolute 0.1 compute floor is
`~1.294e-7`; therefore a genuine `1e-7` result requires at least a `22.7%` raw
improvement *and* reaching the floor.  Quantization/bitpacking and complex
packing discussed on the forum are accounting shortcuts subject to repricing,
not that genuine path.

The immediate `1.5e-7` problem is narrower: make row-specific tail screening
backend-efficient, or find another `~0.3-0.5B` exact/low-risk arithmetic saving
without a large gather.  The longer-term `1e-7` route still needs a stronger
late-state/final-preactivation estimator—most likely signed covariance/higher-
cumulant information or variance reduction that improves information per
propagated row.  Direct final sampled moments, diagonal/fullcov sketches,
simple learned residuals, RQMC add-ons, and cheap routers have not transferred
broadly enough.

## Late-screen timeout localization and deadline-safe build

The production ragged kernel did transfer statistically, but its initial
runtime variants exposed two independent grader boundaries:

- fixed one-batch screening (`315672`) was fast on ordinary MLPs but timed out
  on public indices `2,10,19,23,26`;
- fixed two-batch screening (`315671`) rescued four of those five but incurred
  enough dispatch/backend overhead to cross the subprocess-respawn limit;
- adaptive one/two/four-way screening with calls 14--29 (`315679`) completed
  all 100 MLPs with zero row failures and scored public `1.502412741e-7`, raw
  `1.301997888e-6`, FLOPs `2.867659445e10`, and effective compute
  `3.133404698e10`, but the outer job still failed after about `16m05s`.
  Its internal evaluation record has `100/100` complete, `mlps_failed=0`,
  `error=null`, all workers idle, and `completed_at=null`; this is consistent
  with the full-session deadline/finalization envelope rather than an estimator
  exception.

Cumulative local FLOP tracing localized every fixed-one-batch timeout:

```text
call 27 complete: ~25.706B
call 28 complete: ~26.406B
call 29 complete: ~27.222B
```

The recurrent index-10 failure stopped near `25.6B`, entering call 28.  The
other four stopped near `26.3--26.65B`, entering call 29.  The targeted build
therefore screens calls 11--27 in the fast single-batch form and sends calls
28--29 through the already-proven exact sparse kernel.  It simultaneously
removes adaptive split overhead, whose remote mean wall was `46.7s`; the prior
fixed-one-batch completed rows averaged `38.2s`, projecting a roughly
13-minute full session on five workers.

Five-model local guard versus the adaptive start-14 build:

```text
adaptive start14 calls14--29: raw=1.369160e-6, FLOPs~28.62B
targeted single calls11--27:  raw=1.360378e-6, FLOPs~28.48B
```

The exact submitted archive is:

```text
whest-starterkit/packages/to_test_remote/
submission_root_breakthrough_global20_fullcovmean_screen60_partition_ragged_f32state_late11_27_exactlate_corder_finalonly_bundle.tar.gz

sha256 4506afdf3ac38139ad57b30d194d6a801074666d83fd4566296c789f9797617e
submission 315681
```

It passed compile, exact rc5 codec/manifest audit, clean extraction, contract
validation, and subprocess prediction checks at depths 2, 16, 20, and 32.

`315681` confirmed the statistical projection at `1.479074939e-7` public
adjusted, raw `1.298854298e-6`, mean FLOPs `2.855499664e10`, and mean effective
compute `3.093388074e10`.  It completed 98/100 rows but private indices 81
(`jeremy-bruce`) and 98 (`tammy-hammond`) failed with `TIME_EXHAUSTED` at
`64.2635s` and `64.5682s`; all budget/residual/combined flags were false.
Completed rows averaged `49.18s`, and total worker time divided by five was
`989.76s` (`16m29.8s`) before retry/finalization overhead.  The outer job was
marked failed while an intermediate inner view still said running, with stale
assignments and a contradictory `0 busy / 5` pool; the final inner record says
completed with 98 complete / 2 failed but still has `completed_at=null`.

The UI's `50/52 public` count is also wrong: `public_scores` contains exactly
indices 0--49 (all 50 scored), while the UI appends the two failed private rows
81 and 98 to the public denominator.  This explains why it simultaneously
shows 52 total and zero public failures.  The official challenge explicitly
documents a wall-clock reliability guard, and recent organizer forum posts
establish that grader bugs have required regrades, but these estimator rows
were still too close to that guard to be deployable.

The bounded replacement keeps row-specific ragged screening only on calls
11--20, then uses common global-drop-20 plus the proven sparse kernel on calls
21--29.  Thus no late product allocates a row-specific gather or prefix.

Five-model guard:

```text
raw=1.359703e-6
FLOPs=~28.73B
elapsed=90.97s
```

For comparison, the `315681` exact-late source took `107.11s` on the same five
models.  The new point is 15% faster locally with essentially unchanged raw,
projects around `1.49e-7`, and maps the prior remote mean/max wall to roughly
`42/55 s` rather than `49.5/65 s`.

Audited archive, submitted as `315684`:

```text
whest-starterkit/packages/to_test_remote/
submission_root_breakthrough_global20_rowcalls11_20_globalcalls21_29_f32state_corder_finalonly_bundle.tar.gz

sha256 d509bdac96d2a09ea4c56cd4c4779f29ab89bcd62bb54f5f492eb2623e6f3db4
```

It passes exact rc5 codec/manifest audit, clean extraction, contract validation,
and variable-depth subprocess checks.  The user explicitly approved one
controlled upload after the repeated grader failures.

## 2026-07-10/11 — analytic-second corder, K28 transfer check, and compact covariance

Submission `315687` established the new fixed-branch incumbent:

```text
adjusted       = 1.5065145012244841e-7
raw            = 1.2893487115661628e-6
F mean         = 2.872002692456e10
effective mean = 3.180009534456e10
residual mean  = 30.80 ms
```

Its two reusable mechanisms are target-free: use the already-paid analytic
post-ReLU second moments to choose a common 24-output drop on calls 6--29, and
carry the chosen hidden-column order into the next weight matrix instead of
restoring 13,312 row values after every layer.

Balanced K28 then gave a strong local statistical signal across three guards,
including raw below `1e-6` on Full spaced20.  Remote `315694` was nevertheless
an adjusted regression:

```text
adjusted       = 1.5399843752414293e-7
raw            = 1.2314804354218722e-6
F mean         = 3.080336993640e10
effective mean = 3.399552596040e10
```

Thus K28 transferred as a real `4.49%` raw improvement, but not the local
`13--16%` improvement, while effective compute rose `6.90%`.  It beat K26 raw
on 29/50 remote rows, but the per-row raw ratio was broad (`median 0.907`, max
`2.217`).  This closes the selected K28 bank as a fixed adjusted-score winner,
while retaining it as evidence that direction errors are complementary.

The next exact operational refinement makes the analytic side rollout model
the same pruned network as the sampled cloud.  After each common drop, both
paths carry only retained coordinates: a 232-column sampled state, plus the
corresponding 232x232 analytic covariance and matching weight rows.  Screening
starts one layer earlier, at call 5.  The covariance is re-symmetrized with the
public counted `flops.symmetrize` operation so symmetry metadata survives the
same-index submatrix extraction in the remote client.

Paired guards versus `315687`'s local analogue:

```text
Full spaced20: raw -0.394%, F -1.010%, raw*F -1.400%
Full offset20: raw -0.775%, F -1.009%, raw*F -1.777%
Mini spaced20: raw +0.194%, F -1.012%, raw*F -0.820%
```

The first archive (`315735`) used `flops.SymmetricTensor` directly and was
rejected during smoke with 0/3 worker passes because that constructor is an
in-process analysis API, not a participant-client API.  Nothing was graded and
the failure did not count against quota.  The traceback isolated the issue.

The corrected archive was tested under disposable environments matching the
grader exactly (`flopscope 0.8.0rc5`, `whestbench 0.12.0rc4`) and against the
actual `flopscope-client 0.8.0rc5`.  The client exposes `symmetrize` and
`SymmetryGroup`, while reproducing the documented rejection for direct
`SymmetricTensor`.  A fresh extracted rc5 prediction gives raw
`5.308205566296e-7` and `2.8272724114e10` FLOPs.

```text
submission 315736
archive:
whest-starterkit/packages/to_test_remote/
submission_root_k26_fullcovsecond_prunedcov_compact_start5_symmetrize_finalonly_bundle.tar.gz

sha256:
e42c01280d15a7ea8ff43132d6034dd3135bf057c0cdeb4f97559963bb688c42
```

Fine probes after submission did not displace this point.  Raising the new
proxy's final blend from `0.05` to `0.07` improved a small Full5 slice but
worsened Mini20 raw by `1.09%`.  Starting common pruning at signed call 4 with
24 columns was also split-sensitive.  An early drop of 16 looked strong on
Full5/Mini5 but failed Mini20 (`1.34707e-6` versus `1.33328e-6` for start-5).
Keep call 5 / drop 24 / lambda 0.05 as the robust branch.

`315736` completed successfully and crossed the requested threshold:

```text
adjusted       = 1.4966543767908342e-7
raw            = 1.2937776358512566e-6
F mean         = 2.843384016136e10
effective mean = 3.139169572136e10
residual mean  = 29.58 ms
```

Versus `315687`, raw worsened `0.3435%`, but analytical FLOPs fell by
`2.8619e8` and effective compute by `4.0840e8` (`1.284%`); mean residual also
fell from `30.80` to `29.58 ms`.  The net adjusted improvement was `0.6545%`.
This makes `315736` the completed fixed-branch incumbent and confirms that the
self-consistent compact covariance is a real operational gain, although the
remaining route to `1e-7` still requires a large raw-estimation improvement.

## 2026-07-11 — post-1.5e-7 breakthrough sweep

The first analytic H2/K3-tree port was fully audited and submitted as `315772`.
It passed remote smoke but regressed both score components:

```text
adjusted = 1.501432335935115e-7
raw      = 1.2981341501472343e-6
F mean   = 2.846812806664e10
```

Against `315736`, adjusted worsened `0.319%`, raw worsened `0.337%`, and FLOPs
rose `0.121%`.  It won only `24/50` paired public rows.  Keep `315736` as the
remote incumbent and close the H2 correction as a transfer candidate.

Fresh variance-per-row tests also closed several apparently promising routes:

- shifted rank-1 Gaussian lattice directions were `5.4%` worse even for their
  best tight-frame-repaired shift, and usually much worse;
- the `r64` mixed tight-frame bank reduced Full raw by about `21%`, but required
  about `27%` more sampled arithmetic; jointly increasing global pruning hit a
  hard work-product cliff (`drop48`: roughly `+9%` raw for `-2.8%` FLOPs);
- downstream-response-weighted pruning worsened the mixed-bank result;
- checkpoint particle recombination by within-stratum column-group shuffling
  carried essentially no signed final-error signal (best fitted ceiling below
  `0.2%` raw and literal coefficients were catastrophic);
- a current-depth vectorized equivariant residual model and a causal 32-layer
  moment-closure learner both remained near-flat on Full and failed Mini
  transfer.  The remaining error is predominantly quadrature variance, not a
  cheap deterministic residual exposed by the existing telemetry.

An empirical checkpoint-covariance observer produced a useful but ultimately
negative lesson.  Offline, blending only 4--8 spread K26 strata into the
analytic state at layer 8 improved Full100 by `~5.5%` and Mini100 by `~2.7%`.
An interleaved production port avoided a duplicate analytic tail and added only
`~0.44B` FLOPs.  Exact current-pipeline guards overturned the offline result:

```text
layer 8, Full20: raw 1.165947e-6 vs incumbent 1.140898e-6  # +2.20%
layer 4, Full20: raw 1.139997e-6 vs incumbent 1.140898e-6  # -0.08%
layer 4 F:       ~2.900e10 vs incumbent ~2.845e10          # +1.9%
```

Thus neither checkpoint is adjusted-positive and no archive was staged or
submitted.  The mismatch localizes the failure to the production affine/compact
particle state: empirical Gaussian state assimilation that helps the older
power-snap cloud does not preserve a useful correction on the incumbent.

The honest remaining `1e-7` route is narrower.  At the incumbent's remote
effective compute (`~3.139e10`), raw must fall to about `8.67e-7` (`-33%`); at
the absolute `0.1` compute floor it must fall below `1e-6` (`-22.7%`).  Generic
sampler count changes remain on the inverse-compute plateau.  The only measured
coordinate with ample headroom is signed final preactivation mean: true
`E[z_L]` nearly removes the error, and the old response-aligned K3cov analytic
coordinate cut K26 raw by `~23--25%` when treated as free, but its literal
query/carry implementation costs too much.  The next justified algebraic target
is therefore a final-rooted/adjoint contraction of that K3cov contribution that
does not materialize every `256x256` two-equal slice, ideally combined with an
exact sparse compute reduction.  Another generic point set, checkpoint
Gaussianization, or learner over the same seed summaries is not supported by
the completed evidence.

## Correction appendix (2026-07-12; agreement A-004)

The `~23--25%` response-aligned K3cov planning figure above is stale. On the
matched current geometry, the measured free-coordinate effect is about `9%`
raw: the `lambda*=0.08` response-aligned arm had relative raw `0.910` in the
2026-07-06 matched ledger measurement. That smaller figure is the controlling
planning number. It does not change the qualitative conclusion that signed
late-mean structure is valuable, but it rules out projecting a 23--25% gain
from the old unmatched comparison. See the dated 2026-07-06 response-aligned
K3cov entry in `EXPERIMENT_LEDGER.md` and
`agent_debate/MEASURED_AND_DEPRIORITIZED.md` r4.

## 2026-07-12 — D56 remote incumbent (submission 315968)

The dual-agreed P-001/P-008 descendant deepened only the late common column
drop from 24 to 56 on sampled product calls 20--29 and aligned the paid
full-covariance branch to the same schedule.  The exact bundle SHA-256 was
`aaa1c02bca7e61df25a8575634ca65dcf46b472f94464371f4534b412d701868`.
One user-authorized upload became submission `315968`.

Terminal remote result:

```text
adjusted score      1.4273280431276722e-7
raw MSE             1.299467404578536e-6
telemetry complete  100/100
failures            0
mean counted F      27.25745456072B
mean effective      29.88348621772B
mean/max wall       15.489281811 s / 21.971986029 s
```

Against parsed submission `315736`, adjusted improves `4.6321%`, counted
FLOPs fall `4.2669%`, effective compute falls `4.9361%`, and mean wall falls
`20.0359%`.  Remote raw MSE moves `0.4398%` worse, so the correct conclusion
is not “accuracy-free”; it is a validated adjusted-score efficiency win whose
compute saving exceeds its small raw drift.  `315968` is now the measured
incumbent.  The upload/result audit is
`agent_debate/probes/p008_remote_upload_315968_20260712.md`.

## 2026-07-12 — resumed successor closeout

The post-D56 search applied frozen price/representation gates rather than
promoting single-row projections:

- P-006 rev4's c8 paid-penultimate head passed exact capture/parity and had a
  plausible `+0.869288768B` real-site price, but generated nested four-fold
  OOF was decisively wrong-signed: hidden ratio `1.379452` and projected-Ez
  ratio `1.370699`. No c16, second seed, retune, bake, or package followed.
- Complete-Kerdock-129 plus exact zero-sparse arithmetic preserved Mini-0 raw
  at `1.05768235e-7`, but counted work fell only `162.532B -> 155.659B`
  (`4.23%`), with `207.015B` effective on that profile. The cross cannot
  convert the broad `2.90169e-7` raw teacher into a score successor.
- Exact sparse PathCV76 reaches `~90.14B` and raw `1.92677e-7` on Mini-0,
  but the frozen Full-spaced20 path-only raw is `5.539937e-7`; its broad price
  is near `1.8e-7` before residual. The one-row `~8e-8` projection is retired
  as non-transferable evidence, not a package candidate.
- Cheap common-skew passed exact parity and its `0.249160824B` counted cap,
  but an exposed contaminated timing row had effective point/q95 increments
  `0.864998102B/0.904024844B`, about `2.88x/3.01x` its cap. It closed with
  zero accuracy access and no rerun.
- The exact rank-one spherical Stein/C4 identities remain valid, but the sole
  rev3 price launch was invalid at start due an orphaned prior evaluator. Its
  explicit infrastructure-no-retry rule was enforced: no output, no price
  row, no accuracy access, and no retry.

The internal launcher now uses an atomic global registry guard, refuses every
pre-existing experiment lock (not merely the same name), and turns SIGINT or
SIGTERM into lock-cleaning exits. The associated A2.2 protocol requires
host-visible `ps -p` verification before any stale-lock removal; a dead tmux
pane is not proof of a dead process.

No successor is supported for remote upload. The sole retained analytic ember
is the component-specific covariance mixture described in
`agent_debate/probes/component_specific_covariance_mixture_static_feasibility_20260712.md`,
under source-only PSD/K4 feasibility, `dE<=5B`, and three-guard signed-Ez
`<=0.75` gates. Low-rank ACG importance sampling and deep-NNGP block
quadrature remain target-blind pre-gates only.

## 2026-07-14 — whole-tail LUT9 arithmetic successor (submission 316263)

An exact counted arithmetic-memory exchange replaced sampled nonnegative row
products on calls 7--29.  Each row is quantized to 512 levels relative to its
own maximum; a per-product table stores `code * W[i,j]`, and propagation uses
counted table gathers, reductions, and one row-scale multiplication.  This is
not hidden arithmetic or an encoded answer: all MLP-dependent products remain
inside the paid prediction path and are visible to flopscope.

On the disjoint Full20 + Mini20 broad guard, the frozen Mixed238/D56 candidate
measured:

```text
raw MSE                  1.505023522829e-6
exact-K26 paired raw     1.728434392918e-6
paired raw ratio         0.8707437951
mean counted FLOPs       21.9698818836B
mean residual            0.135098842s
10%-floor projection     1.131503179e-7
```

The stratified Full/Mini bootstrap gave raw-ratio 95% interval
`0.833635--0.925689` and floor-score interval
`1.08328e-7--1.20290e-7`; `P(raw improvement)=0.999965`.  These remain local
paired estimates, not remote results.

The exact archive passed RC5 NPZ codec/manifest audit, extracted contract and
prediction checks, and repeated isolated latency gates.  The local WSL worker
setup timeout is not a candidate regression: four alternating fresh starts
gave median `5.622s` for LUT9 versus `6.236s` for the remote-proven 315968
archive.  Nine isolated predictions over the historically slowest selected
Mini and Full rows had maximum worker wall `15.693s`, maximum end-to-end call
`17.236s`, and maximum residual `0.190s`, with stable predictions.

The user-authorized exact archive was submitted at
`2026-07-14T10:54+03:00`:

```text
submission              316263
archive SHA-256          71f4b0d834d0ad8ee3bb64a2a21adc3f2bcf088b37f87f8b2efdcfa53bfa18b7
archive                  submission_root_mixed238_d56_lut9_b4096_finalonly_20260714_bundle.tar.gz
remote status            graded
adjusted score           1.1920758902441732e-7
raw final-layer MSE      1.1907924300658123e-6
adjusted / raw           0.1001077820
inferred effective mean  27.229316710B
```

This is the new measured incumbent.  It improves the prior `315968`
adjusted score by `16.481996%`.  The inferred effective compute sits only
`0.1078%` above the challenge's 10% multiplier floor, so further arithmetic
savings alone have almost no value unless they also reduce raw error or
protect reliability.  Remote raw was `5.2399%` worse than the local
remote-anchored projection (`1.131503179e-6`), which is the calibration to
apply to future local projections rather than treating the local mean as a
one-to-one remote forecast.

### Immediate post-incumbent reinvestment falsifiers

Tested whether the counted-FLOP dividend could buy seven whole bases from an
independent, fixed second Kerdock orientation.  The bases were selected
without targets by exact H6 cross-energy.  The candidate was encouraging on
Full but did not transfer to Mini:

```text
initial Full5 raw ratio       0.873917
initial Mini5 raw ratio       1.037160
disjoint Full5 raw ratio      0.696644
disjoint Mini5 raw ratio      1.234967  # all five rows worse
combined disjoint10 ratio     0.919733
counted work                  about 26.47B vs 21.97B
```

The raw gain is insufficient for the roughly 21% work increase and is
split-unstable.  A control using seven fresh target-independent QR bases was
even clearer: Mini index 0 worsened `1.497x`.  Close both partial-frame
augmentation forms; no broad run, package, or submission is supported.

Selective 10-bit precision was also tested without changing the cloud or
batching.  Calls 7--19 remained at 9 bits and only calls 20--29 used 10 bits,
the portion where the prior 8-to-9-bit ablation had the best return.  On the
paired five-row screen:

```text
                 raw ratio vs LUT9   wins   counted-FLOP ratio
Full5                 0.992213        4/5        1.009274
Mini5                 1.008867        0/5        1.009196
combined10            1.002440
```

The higher precision is wrong-signed overall and strictly loses adjusted
economics.  This indicates that 9-bit propagation is already providing useful
finite-precision regularization, rather than being an approximation whose
raw score improves monotonically toward exact arithmetic.  Close LUT10; no
broad run or package.

## 2026-07-14 — complete-design frontier after submission 316263

The honest real K129/D56/LUT9 implementation was tightened without changing
its prediction.  Global cross-bucket batches and an equivalent counted einsum
reduction reproduce Full-0 raw `1.989072665761e-7` and `87.703617419B` FLOPs,
while clean-extraction wall fell to `43.78s`.  The explicit six-file archive is:

```text
submission_root_kerdock129_d56_lut9_lambda015_globalbatch_einsum_real_20260714_explicit_bundle.tar.gz
sha256 3974170ba4ea0b3a1b3cfffd8cf1e86762f501f4f56103c3916dcacf34dff9ff
```

All manifest hashes, compilation, `whest validate`, Full-0, Mini-0, and
Mini-65 isolated checks passed.  It is nevertheless not a supported upload.
The original 36-row guard plus ten untouched rows gives raw
`2.829599393111e-7` (family-stratified bootstrap 95% interval
`2.315e-7--3.424e-7`).  Applying submission 316263's measured raw and residual
transfer gives a point adjusted projection around `1.17e-7`, essentially tied
with the incumbent and too uncertain to spend a remote attempt.

Two complete independent K129 orientations produced the real statistical
breakthrough: their equal average cut the original broad-36 raw to
`1.300278007874e-7` (`-46.10%`).  An exact compact-FWHT K258 implementation
reconstructs every frozen direction bit for bit and passes the strict 8-GiB,
60-second worker.  On ten untouched rows the quarter-H2 and inherited-H2 arms
score `1.281866255541e-7` and `1.264928294843e-7`; the small `1.3%` pooled
difference reverses between Full and Mini, while every row completes within
`31.87s`.  Thus the complete-design cancellation transfers; the H2 choice is
not the source of the breakthrough.

The compact output-packed L3 archive subsequently graded as submission
`316276`:

```text
remote raw                 1.161788501847e-7
remote adjusted            7.598512223542e-8
mean counted F             173.958406723B
mean effective             177.901836091B
mean residual              39.434ms
failures                    0
```

This is the first measured sub-`1e-7` result and validates the complete-design
science and operational package.  It does not close the durable objective:
the kernel packs two real output columns into `complex64`, and organizers have
announced complex-operation repricing.  Keep it as the current measured score
and high-value teacher, not as proof that the same adjusted economics survive
the scheduled accounting change.

An honest real-LUT attempt to obtain the needed wall reduction by retaining
only the largest 96 positive terms per row is closed.  On Full-0, raw moved
from `1.989e-7` to `9.961e-2`; rowwise L1 rescaling gave `7.467`, and adding
the exact omitted cloud-product mean still gave `2.078e-2` while wall rose to
`57.7s`.  The small positive tail carries essential rowwise fluctuations;
neither scale nor first-moment repair reconstructs them.
