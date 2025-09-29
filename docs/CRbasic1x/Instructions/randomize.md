# Randomize

The Randomize instruction initializes the random-number generator. (The random-number generator is called by using the RND function.)

## Syntax

Randomize( number )

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
Wild1 = Int( 100 * RND + 1 )'Generate first random value.
Wild2 = Int( 100 * RND + 1 )'Generate second random value.
NextScan
EndProg
```

## Remarks

The Number argument is used to initialize the random-number generator by giving it a new seed value. Number can be any valid numericexpression.If Randomize is not used, the RND function returns the same sequence of random numbers every time the program is run. To have the program generate a different random-number sequence each time it is run, use the Randomize statement with the Number argument assigned to a changing variable or expression to initialize the random-number generator with a different value before RND is called.
