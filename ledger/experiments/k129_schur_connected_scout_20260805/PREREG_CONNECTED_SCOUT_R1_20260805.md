# Connected-scout Schur selector R1

Date: 2026-08-05

Evidence sought: **component**.  This is a fast ordinary-CUDA accuracy
falsifier.  It opens no targets during capture and performs no FlopScope
row, package, upload, submission, or remote action.

## Prior-work boundary

Blocking searches covered empirical/realised gate pullbacks, trajectory
selectors, scout-conditioned frames, gate-aligned QR, paired gate
innovation, response-projected trajectory features, and adaptive frames.
The nearest failures either rotated a complete secondary arm from `W0`
alone, learned a correction from averaged trajectory summaries, or selected
partial alternate-frame atoms.  No recorded experiment uses a small
realised particle trajectory solely to choose the invariant planes of the
already-successful complete K129 Schur frame.  This spelling is therefore
new in the capsule.

## Fixed mechanism

1. Propagate the 512 antipodal rows of the first q0 orthogonal basis through
   the uncompressed, realised 32-layer MLP in float32.
2. At each layer record the per-neuron empirical active fraction `p_l`.
3. Replace R84's scalar-gate pullback by

   ```text
   s_(l-1) = W_l^2 @ (p_l * s_l),   l = 31,...,1,
   ```

   renormalising after every layer.
4. Keep the polar frame, signed-angle pairing, gamma `0.50`, one-plane-per-
   pair rule, K129 propagation, and transferred R84 output scale unchanged.

The scout is estimated to add about `1.08B` instrumented operations, under
one percent of the current effective compute.  It must therefore buy more
than a cosmetic raw change.

## Fixed pilot and gate

- Full and Generated positions `16..31` of the sealed Schur broad bank.
- Official Mini100 rows `16..31`.
- Controls: q0 and the existing gamma-0.50 R84 prediction, both with the
  transferred R84 scale.

Promote only if all of the following hold:

1. candidate/R84 raw-MSE ratio is at most `1.0` in every family;
2. pooled ratio is at most `0.985`;
3. at least `8/16` rows improve in every family.

Otherwise kill this exact one-basis connected selector without tuning scout
size, exponent, layer weights, or thresholds on the opened pilot.

