# DiffChan (Differential Channel)

Specifies thedifferential channelon which to make the first measurement. If the Reps parameter is greater than 1, additional measurements will be made on sequential channels.

If the DiffChan number is entered as a negative value, all Reps are performed on the same channel (burst measurement). TheCR300burst measurement samplingrateis determined by the fN1(first notch frequency) parameter.

| fN1 (Hz) | Burst Sampling Rate                   |
| -------- | ------------------------------------- |
| 50 or 60 | 50 mSec (or 20 samples per second)    |
| 400      | 6.25 mSec (or 160 samples per second) |
| 4000     | 0.5 mSec (or 2000 samples per second) |

Type: Constant
