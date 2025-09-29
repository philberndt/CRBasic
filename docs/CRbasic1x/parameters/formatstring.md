# FormatString

Determines how the floating point value will be represented in the converted string The options are (m = mantissa; d = decimal; x = exponent):

| Code                 | Description                                                                                                                                                                                                                                    |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| %f                   | Decimal notation in the form of mmm.dddddd; precision is 6 places to the right of the decimal.                                                                                                                                                 |
| %e (or %E)           | Decimal notation in the form of m.dddddd e xx; precision is 6 places to the right of the decimal.                                                                                                                                              |
| %g (or %G)           | Mantissa and decimal are variable; trailing 0s and decimals are omitted if the input has a precision less than specified by the format string.                                                                                                 |
| %_Y.\*\*Z _ f        | Decimal notation in the form of m.d; field width is defined by*Y * and includes the sign and decimal place. Precision is defined by*Z *.                                                                                                       |
| %0* Y.*0 * Z*f       | Decimal notation in the form of m.d; field width is defined by * Y*and includes the sign and decimal place. The mantissa will be padded by leading 0s if necessary. Precision is defined by _ Z_. The decimal will be padded with trailing 0s. |
| %* Y *e (or %* Y *E) | Decimal notation in the form of m.d e xx; field width is defined by* Y *and includes the sign and decimal place.                                                                                                                               |
| %* Y *g (or %* Y *G) | Mantissa and decimal are variable; field width is defined by* Y *and includes the sign and decimal place.                                                                                                                                      |

Right-click the parameter to display a list of options.**The format string must be enclosed in quotes.**

** NOTE:**Other ASCII characters can be included in the FormatString (for example,`FormatFloat(Variable,"The current reading is %2.3G"`).
