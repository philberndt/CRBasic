# Routes (List Known PakBus Routes)

The Routes instruction returns a list of known dynamic routes for aPakBus

**Note:** PakBus is a proprietary communication protocol developed by Campbell Scientific to facilitate communications between Campbell Scientific devices. Similar in concept to IP (Internet Protocol), PakBus is a packet-switched network protocol with routing capabilities. A registered trademark of Campbell Scientific, Inc.

datalogger that has been configured as a router in a PakBus network.

## Syntax

Routes(Dest)

The following example shows the use of the Routes instruction. The destination variable for the PakBus routes is dimensioned to 21, which will accommodate up to 5 PakBus routes for this datalogger. The`Move`instruction is used to initialize the array that will hold the results of the instruction to 0 so that if the number of routes decreases the old values will not remain in the array.

```
Public MyRoute(21)

BeginProg
Scan (1,Sec,3,0)
Move (MyRoute(),21,0,1)'init array to all 0's
Routes(MyRoute())
NextScan
EndProg
```

## Remarks

This instruction stores four values for each known route into the Dest parameter. The four values for each route are: ComPort used for communication, neighbor PakBus address, destination PakBus address, and expected response time (in milliseconds). The list of routes is terminated with a -1. Dest must be an array dimensioned large enough to accommodate the number of routes (4\*routes), plus one for the termination character.

**NOTE:** The destination array is not cleared each time a route is written, so if the number of routes decreases, data from old routes will still remain in the array.
