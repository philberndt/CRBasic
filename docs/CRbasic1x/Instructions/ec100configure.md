# EC100Configure (Set/Get EC100/EC150 Settings)

The EC100Configure instruction provides a generic get/set settings interface to configure the EC150 or EC155.

## Syntax

EC100Configure(EC100Result,SDMAddress,EC100ConfigCmd,DestSource)

The following program is an example of collecting SDM data from an EC150. The EC100Configure instruction is used to get the baud rate setting for the EC150. This result will be output in data table named ts_data.

```
Public sonic_irga(12)
Public configresult, ec_baudrate
Alias sonic_irga(1) = Ux
Alias sonic_irga(2) = Uy
Alias sonic_irga(3) = Uz
Alias sonic_irga(4) = Ts
Alias sonic_irga(5) = diag_sonic
Alias sonic_irga(6) = CO2
Alias sonic_irga(7) = H2O
Alias sonic_irga(8) = diag_irga
Alias sonic_irga(9) = amb_tmpr
Alias sonic_irga(10) = amb_press
Alias sonic_irga(11) = CO2_sig_strgth
Alias sonic_irga(12) = H2O_sig_strgth
Units Ux = m/s
Units Uy = m/s
Units Uz = m/s
Units Ts = C
Units diag_sonic = arb
Units CO2 = mg/m^3
Units H2O = g/m^3
Units diag_irga = arb
Units amb_tmpr = C
Units amb_press = kPa
Units CO2_sig_strgth = arb
Units H2O_sig_strgth = arb

DataTable (ts_data,TRUE,-1)
DataInterval (0,0,mSec,10)
Sample (1,ec_baudrate,IEEE4)
Sample (12,Ux,IEEE4)
EndTable

BeginProg
Scan (100,mSec,0,0)
EC100Configure(configresult,7,10,ec_baudrate)
EC100(Ux,1,1)
CallTable ts_data
NextScan
EndProg
```

## Remarks

This instruction is a processing instruction. Whether running in pipeline mode or sequential mode the datalogger will execute the instruction from processing. This functionality allows the instruction to be placed in conditional statements. Running from processing also introduces ramifications when attempting to execute the EC100Configure instruction while other SDM instructions are executing in pipeline mode. This instruction locks the SDM port during the duration of its execution. If the pipelined SDM task sequencer needs to run while the SDM is locked it will be held off until the instruction completes. This locking will likely result in skipped scans when reconfiguring an EC150 or EC155.

For the EC150 or EC155 to save settings, it must go through a lengthy write/read/verify process. To avoid saving the settings after each set command, the result code can be used to determine if any settings were modified from their original value. When a change is detected the save settings command (command code 99) can then be sent to the EC150 or EC155. The DestSource parameter variable should be set to 2718 to save settings. The reception of this command is acknowledged but since it takes up to a second to complete, a successful return code does not mean that all of the data was successfully written to the appropriate non-volatile memory.

The EC150 or EC155 can also be configured outside of the datalogger program using Device Configuration Utility or ECMon.

## Parameters

# EC100Result

A variable that contains a value indicating the success or failure of the command. A result code of 0 means that the command was successfully executed. If reading a setting, 0 in the result code means that the value in the DestSource variable is the value the desired setting has in the EC150 or EC155. When writing a setting if the result code is 0 the value and setting were compatible, but the value was not changed because it contained the same value that was sent. A return code of 1 from the set operation means that the value was valid, different, set and acknowledged. This allows CRBasic code to control whether or not to save the settings. NAN indicates that the setting was not changed or acknowledged or a signature failure occurred.

Type: Variable

# SDMAddress (Address of Device)

Defines the address of the device with which to communicate. ValidSDM

**Note:** Synchronous Device for Measurement. A processor-based peripheral device or sensor that communicates with the datalogger via hardwire over a short distance using a protocol proprietary to Campbell Scientific.

addresses are 0 through 14. Address 15 is reserved for the SDMTrigger instruction.

Some SDM instructions support repetitions. If a Reps parameter is present and it is greater than 1, the data logger will increment the SDM address used in the instruction for each subsequent device with which it communicates.

