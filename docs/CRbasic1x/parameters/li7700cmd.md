# LI7700Cmd

Requests the data to be retrieved from the sensor. The command is sent first to the device specified by the SDMAddress parameter. If the Reps parameter is greater than 1, subsequent LI-7700s will be issued the command with each repetition. The results for the command will be returned in the array specified by the Dest parameter. A numeric code is entered to request the data:

| Code | Description                                                                                                                                         |
| ---- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0    | CH4D (mmol/m^3), pressure (kPa), Temp (C)                                                                                                           |
| 1    | CH4D (mmol/m^3), pressure (kPa), Temp (C), diagnostic, RSSI                                                                                         |
| 2    | CH4D (mmol/m^3), pressure (kPa), Temp (C), diagnostic, RSSI, AUX1, AUX2, AUX3, AUX4                                                                 |
| 3    | CH4D (mmol/m^3), CH4 (umol/mol), pressure (kPa), Temp (C), diagnostic value, RSSI, AUX1, AUX2, AUX3, AUX4, AuxTc1, AuxTc2, AuxTc3                   |
| 4    | CH4D (mmol/m^3), CH4 (umol/mol), pressure (kPa), Temp (C), diagnostic, RSSI, AUX1, AUX2, AUX3, AUX4, AuxTc1, AuxTc2, AuxTc3, AUX5, AUX6, AUX7, AUX8 |
| 5    | RSSI, diagnostic, drop rate                                                                                                                         |
| 6    | RSSI, diagnostic, heater, drop rate, OpticsRH (%), LCTActual, LCSetPT                                                                               |
| 7    | RSSI, heater, RefRSSI, drop rate, LCTActual, LCTSetPT, BCTActual, BCTSetPT, OpticsRH (%)                                                            |
