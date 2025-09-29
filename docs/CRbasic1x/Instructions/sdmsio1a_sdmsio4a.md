# SDM-SIO1A, SDM-SIO4A, and SDM-SIO2R

The SDM-SIO1A acts like a virtual serial port to a CRBasic datalogger. The SDM-SIO4A module is functionally the same as four SDM-SIO1As fitted inside a compact case providing four serial ports. The SDM-SIO4A is used in the same way as the SDM-SIO1A and uses the same CRBasic code in the datalogger. For simplicity, only the SDM-SIO1A is referred to in the text below, though the text applies to both the SDM-SIO1A and the SDM-SIO4A.

The SDM-SIO2R Serial Input/Output Module is designed to allow expansion of the number of serial ports available on a data logger for communicating with intelligent sensors or driving external displays. It also has built-in power pass-through capabilities. Each SDM-SIO2R provides two separate communication channels and four different switched voltages.

The datalogger's serial functions (e.g.,SerialOpen,SerialClose, etc.) are used to control the SDM-SIOx devices. Some of theSerialOpenparameters have special functions when used with SDM-SIOx devices, as outlined below. Refer to the SDM-SIO1A or SDM-SIO2R manuals for complete information.

SerialOpen( ComPort, BaudRate, SerialOpenFormat, TXDelay, BufferSize )

SerialOpen will initialize the datalogger's internal buffers and send a setup command to the SDM-device to switch it out of low power mode and set up the speed and modes of operation. When the SDM-device receives the setup command, it will automatically turn on its serial port and flush its internal buffers.

## SerialOpen Parameters

# ComPort

Used to specify the address of the SDM-SIO1A device.

| SDM address | ComPort Number |
| ----------- | -------------- |
| 0           | 32             |
| 1           | 33             |
| 2           | 34             |
| 3           | 35             |
| 4           | 36             |
| 5           | 37             |
| 6           | 38             |
| 7           | 39             |
| 8           | 40             |
| 9           | 41             |
| 10          | 42             |
| 11          | 43             |
| 12          | 44             |
| 13          | 45             |
| 14          | 46             |

Address 15 (Com port 47) is reserved as the SDM broadcast address. If 15 is used in this instruction, the SDM address will be set internally to 0 (32). The SDM will not respond to an SDM broadcast command.

# BaudRate

Used to set up the SDM-SIOx device baud rate as you would with any RS232 interface. However, SDM-SIOx devices do not support automatic baud rate recognition. Setting BaudRate to a negative number will set the automatic flow control system (RTS/CTS). SeeUsing RTS/CTS and Automatic Handshakingin the SDM-SIO Manual for details.

For the **SDM-SIO2R** in **RS-485 Full Duplex mode**, if a negative baud rate is specified, the client device continuously drives the transmit line. With a positive baud rate, the client lets the transmit line float between transmissions to conserve power.

# SerialOpenFormat

Used to set the format for the serial data.

SerialOpen format codes for RS-232 communications

| Code                                                                                                                 | Parity   | No. stop bits | No. data bits |
| -------------------------------------------------------------------------------------------------------------------- | -------- | ------------- | ------------- |
| 0                                                                                                                    | None     | 1             | 8             |
| 1                                                                                                                    | Odd      | 1             | 8             |
| 2                                                                                                                    | Even     | 1             | 8             |
| 3                                                                                                                    | None     | 1             | 8             |
| 4                                                                                                                    | Not Used |
| 5                                                                                                                    | Odd      | 2             | 8             |
| 6                                                                                                                    | Even     | 2             | 8             |
| 7                                                                                                                    | None     | 2             | 8             |
| 8                                                                                                                    | Not Used |
| 9                                                                                                                    | Odd      | 1             | 7             |
| 10                                                                                                                   | Even     | 1             | 7             |
| 111                                                                                                                  | None     | 1             | 7             |
| 12                                                                                                                   | Not Used |
| 13                                                                                                                   | Odd      | 2             | 7             |
| 14                                                                                                                   | Even     | 2             | 7             |
| 15                                                                                                                   | None     | 2             | 7             |
| 1This mode is only supported if there is at least a one-bit delay between characters received by the SDM-SIO device. |          |               |               |

SerialOpen format codes for RS-485 or RS-422 full duplex communications

| Code                                                                                                                 | Parity   | No. stop bits | No. data bits |
| -------------------------------------------------------------------------------------------------------------------- | -------- | ------------- | ------------- |
| 16                                                                                                                   | None     | 1             | 8             |
| 17                                                                                                                   | Odd      | 1             | 8             |
| 18                                                                                                                   | Even     | 1             | 8             |
| 19                                                                                                                   | None     | 1             | 8             |
| 20                                                                                                                   | Not Used |
| 21                                                                                                                   | Odd      | 2             | 8             |
| 22                                                                                                                   | Even     | 2             | 8             |
| 23                                                                                                                   | None     | 2             | 8             |
| 24                                                                                                                   | Not Used |
| 25                                                                                                                   | Odd      | 1             | 7             |
| 26                                                                                                                   | Even     | 1             | 7             |
| 271                                                                                                                  | None     | 1             | 7             |
| 28                                                                                                                   | Not Used |
| 29                                                                                                                   | Odd      | 2             | 7             |
| 30                                                                                                                   | Even     | 2             | 7             |
| 31                                                                                                                   | None     | 2             | 7             |
| 1This mode is only supported if there is at least a one-bit delay between characters received by the SDM-SIO device. |          |               |               |

