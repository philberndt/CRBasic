# DNPVariable (DNP Variable)

The DNPVariable instruction sets up theDNP

**Note:** Distributed Network Protocol is a set of communications protocols used between components in process automation systems. Its main use is in utilities such as electric and water companies.

implementation in the datalogger that is a DNP outstation device.

## Syntax

DNPVariable(Source,Swath,DNPObject,DNPVariation,DNPClass,DNPFlag,DNPEvent,DNPNumEvents)

Example 1. This program sets up a DNP3 array of 2 containing Panel Temperature in the first index and Battery Voltage in the second index. Events are triggered if the panel temperature is above 25 C or if the battery voltage drops below 12 volts.

```
Public PTemp, BattV

Public DNP3_Data(2) As Long'Declare array for DNP3 Data
Public DNP3_Flag(2) As Long'Declare array for DNP3 Data Quality Flags

'Main Program
BeginProg

DNP3_Flag(1) = 1'initialize data quality flag for Ptemp to be "online"
DNP3_Flag(2) = 1'initialize data quality flag for Battery voltage to be "online"

DNP(20000,115200,1000)
DNPVariable(DNP3_Data(),2,30,1,0,DNP3_Flag(),0,0)'DNPVariable instruction for static data
DNPVariable(DNP3_Data(1),1,32,1,1,DNP3_Flag(),PTemp > 25,1)'generate a class 1 event if PanelTemp > 25
DNPVariable(DNP3_Data(2),1,32,1,2,DNP3_Flag(),BattV < 12,1)'generate a class 2 event of Battery voltage < 12

Scan (10,Sec,0,0)
PanelTemp (PTemp,15000)
Battery (BattV)

DNP3_Data(1) = PTemp * 100'Scaling by 100
DNP3_Data(2) = BattV * 100'Scaling by 100

DNPUpdate(1,10)'Slave Address of 1 and Master Address of 10
NextScan
EndProg
'///////////////END OF PROGRAM 1\\\\\\\\\\\\\\\\\\\
```

Example 2. The following program example demonstrates using analog output object 41 to set an analog value and object 40 to report the status of an analog output point. In this case, a counter is used for demonstration.

```
'DNP3 Program for Analog Outputs
'Declare DNPvariable types ot match the DNP variation
'Var 1 = 32 bit
'Var 2 + 16 bit
'Var 3 = floating point

Public Link_Conf As Long
Public Batt_volt
Public Obj40Var As Long'Use type Float for 41 variation 3; se Long for other variations
Alias Obj40Var = Stage_height
Public Obj40Var_Flag as Long
Public Counter'To generate data for demonstration

BeginProg
Link_Conf = 1000'Disables datalink confirmation if using serial comms
Obj40Var_fLAG = 1
DNPVariable(Obj40Var,1,40,1,0,Obj40Var_Flag(),0,0)'Make variations for objects 40 and 41 the same
DNPVariable(Obj40Var,1,41,1,0,Obj40Var_Flag(),0,0)'Object 41 is used for writing value, 40 is used to read it back out
Scan (1,Sec,0,0)
Counter += 1'Increment counter each scan
Stage_height = Counter
'DNP(ComC1,115200,Link_Conf) 'Serial Example
DNP(20000,115200,Link_Conf)'IP Example
Battery (Batt_volt)
DNPUpdate(1,10)'Slave Address of 1 and Master Address of 10
NextScan
EndProg
'////////////////End Of Program 2\\\\\\\\\\\\\\\\\
```

Example 3. Send Unsolicited Response from Datalogger 1 via TCP/IP using optional parameter, ConnectHandle:

```
Public Binput (2) As Boolean
Public counter As Long
Public IP_Socket As Long
Const = IPAddress ="192.168.xx.xx"
BeginProg
DNP(20000,115200,1000)
DNPVariable(Binput(),2,1,2,0,0,0,0)
DNPVariable(Binput(),2,2,1,1,0,0,2)
Scan(1,Sec,1,0)
IP_Socket = TCPOpen(IPAddress,20000,0)
counter = counter +1
DNPUpdate(1,10,5,0,IP_Socket)
NextScan
EndProg
```

