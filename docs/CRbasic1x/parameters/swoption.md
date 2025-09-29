# Option (Run Mode Option)

An optional parameter that determines whether the instruction will run in the measurement task sequence or the processing task sequence, and also affects whether the program will compile and run in [SequentialMode or PipelineMode](../Instructions/sequentialmodepipeli2.md):

| Option  | Description                                                                                                |
| ------- | ---------------------------------------------------------------------------------------------------------- |
| Omitted | Instruction is run within the measurement task sequence; program will compile in SequentialMode            |
| 0       | Instruction is run within the measurement task sequence; program will attempt to compile in PipelineMode\* |
| 1       | Instruction is run within the processing task sequence; program will attempt to compile in PipelineMode\*  |

\* other programming may force the program into SequentialMode

Running this instruction in the processing task when the program is run inpipeline mode

**Note:** A CRBasic program execution mode wherein instructions are evaluated in groups of like instructions, with a set group prioritization.

can prove to be problematic if you are using the instruction to power a sensor. Because processing tasks can lag behind measurement tasks in PipelineMode, the instruction may be processed by the device after the measurement has already been made. To avoid this scenario, program the datalogger to operate in SequentialMode.

Type: Constant
