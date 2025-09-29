# Optional

The Optional keyword is used to define a list of optional parameters that can be passed into a subroutine or function.

## Syntax

Function (FunctionName) Param1, Param2,OptionalParam3, Param4

## Remarks

Subroutines and functions include the ability to pass in optional arguments. These optional parameters are preceded by the Optional keyword. Optional parameters must be initialized using a constant value (required parameters cannot be initialized). The default for an optional parameter is used by passing in a comma; multiple default parameters are specified using multiple commas. Required parameters cannot follow optional parameters in the definition of the function.
