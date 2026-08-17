# Preregistration: predicted local packet-basis S16 fallback L2/R1

Date: 2026-08-08

Evidence scope: `component`, target-free selector test.

The direct local-GP estimator failed post-seal because its source prediction
missed a large common mode before a near cancellation.  This fallback asks a
narrower question: can the predicted *basis geometry* choose 16 exact packet
bases whose signed weighted response reconstructs the true packet source?

Freeze the following before selection:

- use only the sealed `basis_energy` predicted source vectors;
- select separately per network;
- support size exactly 16;
- initialize with OMP plus eight fixed random supports;
- run exact one-for-one swap descent in predicted 256-output space;
- fit unrestricted least-squares weights on each predicted support;
- use no exact packet source, replicate identity, or expectation target in
  support or weight selection.

After sealing supports and weights, evaluate them target-free against:

1. the 32-replicate expected packet source;
2. each independent 16-replicate half;
3. every individual antithetic packet replicate.

The fallback passes only if the pooled expected-source residual MSE is at
most `3.1e-8`, both families are below `5e-8`, and neither half exceeds twice
the pooled central residual.  These are deliberately looser than the known
`8.68e-9` allowance for retaining 70% of packet gain.  A miss rejects this
specific predicted signed-S16 bridge; it does not reject the unavailable
exact-source S16 capacity theorem or all possible common-mode observables.

No expectation target, Mini100 row, physical benchmark row, package, upload,
or remote action may be opened.
