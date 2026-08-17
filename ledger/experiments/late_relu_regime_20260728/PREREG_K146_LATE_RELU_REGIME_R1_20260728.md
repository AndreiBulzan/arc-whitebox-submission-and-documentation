# K146 late ReLU dead/on/kink regime R1

Date: 2026-07-28  
Evidence: target-free two-family `component`; cost remains a separately
derived `projection`.

## Fixed question

At late hidden layers 24--30, classify each retained preactivation
coordinate as:

```text
always-on  if mean(z) - 7 * std(z) > margin
dead       if mean(z) + 7 * std(z) < -margin
kink       otherwise
margin     = 2^-18 * max(rms(z), 1e-6)
```

The fixed seven-sigma threshold has a one-sided Gaussian tail below
`1.3e-12`. Classification is recomputed target-free from the current
particle cloud for each supplied MLP. Always-on ReLU is replaced by the
exact identity `ReLU(z)=z`; dead ReLU is replaced by exact zero; kink
coordinates retain literal ReLU.

R1 also records an exact observed-sign upper bound, using `min(z)>margin`
and `max(z)<-margin`. That arm is pathwise diagnostic, not a remote
generalization assumption.

## Fixed grouping and candidates

Classification is tested both globally per quadrature arm and on natural
contiguous 256-row half-basis blocks. No row permutation is performed.

```text
baseline
global_tau7
halfblock_tau7
global_exact_sign
halfblock_exact_sign
```

Every candidate otherwise uses the associated K146/m17 width-216 graph,
including its late armwise width-192 selection, final width 176, mean
restoration, gamma readout, and literal 129:17 blend.

## Fixed rows

No challenge target is needed or opened. Prediction deltas are measured
against the sealed-in-capture baseline on:

```text
Full       409, 419, 421, 431
Generated    7,  11,  15,  19
```

## Gates

The baseline must associate on Full0 under relative RMSE `<=2e-6` and
maximum absolute error `<=3.2e-5`.

A regime replacement preserves pathwise output only if, separately in
both families:

- maximum absolute output delta is `<=1e-7`;
- mean prediction-delta MSE is `<=1e-12`;
- all outputs are finite.

An implementation can advance only if an independent released-rule cost
derivation shows positive net savings after classification, masks,
gathers, setup fusion, and all tall contractions are priced. Merely
skipping ReLU is not assumed material.

## Boundaries

The shared benchmark lock is mandatory for CUDA capture. No targets,
physical rows, FlopScope executions, packages, network actions, uploads,
submissions, or `STATUS.json` mutations are authorized.

