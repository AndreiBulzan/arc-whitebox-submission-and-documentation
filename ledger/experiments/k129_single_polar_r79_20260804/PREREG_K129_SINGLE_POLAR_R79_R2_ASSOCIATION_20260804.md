# K129 single-polar R79 association gate R2

R1 remains a recorded failure: its absolute ordinary-CUDA association gate
required final-row RMSE at most `2e-5`, while R79 measured `2.2582e-5`.

The target-free diagnostic performed after that failure isolated the source:
the R79 frame itself agreed with its NumPy construction to `3.77e-10` RMSE,
while unchanged parent R78/q0 already differed from the ordinary-CUDA capture
by `2.2055e-5`.  Therefore R2 measures incremental implementation error rather
than requiring the child to erase the parent's existing backend association
gap.

Before rerunning R79, freeze these gates:

- a separately receipted parent R78 Full17 q0 RMSE no greater than `2.3e-5`;
- R79 final-row RMSE no greater than that receipted parent RMSE plus `1e-6`;
- separately receipted R79 frame RMSE no greater than `1e-7` and max error no
  greater than `1e-6`;
- repeated R79 outputs bit-identical;
- steady count no greater than `128B` and frame delta no greater than `3.661B`;
- both local whole-row walls below `55s`.

The estimator source and Mini100 mechanism are unchanged.  This R2 gate does
not waive or overwrite the R1 failure.
