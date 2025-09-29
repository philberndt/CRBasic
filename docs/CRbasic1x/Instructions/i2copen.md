# I2COpen (Set up I2C)

The I2COpen instruction sets up the datalogger for Inter-Integrated Circuit (I2C) communications.

**NOTE:** Campbell Scientific documentation has been updated to replace legacy industry terms with modern terminology. Controller-peripheral are now used to describe I2C communications. The I2C controller initiates communications and makes requests of peripheral device(s). Peripheral devices process requests and return an appropriate response.

## Syntax

I2COpen(BeginPort,BitRate)

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

I2C uses two bidirectional open-drain lines, the Serial Data Line (SDA) and Serial Clock Line (SCL) pulled up with resistors. By definition, I2C is a Multi-Controller, Multi-Peripheral communication interface; however the datalogger only supports Single-Controller, Muti-Peripheral configurations. The datalogger provides pull-ups to allow communications, but the pull resistors are not strong enough for high speed communications. If higher speeds are required, external pull-up resistors will be necessary. The datalogger does not support clock stretching , where the peripheral can slow down the clock by holding the SCL line low. However, the BitRate parameter can be used to slow or speed up the clock.

**NOTE:** A prior understanding of the operation and details of the I2C protocol is assumed.

I2COpen must be declared prior to using I2CRead or I2CWrite.

## Parameters

# BeginPort (Begin Port)

Specifies the beginning port used for the two signals for I2C communications. The first port is the clock signal and the next higher port is used for the data signal. Valid options are C1, C3,C5, and C7.

Type: Constant

# BitRate (Bit Rate)

The bit rate, in Hertz, to use for communications.

Type: Constant

**NOTE:** The datalogger's 100k ohm pull-up resistors are too weak for communication at the standard clock frequency of 100kHz. Therefore, to run at faster speeds, an external pull-up resistor should be added. The external resistor can either be pulled to the +5V supply on the terminal blocks or it can be pulled high by another control port that is not being used for the I2C function. The control port (Cx) can be configured to drive at either +5V or +3.3V with the [PortPairConfig()](portpairconfig.md) instruction. The external pull-up resistor should be sized appropriately for the speed and capacitance of the cable. Refer to the following graph to determine the appropriate external pullup resistor size.
