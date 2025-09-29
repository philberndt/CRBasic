# WaitDigTrig (Use an External Digital Trigger)

The WaitDigTrig instruction is used to trigger a measurement scan with an external digital trigger.

## Syntax

WaitDigTrig(ControlPort,Option)

The following program shows how WaitDigTrig can be used in a datalogger program to control the scan rate of another datalogger.

```
'Program running in Main datalogger
'Port set is used to trigger the scan in the second datalogger
Public Voltage(3)
DataTable (Test,1,-1)
DataInterval (0,50,Sec,10)
Sample (3,Voltage,IEEE4)
EndTable
BeginProg
Scan (50,mSec,0,0)
PortSet (C1 ,1 )'trigger scan in second logger
VoltDiff (Voltage(),3,mv2500,1,False,0,4000,1.0,0)

CallTable Test
PortSet (C1 ,0)'reset until next scan
NextScan
EndProg

'Program for Second datalogger
'WaitDigTrig is used to trigger the scan rate
Public Voltage(4)
DataTable (Test,1,-1)
DataInterval (0,50,Sec,10)
Sample (3,Voltage,IEEE4)
EndTable
BeginProg
Scan (50,mSec,0,0)'setting the timestamping resolution
WaitDigTrig(C1 ,0)'scan executes based on external trigger, not on Scan Interval
VoltDiff (Voltage(),3,mv2500,1,False,0,4000,1.0,0)

CallTable Test
NextScan
EndProg
```

## Remarks

The WaitDigTrig instruction is used to trigger measurement scans from an external digital input to a datalogger terminal. When the digital channel is in the state specified by the Option parameter, the scan takes place. For subsequent scans to occur, the digital channel s state must evaluate as false and then true again (i.e., if the trigger condition remains true, only one scan will occur). A common use of this instruction is to control the scan rate of one datalogger based on another datalogger or control the scan rate of a datalogger based on an external device such as a GPS.

The WaitDigTrig instruction can be executed within a Scan/NextScan sequence or it can be executed outside a scan (most commonly in a SlowSequence). When used inside a scan, execution of the scan occurs when WaitDigTrig evaluates as true (for example, port rises/falls or goes high/low) rather than basing the scan rate on the datalogger's clock. Thus, an external trigger controls the scan rate. This has ramifications:

- If WaitDigTrig is placed in the main scan after the Scan instruction and prior to the end of measurement tasks in the scan, any SlowSequences in the program will not be allowed access to the measurement hardware until the main scan gets a trigger and the datalogger runs through its measurements to completion.

- The timestamp in the datalogger's data tables is clocked by the execution of the scan rate. When the scan starts, the time stamp is incremented by the interval specified by the Scan instruction. Thus, if the scan rate is set at 2 seconds, but the trigger is activated every 4 seconds, the timestamp will still increment only 2 seconds every time the trigger activates the scan (increment value is off by a factor of 2). To avoid misleading timestamps, it is recommended the trigger application be repeated at the same rate as the main scan rate of the program. Each application of the trigger will cause the scan to execute one time only.

When used outside a scan, WaitDigTrig behaves similarly to a Delay instruction, though the delay is based on the state or transition of a control port instead of based on time. In this instance, the WaitDigTrig instruction can be placed inside an infinite Do/Loop, and the remaining instructions in that loop will be performed only when WaitDigTrig is true. This results in functionality similar to the Interrupt Subroutine available in Edlog-programmed dataloggers (such as the CR10X or CR23X's subroutine 96, 97, or 98).

**NOTE:** If the program is running in sequential mode and has a slow sequence that includes a WaitDigTrig instruction, once triggered, that sequence will not be able to perform any measurement tasks when the main scan is running. The slow sequence will pause before its first measurement instruction, until the main scan is completed, after which it will continue. If the slow sequence contains only processing tasks, these tasks can run in conjunction with the main scan.

## Parameters

# ControlPort (Control Port)

The ControlPort parameter is used to specify the digital channel that will be used to trigger the scan. Right-click to display a list.

| Code | Description            |
| ---- | ---------------------- |
| C1   | Control Port 1         |
| C2   | Control Port 2         |
| SE1  | Single ended channel 1 |
| SE2  | Single ended channel 2 |
| SE3  | Single ended channel 3 |

Type: Constant

# Option (Signal Type)

Used to specify what type of signal will cause the scan to begin. Right-click to display a list of options:

| Code | Description                                         |
| ---- | --------------------------------------------------- |
| 0    | ControlPort going from low to high (edge triggered) |
| 1    | ControlPort going from high to low (edge triggered) |
| 2    | ControlPort is high (level triggered)               |
| 3    | ControlPort is low (level triggered)                |

Type: Constant

**Caution:** The port for WaitDigTrig is configured at compile time. Once a port has been configured using WaitDigTrig, you cannot use a different option for that port using a different WaitDigTrig instruction later in the program.
