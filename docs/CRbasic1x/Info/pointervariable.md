# Pointer Variable

A pointer can point to a variable, an array of variables, a structure, an array of structures, or a user-defined function.

## Syntax

```crbasic
Dim ptr as long

Ptr = @MyVariable

OR

MyVariable = !ptr

```

### Example #1

This example shows the basic functionality of the pointers.

```crbasic

Public test_1
Public test_2
Public test_3 As String
Public test_4 As Long
Public test_5 As Long
Public test_6
Public test_7 As Long
Public test_9
Public test_10

Public floatArr As Float(5) = {100, 2, 55, 371, 244}

'Declare pointers
Dim pointer_float As Float!'The Type must be specified (Float,Long,String,Boolean)
Dim pointer_long As Long!
Dim greatestPtr As Float!

'Function that uses two pointers to swap the values of the variables they are pointing to outside
'of this function
Function SwapValues(val1 As Float!, val2 As Float!)
Dim temp'Local variable to temporarily store the value of the variable val1 is pointing to

temp = !val1'Dereference val1 and store the value at its address in temp
!val1 = !val2'Store value at the address stored by val2 into the memory pointed to by val1
!val2 = temp'Put the stored value of val1

'No return value
EndFunction

'Function that takes a pointer to an array of floats and returns a pointer to the greatest value
'in the array
'
' Params:
' data - pointer to array of floats
' dataLength - Length of the array of floats (Must be >= 1 and <= the length of the array data is
' pointing to. It is the caller's responsibility to check this)
' Returns:
' A float pointer that points to the greatest value in the array.
Function FindGreatest(data As Float!, dataLength As Long) As Float!
Dim i As Long
Dim greatest As Float!
For i = 1 To dataLength
If (i = 1) Then'First time through, the first element in the array is our baseline
'Initialize
greatest = data
ElseIf (!greatest < !data(i)) Then
'We found an element in the array that is greater than the value we thought was the greatest.
'Use pointer arithmetic to assign greatest to the address of the current element.
greatest = data + (i - 1)
EndIf
Next

Return greatest
EndFunction

'Function that returns the value of a pointer
Function GetValueOfLongPointer (local_pointer As Long!) As Long
'Dereference the pointer and return the value
Return(!local_pointer)
EndFunction

'Begin Main Program
BeginProg

'Example 1: Working with memory addresses and dereferencing
'\***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\***
' Initialize a variable with a value
test_1 = 111.11

' Get the memory address of test_1 and store it in pointer_float
pointer_float = @test_1'Expected: pointer_float = Random Address number

' Dereference the pointer to get the value stored at that memory address
test_2 = !pointer_float'Expected: test_2 = 111

' Retrieve the variable name associated with the memory address of test_1
test_3 = !(pointer_float)'Expected: test3 = "test_1"
'\***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\***

'Example 2: Modifying memory values using pointers
'\***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\***
' Initialize a variable with a value
test_4 = 2222

' Get the memory address of test_5 and store it in pointer_long
pointer_long = @test_5'Expected: pointer_long = Random Adress number

' Set the value at the memory address(test_5) to the value of test_4
!pointer_long = test_4'Expected: test_5 = 2222
'\***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\***

'Example 3: Function that returns a pointer to the greatest value in an array
'of floats. The function takes a pointer to an array of floats and a Long holding
'the length of the array as parameters.
'The greatest value in floatArr is 371 at index 4 of the array, so test_6 will
'equal 371
'\***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\***
greatestPtr = FindGreatest(@floatArr, 5)
test_6 = !greatestPtr

'Now that we've set test_6, we can change the value at index 4 in floatArr using greatestPtr
!greatestPtr = 1000
'\***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\***

'Example 4: Function that returns the value of a pointer
'\***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\***
' Set test_7 to the value of test_4 using the address of test_4 in a function
test_7 = GetValueOfLongPointer(@test_4)'Expected: test7 = 2222
'\***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\***

'Example 5: Function that swaps the values of two variables using pointers
'\***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\***
' Swap the values of test_9 and test_10 using pointers (see SwapValues Function)
test_9 = 10.1
test_10 = 1.5
SwapValues(@test_9, @test_10)
'\***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\***\*\*\*\*\***

EndProg

```

