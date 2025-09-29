# SDI12Watch (Watch or "Sniff" SDI-12 Measurements)

The SDI12Watch instruction is used to watch or sniff SDI-12 measurements that are made on another datalogger.

## Syntax

_Result_ = SDI12Watch(Destination,SDIPort,SDIAddress,"SDICommand")

```
'This program uses SDI12Watch() to monitor SDI-12 measurements on a separate datalogger.
'The separate datalogger is measuring two CS215 sensors on terminal C1.
'TheCR350running SDI12Watch() is connected to the first datalogger with two lead wires:

'Wiring
'Datalogger1CR350
'----------- -------
' C1 C2
' G G

'Note that I! command must be written to a string

'M! returns Temp and RH from each CS215

Public SDI12_Sniff(4), Sniff_I(2) As string *50

BeginProg
Scan (60,Sec,0,0)
SDI12Watch(SDI12_Sniff(1),C2,"A","M!")'sniff M! measurement for address A
SDI12Watch(SDI12_Sniff(3),C2,"9","M!")'sniff M! measurement on address 9
SDI12Watch(Sniff_I(1),C2,"A","I!")'sniff identity of address A
SDI12Watch(Sniff_I(2),C2,"9","I!")'sniff identity of address 9
NextScan
EndProg
```

## Remarks

The SDI12Watch instruction allows theCR350to read SDI-12 sensor data from another datalogger. A typical application is to sniff SDI-12 data from another manufacturer s datalogger and sensor. TheCR350is connected to the other datalogger s SDI-12 measurement port via a lead wire to C1 or C2 of theCR350. TheCR350should also be electrically grounded to the other datalogger. The sameCR350port can be used for multiple SDI12Watch instructions.

Note that if any other SDI-12 instruction is called on the sameCR350port, SDI12Watch will pause until the SDI12Watch instruction is called again.

## Parameters

# Result

The Result variable indicates if the instruction succeeded. Possible result codes and their descriptions:

| Code    | Description                                                         |
| ------- | ------------------------------------------------------------------- |
| -1      | Success                                                             |
| -2      | Success missed 1 SDI-12 command not calling SDI12Watch fast enough  |
| -3      | Success missed 2 SDI-12 commands not calling SDI12Watch fast enough |
| 0       | Fail                                                                |
| 1...xxx | Fail xxx seconds since last successful SDI-12 command               |

# Destination

Variable or variable array in which to store the SDI-12 data. Destination must have enough elements to store all of the data that is returned by the SDI-12 sensor on the other datalogger.

# SDIPort (SDI-12 Port)

TheCR350control port that is wired to the SDI-12 measurement port of the other datalogger.

| Code | Description    |
| ---- | -------------- |
| C1   | Control port 1 |
| C2   | Control port 2 |

# Address (SDI-12 Sensor Address)

The address of the SDI-12 sensor that is read by the other datalogger. Valid addresses are 0 through 9, A through Z, and a through z. Alphabetical characters should be enclosed in quotes (for example, "A").

# SDICommand (SDI-12 Command)

Specifies the command-string data to read from the other datalogger. The command should be enclosed in quotes. Multiple SDI12Watch instructions can be used to retrieve data from different SDI-12 commands.

| Command   | Description                                                    |
| --------- | -------------------------------------------------------------- |
| ?!        | Address query                                                  |
| An!       | Change address (where n = address)                             |
| C!        | Initiate concurrent measurements                               |
| CC!       | Initiate concurrent measurements (with checksum)               |
| M!        | Initiate measurements                                          |
| MC!       | Initiate measurements (with checksum)                          |
| M1! - M9! | Additional measurement commands specified by the SDI-12 sensor |
| RC!       | Continuous measurement (with checksum)                         |
| R0! - R9! | Continuous measurement commands                                |
| V!        | Initiate verify sequence                                       |
