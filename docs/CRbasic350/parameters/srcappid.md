# SrcAppID (Source Device ID)

An application ID number that the user assigns to the source device, which will be used to identify the application in thePakBus

**Note:** PakBus is a proprietary communication protocol developed by Campbell Scientific to facilitate communications between Campbell Scientific devices. Similar in concept to IP (Internet Protocol), PakBus is a packet-switched network protocol with routing capabilities. A registered trademark of Campbell Scientific, Inc.

network. The number can be any value from 0 to 65535.

**NOTE:** Application IDs 0 through 999 are reserved for common applications (for instance, 502 is reserved forModbus

**Note:** Communication protocol published by Modicon in 1979 for use in programmable logic controllers (PLCs).

protocol). Therefore, a user application ID should be in the range of 1000 to 65535.

Type: Integer
