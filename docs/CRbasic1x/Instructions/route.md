# Route (Route to PakBus Datalogger)

The Route function is used to return the neighbor address of (or the route to) a PakBus datalogger.

## Syntax

_variable_ = Route(PakBusAddr)

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

The Route function returns the address of the neighbor (first hop) to the specified PakBus datalogger. If no neighbor is found, a 0 is returned.

If the PakBusAddr parameter is negated, this function will return the COM port or TCP connection handle.

## Parameter

# PakBusAddr (PakBus Address)

ThePakbus

**Note:** PakBus is a proprietary communication protocol developed by Campbell Scientific to facilitate communications between Campbell Scientific devices. Similar in concept to IP (Internet Protocol), PakBus is a packet-switched network protocol with routing capabilities. A registered trademark of Campbell Scientific, Inc.

address of the device that will be contacted as a result of this instruction. Each PakBus device in the network must have a unique address.

If PakBus encryption is enabled in the datalogger, by default the datalogger will encrypt the communications sent using PakBus communication. To disable encryption for one or more Pakbus addresses, use the EncryptExempt instruction. This is useful if a remote device does not support encryption (such as a CR200 or an AVW200).

Type: Integer between 1 and 4094

**NOTE:** By default,Campbell Scientificsoftware uses the following PakBus addresses: LoggerNet 4094, VisualWeather 4094, PC400 4093, PC200W 4092, PConnect/PConnectCE 4091, RTDAQ 4090, Device Configuration Utility 4089, Konect GDS a value in the range of 4000 4050. 4095 is a broadcast address that can be used in a limited number of instructions.
