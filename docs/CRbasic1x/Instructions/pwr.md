# PWR (Power - Exponentiation on a Variable)

The PWR function performs an exponentiation (raise to the power) on a variable and returns a value of type Float.

## Syntax

PWR(X, Y)

The following example program uses the PWR function to apply an exponent to a variable with a value of 10. The result is stored in the variable Expon. A For/To loop is used to set Y to 1, 2, 3..25 to demonstrate the effects of the exponent on the base. Collect the Table from the datalogger and view it in a text editor to see the results.

```
Public X, Y, Expon

DataTable (Table,True,-1)
Sample (1,X,IEEE4)
Sample (1,Y,IEEE4)
Sample (1,Expon,IEEE4)
EndTable

BeginProg
X=10
Scan (1,Sec,3,1)
For Y = 1 to 25
Expon=PWR(X,Y)
CallTable (Table)
Next Y
NextScan
EndProg
```

## Remarks

The PWR function applies the exponent Y to the base X (). This function can be used in an expression or as the source for a variable (for example, Result = PWR(X,Y).
