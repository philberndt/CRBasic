# PortsConfig (Configure Ports)

The PortsConfig instruction is used to configure one or more digital ports as either input or output.

## Syntax

PortsConfig(Mask,Function)

The following short program configures ports C8, C5, and C2 as output. Even though the output code is used for all eight ports, the mask prevents ports C7, C6, C4, C3 and C1 from being affected by the instruction

```
BeginProg
PortsConfig(&B10010010,&B11111111)
Scan (1,Sec,3,0)
'other code
NextScan
EndProg
```

## Remarks

By default, ports are configured as input. The PortsConfig instruction may be needed if a port is configured as output by a [WriteIO](writeio.md) or [PortSet](portset.md) instruction and then subsequently needs to function as an input.

Typically, this instruction is executed only once in a program, by placing it between the BeginProg and Scan instructions.

## Parameters

# Mask

Used to select which ports will be affected by this instruction. It is a binary representation of the portsas follows:

|         |     |     |     |     |     |     |     |     |
| ------- | --- | --- | --- | --- | --- | --- | --- | --- |
| Bit No. | 8   | 7   | 6   | 5   | 4   | 3   | 2   | 1   |
| Port    | C8  | C7  | C6  | C5  | C4  | C3  | C2  | C1  |

If a port position is set to 1, the datalogger configures that port. Binary numbers are entered by preceding the number with "&B". Leading zeros can be omitted. As an example, if &B110 is entered for this parameter, ports at bit no 3 and 2 will be configured, based on the Function parameter.

Type: Binary value (preceded by &B) or Integer (from 0 to 255 representing the binary value)

# Function

Used to configure the port. A binary value is entered to set each port location. 0 configures the port for input; 1 configures the port for output. Using the mask &B110, if the Function parameter is set to &B110, ports at bit no 3 and 2 will be configured for output (port at bit no 1 uses the code for input, but it is not affected because of the mask).

Type: Binary value (preceded by &B) or Integer (from 0 to 255 representing the binary value)