## Remarks

DNPVariable is a DNP outstation instruction and must be used with a DNP and DNPUpdate instruction.

## Parameters

# Source

The name of the Variable that is the input for the instruction. Right-click the parameter to display a list of defined variables.

Type: Variable

For the DNPVariable instruction, the Source parameter is a public variable or variable array that specifies the source of data to populate the DNP outstation array. This variable must be of typeLong

**Note:** Data type used when declaring a variable as an integer.

for Analog Input or Analog Output Objects, unless the variation specified is single precision floating point (see DNPObject), in which case the variable must be of typeFloat

**Note:** Four-byte floating-point data type. Default datalogger data type for Public or Dim variables. Same format as IEEE4.

. If the object specified is a Binary Input or Binary Output, then the source variable must be of typeBoolean

**Note:** Data type used to represent conditions or hardware that have only two states (true or false) such as flags and control ports.

.

# Swath

The number of values of the array over which to perform the specified operation.

Type: Constant (or expression that evaluates as a constant)

For the DNPVariable instruction, the Swath parameter is the number of elements that are in the DNP outstation array. This number must be equal to or less than the length of the array the data is mapped from in the Source parameter. An array index can be specified in Swath to write a class to a specific element or series of elements within the array. For example, with an array of 10, such as BinaryInput(10):

- DNPVariable (BinaryInput(1),5,2,1,1,BinaryInput_Flag(),0,20) 'writes class 1 data to array elements 1-5.

- DNPVariable (BinaryInput(6),5,2,1,2,BinaryInput_Flag(),0,20) 'writes class 2 data to array elements 6-10.

# DNPObject (DNP Object)

The DNPObject parameter assigns the Source parameter to a specific DNP object type. DNP data is organized into arrays with different Object types. The Objects are divided into groups and each group has variations of that group. All communications are referenced from the DNP master. For example the Analog Input object means the Master is requesting data from a datalogger DNP outstation array.

Type: Constant

# DNPVariation (DNP Variation)

Assigns the Source parameter to a specific variation, which describes the data format within its Object group. Datalogger supported DNP objects and variations:

**NOTE:** Object 0 device attribute variations are read only and cannot be specified in the DNPVariable instruction. Rather, they are sent only when requested by a master device.