### Example #2

This example demonstrates using a pointer to reference user-created functions.

**NOTE:** A pointer to a function behaves the same as referencing the function directly.

If the parentheses for the parameters are present, the function is called.

(!ptr)()

If the parentheses for the parameters are not present, the pointer returns the value that the function returned when called last.

(!ptr)

```crbasic

'This example demonstrates using a pointer to reference user-created functions.

Function f1(f)
Return f
EndFunction

Function f2
Return 2.2
EndFunction

BeginProg
Scan (1,Sec,0,0)

Dim ptr1 As f1!, ptr2 As f2!
ptr1 = @f1
Public v1,v2
v1 = (!ptr1)(1.1)
ptr2 = @f2
v2 = (!ptr2)()

NextScan
EndProg

```

### Example #3

```crbasic

'This example demonstrates using a pointer to reference a structure.

'set up structure type
StructureType s
value As Float
flag As Boolean
EndStructureType

Const size = 10
'declare structures as type s
Public s1(size) As s, s2(size) As s, input As s

BeginProg
Scan (1,Sec,0,0)

'note: structure.structure-variable is analogous to tablename.fieldname syntax
input.value = Public.Record
input.flag = Public.Record MOD 2
Dim i As Long
'add input to the array, throwing out the oldest
For i = 1 To size-1
s1(i) = s1(i+1)
Next i
s1(size) = input
'use pointers to reference a structure
Dim ptr3 As s!, ptr4 As s!
ptr3 = @s1 : ptr4 = @s2
For i = 1 To size
!ptr4 = !ptr3
ptr4 += 1 : ptr3 += 1
Next i
NextScan
EndProg
```

## Remarks

A pointer can point to a variable, an array of variables, a structure, an array of structures, or a user-defined function. Pointers are a convenient way to reference the memory location of a variable in a program rather than referencing it by name. Pointers are useful in functions where parameters are local to the function and changes to the parameters have no effect on the original arguments.

Pointer variables must be initialized by the @ operator before they can be used. E.g.,

```crbasic
PTR as Long!
PTR = @X
Reference to an uninitialized pointer will return a variable out of bounds error.
```

In most applications, pointer variables are declared as Type Long! However, pointer variables may also be declared as type Float!, Boolean!, or String! (for example, Dim P as Float!).

@ - The "@" symbol is used to reference the memory location of a variable in a program

! The "!" symbol is used to dereference a pointer. This will return the value at the pointer rather than the memory location of the pointer.

! (VariableName) The "()" around the variable name will return the variable name associated with the memory location. This must be declared as type string.

Pointers can also be used with Functions (see Example Program).

```crbasic
!Pointer(X)
```

This will access the xth element of an array of pointers.

The ! operator can also be applied to a user function call, when the function returns a pointer. For example:

> ```crbasic
> Function ConstrainFunc(Value As Long,Low As Long,High As Long) As Long
> If !Value < !Low Then
> Return Low
> ElseIf !Value > !High Then
> Return High
> Else
> Return Value
> EndIf
> EndFunction
> 'Call within program
> FuncFltRes = !ConstrainFunc(@FltVal,@FltLow,@FltHigh)
> ```

A pointer referenced inside an expression can also be cast as a specific type (e.g., !(FLOAT!)(p+1) = !(FLOAT!)(p-3) + !(float!)(p-2) ).

Due to how memory is managed in the datalogger, there cannot be more than 4094 variable declarations before declaring a variable that will be accessed via a pointer. Additionally, the size of an array that can be accessed via a pointer is limited to 1,048,576.

Pointers can reference values in data tables by using the TableName.Fieldname syntax. This allows both writing and reading via a pointer reference to a data table.
