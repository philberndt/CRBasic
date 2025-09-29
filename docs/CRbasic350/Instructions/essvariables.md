# ESSVariables (Declare NTCIP ESS Variables)

The ESSVariables instruction is used to automatically declare the 1665 variables the datalogger needs to read from and write to the local NTCIP ESS objects. This instruction should always be used in conjunction with [ESSInitialize](essinitialize.md).

## Syntax

```
ESSVariables Public
```

or

```
ESSVariables Dim
```

(Default is Public if the Public or Dim modifier is not included)

In the example program below, the Dim modifier is used to avoid having the ESS variables appear in the Public table of the datalogger. Two variables, essNtcipSiteDescription and essBatteryStatus, are forced Public by declaring them manually before ESSVariables. ESSInitialize sets the private (read-write) and public (read-only) community strings to "private" and "public", respectively.

```
Public essNtcipSiteDescription As String * 255
Public essBatteryStatus As Long
ESSVariablesDim
Public BattV

BeginProg
ESSInitialize("private,public")
essNtcipSiteDescription = "Main Street"
Scan (1,Sec,0,0)
'Main Program Battery(BattV)
essBatteryStatus = (BattV / 13.5)
NextScan
EndProg
```

## Remarks

The ESSVariables instruction is placed in the declarations portion of the program (before the BeginProg instruction). This instruction automatically declares all the variables required for the datalogger when used in an NTCIP ESS application. Use [ESSInitialize](essinitialize.md) to initialize the variables declared by ESSVariables and thus the local NTCIP ESS objects.

Use the Dim modifier to avoid having the ESS variables appear in the Public table of the datalogger (e.g., ESSVariables Dim). When using the Dim modifier, some of the variables can be forced Public by declaring them manually before ESSVariables (see the Example program).

CRBasic Editor tools can be used as one method for viewing all of the related ESS variables and supported objects. Include ESSVariables (without Dim) and ESSIntialize in your program. Then save and compile the program. Finally, from the CRBasic toolbar, select "Tools" and then "Show Tables". There you will find that ESSVariables and ESSInitialize declare and initialize approximately 1,665 NTCIP ESS related variables.

Additional SNMP variables can be defined using the [SNMPVariable](snmpvariable.md) instruction. When ESSVariables and SNMPVariable are used in the same program, these user-defined OIDs must begin with an identifier of 1.3.6.1.4.1.1207 or greater (the last OID in the datalogger OS structure for ESSVariables is 1.3.6.1.4.1.1206).

In addition to the user defined OIDs, the following OIDs are supported in the datalogger:

1.3.6.1.2.1.1.1 sysDescr

1.3.6.1.2.1.1.2 sysObjectID

1.3.6.1.2.1.1.3 sysUpTime

1.3.6.1.2.1.1.4 sysContact

1.3.6.1.2.1.1.5 sysName

1.3.6.1.2.1.1.6 sysLocation

1.3.6.1.2.1.1.7 sysServices
