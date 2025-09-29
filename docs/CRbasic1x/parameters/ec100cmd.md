# EC100Cmd

Requests the data to be retrieved from the sensor. The results for the command will be returned in the array specified by the Dest parameter. A numeric code is entered to request the data:

| Code | Description                                                                                                                                                                                                                                                                 |
| ---- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0    | Ux (m/s), Uy (m/s), Uz (m/s), Ts (C), sonic diagnostic (arb), CO2\*, H2O\*, gas diagnostic (arb)                                                                                                                                                                            |
| 1    | Ux (m/s), Uy (m/s), Uz (m/s), Ts (C), sonic diagnostic (arb), CO2\*, H2O\*, gas diagnostic (arb), ambient temperature (C), ambient pressure (kPa), CO2 signal strength (arb), H2O signal strength (arb)                                                                     |
| 2    | Ux (m/s), Uy (m/s), Uz (m/s), Ts (C), sonic diagnostic (arb), CO2\*, H2O\*, gas diagnostic (arb), cell temperature (C), cell pressure (kPa), CO2 signal strength (arb), H2O signal strength (arb), sample cell pressure differential (kPa).\*\* (Available only from EC155) |

\* An EC150 outputs CO2 density (mg/m^3) and H2O density (g/m^3). An EC155 outputs CO2 molar mixing ratio (umol/mol) and H2O molar mixing ratio (mmol/mol). Molar mixing ratio is the concentration relative to dry air.

\*\* If EC100 OS version 7.01 or later is loaded and an open-path gas analyzer (i.e., EC150 or IRGASON) is connected to the EC100, the 13th data field will be CO2 density (mg/m^3) calculated from fast-response temperature. Specifically, this additional CO2 density output uses humidity-corrected sonic temperature, instead of ambient temperature measured by the EC100 temperature probe, in the conversion of absorption measurements to CO2 density. Using the sonic anemometer s fast-response temperature measurements compensates for spectroscopic effects during high sensible heat flux regimes as explained in Helbig et al. (2016). For open-path gas analyzers connected to an EC100 running an OS version less than 7.01, the 13th data field is unused.
