# PPPOpen (Open PPP Connection)

The PPPOpen function is used to enable a PPP network through an external modem (excluding theCampbell ScientificCELL2XX-Series) and return the network s IP address(es). To enable/disable PPP connections made with CELL2XX-Series cellular modems (internal or external), use [IPNetPower](ipnetpower.md).

## Syntax

PPPOpen(Option)

or

_variable_ = PPPOpen(Option)

The following example shows the use of the PPPOpen and PPPClose functions when using the datalogger to control power to a cellular modem using the switched 12V (SW12V) terminal on the datalogger. This is useful in applications where the modem uses more power than the datalogger.

In this example, the TimeIsBetween instruction is used to turn on SW12 for 15 minutes every 60 minutes between 9:00 a.m. and 5:00 p.m.

```
'Declare Variables and Units
Public BattV

Public P3Open as String, P3Close

Units BattV=Volts

'Define Data Tables
DataTable(Table2,True,-1)
DataInterval(0,1440,Min,10)
Minimum(1,BattV,FP2,False,False)
EndTable

'Main Program
BeginProg
'Main Scan
Scan(5,Sec,1,0)

'Datalogger Battery Voltage measurement 'BattV'
Battery(BattV)

'SW12 Timed Control
'Turn ON SW12 between 0900 hours and 1700 hours
'for 15 minutes every 60 minutes
If TimeIsBetween(540,1020,1440,Min) And TimeIsBetween(0,15,60,Min) Then

SW12(1)'Turn on SW12
P3Open =PPPOpen
Else
P3Close =PPPClose
SW12(0)'Turn off SW12

EndIf

'Always turn OFF SW12 if battery drops below 11.5 volts
If BattV<11.5 ThenSW12(0)

'Call Data Tables and Store Data
CallTable Table2
NextScan
EndProg
```

## Remarks

If PPP is configured on a datalogger, then at program start up, the datalogger automatically tries to open a PPP session. Hence, PPPOpen is not necessary to start a PPP session. In most cases PPPOpen and [PPPClose](pppclose.md) are used with cellular modems that are configured to work over PPP. PPPOpen and PPPClose allow the user to enable and disable the PPP connection after turning on or before powering off a modem. Without PPPClose, the datalogger will constantly try to poll the serial port that is configured for modem communication in order to re-establish the PPP connection. Hence, significant power savings can be achieved by using PPPClose, which allows the communication port to go to sleep when no communication is occurring. PPPOpen is then used to reopen the port for a connection. PPPOpen and PPPClose can also be used to reduce unnecessary acknowledgement (ACK)-based traffic on cellular networks where PPP server/client communication all happen over the cellular network (not just between the cell modem and the datalogger).

By default, PPPOpen has no parameters and waits up to 30 seconds for the IPv4 address assigned to the peripheral PPP device to be returned.However, beginning with Operating System7, an optional parameter may be used to specify the IP version of the address that should be returned by the function. The optional parameter may be configured as follows:

| Code        | Description                                   |
| ----------- | --------------------------------------------- |
| 0 or Absent | Returns IPv4 address                          |
| 1           | Returns all IPv6 addresses (local and global) |
| 2           | Returns all IPv4 and all IPv6 addresses       |

When multiple addresses are returned, each address is separated by a space. IPv6 addresses are returned in the order they are reported.

**NOTE:** If a variable is used to store the IP address(es) returned by the function, the variable should be declared as typeString

**Note:** A data type used when declaring a variable consisting of alphanumeric characters.

. The variable is not initialized when the instruction is first called; rather, the IP address is written to the variable if a connection is made within 30 seconds of PPPOpen being called. The timeout period for all options is 30 seconds. For option 2, if no IPv4 address is found within 30 seconds, then the datalogger will search one time for IPv6 addresses before timing out. If the PPPOpen function times out with options 0 or 2, an IPv4 address of 0.0.0.0 will be returned. If the Option parameter is configured to return IPv6 addresses (Option 1 or 2) and the function times out, then :: (double colons) are returned.

**Caution:** Program execution will be delayed during PPPOpen negotiation. If PPPOpen does not succeed, or if PPP is in a closed state for any reason, the datalogger will automatically retry connecting, unless PPPClose is executed. The status of the PPP connection can also be monitored with the [IPInfo](ipinfo.md) instruction, which does not have a timeout (in contrast to PPPOpen).

The datalogger is configured for PPP by editing values in its Settings table using the Device Configurator. The settings include the datalogger port to use for PPP, the IP address for the connection, log-in user name and password, the modem dial string to use, and the modem response string.

PPP connections return the following states, which can be viewed by watching IP trace information in the datalogger:

dialing START

dialing OPENED

dialing ECHO_ATH

dialing ECHO_ATH_DELAY

dialing ECHO_ATD

dialing ECHO_CLIENT

dialing ECHO_DELAY

dialing ECHO

dialing OFFLINE

dialing OK_ATH

dialing OK_ATD

dialing OK

dialing CONNECT

dialing DIAL_DONE

**NOTE:** If the dial string for the modem is **CLIENT**, the datalogger will send out the string and wait for ** CLIENTSERVER<CRLF>CONNECT<CRLF>**to come back from the modem. This provides an alternate way to put some Airlink modems (such as the Raven) into PPP mode.
