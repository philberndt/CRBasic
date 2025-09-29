# BeginPort

The beginning port used for the three signals for SPI communications. The first port is the SPI clock signal. The next higher port from the clock port is the Controller Out Peripheral In (COPI) signal, and the next higher port is Controller In Peripheral Out (CIPO). BeginPort can be C1or C5. If an additional chip select (CS) signal is required, it can be managed with [PortSet](../Instructions/portset.md) instructions surrounding the SPI accesses.

Type: Constant
