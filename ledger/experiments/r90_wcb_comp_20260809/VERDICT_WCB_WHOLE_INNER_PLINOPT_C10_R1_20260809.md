# Verdict: WCB whole-inner PLinOpt C10-R1

Date: 2026-08-09

Verdict: **kill this direct whole-inner PLinOpt spelling**.

Evidence label: **component plus projection**. No target, MLP, FlopScope
session, benchmark row, Mini100 row, package, upload, submission, or remote
action was opened.

The audit constructed and coefficient-verified the complete mixed
`<6,3,3:40> tensor <2,2,2:7> = <12,6,6:280>` U, V, and W maps, then compiled
each map independently with PLinOpt under three frozen spellings and checked
every emitted SLP against the exact source matrix.

The separable live baseline and best verified direct-map counts were:

```text
map     baseline     best direct PLinOpt     saved nodes
U          590              588                   +2
V          407              416                   -9
W          848              889                  -41
```

Using the live R90 shape weights, the net projected saving is
`-237,715,020` operations: a regression of about 0.238B. The 1.5B whole-inner
promotion gate, 3.6B direct rewrite gate, and 4.5B implementation-headroom
gate all fail.

This result is stronger than the earlier recursion-local compiler checks:
it applies an independent verified optimizer directly to the complete mixed
inner factors. It closes this direct linear-circuit spelling. It is not a
rank lower bound and does not close a nonmultiplicative or serendipitous
decomposition of the full `<72,18,18>` tensor.

Controlling receipt:

`runtime/artifacts/r90_wcb_whole_inner_plinopt_c10_r1_20260809.json`

Pinned hashes:

```text
preregistration 13d0676cc2b08f035746a2481eaf656db7ebb672bf8d94d9f17c89b9e08cadad
source          6b5fe012dc9754aaa10f75f95154a871a2430ca78ba582b6475ffc656b442a50
receipt         ac5b9048941173c175cf62d6de4d97ddc7dd58c13e11e2d9725f111c0e7f02ee
```
