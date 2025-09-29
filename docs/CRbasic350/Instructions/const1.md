# Const (Assign a Name to a Value)

The Const declaration is used to assign a name that can be used in place of a value in the datalogger program.

## Syntax

```
Const
```

ConstantName = Expression

The following example uses Const to define a constant for the variable PI.

```
'This program calculates the circumference and area of a circle
'if the radius of the circle equals 5, 10, or 15. This program
'illustrates the difference between variables and constants.
'While the variables Radius, Circum, and Area can change within
'the program, the constant PI cannot change. An attempt to redefine
'PI after its initial declaration would result in an error.

CONSTPI = 3.141592654'Define constant.
Public Area, Circum, Radius'Declare variables.

BeginProg
Scan(5,Sec,3,0)'Radius, Circum, and Area will be recalculated every 5 seconds
Radius+=5'Increment radius by 5
If Radius > 15 Then Radius = 5'Reset radius to 5 after 15
Circum = 2 * PI * Radius'Calculate circumference.
Area = PI * ( Radius ^ 2 )'Calculate area.
NextScan'Change Radius and calculate circumference and area again
EndProg
```

## Remarks

Once a value is assigned to a constant using the Const declaration, each time the value is needed in the program, the programmer can type in the constant name instead of the value itself. The use of the Const declaration can make the program easier to follow, easier to modify, and more secure against unintended changes. Unlike variables, constants cannot be changed while the program is running.

Constants must be defined before they are used in the program. Constants can be defined in a [ConstTable/EndConstTable](consttableendconsttable.md) construct allowing them to be changed usingsoftwareorthe C command in terminal mode.

Constants can also be typed (Const A as Long = 9999, Const B as String = MyString ). Valid data types for constants are:Long

**Note:** Data type used when declaring a variable as an integer.

,Float

**Note:** Four-byte floating-point data type. Default datalogger data type for Public or Dim variables. Same format as IEEE4.

, Double, andString

**Note:** A data type used when declaring a variable consisting of alphanumeric characters.

. Other data types return a compile error. If constants are not typed, the data type will be set according to the value of the constant:

Const MyLong = 0'data type = Long

Const MyFloat = 0.0'data type = Float

Const MyString = "text"'data type = String

The Const statement includes:

## Parameters

# ConstantName

Name of the constant.

# Expression

Expressionassigned to the constant. The expression can consist of literals (such as 1.0), a string, other constants, or any of the arithmetic or logical operators.

**NOTE:** Only one constant can be defined with each Const declaration. Unlike other similar languages, CRBasic does not allow multiple constants to be defined with one declaration.
