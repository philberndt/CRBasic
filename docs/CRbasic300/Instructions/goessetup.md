# GOESSetup (Set Up GOES Transmitter)

The GOESSetup instruction is used to program the GOES satellite transmitter (that is, TX321, TX320, or TX312) for communication with the satellite._This instruction is not compatible with the TX325_.

## Syntax

GOESSetup(ResultCode,PlatformID,MsgWindow,STChannel,STBaud,RChannel,RBaud,STInterval,STOffset,RInterval)

The following program is an example of setting up the GOES transmitter using CRBasic.

```
'NESDIS Assignments
'ID = H0104C186
'ST MsgWindow = 10 sec
'ST Channel = 151
'ST BPS = 100
'Random Channel = 195
'Random BPS = 300
'ST Repeat Interval = one hour
'ST Offset = 10 minutes, 5 sec
'Random Repeat Interval = one hour

Public GOESResult

BeginProg
Scan (1,Sec,3,0)
GOESSetup(GOESResult,&H0104C186,10,151,300,195,300,"0_1_0_0" ,"0_10_5" ,"1_0_0" )
NextScan
EndProg
```

## Remarks

Since the purpose of this instruction is to set up the transmitter for communication, it only has to be run once within the datalogger program. Information for all parameters in this instruction is supplied by NESDIS.

## Parameters

# ResultCode (Result Code)

The Variable in which to store the results indicating the success of the program instruction. For the GOESData and GOESSetup instructions, this array is dimensioned to 1. For the GOESStatus instruction, the size of the array required will depend upon the option chosen for the StatusCommand. Right-click the parameter to display a list of defined variables.

| Code | Description                                                                     |
| ---- | ------------------------------------------------------------------------------- |
| 0    | Command executed successfully.                                                  |
| 2    | Timed out waiting for STX character from transmitter after SDC addressing.      |
| 3    | Wrong character received after SDC addressing                                   |
| 4    | Something other than ACK returned when select data buffer command was executed. |
| 5    | Timed out waiting for ACK.                                                      |
| 6    | Port not available; GOES not attached.                                          |
| 7    | ACK not returned following data append or overwrite command.                    |
| -11  | Buffer control error.                                                           |
| -12  | Message window error.                                                           |
| -13  | Channel error.                                                                  |
| -14  | Baud error.                                                                     |
| -15  | RCount error.                                                                   |
| -16  | Illegal data format.                                                            |
| -17  | Data format 0 or 1 was chosen, but table values were not FP2 or ASCII.          |
| -18  | STInterval error.                                                               |
| -19  | STOffset error.                                                                 |
| -20  | RInterval error.                                                                |
| -21  | Platform ID error.                                                              |
| -22  | Transmitter not set up.                                                         |

Type: Variable or Array

# PlatformID (NESDIS ID Number)

The 8-digit hexadecimal identification number assigned by NESDIS. Hexadecimal numbers are entered into CRBasic by preceding them with &H.

Type: Hexadecimal value preceded by &H

# MsgWindow (Transmission Time)

Used to indicate the assigned window of transmission for self-timed transmissions (i.e., how long the transmitter should stay online transmitting data). This value is in seconds; valid entries are 1 to 120 seconds.

Type: Integer, constant, or variable

# STChan (NESDIS Self-Timed Transmission Channel)

The channel for the self-timed transmission assigned by NESDIS. Valid channel numbers are between 0 and 266 for 100 and 300 baud (STBaud parameter) and 0 and 133 for 1200 baud. If 0 is entered for this parameter, self-timed transmissions are disabled.

Type: Integer or variable

# STBaud (Baud Rate for Self-Timed Transmissions)

Thebaud rate

**Note:** The rate at which data is transmitted.

at which self-timed transmissions should take place (100, 300, or 1200 baud). The baud rate must match your NESDIS channel assignment. Right-click this parameter to display a drop-down list of valid options.

Type: Integer or variable

# RChan (NESDIS Random Channel)

The channel for the random transmission assigned by NESDIS. Valid channel numbers are between 0 and 266 for 100 and 300 baud (RBaud parameter) and 0 and 133 for 1200 baud. If 0 is entered for this parameter, random transmissions are disabled.

Type: Integer or variable

# RBaud (Random Transmissions Baud Rate)

Thebaud rate

**Note:** The rate at which data is transmitted.

at which random transmissions should take place (100, 300, or 1200 baud). The baud rate must match your NESDIS channel assignment. Right-click this parameter to display a drop-down list of valid options.

Type: Integer or variable

# STInterval (Time between Self-Timed Transmissions)

Used to define the time between self-timed transmissions. The value is a constant string entered in the format of "Days_Hours_Minutes_Seconds". Typically the assigned interval is in hours, so the days, minutes and seconds parameters are left at 0 (example, "0_1_0_0" sets up an hourly interval). Maximum interval is 14 days; minimum interval is 5 minutes.

# STOffset (Self-Timed Transmission Offset)

The time after midnight for the first self-timed transmission. The value is a constant string entered in the format of "Hours_Minutes_Seconds". Typically, only the Hours_Minutes parameters are used and Seconds is left at 0, unless the window of transmission is less than 60 seconds. Maximum offset is 23:59:59. This parameter can be set to 0 for no offset.

# RInterval (Time between Random Transmissions)

Used to define the average time between random transmissions. The value is a constant string entered in the format of "Hours_Minutes_Seconds". Typically the assigned interval is in hours, so the minutes and seconds parameters are left at 0 (example, "1_0_0" sets up an hourly interval). Maximum interval is 24 hours; minimum interval is 5 minutes.
