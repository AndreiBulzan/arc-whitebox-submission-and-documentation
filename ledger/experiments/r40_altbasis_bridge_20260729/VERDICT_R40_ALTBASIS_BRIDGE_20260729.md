# R40 alternative-basis bridge verdict

Date: 2026-07-29

Status: **translation bridge passes; quick K162 runtime successor killed**

Evidence label: **projection**. No estimator import, production array,
FlopScope operation, target, benchmark lane, package, upload, submission, or
remote action occurred.

## Result

The upstream opportunity is real and reproducible:

- `/tmp/Fast-Matrix-Multiplication-r40` is present at pinned commit
  `8bb354d63061504d1a712efafc8d06a0e8fa3f07`;
- all five MATLAB SLP hashes match the prior audit;
- the capsule's existing M185 `Normalizer`, `compile_plan`, and
  `execute_plan` translate all six alternative plan families; and
- all 18 tiny integer compiler-parity cases pass exactly.

The sparse-C program's one `iC38*2` is rewritten as `iC38+iC38`, which was
byte-identical on four float32 checks. The normalized binary-node inventory
is:

| plan | nodes | offline compiled calls | offline peak logical slots |
|---|---:|---:|---:|
| left basis | 74 | 55 | 65 |
| right basis | 21 | 17 | 17 |
| sparse A | 44 | 26 | 34 |
| sparse B | 36 | 25 | 31 |
| sparse C | 69 | 48 | 63 |
| inverse basis | 72 | 52 | 53 |

The translated node/output metadata is banked in the JSON artifact, so this
parsing work does not need to be repeated.

## Why it is not a quick successor

The live R40 cell-wave chassis exposes exactly three recursive plan slots:
`A`, `B`, and `C`. The alternative algorithm needs staged basis and sparse
transforms on both recursive levels.

This is not a metadata swap. Naively composing the stages into the live
three slots gives:

```text
                 live direct       naive basis+sparse
A nodes/digit         108                   118
B nodes/digit          54                    57
C nodes/digit         132                   141
```

That loses the opportunity. The audited `1.569409632B` saving exists only
because the basis stages branch over `18/9/18` nodes while the sparse core
branches over rank 40. Preserving that topology requires new cap-safe
carriers and bindings for:

1. the natural 18-block left basis at both recursive levels;
2. the natural 9-block right basis before sparse B; and
3. sparse-C output followed by the 18-block inverse basis before the live
   state/ReLU owner.

It also requires new liveness, scaling, decoder, teardown, and cap proofs.
Several flat compiled plans already peak above the live 32-slot workspace,
which confirms that the current storage contract is not reusable unchanged.

The alternative spelling changes float32 association (the prior tiny audit
measured relative RMSE around `8e-7`), so it cannot inherit the accepted
prediction or broad raw-score evidence. The projected count saving is only
`0.958%` of the prior whole-count anchor, with no wall or accuracy saving
established.

## Decision

Do not put this on the current fast K162 checkpoint path. Reopen it only as a
dedicated accuracy-changing runtime project after a static cap/liveness
design for the three staged carrier families. The translated plans make that
future project possible, but there is no honest quick mixin or metadata-only
bridge into persistent-operands R1.

Authority:

- `audit_altbasis_codegen_bridge_static_20260729.py`
  SHA-256 `4fffbb279407969478a30a6b902431c20cc25105899e3631054e6f8a0d472c86`
- `altbasis_codegen_bridge_static_r1_20260729.json`
  SHA-256 `eb5e6a5fc1a78e5b8e3812ef94f7880d90ff8805b083d4255b9924dc68de8c4a`

