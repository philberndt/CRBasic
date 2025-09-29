# Address (Address of I2C Peripheral)

The address of the I2C peripheral device. The read bit is appended to the address and should not be specified in the address.

Note: this is a 7-bit address. To represent the address correctly, rotate the bit pattern to the right by one bit. for example., an I2C address of &HB8 would be &H5C.

Type: Constant
