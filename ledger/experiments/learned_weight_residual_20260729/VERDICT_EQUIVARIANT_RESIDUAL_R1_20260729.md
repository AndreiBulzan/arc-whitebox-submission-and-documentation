# Lawful learned weight/readout residual — R1 verdict

Date: 2026-07-29.

## Verdict

**Killed after the first decisive screen.** A shared
output-permutation-equivariant correction fitted on Full does not transfer to
either a disjoint Full partition or the process-separated Generated family.
It is far from the required raw `1.6e-7`.

The selected model was a 156-feature ridge. Its target-free inputs were the
already-paid K146 final-cloud arm moments and Gaussian/Edgeworth readouts,
plus a cheap diagonal Gaussian closure. The broader screen also tested
equivariant summaries of the final two weight matrices, a small neural
network, boosted trees, and extra trees.

## Broad statistical result

| family | baseline raw | corrected raw | ratio | rows improved |
|---|---:|---:|---:|---:|
| Full train+dev refit | `2.4051e-7` | `2.0913e-7` | `0.8695` | `66.2%` |
| Full disjoint held | `2.5428e-7` | `2.7487e-7` | `1.0810` | `23.1%` |
| Generated transfer, noise-corrected | `2.4600e-7` | `2.5787e-7` | `1.0482` | `34.4%` |

Full split:

- train: dataset index modulo 4 in `{0,1}` (`47` MLPs);
- development: modulo 4 equal to `2` (`27` MLPs);
- held: modulo 4 equal to `3` (`26` MLPs).

Only Full train/development targets participate in fitting or selection.
Generated targets are opened once for the final transfer score.

## Interpretation

The in-sample gain is ordinary fit capacity, not a transferable estimator
signal. Weight-conditioned features did not win the Full development screen;
the selected subset used final-cloud/closure features only. The disjoint Full
and Generated reversals close this learned endpoint-residual lane under the
current observables. Reopening it would require a genuinely new
inference-time state, not a larger fitted model over the same readout.

Exact source and receipt:

- `screen_equivariant_residual_r1_20260729.py`
- `equivariant_residual_r1_20260729.json`

No estimator propagation, physical row, FlopScope session, package, network
action, upload, submission, or remote action occurred.
