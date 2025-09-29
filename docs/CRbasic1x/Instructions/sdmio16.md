# SDMIO16 (SDM-IO16 Control Port Expansion Device)

The SDMIO16 instruction is used to set up and measure an SDM-IO16 control port expansion device.

## Syntax

SDMIO16(Dest,IO16Status,SDMAddress,IO16Cmd,Mode16_13,Mode12_9,Mode8_5,Mode 4_1,Multiplier, Offset)

The following program sets up and measures an SDM-IO16.

```
'Declare Variables
Public Version(4)
Public ComsStat(3)
Public Freq(16)
Public Setup(1)

Alias version(1)=OS_Ver
Alias version(2)=OS_Sig
Alias version(3)=WatchDog
Alias version(4)=ComErr

BeginProg

'Set up the SDM-IO16.
Scan(1,sec,0,2)
'Set IO ports 1-16 to input switch closure.
SDMIO16(Setup,ComsStat(1),0,90,3333,3333,3333,3333,1,0)
NextScan

'Measure.
Scan(1,sec,0,0)
'get signature from io16
SDMIO16(Version,ComsStat(2),0,99,0,0,0,0,1,0)
'read frequency of all 16 channels
SDMIO16(Freq(),ComsStat(3),0,46,0,0,0,0,1,0)
NextScan
EndProg
```

## Remarks

The ports on the SDM-IO16 can be configured for either input or output. When configured as input, the SDM-IO16 can measure the logical state of each port, count pulses, and measure the frequency of and determine the duty cycle of applied signals. The module can also be programmed to generate an interrupt signal to the datalogger when one or more input signals change state. When configured as an output, each port can be set to 0 or 5 V by the datalogger. In addition to being able to drive normal logic level inputs, when an output is set high a boost circuit allows it to source a current of up to 100 mA, allowing direct control of low voltage valves, relays, etc.

## Parameters

# Dest

The Dest parameter is a variable or variable array in which to store the results of the measurement (Command codes 1 - 69, 91, 92, 99) or the Source value for the Command Codes (70 - 85, 93 - 98). The variable array for this parameter must be dimensioned to accommodate the number of values returned (or sent) by the instruction.

# IO16Status

Holds the result of the command issued by the instruction. If the command is successful a 0 is returned; otherwise, the value is incremented by 1 with each failure.

Type: Variable

# SDMAddress (Address of Device)

Defines the address of the device with which to communicate. ValidSDM

**Note:** Synchronous Device for Measurement. A processor-based peripheral device or sensor that communicates with the datalogger via hardwire over a short distance using a protocol proprietary to Campbell Scientific.

addresses are 0 through 14. Address 15 is reserved for the SDMTrigger instruction.

Some SDM instructions support repetitions. If a Reps parameter is present and it is greater than 1, the data logger will increment the SDM address used in the instruction for each subsequent device with which it communicates.

# IO16Cmd

A code used to set up the SDM-IO16:

