# Double-Precision Arithmetic

Double (or IEEE8) data type is not supported in the CR1000, CR800, and the CR3000 dataloggers, although if it is used, the program will still compile. In this instance, the value is cast as an IEEE4 4-byte floating point value.

All declared variables are single-precision IEEE four-byte floating point unless forced otherwise. In order to declare a variable as double-precision, declare the variable type **As Double**. For example:

```
PublicNDAs Double'Declares ND as double-precision IEEE eight-byte floating point
```

To force double-precision arithmetic, declare all involved variables **As Double** and append all involved numbers with **R**. For example, 1.1 will be a single-precision value and 1.1R will be a double-precision value.

When a number is assigned to a variable declared ** As Double**, the number carries through to the variable with single-precision unless the number assigned to the variable is appended with ** R**. Consider the following examples:

CRBasic EXAMPLE 1: Double-Precision Math: (Double = Double + Double) Results in Double-Precison

```
PublicDoubleRAs Double'Double-precision declaration
BeginProg
Scan(1,Sec,3,1)
'The DoubleR variable is declared as Type Double and all values in the
'equation are appended with R, so double-precision is carried through to
'the calculation, resulting in a 64-bit floating-point number with 15 digits of precision.
DoubleR = 2.15R + 3.14159265358979R'result is 5.141592654 (double-precision result)
NextScan
EndProg
```

CRBasic EXAMPLE 2: Double-Precision Math: (Single = Double + Double) Results in Single-Precision

```
PublicSingleR'Single-precision declaration
BeginProg
Scan(1,Sec,3,1)
'The SingleR variable was not declared as Type Double, so even though R is appended
'to all values in the calculation, the result is a 32-bit floating point number.
SingleR = 2.15R + 3.14159265358979R'result is 5.141593 (single-precision result)
NextScan
EndProg
```

CRBasic EXAMPLE 3: Double-Precision Math: (Double = Single + Double) Results in Single-Precision

```
PublicOneRAs Double'Test for double-precision declaration with a single-precision number
BeginProg
Scan(1,Sec,3,1)
'Example 3: The OneR variable is declared as Type Double. However, not all numeric values
'in the equation are appended with R. So, even though the OneR variable holds a 64-bit
'floating-point value, the value was not calculated with double-precision. Thus, the last
'three digits in the result do not carry double-precision.
OneR = 2.15 + 3.14159265358979R'result is 5.141592749 (single-precision result; the last three digits are not precise)
NextScan
EndProg
```

Some operations are performed as double-precision by default. These are A [Average()](../Instructions/average.md),[AvgRun()](../Instructions/avgrun.md),[AvgSpa()](../Instructions/avgspa.md),[CovSpa()](../Instructions/covspa.md),[RMSSpa()](../Instructions/rmsspa.md),[StdDev()](../Instructions/stddev.md),[StdDevSpa()](../Instructions/stddevspa.md),[Totalize()](../Instructions/totalize.md), and [TotalRun](../Instructions/totalrun.md)(). Double-precision has a 32-bit extension of the mantissa, resulting in 56 bits of precision for these operations.
