# CommsMode

This optional parameter is left blank when used with the SDM-SIO1A or SDM-SIO4A. The CommsMode specifies the configuration of the datalogger's control port used by this instruction. The ports can also be configured using Device Configuration Utility. Configuring the ports using the SerialOpen instruction will change the setting if it was previously set by other means.

| Code | Description                                                                                                  |
| ---- | ------------------------------------------------------------------------------------------------------------ |
| 0    | Configures the port as RS-232, 5V voltage levels (All ports)                                                 |
| 1    | Configures the port as TTL, 5V and 0V voltage levels (Com1, ComC1_Tx, ComC1_Rx, ComC2_Tx, and ComC2_Rx Only) |
| 2    | Not supported in CR350                                                                                       |
| 3    | Configures the port as RS-485 half-duplex, PakBus communication (Com2 and Com3 only)                         |
| 4    | Configures the port as RS-485 half-duplex, transparent (Com2 and Com3 only)                                  |
| 5    | Configures the port as RS-485 full-duplex(Com2 only, requires Com3 as well)                                  |