| Code | Description                                                                                                                                                                                         |
| ---- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1    | Read port 1's accumulated counts into Dest                                                                                                                                                          |
| 2    | Read port 2's accumulated counts into Dest                                                                                                                                                          |
| 3    | Read port 3's accumulated counts into Dest                                                                                                                                                          |
| 4    | Read port 4's accumulated counts into Dest                                                                                                                                                          |
| 5    | Read port 5's accumulated counts into Dest                                                                                                                                                          |
| 6    | Read port 6's accumulated counts into Dest                                                                                                                                                          |
| 7    | Read port 7's accumulated counts into Dest                                                                                                                                                          |
| 8    | Read port 8's accumulated counts into Dest                                                                                                                                                          |
| 9    | Read port 9's accumulated counts into Dest                                                                                                                                                          |
| 10   | Read port 10's accumulated counts into Dest                                                                                                                                                         |
| 11   | Read port 11's accumulated counts into Dest                                                                                                                                                         |
| 12   | Read port 12's accumulated counts into Dest                                                                                                                                                         |
| 13   | Read port 13's accumulated counts into Dest                                                                                                                                                         |
| 14   | Read port 14's accumulated counts into Dest                                                                                                                                                         |
| 15   | Read port 15's accumulated counts into Dest                                                                                                                                                         |
| 16   | Read port 16's accumulated counts into Dest                                                                                                                                                         |
| 17   | Read ports 1-4's accumulated counts into Dest (Dest must be dimensioned to 4)                                                                                                                       |
| 18   | Read ports 5-8's accumulated counts into Dest (Dest must be dimensioned to 4)                                                                                                                       |
| 19   | Read ports 9-12's accumulated counts into Dest (Dest must be dimensioned to 4)                                                                                                                      |
| 20   | Read ports 13-16's accumulated counts into Dest (Dest must be dimensioned to 4)                                                                                                                     |
| 21   | Read ports 1-8's accumulated counts into Dest (Dest must be dimensioned to 8)                                                                                                                       |
| 22   | Read ports 9-16's accumulated counts into Dest (Dest must be dimensioned to 8)                                                                                                                      |
| 23   | Read ports 1-16's accumulated counts into Dest (Dest must be dimensioned to 16)                                                                                                                     |
| 24   | Read port 1's frequency into Dest                                                                                                                                                                   |
| 25   | Read port 2's frequency into Dest                                                                                                                                                                   |
| 26   | Read port 3's frequency into Dest                                                                                                                                                                   |
| 27   | Read port 4's frequency into Dest                                                                                                                                                                   |
| 28   | Read port 5's frequency into Dest                                                                                                                                                                   |
| 29   | Read port 6's frequency into Dest                                                                                                                                                                   |
| 30   | Read port 7's frequency into Dest                                                                                                                                                                   |
| 31   | Read port 8's frequency into Dest                                                                                                                                                                   |
| 32   | Read port 9's frequency into Dest                                                                                                                                                                   |
| 33   | Read port 10's frequency into Dest                                                                                                                                                                  |
| 34   | Read port 11's frequency into Dest                                                                                                                                                                  |
| 35   | Read port 12's frequency into Dest                                                                                                                                                                  |
| 36   | Read port 13's frequency into Dest                                                                                                                                                                  |
| 37   | Read port 14's frequency into Dest                                                                                                                                                                  |
| 38   | Read port 15's frequency into Dest                                                                                                                                                                  |
| 39   | Read port 16's frequency into Dest                                                                                                                                                                  |
| 40   | Read ports 1-4's frequency into Dest (Dest must be dimensioned to 4)                                                                                                                                |
| 41   | Read ports 5-8's frequency into Dest (Dest must be dimensioned to 4)                                                                                                                                |
| 42   | Read ports 9-12's frequency into Dest (Dest must be dimensioned to 4)                                                                                                                               |
| 43   | Read ports 13-16's frequency into Dest (Dest must be dimensioned to 4)                                                                                                                              |
| 44   | Read ports 1-8's frequency into Dest (Dest must be dimensioned to 8)                                                                                                                                |
| 45   | Read ports 9-16's frequency into Dest (Dest must be dimensioned to 8)                                                                                                                               |
| 46   | Read ports 1-16's frequency into Dest (Dest must be dimensioned to 16)                                                                                                                              |
| 47   | Read port 1's duty cycle into Dest                                                                                                                                                                  |
| 48   | Read port 2's duty cycle into Dest                                                                                                                                                                  |
| 49   | Read port 3's duty cycle into Dest                                                                                                                                                                  |
| 50   | Read port 4's duty cycle into Dest                                                                                                                                                                  |
| 51   | Read port 5's duty cycle into Dest                                                                                                                                                                  |
| 52   | Read port 6's duty cycle into Dest                                                                                                                                                                  |
| 53   | Read port 7's duty cycle into Dest                                                                                                                                                                  |
| 54   | Read port 8's duty cycle into Dest                                                                                                                                                                  |
| 55   | Read port 9's duty cycle into Dest                                                                                                                                                                  |
| 56   | Read port 10's duty cycle into Dest                                                                                                                                                                 |
| 57   | Read port 11's duty cycle into Dest                                                                                                                                                                 |
| 58   | Read port 12's duty cycle into Dest                                                                                                                                                                 |
| 59   | Read port 13's duty cycle into Dest                                                                                                                                                                 |
| 60   | Read port 14's duty cycle into Dest                                                                                                                                                                 |
| 61   | Read port 15's duty cycle into Dest                                                                                                                                                                 |
| 62   | Read port 16's duty cycle into Dest                                                                                                                                                                 |
| 63   | Read ports 1-4's duty cycle into Dest (Dest must be dimensioned to 4)                                                                                                                               |
| 64   | Read ports 5-8's duty cycle into Dest (Dest must be dimensioned to 4)                                                                                                                               |
| 65   | Read ports 9-12's duty cycle into Dest (Dest must be dimensioned to 4)                                                                                                                              |
| 66   | Read ports 13-16's duty cycle into Dest (Dest must be dimensioned to 4)                                                                                                                             |
| 67   | Read ports 1-8's duty cycle into Dest (Dest must be dimensioned to 8)                                                                                                                               |
| 68   | Read ports 9-16's duty cycle into Dest (Dest must be dimensioned to 8)                                                                                                                              |
| 69   | Read ports 1-16's duty cycle into Dest (Dest must be dimensioned to 16)                                                                                                                             |
| 70   | Set port 1's debounce time from Dest                                                                                                                                                                |
| 71   | Set port 2's debounce time from Dest                                                                                                                                                                |
| 72   | Set port 3's debounce time from Dest                                                                                                                                                                |
| 73   | Set port 4's debounce time from Dest                                                                                                                                                                |
| 74   | Set port 5's debounce time from Dest                                                                                                                                                                |
| 75   | Set port 6's debounce time from Dest                                                                                                                                                                |
| 76   | Set port 7's debounce time from Dest                                                                                                                                                                |
| 77   | Set port 8's debounce time from Dest                                                                                                                                                                |
| 78   | Set port 9's debounce time from Dest                                                                                                                                                                |
| 79   | Set port 10's debounce time from Dest                                                                                                                                                               |
| 80   | Set port 11's debounce time from Dest                                                                                                                                                               |
| 81   | Set port 12's debounce time from Dest                                                                                                                                                               |
| 82   | Set port 13's debounce time from Dest                                                                                                                                                               |
| 83   | Set port 14's debounce time from Dest                                                                                                                                                               |
| 84   | Set port 15's debounce time from Dest                                                                                                                                                               |
| 85   | Set port 16's debounce time from Dest                                                                                                                                                               |
| 86   | Set port 16-13 from Mode parameter                                                                                                                                                                  |
| 87   | Set port 12-9 from Mode parameter                                                                                                                                                                   |
| 88   | Set port 8-5 from Mode parameter                                                                                                                                                                    |
| 89   | Set port 4-1 from Mode parameter                                                                                                                                                                    |
| 90   | Set port 16-1 from Mode parameters                                                                                                                                                                  |
| 91   | Read state of ports 1-16 into one variable (Dest). The result is a 16-bit decimal representation from 0-65535.                                                                                      |
| 92   | Read state of ports 1-16 into 16 separate variables (Dest must be dimensioned to 16). Dest(1) holds the state of port 1, Dest(2) port 2, etc. State is represented by 0 or 1.                       |
| 93   | Set state of ports 1-16 from a single variable (Dest). Dest should be a 16-bit decimal representation from 0-65535.                                                                                 |
| 94   | Set state of ports 1-16 from 16 separate variables (Dest must be dimensioned to 16). Dest(1) sets the state of port 1, Dest(2) port 2, etc. State is represented by 0 or 1.                         |
| 95   | Set direction of ports 1-16 from a single variable (Dest). Dest should be a 16-bit decimal representation from 0-65535.                                                                             |
| 96   | Set direction of ports 1-16 from 16 separate variables (Dest must be dimensioned to 16). Dest(1) sets the direction of port 1, Dest(2) port 2, etc. Direction is represented by 0 or 1.             |
| 97   | Set interrupt mask of ports 1-16 from a single variable (Dest). Dest should be a 16-bit decimal representation from 0-65535.                                                                        |
| 98   | Set interrupt mask of ports 1-16 from 16 separate variables (Dest should be dimensioned to 16). Dest(1) sets port 1, Dest(2) port 2, etc. The mask is represented by 0 or 1.                        |
| 99   | Read the OS signature, OS version and counters for watchdog resets and communication errors into 4 separate variables (Dest must be dimensioned to 4). Using this command also resets the counters. |

