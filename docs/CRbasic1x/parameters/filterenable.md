# FilterEnable

If the FilterEnable parameter is set to 1 (enabled), 50 Hz and 60 Hz frequencies will be eliminated or notched out simultaneously by the sync filter. Note that when the filter is enabled, the fastest that measurements can be made is 1 Hz for all 20 channels. In contrast, if the FilterEnable parameter is set to 0 (disabled), the TEMP120 can measure all 20 channels at speeds up to 10 Hz.

| Option | Description                           |
| ------ | ------------------------------------- |
| 0      | Filter disabled for fast measurements |
| 1      | Filter 50/60 Hz noise (AC Power)      |

Type: Constant
