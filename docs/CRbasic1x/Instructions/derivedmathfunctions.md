# Derived Math Functions

The following is a list of non-intrinsic mathematical functions that can be derived from the intrinsic math functions provided with CRBasic:

| Function                    | CRBasic Equivalent                                           |
| --------------------------- | ------------------------------------------------------------ |
| Secant                      | SEC = 1 / COS(X)                                             |
| Cosecant                    | COSEC = 1 / SIN(X)                                           |
| Cotangent                   | COTAN = 1 / TAN(X)                                           |
| Inverse Sine                | ARCSIN = ATN(X / SQR(-X \* X + 1))                           |
| Inverse Cosine              | ARCCOS = ATN(-X / SQR(-X \* X + 1)) + 1.5708                 |
| Inverse Secant              | ARCSEC = ATN(X / SQR(X \* X - 1)) + SGN(SGN(X) -1) \* 1.5708 |
| Inverse Cosecant            | ARCCOSEC = ATN(X/SQR(X \* X - 1)) + (SGN(X) - 1) \* 1.5708   |
| Inverse Cotangent           | ARCCOTAN = ATN(X) + 1.5708                                   |
| Hyperbolic Sine             | HSIN = (EXP(X) - EXP(-X)) / 2                                |
| Hyperbolic Cosine           | HCOS = (EXP(X) + EXP(-X)) / 2                                |
| Hyperbolic Tangent          | HTAN = (EXP(X) - EXP(-X)) / (EXP(X) + EXP(-X))               |
| Hyperbolic Secant           | HSEC = 2 / (EXP(X) + EXP(-X))                                |
| Hyperbolic Cosecant         | HCOSEC = 2 / (EXP(X) - EXP(-X))                              |
| Hyperbolic Cotangent        | HCOTAN = (EXP(X) + EXP(-X)) / (EXP(X) - EXP(-X))             |
| Inverse Hyperbolic Sine     | HARCSIN = LOG(X + SQR(X \* X + 1))                           |
| Inverse Hyperbolic Cosine   | HARCCOS = LOG(X + SQR(X \* X - 1))                           |
| Inverse Hyperbolic Tangent  | HARCTAN = LOG((1 + X) / (1 - X)) / 2                         |
| Inverse Hyperbolic Secant   | HARCSEC = LOG((SQR(-X \* X + 1) + 1) / X)                    |
| Inverse Hyperbolic Cosecant | HARCCOSEC = LOG((SGN(X) \* SQR(X \* X + 1) +1) / X)          |
| Inverse Hyperbolic Tangent  | HARCCOTAN = LOG((X + 1) / (X - 1)) / 2                       |
| Logarithm                   | LOGN = LOG(X) / LOG(N)                                       |