Type: Constant

# Mode

Used to configure a bank of four ports when a Command code 86 through 90 is used (if any other Command Code is used, enter 0 for the Mode parameters). Mode is entered as a four digit parameter, where each parameter indicates the setting for a port. Ports are represented from the highest port number to the lowest, from left to right (e.g., 16 15 14 13; 12 11 10 9; 8 7 6 5; 4 3 2 1). There is a Mode for Ports 16 - 13, 12 - 9, 8 - 5, and 4 - 1. The valid codes are:

| Code | Description                                                       |
| ---- | ----------------------------------------------------------------- |
| 0    | Output logic low                                                  |
| 1    | Output logic high                                                 |
| 2    | Input digital, no debounce filter                                 |
| 3    | Input switch closure 3.17 msec debounce filter                    |
| 4    | Input digital interrupt enabled, no debounce filter               |
| 5    | Input switch closure interrupt enabled 3.17 msec, debounce filter |
| 6    | Undefined                                                         |
| 7    | Undefined                                                         |
| 8    | Undefined                                                         |
| 9    | No change                                                         |

Type: Constant

# Mult, Offset (Multiplier and Offset)

Factors by which to scale the raw results of the measurement. Typically used to convert the raw measurement to engineering units or to units other than which is output. For example, the TCDiff instruction measures a thermocouple and outputs temperature in degrees C. A multiplier of 1.8 and an offset of 32 will convert the temperature to degrees F.

For temperature measurements, a multiplier (mult) of 1 and an offset of 0, would output in degrees Celsius. For analog measurements, a multiplier (mult) of 1 and an offset of 0, would output the measured voltage in millivolts divided by the excitation voltage in volts.

If Repetitions of greater than 1 are used for this instruction, Repetitions can also be used for the Multiplier and Offset.[SeeMultipliers, Offsets, and Disable Variables with Repetitionsfor more information.](../Info/multipliersoffsets.md)

Type: Constant, Variable, Array, or Expression
