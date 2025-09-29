# NumVals (Number of Historical Values)

The number of historical values (time series) of the field to output. For example, if the values of the field through time were (oldest to newest):

10.1,10.2,10.3,10.4,10.5,10.6,10.7

Then NumVals = 4 with Decimation =1 would give:

10.4, 10.5,10.6,10.7

**NOTE:** The Newest_First parameter of [GOESTable](../Instructions/goestable.md) determines whether newest or oldest data are output first.

Beginning with OS10, if NumVals is 0, new output is appended to the output table in the order set by the Fields_Scan_Order parameter of the [GOESTable](../Instructions/goestable.md) instruction. For example:

| NumVals | Fields_Scan_Order | Output Appended To: |
| ------- | ----------------- | ------------------- |
| 0       | False             | End of last line    |
| 0       | True              | Separate line       |

Type: Constant or Variable
