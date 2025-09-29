# Dim (Dimension)

The Dim statement is used to declare variables and variable arrays, and allocate storage space for these variables.

## Syntax

```
Dim
```

VarName (size, size, size)

Or

```
Dim
```

VarName (size, size, size)As _Type_

This example dimensions TCTemp as three dimensional, with a total of 2 \* 3 \* 4 = 24 elements, sets up an array for the RealTime instruction, and creates a string variable.

```
'This program illustrates how to use Public and Dim variables. Several Public and Dim variables are declared.
'Only the public variables will appear in the public table.
'The data table named AllVars shows all of the variables (both Public and Dim) because both Public and dim variables are sampled into this table.

'Public Variables
Publiccount As Long'create a Long Variable
PublicNumber = 38.3'create a Float Variable and initialize to 38.3
PublicPubString as String'create a String Variable

'Dim (private) Variables
DimDimArray3D(2, 3, 4)'create a 3-dimensional array with 24 elements
DimDimArray1D(9)'create a 1-dimensional array with 9 elements
DimDimString as String * 50'create a string variable that accommodates 50 characters

'AllVars Table displays all of the variables in the program
DataTable (AllVars,True,-1)
'Sample all variables in program
Sample(9,DimArray1D,FP2)'Sample every element in DimArray1D
Sample(1,DimString,String)
Sample(24,DimArray3D,FP2)'Sample every element in DimArray3D
Sample(1,count,Long)
Sample(1,Number,FP2)
Sample(1,PubString,String)
EndTable

BeginProg
PubString = "This is a string"'Initialize PubString
DimString = "This variable does not appear in the public table"'Initialize DimString

Scan (1,Sec,3,0)'Scan at 1 second intervals
count+=1'count equals itself plus 1 (increment count by 1 each scan)
DimArray3D(1,2,3) = count'Change element of DimArray3D at index (1,2,3) to count
DimArray1D = 24 + count'Change first element of DimArray1D to 24 + count

CallTable AllVars'call AllVars table
NextScan
EndProg
```

## Remarks

All variables must be declared before they are used in a program. Typically, Dim statements are placed at the beginning of a program. When variables are initialized, they are initialized to 0.

Valid characters for use in variable names include the letters A through Z (upper and lower case), the underscore character ( \_ ), the dollar sign ($), and numbers 0 through 9. Variable names must start with a letter, an underscore, or a dollar sign. Variable names are not case sensitive.

A Dim statement can be used for each variable declared, or multiple variables can be defined on one line with one Dim statement. If the latter is done, the variables should be separated by a comma (for example,Dim Scratch1, Scratch2, Testdeclares three variables). A variable array is created by following the variable name with the number of elements enclosed in parentheses (for example, DimTemp(3)creates Temp(1), Temp(2), and Temp(3)). Two- and three-dimensional arrays can also be defined. A declaration ofDim Temp(3,3,3)would create the variables Temp(1,1,1), Temp(1,1,2) Temp(3,3,3). In the program, the array can be referenced using the multi-dimensional form, or using an index into the array.[For more information, seeMulti-dimensional Arrays.](../Info/multidimensionalarrays.md)

Variables declared using the Dim statement cannot be viewed using a software package's numeric monitor. To make variables available for display, use the Public declaration.

Variables declared by Dim within a subroutine or function are local to that subroutine or function. The same variable name can be used within other subroutines or functions or as a global variable without conflict.

## Parameters

# VarName

This parameter is the name for the defined variable. Variables names can be up to 39 characters in length. Note, however, when outputting the variable to a data table, the suffix containing the output type (for example, \_avg) is appended to the end of the variable name. Therefore, to stay within the 39 character limit, most variables should be no more than 35 characters (which allows for the 4 additional characters that may be needed for output processing identifiers).

# Size

The size parameter is optional. It is used to set up the dimensions of a variable array. The maximum number of array dimensions allowed in a Dim statement is three. If you attempt to dimension a variable higher than three dimensional, an error occurs.

For example:

- **Dim ** Flow(8,3,5) would create a three-dimensional array called Flow that is 8 x 3 x 5.

- **Dim** TCTemp(9) would create a one-dimensional array with 9 elements called TCTemp.

The Option Base for dimensions is always 1; therefore, the lowest number in a dimension is 1 and not 0. If a variable is dimensioned to a size that is too small for its use in the program, a "Variable out of bounds" error is returned when the program is compiled by the datalogger.

The Dim instruction can be used with the optional As * Type*descriptor to define the data format for the variable (for example, DIM Flag1 As BOOLEAN). The four data types are:

Float

The defaultIEEE4

**Note:** Four-byte, floating-point data type. IEEE Standard 754. Same format as Float.

data type; a 32-bit floating-point with a 24-bit mantissa data type.Float

**Note:** Four-byte floating-point data type. Default datalogger data type for Public or Dim variables. Same format as IEEE4.

gives a range of 1.4 x 10-45to 3.4 x 1038with about seven digits of precision. If no data type is specified, Float is used. The keyword "IEEE4" can also be used to specify this data type. **Double ** Sets the variable to a double-precision, 64-bit floating-point value, with 14 digits of precision.The keyword IEEE8 can also be used to define a double precision value.

** NOTE:**To force double-precision arithmetic, declare all involved variables ** As Double**and append all involved numbers with R. If any value used in a calculation is not forced to double-precision, the result of the calculation will be single-precision. Click [here](../Info/DoublePrecArith.md) for additional information regarding double-precision arithmetic

