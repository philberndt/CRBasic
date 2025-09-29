# ArgosTransmit (Transmit to Argos)

The ArgosTransmit instruction is used to initiate a single transmission to an Argos satellite when the instruction is executed.

## Syntax

ArgosTransmit(ResultCode,ST20Buffer)

The following is a sample program to write hourly data to buffers 0 and 1 of the ST-20. ArgosTransmit() is used to transmit the contents of both buffers every 90 seconds. Note: The Argos ID numbers associated with both buffers must be different.

```
Public Ptemp, TCtemp(6)
Public DataResult
Public TransmitResult1
Public TransmitResult2

DataTable (ArgDat0,True,-1)
DataInterval(0,30,min,-1)
Sample (1,Ptemp,FP2)
Average(6,TCtemp(),FP2,0)
EndTable

DataTable (ArgDat1,True,-1)
DataInterval(0,30,min,-1)
Sample (1,Ptemp,FP2)
Average(6,TCtemp(),FP2,0)
EndTable

BeginProg
Scan (10,Sec,3,0)
PanelTemp (PTemp,15000)
TCDiff (TCTemp(),6,mv200C,1,TypeT,PTemp,True,0,15000,1.0,0)

CallTable (ArgDat0)
CallTable (ArgDat1)
if iftime(0,60,min) then
ArgosData (DataResult,0,ArgDat0,2,"FP2")
ArgosData (DataResult,1,ArgDat1,2,"FP2")
endif
if iftime(0,90,sec) thenArgosTransmit(TransmitResult1,0)
if iftime(10,90,sec) thenArgosTransmit(TransmitResult2,1)
NextScan
EndProg
```

## Remarks

This instruction is used most often when transmitting to multiple ID numbers, where more data throughput is required. The user must write the program to account for the retransmission of failed data packets. If transmitting to only one ID number, you may want to use [ArgosData](argosdata.md) and [ArgosDataRepeat](argosdatarepeat.md), which take care of failures based on the parameters in ArgosDataRepeat.

## Parameters

# ResultCode (Instruction Results)

A variable that holds the result of the instruction. The instruction stores a -1 (True) if the transmission is successful or 0 (False) if it fails. If 2 is returned, the transmitter is not connected to the datalogger's communications port or there is some hardware problem with the connection.

Type: Variable declared as a Float, Boolean, or long

# ST20Buffer (ST20 Buffers)

The number of the ST20 buffer that should be set up. Valid entries are 0 through 6 (ArgosData, ArgosTransmit) or 0 through 7 (ArgosSetup).

Type: Constant Integer

For the ArgosTransmit instruction, valid entries are 0 through 6 (7 is reserved for the ST20's internal temperature).
