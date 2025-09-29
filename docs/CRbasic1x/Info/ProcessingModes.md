# Processing Modes

Unless included in one of the lists below, instructions are handled by the Processing Task Sequencer. For measurement instructions, the measurement itself is handled in the Measurement Task Sequencer, but processing of those measurements (i.e., applying multipliers and offsets) is handled in the Processing Task Sequencer.

## Instructions Handled by Measurement Task Sequencer

- AM25T

- Battery

- BrFull

- BrFull6W

- BrHalf

- BrHalf3W

- BrHalf4W

- CS616

- ExciteV

- PanelTemp

- PortGet

- PortSet

- PulsePort

- SerialInBlock

- SerialInRecord

- TCDiff

- TCSe

- Therm107

- Therm108

- Therm109

- VoltDiff

- VoltSe

## Instructions Handled by Digital Task Sequencer

- CS7500

- CSAT3

- SDMAO4

- SDMCAN

- SDMINT8

- SDMSW8A

- SDMTrigger

- SDMX50

- SerialInRecord

- SerialOutBlock

- TDR100

**Notes**:

Even though measurement instructions are handled by the Measurement Task Sequencer, the processing of those measurements (i.e., applying multipliers and offsets) is handled by the Processing Task Sequencer.

The Digital Task Sequencer handles SDM instructions. It runs in parallel with the Measurement Task Sequencer. Two other SDM instructions are handled by the processing task sequencer: SDMSIO4 and SDMIO16.
