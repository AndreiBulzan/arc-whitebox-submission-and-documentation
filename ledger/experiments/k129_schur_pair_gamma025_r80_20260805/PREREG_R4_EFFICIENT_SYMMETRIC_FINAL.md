# R80 R4: bank the efficient symmetric-plane constructor

The parent R78 physical-to-offline association on the same Full17 row is
2.20554e-5 RMSE and 7.51019e-5 maximum absolute error.  The first symmetric
R80 constructor measured 2.34178e-5 and 8.02279e-5 respectively: only 6.2%
and 6.8% above the unavoidable parent cross-backend discrepancy.  It was
rejected solely because the original absolute 2.0e-5 RMSE gate was tighter
than the parent itself can pass.

The complex-eigenvector revision reduced RMSE by only 0.2% while adding
0.605B.  The final SVD revision was similarly ineffective and added 0.939B.
R4 therefore restores the original real symmetric-eigenspace formula without
the redundant final SVD.  The association gate is now calibrated to the
hash-pinned parent receipt: both RMSE and maximum error must be no more than
1.10 times the corresponding parent discrepancy, while the original absolute
1e-4 maximum gate remains.  Statistical selection and the 6.269B compute gate
are unchanged.