# EC100ConfigCmd

A variable that indicates whether to get or set a setting. The options are:

| Set          | Get | Description                                                                                                                                                     |
| ------------ | --- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0            | 100 | Bandwidth: 5 = 5Hz, 10 = 10Hz, 12 = 12.5Hz, 20 = 20Hz, 25 = 25Hz                                                                                                |
| 1            | 101 | Unprompted Output Rate: 10 = 10Hz, 25 = 25Hz, 50 = 50Hz                                                                                                         |
| 2            | 102 | Pressure Sensor: 0 = EC100 Basic, 1 = User-supplied, 2 = EC100 Enhanced, 3 = None (used fixed value)                                                            |
| 3            | 103 | Differential Pressure: 0 = Disable, 1 = Enable, 2 = Autoselect                                                                                                  |
| 4            | 104 | Fixed Pressure Value                                                                                                                                            |
| 5            | 105 | Pressure Offset                                                                                                                                                 |
| 6            | 106 | Pressure Gain                                                                                                                                                   |
| 7            | 107 | Temperature Sensor: 0 = EC150 thermistor probe, 1= EC155 Sample Cell Thermistor, 2 = EC155 Sample Cell Thermocouple, 3 = None (use fixed value), 4 = Autoselect |
| 8            | 108 | Fixed Temperature Value                                                                                                                                         |
| 9            | 109 | Unprompted Output Mode: 0 = Disable, 1 = USB, 2 = RS-485                                                                                                        |
| 10           | 110 | RS-485 Baud Rate                                                                                                                                                |
| 11           | 111 | Span/Zero Control: 0 = Inactive, 1 = Zero, 2 = Span CO2, 3 = Span H2O                                                                                           |
| 12           | 112 | CO2 Span Concentration                                                                                                                                          |
| 13           | 113 | H2O Span Dew Point Temperature                                                                                                                                  |
| 14           | 114 | CO2 Zero result                                                                                                                                                 |
| 15           | 115 | CO2 Span result                                                                                                                                                 |
| 16           | 116 | H2O Zero result                                                                                                                                                 |
| 17           | 117 | H2O Span result                                                                                                                                                 |
| 18 or 218 \* | 118 | Heater Voltage                                                                                                                                                  |
| 19           | 119 | Reserved                                                                                                                                                        |
| 20           | 120 | Analog Output Enable: 0 = Disable, 1 = Enable                                                                                                                   |
| 21           | 121 | PowerDown: 0 = Gas Head On, 1 = Gas Head Off                                                                                                                    |
| 99           | N/A | Save Settings to EEPROM Memory                                                                                                                                  |

Span/Zero Control To perform zeroing of CO2 and H2O the Span/Zero Control setting is written to 1. After the EC150 or EC155 completes the zero, it will write the setting to -1. The datalogger can poll this value or simply wait for a period of time to allow the zeroing to complete. To perform CO2 span, the CO2 Span Concentration setting (ConfigCmd 12) must be written to the proper value in PPM CO2 prior to writing the Span/Zero Control setting (ConfigCmd 11) to 2. After the CO2 span is complete, the value for the Span/Zero Control setting will be written to -2. H2O span is similar to CO2. First the H2O Span Dew Point value (ConfigCmd 13) must be written to the desired value, then the Span/Zero Control setting is set to 3. After the EC150 or EC155 completes the H2O Span, the Span/Zero Control setting is written to -3.

\* ConfigCmd 218 Normally, EC100Configure is run in the datalogger's processing task. Skipped scans can occur when the EC100Configure instruction executes. When changing operational parameters, these skipped scans are acceptable. However, it may not be acceptable when changing the heater voltage. ConfigCmd 218 allows the EC100Configure instruction to operate in the SDM task, thus avoiding skipped scans. When using ConfigCmd 218, the command must be a constant and the instruction cannot be placed in a conditional statement.

# DestSource

A variable that will contain the value to read when getting a setting, or that will contain the value to send when writing a setting to the EC150 or EC155.

Type: Variable
