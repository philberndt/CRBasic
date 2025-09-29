# SDMX50 (SDMX50 Multiplexer)

The SDMX50 instruction is used to switch the SDMX50 multiplexer to the specified channel.

## Syntax

SDMX50(SDMAddress,Channel)

The following program is used to switch to channel 4 of an SDMX50 that has an address of 1.

```
BeginProg
Scan (1,Sec,3,0)
SDMX50(1,4)
NextScan
EndProg
```

## Remarks

This instruction is often used for troubleshooting the SDMX50 multiplexer in TDR applications.

SDMX50 allows individual multiplexer switches to be activated independently of the TDR100 Instruction. It is useful for selecting a particular probe to troubleshoot or to determine the apparent cable length.

Because it is usually easy to hear the multiplexer(s) switch, the SDMX50 instruction is a convenient method to test the addressing and wiring of a level of multiplexers: Program the datalogger to scan every few seconds with the SDM address for the multiplexer(s) and channel 8. The Instruction always starts with channel 1 and switches through the channels to get to the programmed channel. Switching to channel 8 will cause the most prolonged noise.

Remember each multiplexer level has a different SDM Address. Level 1 multiplexers should be set to the address 1 greater than the TDR100, Level 2 multiplexers should be set to the address 2 greater than the TDR100 and Level 3 multiplexers should be set to the address 3 greater than the TDR100. If the SDMX50 multiplexers for a given level are connected and have their addresses set correctly they should all switch at the same time.

## Parameters

# SDMAddress (Address of Device)

Defines the address of the device with which to communicate. ValidSDM

**Note:** Synchronous Device for Measurement. A processor-based peripheral device or sensor that communicates with the datalogger via hardwire over a short distance using a protocol proprietary to Campbell Scientific.

addresses are 0 through 14. Address 15 is reserved for the SDMTrigger instruction.

Some SDM instructions support repetitions. If a Reps parameter is present and it is greater than 1, the data logger will increment the SDM address used in the instruction for each subsequent device with which it communicates.

# Channel

The channel to switch to on the SDMX50 multiplexer. Valid channels are 1 through 8.

Type: Constant
