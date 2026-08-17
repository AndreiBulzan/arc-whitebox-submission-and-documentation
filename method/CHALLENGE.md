# Challenge and current constraints

## Task

The challenge is
[ARC White-Box Estimation Challenge 2026](https://www.aicrowd.com/challenges/arc-white-box-estimation-challenge-2026),
organized by ARC and AIcrowd.

For every supplied random ReLU MLP, an estimator returns a
`(depth, width)` matrix containing the expected post-ReLU activation of each
neuron under `x ~ N(0, I)`. Only the final-layer row determines ranking;
earlier rows are diagnostics.

Current Phase 1 geometry:

```text
width                  256
depth                  32
weight initialization  He-Gaussian
input                   standard normal
effective budget B     272,000,000,000 per MLP
dataset revision       v1-phase1
```

The public challenge overview still contains warm-up examples and an older
`max(0.5, ...)` scoring expression. Do not use those values for Phase 1.
Current starter-kit scoring code, live evaluation payloads, and Phase 1
telemetry agree on the values below.

## Score

For MLP `m`:

```text
F_m = analytical FlopScope count
R_m = residual wall seconds
C_m = F_m + 100,000,000,000 * R_m
q_m = max(0.1, C_m / 272,000,000,000)
s_m = final_layer_MSE_m * q_m
score = mean_m(s_m)
```

Lower is better. Compute is applied row by row before averaging.

- If `C_m > 272B`, the row fails: its prediction is replaced by zero and its
  multiplier is forced to `1.0`.
- Below `27.2B`, the multiplier remains at `0.1`; further compute savings do
  not improve score.
- `all_layers_mse` is diagnostic and does not affect ranking.
- `residual = wall - FlopScope backend - FlopScope overhead`. Backend and
  wrapper overhead do not enter `C_m`, but they still consume operational
  wall time.

## Current meter and hard surfaces

```text
FlopScope             0.9.1
NumPy                 2.2.6
pinned source commit  63a6121c700de80f0eea09a812e2e72482c48501
ordinary result cap   104,857,600 bytes (100 MiB)
campaign design cap   103,809,024 bytes (99 MiB)
submission bundle     at most 50 MiB uncompressed file total and 50 files
```

The 99 MiB value is our safety margin, not a separate organizer limit.
The project uses an 8 GiB per-worker memory simulation as a conservative
gate; the installed SDK may default higher. Treat memory as an operational
gate and record the configured limit in every receipt.

FlopScope 0.9.1 prices dtype width, complex arithmetic, copies,
materialization, gathers, selection, and many previously cheap layouts.
Pre-0.9 complex packing and “free movement” counts are historical.

## Operational wall constraints

Evidence from our owner-visible remote telemetry:

```text
per-MLP guard             about 60 seconds
workers                   about 5
public/private suite      100 MLPs in observed campaigns
failed slow-lane region   about 800 seconds for a 20-row worker
safe campaign target      720--750 seconds per 20-row worker
```

The often-mentioned `999s` event was observed as an outer five-worker suite
stop, not a published 1,000-second guarantee for each worker. Use
`<36--37.5s` average per row for a comfortable 20-row lane and test
stragglers. A candidate can pass every 60-second row and still lose the
suite deadline.

## Compliance

- Use the official estimator contract and counted FlopScope operations.
- No network is available during grading.
- Bundle every artifact and dependency in the tarball.
- Do not modify FlopScope, inspect private seeds or grader state, or use an
  accounting omission as computation.
- LLM assistance and offline training are permitted; disclose them in the
  final write-up.
- Prize-eligible code must be released under an OSI-approved license.
- Final ranking is a fresh private rerun, so public-row routing or seed
  lottery is not a solution.

Authoritative links:

- [challenge](https://www.aicrowd.com/challenges/arc-white-box-estimation-challenge-2026)
- [leaderboard](https://www.aicrowd.com/challenges/arc-white-box-estimation-challenge-2026/leaderboards)
- [starter kit](https://github.com/AIcrowd/whest-starterkit)
- [FlopScope](https://github.com/AIcrowd/flopscope)
