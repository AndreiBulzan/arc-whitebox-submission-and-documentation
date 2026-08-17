# K129 response-conditioned full-design rotation R1

Date: 2026-07-31.

Evidence target: a fast **component development diagnostic** on Full rows
0--3 and Generated rows 0--3.  This authorizes no FlopScope session,
physical benchmark row, package, upload, remote action, or submission.

## Question

The incumbent propagates one complete 129-basis real Kerdock spherical
5-design.  Any common orthogonal rotation of every input node preserves its
node count, weights, antipodal structure, and all degree-at-most-five moment
identities.  Can a rotation conditioned on the supplied MLP reduce the
first missing higher-order alias without changing the estimator downstream?

## Frozen candidates

Construct the diagonal-Gaussian mean gates from weights only and the
mean-gated input-to-output response

```text
J = W0 D0 W1 D1 ... W30 D30 W31.
```

Take the leading eight left singular vectors of `J`, canonicalizing each
sign by making its largest-magnitude coordinate positive.  Prior component
work found that this rank carries nearly all of the mean-gated response
energy.  Construct a common input rotation with sequential Householder
maps, taking those eight directions to:

1. the first eight coordinate axes (`response_axis8`);
2. the first eight normalized Walsh-Hadamard columns
   (`response_walsh8`); and
3. the first eight orthonormal DCT-IV columns
   (`response_cosine8`).

The Householder extension fixes already-mapped directions and avoids using
the arbitrary near-null tail of a full singular frame.  Apply the rotation
only by replacing `W0` with `Q @ W0`; keep K129, all compression selections,
the exact endpoint readout, lambda `0.0075`, and scale `1.000025` unchanged.
The unrotated `current` arm is captured in the same process.

## Kill gate

Seal all four predictions before opening either target bank.  A candidate
is worth a broader test only if, separately on Full4 and Generated4:

- pooled raw-MSE ratio versus `current` is below `0.97`;
- at least two of four rows improve; and
- every output is finite.

The small screen can only promote a candidate to falsification.  It cannot
establish broad accuracy, deployment economics, or submission readiness.

