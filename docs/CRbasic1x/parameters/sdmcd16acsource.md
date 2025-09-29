# Source

An array (dimensioned as Float, Long, or Boolean) which holds the values that will be sent to the SDM-CD16AC to enable/disable its ports. An SDM-CD16AC has 16 ports; therefore, in most instances the source array should be dimensioned to 16 times the number of Repetitions (the number of SDM-CD16AC devices to be controlled). As an example, with the array CDCtrl(32), the value held in CDCtrl(1) will be sent to port 1, the value held in CDCtrl(2) will be sent to port 2, etc. The value held in CDCtrl(32) would be sent to port 16 on the second SDM-CD16AC.

If the Source parameter is defined as a Long variable, but it is dimensioned less than 16 \* Reps, Source will act as a binary control for the instruction whose bits 0..15 will specify control ports 1..16, respectively. In this instance, Source(1) will be used for the first rep, Source(2) will be used for the second, and so on.

Type: Variable defined as Long, Float, or Boolean
