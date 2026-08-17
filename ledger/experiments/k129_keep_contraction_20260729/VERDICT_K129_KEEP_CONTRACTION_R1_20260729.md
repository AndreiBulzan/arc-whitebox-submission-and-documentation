# K129/R21 late/final keep contraction

Date: 2026-07-29

Verdict: **kill for the immediate `1.2e-7` checkpoint**.

Evidence boundaries:

- compressed arrays: **broad statistical development captures**;
- reuse of the R21 `lambda=0.0075` delta: **projection**;
- count deltas: **exact current-graph static projection**;
- remote adjusted values: **projection**.

No new prediction was captured, no previously unopened target or confirmation
target was opened, no physical FlopScope row ran, and no package or remote
action occurred.

## Why the small knob cannot do it

For the O0-only K129 graph, the exact current tail slopes are:

```text
late, per layer and retained channel       18,435,963
late, all seven layers per channel        129,051,741
final, per retained channel                20,829,898
```

Therefore:

```text
late 192 -> 184, final 176 unchanged     1,032,413,928 saved
late 192 -> 184, final 176 -> 160        1,365,692,296 saved
late 192 -> 184, final 176 -> 168        1,199,053,112 linear saving
```

Final keep 168 is not executable by the present M267 owner; it requires a
new/ragged compiler. More importantly, even assuming **zero accuracy loss**,
all three savings are far below the conservative `3,848,047,652` required
beyond R21 to clear the checkpoint.

## Existing broad evidence is decisively negative

The previously sealed late-184/final-176 q0 arrays are exactly associated
with the current R21 control trajectory. Adding the already-sealed R21
`lambda=0.0075` prediction delta back as a diagnostic graft gives:

| family | R21 raw | late184/final176 raw | raw ratio |
|---|---:|---:|---:|
| Full100 | `2.53202e-7` | `2.66940e-7` | `1.05426` |
| Generated64 corrected | `2.59766e-7` | `2.95445e-7` | `1.13735` |

After its `1.032B` count saving, the projected remote adjusted bands are
approximately `1.285e-7--1.290e-7` and `1.386e-7--1.392e-7`,
respectively. The endpoint correction does not rescue the contraction.

The deeper previously captured schedules are much worse:

```text
late176/final160 graft raw ratio   1.733 Full / 2.185 Generated
late168/final152 graft raw ratio   6.326 Full / 7.017 Generated
late160/final144 graft raw ratio  37.084 Full / 33.291 Generated
```

The final-152 schedule is also incompatible with the present M267 geometry.

## Selective-layer boundary

A selective schedule can cross the arithmetic threshold only by ceasing to
be a small contraction. For example:

```text
six of seven late layers 192 -> 160
final 176 -> 160
exact saving 3,872,983,264
```

That exceeds the conservative requirement by only `24,935,612` operations,
leaving roughly `0.018%` raw-error headroom. There is no pre-existing
selective-layer prediction capture, so it cannot support a robust checkpoint
claim. The all-layer evidence already shows that late truncation is
accuracy-sensitive.

If this class is ever reopened, the only meaningful fast falsifier is a
target-free selective capture that forks after layer 23 and tests early-late
compression while leaving layer 30 and the final readout wide. It should not
displace the current checkpoint lanes.

Reproducible receipt:
`k129_keep_contraction_r1_20260729.json`.
