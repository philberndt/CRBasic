# ExclusiveOption

Optional parameteradded with OS8. Determines if the datalogger will attempt a connection through another IPinterface, if the attempt to route through the specified IPinterface fails. The ExclusiveOption is useful if more than one active interface is available (e.g., PPP and Ethernet) but the specified interface should be used exclusively for this outgoing communication.

- 0 or absent = Allow routing through another active interface

- 1 = Do not allow routing through another active interface

Type: Constant
