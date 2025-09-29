# ESSInitialize (Initialize Environmental Sensor Station)

Execution of ESSInitialize enables SNMP communications, initializes all Environmental Sensor Station (ESS) variables to default values, and sets the private (read-write) and public (read-only) community strings.

## Syntax

ESSInitialize

or

ESSInitialize( "private, public" )

In the example program below, the Dim modifier is used to avoid having the ESS variables appear in the Public table of the datalogger. Two variables, essNtcipSiteDescription and essBatteryStatus, are forced Public by declaring them manually before ESSVariables. ESSInitialize sets the private (read-write) and public (read-only) community strings to "private" and "public", respectively.

```
Public essNtcipSiteDescription As String * 255
Public essBatteryStatus As Long
ESSVariables Dim
Public BattV

BeginProg
ESSInitialize("private,public")
essNtcipSiteDescription = "Main Street"
Scan (1,Sec,0,0)
Battery(BattV)
essBatteryStatus = (BattV / 13.5)
NextScan
EndProg
```

## Remarks

This ESSInitialize instruction should always be used with [ESSVariables](essvariables.md) or with [SNMPVariable](snmpvariable.md). ESSInitialize should be placed directly after the BeginProg instruction so that the variables are initialized only at compile time.

ESSInitialize accepts an optional parameter for specifying the private and public community strings used for SNMP read/write access control. The parameter is a string variable, and the names are comma separated. Make sure to remove any unintended leading or trailing spaces as community names. If community strings are not specified, they will default to "private" and "public".

As examples,

ESSInitialize

The default private (read-write access) and public (read-only access) community strings will be "private" and "public", respectively.

ESSInitialize("red,blue")

Sets the private (read-write access) and public (read-only access) community strings to "red" and "blue", respectively.

SNMP is one of the protocols the datalogger uses in support of NTCIP ESS 1204 over an IP connection. The datalogger listens for SNMP requests on IP port 161 and uses NTCIP 1201.

The datalogger will only respond to SNMP requests after the ESSInitialize command has been executed.
