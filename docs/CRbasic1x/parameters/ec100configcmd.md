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
