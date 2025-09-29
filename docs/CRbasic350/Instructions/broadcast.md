# Broadcast

The Broadcast instruction sends a broadcast message out to a PakBus network.

## Syntax

Broadcast(ComPort,Message)

In following program code, a Hello message (14) is broadcast to a PakBus network, followed by a Route request for PakBus address 5. The neighbor address (first hop) to the device is returned if the route request is successful.

```
Public RtAddr5

BeginProg
Scan (1,Sec,3,0)

Broadcast(ComRS232,14)
RtAddr5 =Route(5)

NextScan
EndProg
```

## Remarks

An error is returned if something other than a valid message is sent. Valid messages following.

## Parameters

# ComPort (Communications Port)

The communications port that will be used by the instruction. Right-click to display a list. Options vary depending on the instruction.

| Alphanumeric | Description                                  |
| ------------ | -------------------------------------------- |
| ComUSB       | USB port of the datalogger                   |
| ComRS232     | RS232 port of the datalogger                 |
| Com1         | Datalogger control terminals 1 (TX) & 2 (RX) |
| Com2         | COM2 port of the datalogger                  |
| Com3         | COM3 port of the datalogger                  |
| ComRF        | Integrated RF communication                  |

Type: Constant. For all instructions except SerialOpen, this parameter can also be a variable.

# Message

The code for the message that should be broadcast to thePakBus

**Note:** PakBus is a proprietary communication protocol developed by Campbell Scientific to facilitate communications between Campbell Scientific devices. Similar in concept to IP (Internet Protocol), PakBus is a packet-switched network protocol with routing capabilities. A registered trademark of Campbell Scientific, Inc.

network. Right click the parameter for a list of valid options.

| Code | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| ---- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0    | Beacon **Note:** A signal broadcasted to other devices in a PakBus network to identify "neighbor" devices. A beacon in a PakBus network ensures that all devices in the network are aware of other devices that are viable. If configured to do so, a clock-set command may be transmitted with the beacon. This function can be used to synchronize the clocks of devices within the PakBus network. . When a Beacon is transmitted, all PakBus devices within range will respond and become a neighbor to the beaconing datalogger. The only exception is if a device in range has a neighbor filter that prevents it from responding. |
| 12   | Reset routing tables. Resets the routing tables of the dataloggers in the PakBus network.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| 13   | GoodBye. Removes the datalogger from the neighbor list of other devices in the network.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| 14   | Hello request. A Hello request sets up other devices as neighbors to the datalogger sending the message. Routing information, hop metrics, communication verification intervals, and router indication are exchanged in a Hello request.                                                                                                                                                                                                                                                                                                                                                                                                 |

Type: Variable
