# ReadIO (Read Port Status)

The ReadIO instruction is used to read the status of one or more digital ports and store the results.

## Syntax

ReadIO(Dest,Mask)

This example uses ReadIO to read port 1 when the temperature TBlk1(1) exceeds 77 degrees, or to read port 2 when the temperature is equal to or below 77 degrees.

```
Public Tblk1( 1 )'Block #1 dimensioned source
Public Tref, TempTest'Declare reference temperature variable
Public Flag( 8 )'Flag dimesnion

DataTable( TEST, Flag( 1 ), 1000 )
DataInterval( 0, 1,Sec, 100 )
Average( 1, Tblk1(), FP2, 0 )
EndTable

BeginProg
Scan( 500,mSec, 0, 0 )
TCDiff( Tblk1(), 1,mv200C,1, TypeT, Tref, 0, 0,15000, 1.8, 32 )

If Tblk1( 1 ) > 77 Then
ReadIO( TempTest, &B1 ) 'Read port 1 if temp > 77
Else
ReadIO( TempTest, &B10 )'Read port 2 if temp < 77
EndIf
CallTable TEST
NextScan
EndProg
```

## Remarks

ReadIO is used to read the status of selected digital control ports. The status of these ports is reflected as a binary number where a high port (+5 V or +3.3V) equals 1 and a low port (0 V) equals 0. For example, if ports C1 and SE2 are high and the rest low, the binary representation is 00000101, or decimal 5.

This instruction is not controlled by the task sequencer, but is controlled by processing. Therefore this instruction can be placed within a conditional statement. It should be remembered that processing can lag multiple scans behind measurements.[See alsoPortGet (Status of Port).](portget.md)

## Parameters

# Dest (Destination)

The Variable in which to store the results of the instruction. Right-click the parameter to display a list of defined variables.

If this instruction has a Repetitions parameter and it is greater than 1, the results are stored in an array with the variable name. The array must be dimensioned large enough to hold all of the values returned from all of the Reps.

Type: Variable or Array

# Mask

The Mask parameter is used to select which of the ports to read or write. It is a binary representation of the ports.

**WriteIO **: If a port position in the mask is set to 1, the datalogger sets the port based on the value for that port in the Source. If a port position in the mask is set to 0 the value in the Source is ignored. Binary numbers are entered into CRBasic by preceding the number with "&B". For example, if the mask is entered as &B110 (leading zeros can be omitted in binary format just as in decimal) and the source is 5 decimal (binary 101) port 3 will be set high and port 2 will be set low. The mask indicates that only 3 and 2 should be set. While the value of the source also has a 1 for port 1, it is ignored because the mask indicates 1 should not be changed.

** ReadIO**: If a port position is set to 1, the datalogger reads the status of the port. If a port position is set to 0 the datalogger ignores the status of the port (the Mask is "anded" with the port status; the "and" operation returns a 1 for a digit if the Mask digit and the port status are both 1, and a 0 if either or both is 0). CRBasic allows the entry of numbers in binary format by preceding the number with "&B". For example, if the Mask is entered as &B100 (leading zeros can be omitted in binary format just as in decimal) and ports 3 and 1 are high, the result of the instruction will be 4 (decimal, binary = 100); if port 3 is low, the result would be 0.

Type: Binary value (preceded by &B) or Integer (from 0 to 255 representing the binary value)