In addition to mathematical operations (addition, division, etc.), the following functions will perform the processing in double precision and return a double precision result if at least one of the parameters passed into the function is typed as a Double. These functions are:[ABS](abs.md),[ACOS](acos.md),[ATN](atn.md),[ASIN](asin.md),[ATN2](atn2.md),[Average](average.md),[COS](cos.md),[COSH](cosh.md),[EXP](exp.md),[FIX](intfix.md),[FRAC](frac.md),[IIF](iif.md),[INT](../parameters/interval.md),[LOG](logorln.md),[LOG10](log10.md),[PWR](pwr.md),[RND](rnd.md),[SGN](sgn.md),[SIN](sin.md),[SINH](sinh.md),[SQRT](sqr.md),[TAN](tan.md),[TANH](tanh.md).

A floating point number will be cast as a single precision float unless an R is tagged at the end of the value to specify Double. Thus, 1.1 will be a single-precision value and 1.1R will be a double-precision value.[For more information, seeDouble-Precision Arithmetic.](../Info/DoublePrecArith.md) ** Long ** ** Long **: Sets the variable to a 32-bitlong

** Note:**Data type used when declaring a variable as an integer.

integer, ranging from -2,147,483,648 to +2,147,483,647 (31 bits plus the sign bit).

** NOTE:**If a variable declared as Long returns aNAN

** Note:**Not a number. A data word indicating a measurement or processing error. Voltage overrange, SDI-12 sensor error, and undefined mathematical results can produce NAN.

, it will be reflected as the most negative Long value (-2147483648).

When adding two Longs where the result would be greater than the maximum 32-bit value (+2,147,483,647), the result rolls over to a value of the opposite sign. When operating on Longs, if the result requires greater than 32 bits, then the significant bits beyond 32 are truncated to result in a number within the 32-bit range. Furthermore, if two Longs are added and stored to a Float, they will be added together as Longs and then converted to a Float when stored into the variable. If a Long and a Float are added, the Long is converted to a Float before the addition. ** Boolean ** Sets the variable to a 4-byteBoolean

** Note:**Data type used to represent conditions or hardware that have only two states (true or false) such as flags and control ports.

. Boolean variables are typically used for flags and to represent conditions or hardware that have only 2 states (for example, On/Off, Ports). A Boolean variable uses the same 32-bit long integer format as a Long but can set to only one of two values: True, which is represented as 1, and false, which is represented with 0. Any non-zero number >= 1 will evaluate as true (a float value between .999 and 0 when converted to a Long is 0).

String \* Size

Sets the variable to a string of ASCII characters, NULL terminated, with size specifying the maximum number of characters in the string (note that the null termination character counts as one of the characters in the string). The size argument is optional. The minimum string size is 4 (3 usable bytes and 1 terminating byte), and the default if size is not specified is 24 (23 usable bytes and 1 terminating byte). String size is allocated in multiples of 4 bytes. Thus, a string declared as 29 bytes will actually be 32 bytes (31 usable bytes and 1 terminating byte). A string is convenient in handling serial sensors, dial strings, text messages, etc.

The size of a string variable should be set large enough to accommodate the largest expected string it will hold during run-time. However, care should be taken in specifying strings larger than required. The datalogger will allocate memory for the variables and data tables to the full size defined in the Public or Dim statement even if the variable holds a much shorter string. In addition to using memory, large strings have the potential of slowing communication, since the full size of the string is transmitted.

As a special case, a string can be declared as String \* 1. This allows the efficient storage of a single character. The string will take up 4 bytes in memory and when stored in a data table, but it will hold only one character.

Strings can be dimensioned only up to 2 dimensions instead of the 3 allowed for other data types. (This is because the least significant dimension is actually used as the size of the string.) To begin reading or modifying a string at a particular location into the string, enter the location or begin reading a string at a particular character, enter the character as a third dimension; for example, String(x,y,n) where n is the desired character. For example, given an array of strings Str(10,10), Str(2,2,n) refers to n character in the (2,2) element of the array. Use Str(1,1,n) for a scalar variable and Str(x,1,n) for a one dimensional array element.

Initializing variables

Variables can be initialized when declared. For example:

```
Public MyVar = 3.5 or Public MyVar = {3.5}
```

Public MyArray(3) = {3, 6, 9}

```
or

```

Dim MyVar = 3.5 or Public MyVar = {3.5}

```
Dim MyArray(3) = {3, 6, 9}
```

**NOTE:** Beginning with OS8.00, arrays can be initialized to all the same value with syntax such as Public Array(50) = 1, or Public Array(50) as Boolean = True. This will set all 50 elements of the array to 1 or True, respectively. In prior operating systems, only the first element of the array would be initialized with this syntax.

**NOTE:** Variables initialized within a subroutine or function are initialized only at compile time. They are not re-initialized with each call to the subroutine or function.

The braces are optional if a scalar is being initialized or if only the first variable in an array is being initialized.

When declaring a data type for the variable, the variable is declared before initialization:

```
Public StringVar as String * 30 = "Test String"
```

or

```
Dim StringVar as String * 30 = "Test String"
```

For all arrays, including multi-dimensional arrays, the least significant elements are initialized first.

```
Public Array (2,3) = {1,2,3,4}
```

or

```
Dim Array (2,3) = {1,2,3,4}
```

Thus,

```
Array(1,1) = 1
```

Array(1,2) = 2

```
Array(1,3) = 3
```

Array(2,1) = 4

```
Array(2,2) = 0
```

Array(2,3) = 0

```
If the array is not fully initialized, the first elements will be initialized first, and the remainder will be uninitialized.

For large arrays, initialization of variables can be placed on multiple lines as long as the break is after a , (comma).
```
