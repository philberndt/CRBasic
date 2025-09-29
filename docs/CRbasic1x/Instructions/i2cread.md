# I2CRead (Set up I2C Read)

The I2CRead instruction sets up the datalogger for reading in data via I2C communications.

**NOTE:** Campbell Scientific documentation has been updated to replace legacy industry terms with modern terminology. Controller-peripheral are now used to describe I2C communications. The I2C controller initiates communications and makes requests of peripheral device(s). Peripheral devices process requests and return an appropriate response.

## Syntax

I2CRead(BeginPort,Address,Dest,NumBytes,Option)

The following program configures and reads accelerometer data from an ADLX350 3-axis accelerometer. It is a 3.3V device so C1 and C2 and set up to operate at 3.3V.

The program reads the ID from the device. This requires writing the address to read then reading the data. Example:

```
Dim id_reg as Long = {&H80000000}'This defines the value to send to access the ID register in the ADXL350
I2CWrite(C1,&H53,id_reg,1,&H2)'Start with no stop, Send &H80 to the device, leave the bus active
I2CRead(C1,&H53,tmp,2,&H5)'restart with stop, read the id register
movebytes(id,3,tmp,0,1)'store the value read
```

In order to read accelerometer data, the ADXL350 must be configured. This is done by writing the BWrate register and the pwr_ctrl register. Refer to the ADXL350 for details on the register definitions.

The following lines of CRBASIC configure the ADXL350:

```
Dim pwr_ctrl as Long = {&H2D080000}'Register &H2D is to be written with Data &H08
Dim bw_rate as Long = {&H2C060000}'Register &H2C is to be written with Data &H06
I2CWrite(C1,&H53,bw_rate,2,&H3)'Start and Stop, configure the bwrate register
I2CWrite(C1,&H53,pwr_ctrl,2,&H3)'Start and Stop, enable measurements
```

Here is the complete program:

```
Public Count as Long
Public id as long
Public tmp as Long
Public Dest(2) as Long
Public zero as Long
Dim pwr_ctrl as Long = {&H2D080000}
Dim bw_rate as Long = {&H2C060000}
Dim id_reg as Long = {&H80000000}
Dim Cfg as Long = {&H31000000}
Dim DReg as Long = {&HF2000000}
Public X as Long
Public Y as Long
Public Z as Long

BeginProg

PortPairConfig(C1,2)'Set up C1,C2 as 3.3V
PortPairConfig(C3,2)'Set up C3,C4 as 3.3V (for CS signal)
PortSet(C4,1,1)'Drive the CS signal
I2COpen(C1,1000)
I2CWrite(C1,&H53,id_reg,1,&H2)'Start with no stop
I2CRead(C1,&H53,tmp,2,&H5)'restart with stop, read the id register
movebytes(id,3,tmp,0,1)
I2CWrite(C1,&H53,bw_rate,2,&H3)'configure the bwrate register
I2CWrite(C1,&H53,pwr_ctrl,2,&H3)'enable measurements

Scan(1,sec,0,0)
Count += 1
I2CWrite(C1,&H53,Dreg,1,&H2)'send addr to read
I2CRead(C1,&H53,Dest,6,&H5)'Read X,Y, and Z
movebytes(X,2,Dest,0,2)
movebytes(Y,2,Dest,2,2)
movebytes(Z,2,Dest,4,2)
NextScan
EndProg
```

## Remarks

I2C uses two bidirectional open-drain lines, the Serial Data Line (SDA) and Serial Clock Line (SCL) pulled up with resistors. The datalogger provides pull-ups to allow communications, but the pull resistors are not strong enough for high speed communications. If higher speeds are required, external pull-up resistors will be required.

Note that a prior understanding of the operation and details of the I2C protocol is assumed.

## Parameters

# BeginPort (Begin Port)

Specifies the beginning port used for the two signals for I2C communications. The first port is the clock signal and the next higher port is used for the data signal. Valid options are C1, C3,C5, and C7.

Type: Constant

# Address (Address of I2C Peripheral)

The address of the I2C peripheral device. The read bit is appended to the address and should not be specified in the address.

Note: this is a 7-bit address. To represent the address correctly, rotate the bit pattern to the right by one bit. for example., an I2C address of &HB8 would be &H5C.

Type: Constant

# Dest (Destination)

The variable in which to store the data read from the I2C device.

Type: Variable

# NumBytes (Number of Bytes)

The number of bytes to read from/write to the I2C device.

Type: Variable or constant.

# Option

A bit field parameter used to specify how the I2C transaction starts and ends.

| Option | Description                                                                                                            |
| ------ | ---------------------------------------------------------------------------------------------------------------------- |
| B0     | Stop. Send a stop condition at the end of the transaction                                                              |
| B1     | Start. Send a start condition at the beginning of the transaction. The address will be sent following the start.       |
| B2     | Restart. Send a restart condition at the beginning of the transaction. The address will be sent following the restart. |

If Start or Restart are asserted the address will be sent prior to any data. If the stop is not set, it is assumed that another I2C instruction will occur, and the bus is left in control of the controller.

When all of the bytes are read and a stop is specified, the controller will send a NACK on the last byte. This is in accordance with the I2C specification and allows slave devices to reset their control state machines to a known state.

Examples of the Option parameter include:

- &H02 = Start with no stop at the end

- &H03 = Start, then Stop at the end

- &H05 = Restart, then Stop at the end

Type: Constant
