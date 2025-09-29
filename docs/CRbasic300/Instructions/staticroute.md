# StaticRoute (PakBus Static Route)

The StaticRoute instruction is used to define a static route to a PakBus datalogger.

## Syntax

StaticRoute(ComPort,NeighborAddr,PakBusAddr)

In this example program, the datalogger sends the variable TCTemp to the RemoteData variable in the Public table of another datalogger. Because the packet is being routed via LoggerNet, the StaticRoute instruction is used to establish a known route to the remote datalogger.

```
Public RefTemp, TCTemp, SendResult

StaticRoute(ComRS232,4094,10)

BeginProg

Scan (1,Sec,3,0)
PanelTemp (RefTemp,4000)
TCDiff (TCTemp,1,mv34,1,TypeT,RefTemp,True,0,4000,1.0,0)

If TCTemp > 32 Then
SendVariables(SendResult,ComRS232,4094,10,0000,6000,"Public","RemoteData",TCTemp,1)
endif

NextScan

EndProg
```

## Remarks

The StaticRoute is used in instances where the datalogger does not have a known dynamic route to a remote PakBus node. An example might be where the datalogger is trying to route a PakBus packet through LoggerNet to a remote datalogger, but its only connection to the LoggerNet server is via a telephone modem. In this instance, LoggerNet would send a "good bye" message after each communication with the datalogger, thus, the known route to the remote datalogger would be reset (and unknown).

StaticRoute is a declaration and does nothing at runtime. Multiple StaticRoutes to a device can be defined in a program.

## Parameters

# ComPort (Communications Port)

The communications port that will be used by the instruction. Right-click to display a list. Options vary depending on the instruction.

| Alphanumeric | Description                                  |
| ------------ | -------------------------------------------- |
| ComUSB       | USB port of the datalogger                   |
| ComRS232     | RS232 port of the datalogger                 |
| Com1         | Datalogger control terminals 1 (TX) & 2 (RX) |
| ComRF        | Integrated RF communication                  |

Type: Constant. For all instructions except SerialOpen, this parameter can also be a variable.

When communicating over TCP/IP, use the variable returned by the [TCPOpen](tcpopen.md) function for the ComPort.

# NeighborAddr (Neighbor PakBus Address)

Used to specify the PakBus address of the "neighbor" device that the datalogger can go through to communicate with the remote datalogger.

Type: Integer between 1 and 4094

**NOTE:** By default,Campbell Scientificsoftware uses the following PakBus addresses: LoggerNet 4094, VisualWeather 4094, PC400 4093, PC200W 4092, PConnect/PConnectCE 4091, RTDAQ 4090, Device Configuration Utility 4089.

# PakBusAddr (PakBus Address)

ThePakbus

**Note:** PakBus is a proprietary communication protocol developed by Campbell Scientific to facilitate communications between Campbell Scientific devices. Similar in concept to IP (Internet Protocol), PakBus is a packet-switched network protocol with routing capabilities. A registered trademark of Campbell Scientific, Inc.

address of the device that will be contacted as a result of this instruction. Each PakBus device in the network must have a unique address.

If PakBus encryption is enabled in the datalogger, by default the datalogger will encrypt the communications sent using PakBus communication. To disable encryption for one or more Pakbus addresses, use the EncryptExempt instruction. This is useful if a remote device does not support encryption (such as a CR200 or an AVW200).

Type: Integer between 1 and 4094

**NOTE:** By default,Campbell Scientificsoftware uses the following PakBus addresses: LoggerNet 4094, VisualWeather 4094, PC400 4093, PC200W 4092, PConnect/PConnectCE 4091, RTDAQ 4090, Device Configuration Utility 4089. 4095 is a broadcast address that can be used in a limited number of instructions.
