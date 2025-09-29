# fN1

Determines the lowest frequency that will be eliminated or notched out by the sinc filter. This filter notches out frequencies at integer multiples of fN1by averaging for a time equal to 1/fN1; thus, lower fN1frequencies result in longer measurement times.Set fN1to the lowest frequency that should be filtered out. For example, setting fN1to 60 filters all multiples of 60 and above (60, 120, 180, etc.) but will not effectively filter lower frequencies; for example 30 Hz.Any value between 0.5 Hz and 31.25 kHz can be entered, but common options for filtering noise are:

| Option         | Description                                                       |
| -------------- | ----------------------------------------------------------------- |
| 15000          | Performs a 0.0667 millisecond integration (for fast measurements) |
| 60 (or \_60Hz) | Performs a 16.67 millisecond integration (filters 60 Hz noise)    |
| 50 (or \_50Hz) | Performs a 20 millisecond integration (filters 50 Hz noise)       |

Type: Constant
