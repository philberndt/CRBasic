# LI7200Cmd

Requests the data to be retrieved from the sensor. The command is sent first to the device specified by the SDMAddress parameter. If the Reps parameter is greater than 1, subsequent LI-7200s will be issued the command with each repetition. The results for the command will be returned in the array specified by the Dest parameter. A numeric code is entered to request the data:

| Code | Description                                                                                                                                                                      |
| ---- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0    | CO2 dry (μmol/mol), H2O dry (mmol/mol)                                                                                                                                           |
| 1    | CO2 (mmol/m^3), H2O (mmol/m), Ptotal (kPa), Tin (C), Tout (C)                                                                                                                    |
| 2    | CO2 dry (μmol/mol), H2O dry (mmol/mol), CO2 (mmol/m^3), H2O (mmol/m^3), AGC (%), Ptotal (kPa), Tin (C), Tout (C), Tavg_Tin_Tout (C), Aux channel #1                              |
| 3    | CO2 dry (μmol/mol), H2O dry (mmol/mol), CO2 (mmol/m^3), H2O (mmol/m^3), AGC (%), PTotal (kPa), Tin (C), Tout (C), Aux channel #1, Aux channel #2, Aux channel #3, Aux channel #4 |
| 4    | CO2 (mmol/m^3), H2O (mmol/m^3), CO2 absorptance, H2O absorptance, AGC (%), Ptotal (kPa), Tin (C), Tout (C), Block Temperature (C), diagnostic                                    |
| 5    | CO2 dry (μmol/mol), H2O dry (mmol/mol), AGC (%), Tavg_Tin_Tout (C), Ptotal (kPa)                                                                                                 |
| 6    | Diagnostic                                                                                                                                                                       |
| 7    | Get CO2 dry (μmol/mol), H2O dry (mmol/mol), AGC (%), Ptotal (kPa), and Tavg_Tin_Tout (C) after an SDMTrigger instruction                                                         |
