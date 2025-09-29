# fN1

Determines the lowest frequency that will be eliminated or notched out by the sinc filter. This filter notches out frequencies at integer multiples of fN1by averaging for a time equal to 1/fN1; thus, lower fN1frequencies result in longer measurement times.

| Option | Description                                                                       |
| ------ | --------------------------------------------------------------------------------- |
| 50     | Standard measurement speed; requires 50 msec to filter 50 Hz noise                |
| 60     | Standard measurement speed; requires 50 msec to filter 60 Hz noise                |
| 400    | Medium measurement speed; performs a 6.25 msec integration to filter 400 Hz noise |
| 4000   | Fast measurement speed; performs a 0.5 msec integration to filter 4000 Hz noise   |

Type: Constant
