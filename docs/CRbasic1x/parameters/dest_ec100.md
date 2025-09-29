# Destination

The input variable name in which to store the data from the EC150 or EC155. The length of the input variable array will depend on the selected command. A value of -99999 will be loaded into Dest(1) if a signature error on SDM data occurs.

| Command | Input Variable Length |
| ------- | --------------------- |
| 0       | 8                     |
| 1       | 12                    |
| 2       | 13                    |

Type: Array
