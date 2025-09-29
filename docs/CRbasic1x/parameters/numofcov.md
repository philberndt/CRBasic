# NumOfCov

The number of Covariances to be computed. The maximum number is (DimX/2)\*(DimX+1).

If The Covariance instruction is Covariance(3,X(1),FP2,False,6) then there will be 6 covariance relationships calculated and sent to the output table:

Cov(X(1),X(1)), Cov(X(1),X(2)), Cov(X(1),X(3)), Cov(X(2),X(2)), Cov(X(2),X(3)), Cov(X(3),X(3))

Type: Constant (or expression that evaluates as a constant)
