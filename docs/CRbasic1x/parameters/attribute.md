# Attribute

A numeric code to determine what should happen to the file affected by the FileManage instruction. Attribute codes are a bit field. The codes are as follows:

| Bit        | Decimal | Description            |
| ---------- | ------- | ---------------------- |
| bit 0      | 1       | Program not active     |
| bit 1      | 2       | Run on power up        |
| bit 2      | 4       | Run now                |
| bits 1 & 2 | 6       | Run now/Run on powerup |
| bit 3      | 8       | Delete                 |
| bit 4      | 16      | Delete all             |
| bit 5      | 32      | Hide                   |

Setting a file's attributes to **Hide** makes it inaccessible using communications or the keyboard, but it can still be set as **Run Now** or **Run on Power Up**.

A program marked as ** Run on Power Up**can be disabled when power is first applied to the datalogger by pressing and holding the DEL key on the (optional) CR1000KD.

Type: Constant
