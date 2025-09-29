# PakBusClock (PakBus Clock)

When the datalogger program contains a PakBusClock instruction, the datalogger will set its clock to the clock value of a sending datalogger with the specified PakBus address.

## Syntax

PakBusClock(PakBusAddr)

With the following program loaded into the datalogger, it will broadcast its time on the PakBus network. Any datalogger set up to accept clock reports from this datalogger's PakBus address will use the broadcasted report to set its time. Set SendClockReport to True to trigger a broadcast.

```
Public SendClockReport As Boolean

BeginProg
Scan (1,Sec,0,0)
If SendClockReport Then'send it
'broadcast clock report onto local network
ClockReport (ComRS232,0,4095)
SendClockReport = FALSE
EndIf
NextScan
EndProg
```

This program is loaded into the datalogger that should accept clock reports from the broadcast datalogger.

```
'Load this program into the datalogger
'that should accept and use clock reports 'from PakBus address 1.
BeginProg
PakBusClock (1)
Scan (1,Sec,0,0)
NextScan
EndProg
```

## Remarks

The PakBusAddr parameter is the address of the datalogger from which this datalogger will accept a ClockReport. The [ClockReport](clockreport.md) instruction is used in the sending datalogger to send its clock value to this datalogger. PakBusClock needs to be executed only once for the datalogger to accept clock reports from another PakBus address. Thus, it should be placed immediately after the BeginProg instruction, and before the Scan instruction.

Inclusion of the PakBusClock instruction does not preclude clock sets being accepted from software clients like LoggerNet. When the datalogger sets its clock as a result of this instruction, there is potential for time-base instructions to be skipped (such as Scan, DataInterval, or IfTime). This is true of a clock set from any source.

## Parameter

# PakBusAddr (PakBus Address)

ThePakbus

**Note:** PakBus is a proprietary communication protocol developed by Campbell Scientific to facilitate communications between Campbell Scientific devices. Similar in concept to IP (Internet Protocol), PakBus is a packet-switched network protocol with routing capabilities. A registered trademark of Campbell Scientific, Inc.

address of the device that will be contacted as a result of this instruction. Each PakBus device in the network must have a unique address.

If PakBus encryption is enabled in the datalogger, by default the datalogger will encrypt the communications sent using PakBus communication. To disable encryption for one or more Pakbus addresses, use the EncryptExempt instruction. This is useful if a remote device does not support encryption (such as a CR200 or an AVW200).

Type: Integer between 1 and 4094

**NOTE:** By default,Campbell Scientificsoftware uses the following PakBus addresses: LoggerNet 4094, VisualWeather 4094, PC400 4093, PC200W 4092, PConnect/PConnectCE 4091, RTDAQ 4090, Device Configuration Utility 4089, Konect GDS a value in the range of 4000 4050. 4095 is a broadcast address that can be used in a limited number of instructions.
