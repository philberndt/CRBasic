# For...Next

The For Next statements are used to repeat a group of instructions a specified number of times.

## Syntax

```
For
```

counter = startToend [Stepincrement ]

[statementblock]

[

```
ExitFor
```

]

[statementblock]

```
Next
```

[counter [, counter][, ...]]

The example runs one For..Next loop inside another.

```
Dim I, J'Declare variables.
ForJ = 5To1Step-1'Loop 5 times backwards.
ForI = 1To12'Loop 12 times.
. . . .'Run some code.
NextI
. . . .'Run some code.
NextJ
. . . .'Run some code.
```

This next example fills odd elements of X up to 40 \* Y with odd numbers.

```
ForI = 1To40 * YStep2
X( I ) = I
NextI
```

The example program below has two single loops and one nested loop set. The first For...Next loop is a basic example of filling a 1D array using a For...Next loop. The next For...Next loop is used to fill a 1D array but starts by filling the last element of the array and works backwards. The nested For...Next loop set shows how to fill a 2D array using nested For...Next loops.

```
Dim I, J, K, L
Public M1(5), M2(5), M3(3,3)

BeginProg
'This For...Next loop is used to fill a 1D array starting with the
'first element of the array. Even numbered elements of the array are set
'to -1. Odd numbered elements are set to 1.
ForI = 1To5'Starting with I = 1, increment I by 1 until I = 5.
'If I is even, then M1(I) = -1. If I is odd, then M1(I) = 1.
If I MOD 2 = 0 Then M1 (1) = -1 Else M1(I) = 1
NextI'Increment I and return to the beginning of For...Next instruction..

'This For...Next loop is used to fill a 1D array starting with the
'last element of the array and working backwards. Each element of the
'array is set equal to its index times 2.
ForJ = 5 To 1 Step -1'Starting with J = 5, decrement J by 1 until J = 1.
M2(J) = J * 2'Set each element of M2 equal to its index times 2
NextJ'Decrement J and return to the beginning of the For...Next loop.

'These nested For...Next loops are used to fill a 2D array. Each element
'of the array is set equal to the product of its indexes, K and L.
ForK = 1 to 3'Step through each row of the 2D array.
ForL = 1 to 3'Step through each column of the 2D array.
M3(K,L) = K * L'Set M3(K,L) equal to the product of its indexes
NextL'Increment L and return to the beginning of the inner For...Next loop.
NextK'Increment K and return to the beginning of the outer For...Next loop
EndProg
```

## Remarks

The For...Next statement has these parts:

# For

The For statement begins a For...Next loop control structure. For must appear before any other part of the structure.

# Counter

The Counter is a numeric variable used as the loop counter. If the variable used is an index into an array, the index cannot be a variable (for example, Variable(1) can be used, but Variable(i) cannot).

# Start

The Start parameter is the constant or variable to use for the initial value of counter.

# To

The To statement separates the Start and End values.

# End

The End parameter is the constant or variable to use for the final value of counter.

# Step

The Step argument is used to indicate that the For Next loop should increment the counter by a specific value each pass through the loop.

# Increment

The Increment parameter is the amount the counter is changed each time through the loop. Increment can be a positive or negative value. If you do not use Step Increment, the counter is incremented by 1 each pass through the loop.

# StatementBlock

The StatementBlock is the portion of the program that should be repeated until the loop is terminated. These instructions lie between the For and Next statements.

# ExitFor

The ExitFor statement is used within a For...Next control structure to provide an alternate way to exit. Any number of ExitFor statements may be placed anywhere in the For...Next loop. This statement is often used with the evaluation of some condition (for example, If...Then). ExitFor transfers control to the statement immediately following the Next.

# Next

The Next statement ends a For...Next loop. This statement cause the counter to increase by the Step Increment.

For...Next loops can be nested by placing one For...Next loop within another. Each loop should be given a unique variable name for its counter. If nesting is used, when the For Next loop is exited, program control is transferred to the For Next loop in which it was nested.
