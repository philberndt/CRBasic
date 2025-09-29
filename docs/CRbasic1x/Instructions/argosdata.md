# ArgosData (Argos Data)

The ArgosData instruction is used to specify the data to be transmitted to the Argos satellite.

## Syntax

ArgosData(ResultCode,ST20Buffer,DataTable,NumRecords,DataFormat)

The following is a sample program to write data to buffer zero of the ST-20, then initiate transmissions. Once an hour, 28 bytes are written to buffer 0. ArgosDataRepeat() is used to start transmissions of the contents of buffer zero which will be repeated 255 times, or until the data is updated on the next hour.

```
Public Ptemp, TCtemp(6), i
Public DataResult, RepeatResult, ArgosBuffer(8) as boolean

DataTable (ArgosDat,True,-1)
DataInterval(0,30,min,-1)
Sample (1,Ptemp,FP2)
Average(6,TCtemp(),FP2,0)
EndTable

BeginProg
Scan (10,Sec,3,0)
PanelTemp (PTemp,15000)
TCDiff (TCTemp(),6,mv200C,1,TypeT,PTemp,True,0,15000,1.0,0)

CallTable (ArgosDat)
If iftime(0,60,min) then
ArgosData(DataResult,0,ArgosDat,2,"FP2")
ArgosBuffer(1)=true
For i=2 to 8 step 1
ArgosBuffer(i) = False
Next
ArgosDataRepeat(RepeatResult,203,255,ArgosBuffer())
Endif
NextScan
EndProg
```

## Remarks

Using this instruction, the datalogger can be set up to send two-byte data values (FP2) or you can specify the bit width for each field.

Each of the Argos buffers holds up to 31 or 32 bytes of data (a device with a 20 bit ID can hold 32 bytes; a device with a 28 bit ID can hold only 31 bits). Care should be given to write only the number of bytes to the data table that can be accommodated by the buffer; data that exceeds the buffer is discarded.

The datalogger will attempt to send any new data from the data table when the instruction is executed. In most instances this instruction should be executed only after new data is stored to the data table. When the ArgosData instruction is executed, any existing data in the buffer is erased.

## Parameters

# ResultCode (Instruction Results)

A variable that holds the result of the instruction. The instruction stores a -1 (True) if the transmission is successful or 0 (False) if it fails. If 2 is returned, the transmitter is not connected to the datalogger's communications port or there is some hardware problem with the connection.

Type: Variable declared as a Float, Boolean, or long

# ST20Buffer (ST20 Buffers)

The number of the ST20 buffer that should be set up. Valid entries are 0 through 6 (ArgosData, ArgosTransmit) or 0 through 7 (ArgosSetup).

Type: Constant Integer

For the ArgosData instruction, valid entries are 0 through 6 (7 is reserved for the ST20's internal temperature).

# DataTable

The datalogger DataTable that holds the data to be sent to the transmitter. Right-click the parameter to display a list of declared data tables in the program.

Type: Data table name

# NumRecords (Number of Records)

The number of records from the data table to copy to the buffer of the transmitter.

Type: Variable

# DataFormat (Format for Transmit Data)

The format for the values being transmitted. If "FP2

**Note:** Two-byte floating-point data type. Default datalogger data type for stored data. While IEEE four-byte floating point is used for variables and internal calculations, FP2 is adequate for most stored data. FP2 provides three or four significant digits of resolution, and requires half the memory as IEEE4.

" is entered, the data will be transmitted as a two-byte value. The data in the table must be formatted as FP2, or a compiler error will be returned. Alternately, you can specify the bit width for each field in a data table by entering a comma separated string of integers ("nnn,nnn,nnn " where each nnn represents the width of a field in the table). In this instance, all values must be formatted as Long in the data table, or a compiler error will be returned.

Type: String enclosed in quotes
