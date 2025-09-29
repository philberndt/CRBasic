# DiffChan (Differential Channel)

Specifies thedifferential channelon which to make the first measurement. If the Reps parameter is greater than 1, additional measurements will be made on sequential channels.

If the DiffChan number is entered as a negative value, all Reps are performed on the same channel (burst measurement). TheCR1000Xburst measurement samplingfrequencyis determined by the fN1(first notch frequency) parameterwhich can go up to a maximum of 31250 Hz (minimum sample interval of 32 uS). The SettlingTime parameter is used to delay once prior to beginning the burst. The total time prior to beginning the burst is the SettlingTime plus the ADC flush time (which is 450 uS). The sample interval resolution is 1/31250 Hz. The specified notch frequency will use the nearest multiple of (1/31250 Hz) to get as close to the specified frequency as possible.

| Code | Description                           |
| ---- | ------------------------------------- |
| 1    | Differential Channel 1 (SE 1 and 2)   |
| 2    | Differential Channel 2 (SE 3 and 4)   |
| 3    | Differential Channel 3 (SE 5 and 6)   |
| 4    | Differential Channel 4 (SE 7 and 8)   |
| 5    | Differential Channel 5 (SE 9 and 10)  |
| 6    | Differential Channel 6 (SE 11 and 12) |
| 7    | Differential Channel 7 (SE 13 and 14) |
| 8    | Differential Channel 8 (SE 15 and 16) |

Type: Constant
