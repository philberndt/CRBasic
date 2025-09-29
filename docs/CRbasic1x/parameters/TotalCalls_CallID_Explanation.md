# TotalCalls Call_ID Explanation

The TotalCalls and Call_ID parameters are used when [AvgRun](../Instructions/avgrun.md),[MaxRun](../Instructions/maxrun.md),[MinRun](../Instructions/minrun.md), or [StdDevRun](../Instructions/stddevrun.md) is called in different places in a program, such as when subroutines or functions are used. Each of these instructions require a block of memory to keep the running numbers. Because this memory is allocated at compile time, in order to allocate memory for multiple calls of the instruction, it is necessary to specify the number of times a given instance of the instruction is called throughout the program and to specify a unique ID number for each call of the instruction. The following example with AvgRun demonstrates how memory is allocated for these instructions.

Without the optional parameters, one block of memory is allocated at compile time.

|          |
| -------- |
| MemBlock |

If the TotalCalls parameter is set to 3, then 3 blocks of memory are allocated.

|           |           |           |
| --------- | --------- | --------- |
| MemBlock1 | MemBlock2 | MemBlock3 |

In the example code below, a program has three source variables named X, Y, and Z and a running average is needed for each of these variables. The running averages will be calculated using the AvgRun instruction in a subroutine. These 3 blocks of memory can be associated with three source variables, for example, variables X, Y, and Z,, and a subroutine with these variables can be called at different places in a program. Each call of the instructions requires a unique Call_ID in order to allocate memory correctly for that instance of the instruction.

For example, the X, Y, and Z source variables can be assigned to Call_IDs 1 to 3, respectively, using constants:

`Const`X_ID = 1:`Const`Y_ID = 2:`Const`Z_ID = 3'These are call IDs

Results of the instruction can be stored to corresponding variables in a subroutine. For example:

`Public`X_avg_sub, Y_avg_sub, Z_avg_sub

`Sub`run(ResultAvg As Float, Source As Float, SourceID)
`AvgRun`(ResultAvg,1,Source,10,False,0,**3**, SourceID)'TotalCalls = 3
`EndSub`

Here are the 3 calls to the subroutine that contains the AvgRun instruction. Each call uses a unique Call_ID to ensure the correct memory block is used for the associated source variable:

`Call run`(X_avg_sub, X, X_ID)

```
'running average for X is stored to X_avg_sub; X_ID = 1
```

'Do some work
`Call run`(Y_avg_sub, Y, Y_ID)

```
'running average for Y is stored to Y_avg_sub; Y_ID = 2
```

'Do some work
`Call run`(Z_avg_sub, Z, Z_ID)

```
'running average for Z is stored to Z_avg_sub; Z_ID = 3
```

'Do some work

Notice that the subroutine is passing the source variable for the AvgRun instruction as a parameter, and that each pass uses a unique Call_ID. This allows specific memory allocation for each of the calls. In the case above, 3 memory locations are required to store the running averages:

|                   |                   |                   |
| ----------------- | ----------------- | ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| MemBlock1 (X_ID1) | MemBlock2 (Y_ID2) | MemBlock3 (Z_ID3) | **Using the Optional Parameters with Repetitions** Below is an example of how the TotalCalls and Call_ID parameters can be used with repetitions. |

`Public`X(2), Y(2), Z(2)'source variables
`Const`X_ID = 1'unique call IDs
`Const`Y_ID = 2
`Const`Z_ID = 3

`Public`Xavg_sub(2), Y_avg_sub(2), Z_avg_sub(2)'result variables

The corresponding subroutine is:

`Sub`run(ResultAvg(2) As Float, source(2) As Float, sourceID)
`AvgRun`(ResultAvg,2,source,10,False,0,3,sourceID)
`EndSub`

Corresponding calls of the subroutine are:

`Call run`(X_avg_sub, X, X_ID)
'Do some work
`Call run`(Y_avg_sub, Y, Y_ID)
'Do some work
`Call run`(Z_avg_sub, Z, Z_ID)
'Do some work
