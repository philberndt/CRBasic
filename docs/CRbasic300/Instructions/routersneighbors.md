# RoutersNeighbors (List PakBus Routers and Neighbors)

The RoutersNeighbors instruction is used to return a list of all PakBus routers and their neighbors known to the datalogger.

## Syntax

RoutersNeighbors( DestArray(MaxRouters,MaxNeighbors+1) )

Following is a simple program showing the use of the RoutersNeighbors instruction, along with an example of the returned arrays.

In the example network, the LoggerNet server is PakBus address 4084; the PakBus ports of this server are bridged so that communication can occur between the routers. All devices but PakBus ID 1086 are set up as routers. (PakBus ID 1109 is a non-datalogger router).

```
Const MaxRouters=3
Const MaxNeighbors=5
Public DestArray(MaxRouters,MaxNeighbors) As Long

BeginProg
Scan (1,Sec,3,0)
RoutersNeighbors(DestArray())
NextScan
EndProg
```

The values returned are as follows:

| Array          | Return | Description                                                     |
| -------------- | ------ | --------------------------------------------------------------- |
| DestArray(1,1) | 4084   | Router (LoggerNet server)                                       |
| DestArray(1,2) | 1010   | First neighbor (CR1000)                                         |
| DestArray(1,3) | 1109   | Second neighbor (non-datalogger router)                         |
| DestArray(1,4) | 3000   | Third neighbor (CR3000)                                         |
| DestArray(1,5) | 0      | No more neighbors                                               |
| DestArray(2,1) | 1109   | Router, non-datalogger                                          |
| DestArray(2,2) | 4084   | First neighbor (LN server)                                      |
| DestArray(2,3) | 1086   | Second neighbor (CR800)                                         |
| DestArray(2,4) | 0      | No more neighbors                                               |
| DestArray(2,5) | 0      | No more neighbors                                               |
| DestArray(3,1) | 3000   | Router (CR3000 set up as router, but with no leaf node devices) |
| DestArray(3,2) | 4084   | (LN server)                                                     |
| DestArray(3,3) | 0      | No more neighbors                                               |
| DestArray(3,4) | 0      | No more neighbors                                               |
| DestArray(3,5) | 0      | No more neighbors                                               |

## Remarks

The DestArray must be a two-dimensional array defined as a long. The first dimension should be sized to the maximum number of routers expected and the second dimension should be sized to the maximum number of neighbors expected plus one.

The return for each array will include:

- **(X, 1)**= PakBus ID of router

- **(X, 2)**= PakBus ID of router's first neighbor

- **(X, 3)**= PakBus ID of router's second neighbor

- **(X, N)**= PakBus ID of router's Nth neighbor

0 is returned for any array greater than the number of routers and/or neighbors.
