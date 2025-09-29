# SDMSW8A (SDM-SW8A Switch Closure Module)

The SDMSW8A instruction is used to control the SDM-SW8A Eight-Channel Switch Closure module, and store the results of its measurements to a variable array.

## Syntax

SDMSW8A(Dest,Reps,SDMAddress,FunctOp,SW8AStartChan,Mult, Offset)

The following program measures all eight channels of an SW8A and outputs a sample of the pulse count to a table once every minute.

```
'Program Declarations
Public SW8ACount(8)

'Data Table Declarations
DataTable (CountTab,1,1000)
DataInterval (0,1,Min,10)
Sample (8,SW8ACount(),FP2)
EndTable

'Main Program
BeginProg
Scan (1,Sec,3,0)
SDMSw8a(SW8ACount(),8,0,2,1,1.0,0)
CallTable CountTab
NextScan
EndProg
```

## Parameters

# Dest (Destination)

The Variable in which to store the results of the instruction. Right-click the parameter to display a list of defined variables.

If this instruction has a Repetitions parameter and it is greater than 1, the results are stored in an array with the variable name. The array must be dimensioned large enough to hold all of the values returned from all of the Reps.

Type: Variable or Array

# Reps

Determines the number of channels that will be read on the SW8A. If Reps is greater than 8, measurement will continue on the next sequential SW8A. In this instance, the addresses of the SDM devices must be consecutive.

Type: Constant integer (or expression that evaluates as a constant).

# SDMAddress (Address of Device)

Defines the address of the device with which to communicate. ValidSDM

**Note:** Synchronous Device for Measurement. A processor-based peripheral device or sensor that communicates with the datalogger via hardwire over a short distance using a protocol proprietary to Campbell Scientific.

addresses are 0 through 14. Address 15 is reserved for the SDMTrigger instruction.

Some SDM instructions support repetitions. If a Reps parameter is present and it is greater than 1, the data logger will increment the SDM address used in the instruction for each subsequent device with which it communicates.

# FunctOp

Determines the result returned by the SW8A. A numeric value is entered. Right-click the parameter to display a drop-down list box.

| Code | Function                                                                                                                                            |
| ---- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0    | Returns the state of the signal at the time the instruction is executed. A 0 is stored for low and a 1 is stored for high.                          |
| 1    | Returns the duty cycle of the signal. The result is the percentage of time the signal is high during the scan interval.                             |
| 2    | Returns a count of the number of positive transitions of the signal.                                                                                |
| 3    | Returns a value indicating the condition of the module: - positive integer = ROM and RAM are good - negative value = RAM is bad - zero = ROM is bad |

Type: Constant

# SW8AStartChan

The first channel that should be read on the SW8A. If the Reps parameter is greater than 1, measurements will be made on sequential channels.

Type: Constant

# Mult, Offset (Multiplier and Offset)

Factors by which to scale the raw results of the measurement. Typically used to convert the raw measurement to engineering units or to units other than which is output. For example, the TCDiff instruction measures a thermocouple and outputs temperature in degrees C. A multiplier of 1.8 and an offset of 32 will convert the temperature to degrees F.

For temperature measurements, a multiplier (mult) of 1 and an offset of 0, would output in degrees Celsius. For analog measurements, a multiplier (mult) of 1 and an offset of 0, would output the measured voltage in millivolts divided by the excitation voltage in volts.

If Repetitions of greater than 1 are used for this instruction, Repetitions can also be used for the Multiplier and Offset.[SeeMultipliers, Offsets, and Disable Variables with Repetitionsfor more information.](../Info/multipliersoffsets.md)

Type: Constant, Variable, Array, or Expression
