# CWB100Diagnostics (Return Diagnostics from Wireless Network)

The CWBDiagnostics instruction returns diagnostic information from a wireless network.

## Syntax

CWBDiagnostics(CWBPort,CWSDiag)

## Remarks

This instruction, when executed, returns the results from the last poll of the sensor network (it does not initiate a new poll).

## Parameters

# CWBPort (CWB Terminal)

The CWBPort parameter is used to specify the terminal pair on the datalogger to which the wireless sensor base s Data/A terminal will be connected. Valid options are C1, C3, U1, U3, U5, U7, U9, or U11.

Right-click the parameter to display a drop-down list of valid options.

Type: Constant

# CWSDiag (Instruction Results)

A variable array that holds the results of the instruction. Results are formatted as Longs.

**Array(1)**: Number of sensors discovered by the base.

** Array(2)**: Number of sensors polled by the base.

** Array(3)**: Number of retries the base has performed when polling the sensors.

** Array(4)**: Time to poll all the sensors, in seconds.

** Array(5)**: Network health value. This is a number from 0 to 100, and is the ratio of the actual polled time to the best case poll time for the network, scaled to the difference between the worst and best case times. The value assumes a best case time of 5 seconds per sensor per hop. The maximum number of retries during an attempt is four. Thus, as an example of a network that has 10 single-hop nodes, 5 two-hop nodes, and 4 three-hop nodes, the best case is:

(10\* 5) + (5 \* 10) + (4 \* 15) = 160 seconds

And worst case is:

(10\* 20) + (5\*40) + (4\* 60) = 640 seconds

If the network is polled in 160 seconds or less, the network health would be 100; if it is polled in 640 seconds or more, the network health would be 0. Times falling between these two values would be scaled accordingly.

Type: Variable
