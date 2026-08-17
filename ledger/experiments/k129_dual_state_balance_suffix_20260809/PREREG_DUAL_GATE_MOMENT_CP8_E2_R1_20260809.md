# Preregistration: frozen checkpoint-8 D3 replication E2

Date: 2026-08-09

Evidence: target-free-sealed Full4/Generated4 component replication; projected
compute/adjusted score only. No official Mini100, physical FlopScope run,
upload, submission, or remote action.

Freeze the E1 near-miss exactly:

- checkpoint 8;
- q0 and `polar_q0_right_d2` complete prefixes;
- 32 actual layer-9 ReLU coordinates selected by right-column squared norm;
- whole antipodal-pair `state + probe` first and diagonal-second moments;
- unchanged deterministic D3 greedy/two-flip selector;
- one exact selected suffix and the production endpoint readout;
- modeled effective compute `180.429423672B`.

Rows are disjoint from D1--D4, broad B1, and E1:

- Full: `408, 508, 608, 708`;
- Generated: `120, 121, 122, 123`.

Promote to a bounded broad replay only if projected adjusted score is at most
`1.10e-7` and at least 3/4 rows improve over q0 in both families. Otherwise
the checkpoint-8 spelling is killed.
