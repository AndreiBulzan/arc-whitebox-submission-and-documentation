# K129 late-state coreset R1 preregistration

Evidence target: **component** capacity screen.  This is not a physical-row,
package, or remote candidate.

## Fixed hypothesis

At the output of layer 23, replace the 66,048 equally weighted K129/O0
particle states by 33,024 equal-weight pair centroids, then run the unchanged
layer-24--31 graph.  Pair centroids preserve the empirical layer-23 mean
exactly.  A pair whose members remain in the same downstream ReLU cells also
preserves its final mean contribution exactly.

Six target-free pairing rules are fixed before target access:

1. `antipodal`: pair the positive and negative member with the same
   basis/direction ordinal;
2. `hash8`: use the signs of eight high-norm layer-24 preactivation columns,
   pair within identical sign buckets by normalized sketch radius, then pair
   deterministic leftovers;
3. `hash16`: the same construction with sixteen columns;
4. `hash32`: the same construction with thirty-two columns;
5. `post24_hash16`: compute the ordinary full layer 24, then hash and merge
   its states using sixteen high-energy state coordinates;
6. `post24_hash32`: the same post-layer-24 merge with thirty-two coordinates.

Layer-24 input-channel selection is computed from the full checkpoint state.
Later channel selections are recomputed from the compressed state.  Candidate
readout uses the exact equal-weight analogue of the deployed lambda-0.0075
correction and the frozen 1.000025 output scale.

## Fixed rows and decision

Seal predictions for Full indices `[0, 1]` and Generated indices `[0, 1]`
without opening targets.  Then score the sealed arrays.

Continue only if one fixed rule satisfies both:

- pooled candidate/control raw-MSE ratio `<= 1.05` in each family; and
- its projected adjusted ratio, including the explicit sketch cost, is
  `< 0.93` in each family.

Otherwise kill this exact late-centroid class immediately.  No broad run,
FlopScope row, packaging, upload, or submission is authorized by this screen.
