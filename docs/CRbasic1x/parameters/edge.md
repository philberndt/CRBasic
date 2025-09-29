# Edge

The Edge parameter is a digit (0 or 1) that represents each of the ports, from left to right: C8 .C1. If a 1 is entered for a port, the transition will be counted on the rising edge (from <1.5V to >3.5V). If a 0 is entered, the transition will be counted on the falling edge (from >3.5V to <1.5V). For example, getting a rising edge on C8 starting at C1 would be done with: TimerInput(Dest,C1,10000000,20000000,0,usec).

| Digit | Edge    |
| ----- | ------- |
| 0     | falling |
| 1     | rising  |

Type: Constant
