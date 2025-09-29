# ArgosDataRepeat (Repeat Rate for ArgosData)

The ArgosDataRepeat instruction is used to set the repeat rate for the [ArgosData](argosdata.md) instruction.

## Syntax

ArgosDataRepeat(ResultCode,RepeatRate,RepeatCount,BufferArray)

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

The ArgosData/ArgosDataRepeat instructions are most useful when transmitting to only one ID number. When transmitting to multiple ID numbers,[ArgosTransmit](argostransmit.md) should be used so that the datalogger program can control the timing of each transmission.

# ResultCode (Instruction Results)

A variable that holds the result of the instruction. The instruction stores a -1 (True) if the transmission is successful or 0 (False) if it fails. If 2 is returned, the transmitter is not connected to the datalogger's communications port or there is some hardware problem with the connection.

Type: Variable declared as a Float, Boolean, or long

## Parameters

# RepeatRate (Repeat Rate)

The amount of time, in seconds, between each packet being sent. Valid rates are 0 through 255, or enter a negative number to use the default repetition rate for the PTT. Note that the transmitter adds 42 seconds to the rate entered for this parameter (so if 0 is entered, the rate will be 42).

Type: Constant Integer

# RepeatCount (Message Repeat Count)

Specifies how many times the message will be repeated. Valid entries are 0 to 255. Enter a negative value to use the default PTT settings.

Type: Constant Integer

# BufferArray (Buffer Array)

An Boolean variable array used to set the buffers in the transmitter to true (use) or false (don't use). BufferArray is a variable array dimensioned to 8 and declared as a Boolean. Each element in the array corresponds to the Buffer Number minus 1 (e.g., 1 = buffer 0, 2 = buffer 1 8 = buffer 7).

Type: Boolean variable dimensioned to 8 (e.g., Buffer(8) as Boolean)