| Object | Variation | Description                                                                        |
| ------ | --------- | ---------------------------------------------------------------------------------- |
| 0      | 240       | Device attribute, maximum transmit fragment size                                   |
| 0      | 241       | Device attribute, maximum receive fragment size                                    |
| 0      | 242       | Device attribute, OS version                                                       |
| 0      | 243       | Device attribute, rev board, TI chip                                               |
| 0      | 247       | Device attribute, station name                                                     |
| 0      | 248       | Device attribute, serial number                                                    |
| 0      | 250       | Device attribute, product name and model                                           |
| 0      | 252       | Device attribute, name of device manufacturer                                      |
| 0      | 254       | Device attribute, return all devices attributes in single response                 |
| 0      | 255       | Device attribute, retrieve all of the supported device attribute variation numbers |
| 1      | 1         | Static, Binary Input                                                               |
| 1      | 2         | Static, Binary Input with Status                                                   |
| 2      | 1         | Event, Binary Input Change without Time                                            |
| 2      | 2         | Event, Binary Input Change with Time                                               |
| 2      | 3         | Event, Binary Input Change with Relative Time                                      |
| 10     | 1         | Static, Binary Output, Packed Format                                               |
| 10     | 2         | Static, Binary Output Status, with Flags                                           |
| 12     | 1         | Binary command, control relay output block (CROB)                                  |
| 20     | 1         | Counter, 32-bit with flag                                                          |
| 20     | 2         | Counter, 16-bit with flag                                                          |
| 20     | 3         | Counter, 32-bit with flag, delta                                                   |
| 20     | 4         | Counter, 16-bit with flag, delta                                                   |
| 20     | 5         | Counter, 32-bit without flag                                                       |
| 20     | 6         | Counter, 16-bit without flag                                                       |
| 20     | 7         | Counter, 32-bit without flag, delta                                                |
| 20     | 8         | Counter, 16-bit without flag, delta                                                |
| 21     | 1         | Frozen counter, 32-bit with flag                                                   |
| 21     | 2         | Frozen counter, 16-bit with flag                                                   |
| 21     | 5         | Frozen counter, 32-bit with flag and time                                          |
| 21     | 6         | Frozen counter, 16-bit with flag and time                                          |
| 21     | 9         | Frozen counter, 32-bit without flag                                                |
| 21     | 10        | Frozen counter, 16-bit without flag                                                |
| 22     | 1         | Counter event, 32-bit with flag                                                    |
| 22     | 2         | Counter event, 16-bit with flag                                                    |
| 22     | 5         | Counter event, 32-bit with flag and time                                           |
| 22     | 6         | Counter event, 16-bit with flag and time                                           |
| 30     | 1         | Static, 32-Bit Analog Input                                                        |
| 30     | 2         | Static,16-Bit Analog Input                                                         |
| 30     | 3         | Static, 32-Bit Analog Input without Flag                                           |
| 30     | 4         | Static, 16-Bit Analog Input without Flag                                           |
| 30     | 5         | Analog input, single precision floating point with flag                            |
| 32     | 1         | Event, 32-Bit Analog Change Event without Time                                     |
| 32     | 2         | Event, 16-Bit Analog Change Event without Time                                     |
| 32     | 3         | Event, 32-Bit Analog Change Event with Time                                        |
| 32     | 4         | Event, 16-Bit Analog Change Event with Time                                        |
| 32     | 5         | Analog input event, single precision floating point without time                   |
| 32     | 7         | Analog input event, single precision floating point with time                      |
| 40     | 1         | Analog output status, 32-bit with flag                                             |
| 40     | 2         | Analog output status, 16-bit with flag                                             |
| 40     | 3         | Analog output status, single precision floating point with flag                    |
| 41     | 1         | Analog output, 32-bit                                                              |
| 41     | 2         | Analog output, 16-bit                                                              |
| 41     | 3         | Analog output, single precision floating point                                     |
| 50     | 1         | Read, Time and Date                                                                |
| 60     | 2         | Class 1 Data                                                                       |
| 60     | 3         | Class 2 Data                                                                       |
| 60     | 4         | Class 3 Data                                                                       |
| 110    | X         | Octet string; variation parameter is used to declare maximum length of string      |

Type: Constant

# DNPClass (DNP Class)

Assigns the Object class to the Source parameter. Valid classes are 0 for static data or 1, 2, and 3 for event data.

Type: Constant

# DNPFlag (DNP Flag)

A constant, expression, variable, or variable array that indicates the value of the DNP3 data quality flag corresponding to the Source parameter. If a constant is used, then by default, all quality flags will be set to online, regardless of the value entered (for example, 0 or 1 both evaluate to online ). If a variable or variable array is used, the variable must be declared as a data type of Long, and the DNP3 flags will reflect the value of the variable or variable array. The variable or variable array can then be set under program control. A 1 sets a quality flag to online; a 0 sets a flag to offline.

If the array for the flag parameter is smaller than the array for the source parameter, then the last element of the flag array is used for all of the remaining source array elements. A single variable (scalar) is treated like an array of one element.

Type: Variable or Constant

# DNPEvent (DNP Event)

Used to create event data. An expression or variable array may be entered and used to trigger the creation of change events. If a variable or variable array is used, it must be of type Long. If the array is smaller than the source array, the last element of the array is used for all the remaining array elements. A single variable (scalar) is treated like an array of one element. Any time the event trigger variable evaluates as true, a change event will be created for the Source parameter. This allows the user to define the conditions that create change events. The default value is zero and indicates that a change event will be created whenever there is any change to the value of the Source parameter.

Type: Variable or constant

# DNPNumEvents (DNP Number of Events)

The number of events that will be saved in the history before events are successfully received at the master; that is, the size of the history. For event objects, this number must be greater than 0.

Type: Constant or variable
