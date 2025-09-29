# SemaphoreGet, SemaphoreRelease (Semaphore Get, Semaphore Release)

The SemaphoreGet and SemaphoreRelease instructions are used to lock a hardware component or memory shared by two different sequences within a program, so that one sequence cannot access the hardware or memory while the other is acting upon it.

## Syntax

SemaphoreGet(SemNumber)

'section of code

SemaphoreRelease(SemNumber)

### Example #1

In the following example program, the SDI12Recorder instruction is running in a slow sequence, retrieving temperature data from an SDI12Sensor. The data is then moved from the SDI12Recorder Destination variable into another variable in the main program. The difference between the datalogger's thermocouple temperature and the sensor temperature is then calculated in the main program. SemaphoreGet/Semaphore release is used to prevent the Move from occurring when the main scan is calculating the difference and vice versa.

```
const sdi_reps = 3
Dim new_data as BOOLEAN
Public RefTemp
Public TCT(sdi_reps)
Public tdiff(sdi_reps)
Dim sdi_dst(sdi_reps)
Dim slow_data(sdi_reps)
Const DATA_SEM = 1
Dim i as LONG

BeginProg
Scan(1,sec,0,0)
PanelTemp (RefTemp,4000)
TCDiff (TCT,sdi_reps,mv34,1,TypeT,RefTemp,True,400,4000,1.8,32)

'Enable semaphore lock
SemaphoreGet(DATA_SEM)
If new_data then
for i = 1 to sdi_reps
tdiff(i) = TCT(i) - slow_data(i)
Next i
new_data = FALSE
Endif
SemaphoreRelease(DATA_SEM)

NextScan

SlowSequence
Scan(10,sec,0,0)
' SDI12Recorder (dst,port,addr,command,Mult,Offset)
SDI12Recorder (sdi_dst,C1,1,"C!",1,0)
' Enable semaphore lock
SemaphoreGet(DATA_SEM)
' Move sensor data into another array
move(slow_data,sdi_reps,sdi_dst,sdi_reps)
new_data = TRUE
SemaphoreRelease(DATA_SEM)

NextScan
EndProg
```

### Example #2

This program measures three SDI-12 sensors with different SDI-12 addresses on the same control port. Because two of the three [SDI12Recorder()](sdi12recorder.md) instructions occur in a different scan, SemaphoreGet and SempaphoreRelease are used to avoid a hardware conflict. Without SempaphoreGet and SemaphoreRelease,NAN

**Note:** Not a number. A data word indicating a measurement or processing error. Voltage overrange, SDI-12 sensor error, and undefined mathematical results can produce NAN.

is likely to occur for one or more SDI-12 measurements due to resource conflicts with the control port.

```
'CR300 Series

'Declare Variables
Public PTemp, Batt_volt
Public Sensor1(2)
Public Sensor2(2)
Public Sensor3(2)

BeginProg
Scan (5,Sec,0,0)
PanelTemp (PTemp,15000)
Battery (Batt_volt)
NextScan

SlowSequence
Scan (10,Sec,100,0)
SemaphoreGet(1)
SDI12Recorder(Sensor1(),C1,1,"M!",1.0,0)
SemaphoreRelease(1)
NextScan
EndSequence

SlowSequence
Scan (30,Sec,100,0)
SemaphoreGet(1)
SDI12Recorder(Sensor2(),C1,2,"M!",1.0,0)
SDI12Recorder(Sensor3(),C1,3,"M!",1.0,0)
SemaphoreRelease(1)
NextScan
EndSequence
EndProg
```

### Example #3

This example demonstrates both Semaphore (1) and reserved Semaphore (4). Semaphore(1) is used to ensure a data table is not written to while data is retrieved from the table in a SlowSequence scan. Semaphore (4) is used to prevent a client from accessing a file on the data logger while it is being updated with new data. It is assumed that the user has configured data logger settings that allow it to run as an HTTP server.

```
Const newLine As String = CHR(13) + CHR(10)'\r\n
Const nanStr As String = "-2147483648"'Tablename.timestamp returns this if an error occurs

Public PTemp, Batt_volt, err
Dim fHandle As Long
Dim timeStamp As String * 30
Dim temp_batt, temp_PTemp, temp_recNum

DataTable (Test,1,-1)
DataInterval (0,15,Sec,10)
Minimum (1,batt_volt,FP2,False,False)
Sample (1,PTemp,FP2)
EndTable
```

BeginProg
Scan (1,Sec,0,0)
PanelTemp (PTemp,15000)
Battery (Batt_volt)
'use semaphore 1 to ensure the table is not written to while the slowsequence is retrieving the most recent record
SemaphoreGet(1)
CallTable Test
SemaphoreRelease(1)
NextScan

SlowSequence
Scan(1, Min,0,0)
SemaphoreGet(1)
timeStamp = Test.timestamp(5,1)
temp_recNum = Test.Record(1,1)
temp_batt = Test.Batt_volt_Min
temp_PTemp = Test.Ptemp
SemaphoreRelease(1)

If timeStamp = nanStr Then
'Invalid timeStamp, do not execute the rest of this code
err += 1
ContinueScan
EndIf

'Use Semaphore 4 here so that if a client tries to access device_info.txt over HTTP while the file is being updated, the data logger will wait until the update is complete before sending the file.
SemaphoreGet(4)
fHandle = FileOpen("CRD:device_info.txt","a",-1)
FileWrite(fHandle,timeStamp + " Rec: " + temp_recNum + " Batt Min: " + temp_batt + " PTemp: " + temp_PTemp + newLine,0)
FileClose(fHandle)
'Release when done (If semaphore 4 is never released, the datalogger will not be able to serve up any files over HTTP)
SemaphoreRelease(4)
NextScan
EndProg

```

## Remarks


These instructions are useful if two concurrent sequences are both accessing the same hardware (for example, COMport) or the same memory (for example, variable). SemaphoreGet() will wait until the semaphore is not in use to flag it as available and then proceed. SemaphoreRelease() will release the semaphore so that some other sequence can access it. The SemNumber argument is a constant assigned to the semaphore.

An example application is when separate SDI12Recorder instructions that use the same control port occur in different but concurrent scans. SemaphoreGet and SempaphoreRelease can be used to avoid a hardware conflict that can result inNAN

**Note:** Not a number. A data word indicating a measurement or processing error. Voltage overrange, SDI-12 sensor error, and undefined mathematical results can produce NAN.

returned by the [SDI12Recorder](sdi12recorder.md) instruction (see example #2).

Care should be taken when using semaphores as they can hold up program execution. In general, the most efficient programs get the semaphore for the least amount of time possible to do the necessary operations.


# Reserved Number, 4


**NOTE:** Semaphore number 4 is fully implemented in OS versions newer than11.01.

Semaphore number 4 is only applicable when the data logger is configured as a server. In this mode, another device (the client) initiates a connection and tries to send or receive data. When Semaphore 4 is used, it prevents the data logger from returning a file to the client for a specific reason, usually because the requested file is currently being edited.

An example application is a data logger generating a data file or image that is also served via HTTP. SemaphoreGet(4) and SemaphoreRelease(4) can be used around the data file generation process to ensure that the data logger does not change the contents of the file while it is being uploaded over HTTP.

**NOTE:** Semaphore 4 has no effect on CRBasic client instructions such as FTPClient(), HTTPGet(), or HTTPPost() because, in these situations, the data logger is acting as a client rather than a server.
```
