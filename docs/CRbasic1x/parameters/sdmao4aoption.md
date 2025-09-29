# SDMAO4AOption

Used to set the operating mode for the SDMAO4A.

| Option Code | Description     |
| ----------- | --------------- |
| 0           | Power down      |
| 1           | 5V synchronous  |
| 2           | 5V sequential   |
| 3           | 10V synchronous |
| 4           | 10V sequential  |

In the synchronous mode, all channels are set at the same time. This mode is slower since for large changes in voltage it may take multiple charging cycles to arrive at the final voltage. The steps occur at 5 ms intervals, thus, for a 10V step it may take up to three charge cycles (or 15 ms) to settle to the 16-bit level.

In sequential mode, the channels are set sequentially. The output signal can take from 600 usecs to 1 ms (worst case) to settle to 16-bit resolution with a 10V step change. The four outputs then update 1 ms apart.

Type: Constant
