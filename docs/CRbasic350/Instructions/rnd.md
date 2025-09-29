# RND (Random Number)

The RND function is used to generate a random number.

## Syntax

variable =RND

The example uses the RND function to generate two random integer values from 1 to 9. The SecsSince1990() instruction is used to generate the seed value for the Randomize() instruction.

```
'Declare Public Variables
Public SS1990 As Long'Declare as public Long
Public Wild1, Wild2'Declare variables.

'Main Program
BeginProg
'Create Seed.
SS1990=SecsSince1990(Status.TimeStamp(1,1),1)
'Seed random number generator.
Randomize(SS1990)
Scan( 1, Sec, 1, 0 )
Wild1 = Int( 100 *RND+ 1 )'Generate first random value.
Wild2 = Int( 100 *RND+ 1 )'Generate second random value.
NextScan
EndProg
```

## Remarks

The RND function returns a single value less than 1 but greater than or equal to 0.

Unlike other CRBasic dataloggers, with the CR300 the RND function does not have to be initialized with Randomize to create a random sequence of numbers.

To produce random integers in a given range, use this formula:

INT( ( upperbound - lowerbound + 1 ) \* RND + lowerbound )

Here, upperbound is the highest number in the range, and lowerbound is the lowest number in the range.
