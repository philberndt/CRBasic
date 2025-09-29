# SEChan (Singe-Ended Channel)

The single-ended channel number on which to make the first measurement. When Reps are used, subsequent measurements are made on sequential channels.

| Code | Description |
| ---- | ----------- |
| 1    | SE1         |
| 2    | SE2         |

If the SEChan number is entered as a negative value, all Reps will be performed on the same channel (burst measurement). The CR300 burst measurement sampling rate is determined by the fN1(first notch frequency) parameter.

Type: Constant