SerialOpen format codes for RS-485 or RS-422 half duplex communications

| Code                                                                                                                 | Parity   | No. stop bits | No. data bits |
| -------------------------------------------------------------------------------------------------------------------- | -------- | ------------- | ------------- |
| 48                                                                                                                   | None     | 1             | 8             |
| 49                                                                                                                   | Odd      | 1             | 8             |
| 50                                                                                                                   | Even     | 1             | 8             |
| 51                                                                                                                   | None     | 1             | 8             |
| 52                                                                                                                   | Not Used |
| 53                                                                                                                   | Odd      | 2             | 8             |
| 54                                                                                                                   | Even     | 2             | 8             |
| 55                                                                                                                   | None     | 2             | 8             |
| 56                                                                                                                   | Not Used |
| 57                                                                                                                   | Odd      | 1             | 7             |
| 58                                                                                                                   | Even     | 1             | 7             |
| 591                                                                                                                  | None     | 1             | 7             |
| 60                                                                                                                   | Not Used |
| 61                                                                                                                   | Odd      | 2             | 7             |
| 62                                                                                                                   | Even     | 2             | 7             |
| 63                                                                                                                   | None     | 2             | 7             |
| 1This mode is only supported if there is at least a one-bit delay between characters received by the SDM-SIO device. |          |               |               |

SerialOpen format codes for RS-232 receive only mode communications

| Code                                                                                                                 | Parity   | No. stop bits | No. data bits |
| -------------------------------------------------------------------------------------------------------------------- | -------- | ------------- | ------------- | --------------------------------------------------------------------------------------------------- |
| 64                                                                                                                   | None     | 1             | 8             |
| 65                                                                                                                   | Odd      | 1             | 8             |
| 66                                                                                                                   | Even     | 1             | 8             |
| 67                                                                                                                   | None     | 1             | 8             |
| 68                                                                                                                   | Not Used |
| 69                                                                                                                   | Odd      | 2             | 8             |
| 70                                                                                                                   | Even     | 2             | 8             |
| 71                                                                                                                   | None     | 2             | 8             |
| 72                                                                                                                   | Not Used |
| 73                                                                                                                   | Odd      | 1             | 7             |
| 74                                                                                                                   | Even     | 1             | 7             |
| 751                                                                                                                  | None     | 1             | 7             |
| 76                                                                                                                   | Not Used |
| 77                                                                                                                   | Odd      | 2             | 7             |
| 78                                                                                                                   | Even     | 2             | 7             |
| 79                                                                                                                   | None     | 2             | 7             |
| 1This mode is only supported if there is at least a one-bit delay between characters received by the SDM-SIO device. |          |               |               | ** TIP:** RS-422 protocol is similar to RS-485. Most RS-422 sensors will work with RS-485 protocol. |

# TxDelay

No special function.

# BufferSize

Sets the datalogger's internal buffer size. The SDM-SIO1A has an internal 2047 byte buffer that stores information as it is received from the sensor. The datalogger's buffer can be set to this size. To ensure that no data is lost, the data should be retrieved before the internal buffers are full. This can be achieved using the SerialIn and SerialinBlock functions.

### Other Instructions

```
SerialClose
```

( ComPort )

The datalogger will send a command to shut down the SDM-SIOx serial port. SDM communications will take place as normal, but any data coming into the SDM on the RS-232/RS-485/RS-422 interface will be lost.

This is the lowest possible power mode. For optimum power efficiency the SDM should be kept in this mode as much as possible.

SerialFlush( ComPort )

The datalogger will send a command to clear the SDM's receive buffer. For SDM-SIOAs with firmware versions below 3 or SDM-SIO2Rs with firmware versions below 2, the transmit buffer will also be cleared.

```
SerialIn
```

( Dest, ComPort, TimeOut, TerminationChar, MaxNumChars )

The datalogger will query the SDM to determine how much data it has to send. If the SDM has data, the datalogger will then send a command to retrieve the data. The data will be transferred to the datalogger's internal buffer and processed as normal.

If TerminationChar or MaxNumChars is not met and TimeOut is not reached, the datalogger will keep polling the SDM at regular intervals (approximately every 20 ms or as time allows).

```
SerialOut
```

( ComPort, OutString, WaitString, NumberTries, TimeOut )

This function will send the data to the SDM. If data is expected back (WaitString setting) it will then act like SerialIn and check for and collect data, repeating this every 20 ms until the TimeOut is met.

```
SerialInBlock
```

( ComPort, Dest, MaxNumberBytes )

The datalogger will send a command to check for and collect data. It will only get data up to the MaxNumberBytes with each attempt. Implicitly, this means MaxNumberBytes cannot be >2047 for the SIO1A.

```
SerialOutBlock
```

( ComPort, Expression, NumberBytes )

When this function is executed, the datalogger will send data to the SDM. It can also be used to manually set the outgoing handshake line. To do this, set NumberBytes to 0 and set Expression to 0 to turn off the port or set Expression to a non-zero number to turn on the port.

```
SerialInChk
```

( ComPort )

This function returns a 13-bit value representing the number of bytes available within the SDM's receive buffer. This value can be anywhere from 0 to 2047.

```
SerialInRecord
```

( COMPort, Dest, BeginWord, NBytes, EndWord, NbytesReturned, SerialInRecOption )

SerialInRecord can be used to read a record from the datalogger's serial buffer. The record can be the current record or a buffered record a certain number of records back from the current one (specified by SerialInRecOption).
