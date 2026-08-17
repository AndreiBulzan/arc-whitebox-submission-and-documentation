# Balanced multi-orientation Kerdock herding R1

Date: 2026-07-29

Evidence target: **component**.  Any final-score or compute arithmetic is a
**projection**.  This experiment performs no FlopScope work, packaging,
upload, submission, or remote action.

## Question

The complete equal mixture of all eight Kerdock orientations is a strong
signed-final-preactivation oracle (`0.2216x` Full and `0.3335x` Generated
versus the current K146 rule), but all 1,032 bases are unaffordable.  The
previous K120 all-eight portfolio fit unrestricted atom weights and
orientation counts; its training covariance advantage reversed on held
endpoint rows.  The earlier population-code scout fit each orientation
independently and stopped at total K64.

This experiment tests a distinct, symmetry-preserving construction:

- every selected atom has exactly equal weight;
- every orientation has exactly the same quota at each reported checkpoint;
- one fixed support is learned from the combined Full/Generated training
  halves;
- there is no MLP-specific routing, fitted coefficient, target access,
  ridge, retry, or family-specific support.

## Frozen construction

For every training family, subtract the complete equal all-eight population
endpoint from each of its 1,032 basis endpoints.  Form a family-normalized
endpoint Gram and combine the two family Grams with literal weight `1/2`.

Starting with the empty support, greedily add the atom that gives the
smallest exact increase

```text
G[i,i] + 2 * sum(G[i,j] for j in selected)
```

in the unnormalised equal-weight population-error objective.  At each step,
only orientations at the current minimum quota are eligible.  Therefore the
orientation counts differ by at most one during construction and are exactly
equal after every eight additions.  Atom reuse is forbidden and ties use the
lowest flattened atom ID `129 * orientation + basis`.

Freeze the nested equal-quota checkpoints

```text
K = 64, 80, 96, 112, 120, 128, 136, 144
```

before exact signed targets are opened.

## Target-free held screen and fixed promotion choice

For every checkpoint, compare the held endpoint MSE to the complete
all-eight population against the current fixed K146 rule
(`129` complete O0 bases plus its frozen O1 support17).

The single checkpoint admitted to signed-target scoring is chosen without
targets as follows:

1. require both family aggregate population-error ratios `<= 0.95`;
2. require at least `55%` of rows improve in each family;
3. require row-ratio p95 `<= 1.75` in each family;
4. among passing checkpoints minimize
   `max(full_ratio, generated_ratio) * K / 146`;
5. ties choose smaller K.

If none passes, freeze no promotion candidate and stop before signed-target
access.  All checkpoint metrics may still be retained as a target-free
falsifier.

## Exact signed gate

For the one target-free-selected checkpoint only, compare its held
signed-final-preactivation MSE with current K146.  A full final-prediction
capture is merited only if, separately on Full100 and Generated64:

- aggregate signed MSE ratio is `<= 0.80`;
- at least `55%` of rows improve;
- row-ratio p95 is `<= 1.50`.

This is a capacity gate, not a final-ReLU score claim.  Passing it only
authorizes a target-free full-propagation capture of the exact frozen
support.

