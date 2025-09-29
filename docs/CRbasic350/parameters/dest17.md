# Dest (Destination)

The variable array in which to store the results of the measurement. Dest must be dimensioned to four to hold the following values:

- **Dest(1)**: Accumulator. Net number of counts from channel A and channel B. Count will increase if Channel A leads Channel B. Count will decrease if Channel B leads Channel A.

- ** Dest(2)**: Net direction. Indicates direction if a change occurs.
  - 1 = A change in pulse count has occurred, where A is leading B since the last stop in pulse count.
  - 0 = No change (default).
  - -1 = A change in pulse count has occurred, where B is leading A since the last stop in pulse count.

- ** Dest(3)**: Number of counts in direction 1 (A leading B) that have occurred since the last time the instruction was executed.

- ** Dest(4)**: Number of counts in direction 2 (B leading A) that have occurred since the last time the instruction was executed.

Type: Variable Array
