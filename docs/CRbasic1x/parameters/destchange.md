# DestChange

Variable or array in which to store the change from the previous peak or valley. When a new peak or valley is detected, the change from the previous peak or valley is loaded in the DestChange argument. When a new peak or valley has not yet been reached, 0 will be stored. When Reps are greater than 1, DestChange must be an array that is dimensioned to Reps+1. The last array element [e.g., DestChange(Reps+1)] is used to flag when a new peak or valley is detected in any of the source inputs. The flag element is stored after the changes and is set to -1 (true) when a new peak or valley is detected and set to 0 (false) when none are detected.

Type -- Variable or Array
