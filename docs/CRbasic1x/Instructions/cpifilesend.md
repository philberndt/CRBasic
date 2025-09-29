# CPIFileSend (Send File to a GRANITE or CDM Module)

The CPIFileSend instruction is used to send an Operating System to a GRANITE or CDM module remotely using a datalogger s memory card (CRD), user defined drive (USR),or SC115 (USB).

## Syntax

```
CPIFileSend
```

(ResultCode,FileSendProgress,CPIAddress,OSFlag,OSVersion)

Following are two example programs that demonstrate using CPIFileSend to update the OS of one GRANITE module and multiple GRANITE modules. **Example 1 - CPIFileSend to one Module** ```
'CPIFileSend Example

Public Fs_result
Public Fs_progress
Public OSFlag
Public Count as Long
Public CPI_addr as long

BeginProg
CPI_addr = 1
Scan(1,sec,0,0)
Count += 1
If (OSFlag) Then
CPIFileSend(Fs_result,Fs_progress,CPI_addr,1,"CRD:CDM-A100.Std.02.00.obj")
OSFlag = 0
EndIf
NextScan
EndProg

```**Example 2 - CPIFileSend to multiple modules** The following program shows how to use the CPIFileSend instruction to send an Operating System to multiple GRANITE modules, one at a time. With this program, a new Operating System is sent to CPI addresses 1 through 10. The fs_progress variable can be monitored to observe file send progress for each GRANITE module. Once fs_progress reaches 100, there will be a 30 second delay, allowing time for the Operating System to finish loading before moving on to the next CPI address.

```

Const MAX_TRY = 5
Public fs_result
Public fs_progress
Public SendOSFlag'Use Table Monitor to set this flag true
Public count as Long
Public CPI_addr as long
Public BegAddr as long
Public EndAddr as long
Public try as long
Public fail as long
Public Failed(10) as long
Public Fname as string \* 64 = {"CRD:VOLT100.Std.06.01.obj"}'location and name of OS

BeginProg
CPISpeed(125)
BegAddr = 1 'CDM address to start with
EndAddr = 10 'CDM address to end with
Scan(1,sec,0,0)
count += 1
If (SendOsFlag) Then
For CPI_addr = BegAddr to EndAddr
fs_result = -1
try = 1
While try < MAX_TRY AND fs_result <> 0
CPIFileSend(fs_result,fs_progress,Cpi_Addr,1,Fname)
Delay(1,6,sec)' allow them to timeout
try += 1
Wend
If try = MAX_TRY Then
fail += 1
failed(fail) = CPI_addr
ElseIf(CPI_addr <> EndAddr) Then
Delay(1,30,sec)' give them time to load the OS
EndIf
Next CPI_addr
SendOSFlag = 0
EndIf
NextScan
EndProg

```

## Remarks


The CPIFileSend instruction is commonly placed in a conditional statement and triggered with a variable flag. Sending the Operating System (.obj file) to the GRANITE module may take several seconds to several minutes, depending on the drive used. For example, sending the operating system from a CRD: or USR: is much faster than sending the .obj file from an SC-115 (USB:). Care should be taken not to interrupt or remove the drive where the Operating System is stored until the FileSendProgress variable has reached 100, which indicates the file transfer is complete. Once the file send is complete, it may take several seconds to a few minutes for the GRANITE module to compile the new operating system and configure the device. During configuration, the COMM Status LED on the GRANITE module will illuminate solid red, solid orange, or flashing an alternating red/green pattern. After configuration is complete, the LED will blink orange once per second. Do not remove power from the device until the COMM Status LED is blinking orange once per second.


## Parameters



# ResultCode (Instruction Result)


The variable in which a response code for the transmission will be stored. ResultCode will return:

| Code | Description |
| --- | --- |
| 0 | Success. |
| 1 | The module did not return success. |
| 2 | Unable to open the file. |
| 3 | Module not identified on the bus. |
| 4 | CAN send failure. |
| 5 | Timeout waiting for a response from the module after sending an image. |

Type: Variable (Float)


# FileSendProgress (File Send Progress)


File send progress in percent. Shows 100 when file send is complete.

Type: Variable


# CPIAddress (CPI Address)


TheCPI

**Note:** CPI is a proprietary interface for communications between Campbell Scientific dataloggers and Campbell Scientific CDM peripheral devices. It consists of a physical layer definition and a data protocol. CDM devices are similar to Campbell Scientific SDM devices in concept, but the use of the CPI bus enables higher data-throughput rates and use of longer cables. CDM devices require more power to operate in general than do SDM devices.

address configured in theCDM **Note:** module. Valid range is 1 through 120. Note that the CPIAddress cannot be a variable because the datalogger must know all information for a given CDM module at program compile.(e.g., before the program runs).

Type: Constant integer


# OSFlag (Operating System Trigger)


Variable used to trigger the Operating System send.

Type: Variable


# OSVersion (Operating System Location and Version)


String expression that specifies the drive where the Operating System is stored and the version of the Operating System to send. The drive can be CRD:, USR:, or USB:.

Type: String expression
```
