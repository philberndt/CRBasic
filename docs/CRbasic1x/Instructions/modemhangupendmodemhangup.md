# ModemHangup, EndModemHangup (Instructions to Run When a COM port hangs up)

The ModemHangup and EndModemHangup instructions are used to enclose code that should be run when a COM port hangs up communication.

## Syntax

ModemHangup(ComPort)

instructions to be run upon hangup

EndModemHangup

The following program shows the use of the ModemHangup sequence to shut down a modem after a call is finished.

```
'Declare Variables
Public PTemp, batt_volt, result
Public DialSuccess, RemBattery

DialSequence (2)
StaticRoute(ComRS232,2,2)'Creates route to destination DL
DialSuccess = DialModem (ComRS232,9600,"3217654","CONNECT 9600")
EndDialSequence (DialSuccess)' -1 indicates successful response string rcvd

'Forces immediate modem hang up for lower average current drain
ModemHangup(ComRS232)
SerialOpen (COMRS232,9600,0,0,1000)
'Escape characters for modem command mode
Delay (1,1,Sec)'guard time
SerialOut (ComRS232,"+++","OK",0,1000)
Delay (1,1,Sec)'guard time
SerialOut (ComRS232,"ATH0" + CHR(13),"O",0,100)'H0 (zero) hangs up
SerialClose(ComRS232)
EndModemHangup

DataTable (Test2,1,10000)
DataInterval (0,0,Sec,10)
Minimum (1,batt_volt,FP2,0,False)
Sample (1,Ptemp,FP2)
EndTable

BeginProg
SerialOpen (COMRS232,9600,0,0,1000)
'US Robotics Initialization string
SerialOut (COMRS232,"AT &F1 &N6 V1 X1" + CHR(13),"OK",1,100)
SerialClose (ComRS232)

Scan (60,Sec,0,0)
PanelTemp (PTemp,15000)
Battery (Batt_volt)
GetVariables (Result,ComRS232,2,2,0000,2500,"Public","Batt_Volt",RemBattery,1
'Result = 0 is success
CallTable Test2
NextScan
EndProg
```

## Remarks

The ModemHangup instruction indicates the beginning of the code; the EndModemHangup indicates the ending. The code is entered in the declarations section of the program, prior to the main program (defined by the BeginProg/EndProg instructions). When the datalogger detects that a COM port is hanging up, the ModemHangup code will be run.

This instruction set is most often used with modems that must be sent a command sequence to disconnect and go into a low power state.

Note that each COM port operates independently; therefore, commands to hang up modems can be processed concurrently.

**NOTE:** This instruction runs sequentially from the processing task sequencer, regardless of whether the datalogger is in pipeline or sequential mode.

## Parameter

# ComPort

The ComPort parameter specifies the communication port and mode that will be used for this instruction.

| Alphanumeric | Description                           |
| ------------ | ------------------------------------- |
| ComRS232     | RS232 port of the datalogger          |
| ComME        | Datalogger CS I/O port; modem enabled |
| Com310       | Datalogger CS I/O port; COM310 modem  |
| Com320       | Datalogger CS I/O port, COM320 modem  |
| ComSDC7      | Datalogger CS I/O port; SDC7          |
| ComSDC8      | Datalogger CS I/O port; SDC8          |
| ComSDC10     | Datalogger CS I/O port; SDC10         |
| ComSDC11     | Datalogger CS I/O port; SDC11         |
| ComC1        | Datalogger control terminals 1 & 2    |
| ComC3        | Datalogger control terminals 3 & 4    |
| ComC5        | Datalogger control terminals 5 & 6    |
| ComC7        | Datalogger control terminals 7 & 8    |

A variable can be used in the ComPort parameter for use with functions that return a communication port. If communication occurs using TCP/IP, enter the variable for the socket returned by TCPOpen.

Type: Constant
