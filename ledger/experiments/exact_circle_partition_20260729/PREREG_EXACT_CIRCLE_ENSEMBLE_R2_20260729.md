# Preregistration: exact-circle ensemble R2

Date: 2026-07-29

Evidence label: **component**.

## Question

Does exact integration over every angular ReLU cell make a very small
great-circle ensemble competitive enough to offset its large per-plane
coefficient-propagation cost?

## Frozen screen

- Full row: `3`
- Generated row: `35`
- Haar plane seeds: the deterministic rule in the capture source
- plane prefixes: `1`, `2`, and `4`
- depth: `32`, width: `256`
- roots: every open-cell zero of every preactivation, no pruning or merging
- outputs: exact final post-ReLU angular mean times the exact
  256-dimensional Gaussian radial mean

The capture reads `weights.npy` only and freezes all predictions before the
scorer opens targets.

## Controls and gate

The post-seal scorer compares each prefix to:

1. the existing equal-row-count Kerdock K8 control on the same row;
2. the existing K146/m17 broad prediction on the same row.

This class is killed if the exact four-plane prefix is worse than Kerdock K8
on either row, or if the optimistic 272B coefficient-work projection fits
fewer than 32 planes. The second condition is deliberately generous: it
charges only two dense matmuls per exact interval and grants the entire
challenge budget, ignoring root discovery, masks, integration, and closure.

No FlopScope session, physical row, package, upload, submission, or remote
action is authorized.
