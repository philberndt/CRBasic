# Attribute

A numeric code to determine what should happen to the file called by the RunProgram instruction. The Attribute codes are actually a bit field. The codes are as follows:

| Bit   | Decimal | Description     |
| ----- | ------- | --------------- |
| bit 1 | 2       | Run on power up |
| bit 2 | 4       | Run now         |

A program marked as "Run on power up" can be disabled when power is first applied to the datalogger by pressing and holding the DEL key.

Type: Constant
