# Format

A string used to define the format to be used for the arguments. Up to 10 format specifiers (with corresponding arguments) can be used in a format string. The format specifiers are:

| Specifier | Description                                                             |
| --------- | ----------------------------------------------------------------------- |
| d or i    | Signed decimal integer                                                  |
| u         | Unsigned decimal integer                                                |
| o         | Unsigned octal                                                          |
| b         | Binary (base 2)                                                         |
| x         | Unsigned hexadecimal integer                                            |
| X         | Unsigned hexadecimal integer (uppercase)                                |
| f         | Decimal floating point, lowercase                                       |
| e         | Scientific notation (mantissa/exponent), lowercase                      |
| E         | Scientific notation (mantissa/exponent), uppercase                      |
| g         | Use the shortest representation: %e or %f                               |
| G         | Use the shortest representation: %E or %F                               |
| c         | Single character                                                        |
| s         | String of characters                                                    |
| y (or Y)  | SDI-12 compatible output. INF and NAN are output as 9999999             |
| z (or Z)  | %m.nZ fits n significant digits into a width of m characters            |
| %         | A % followed by another % character will write a single % to the stream |

**NOTE:** If L is specified before f, e, or g, then the value passed in is of type [double](../Info/DoublePrecArith.md) and, therefore, the precision of double can be displayed. The maximum number of digits possible is 7 for floats (IEEE4, single precision) and 18 for doubles(IEEE8, double precision).

Width and precision may be specified directly in the format string or may be passed in from one of the arguments. When specifying width and/or precision as an argument their place should be indicated by an asterisks (\*). The following examples are equivalent and will return a floating point value with a field width of at least 3 characters and a precision of 2 characters.

sprintf(dest,"%3.2f",67.89)

sprintf(dest,"%\*.2f",3,67.89)

sprintf(dest,"%\*.\* f",3,2,67.89)

Precision for f specifies the number of digits after the decimal point, whereas precision for g and e specifies the total number of digits displayed, including digits before and after the decimal point.

For example:

sprintf(Str(6),"%.10Lf",112.000009R) prints 112.0000090000

sprintf(Str(8),"%.10Lg",112.000009R) prints 112.000009

Up to five Flags can be used immediately following the % sign:

| Flag  | Description                                                                                                                                                           |
| ----- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| +     | Always denote the sign of a number                                                                                                                                    |
| Space | Prefixes a non-negative signed number with a space                                                                                                                    |
| -     | Left-align the output of the placeholder (default is to right-align)                                                                                                  |
| #     | For g and G, trailing zeros are not removed. For f, e, g, or G, the output always contains a decimal point. For o, x, and X, prepend o, Ox, or OX to non-zero numbers |
| 0     | Use 0 spaces to pad a field when the width option is used                                                                                                             |

Type: Variable declared as a string
