# Monitoring Cellular Diagnostics and Data Usage Programmatically

[TableName.FieldName](tablenamefieldname.md) syntax can be used in CRBasic programs to monitor CELL2XX cellular modem diagnostic information or data usage. This is accomplished by declaring a variable in the program and retrieving the desired data usage value from the datalogger **Settings** table.

For more information see this blog article:[How to Monitor Your Campbell Cellular Modem Data Usage: Part 1](https://www.campbellsci.com/blog/monitor-cellular-modem-data-usage-part-1).

For example:

```
PublicDailyUsageas Long'Declare a variable to store the value retrieved from Settings table
PublicCellSignalas Long

BeginProg
Scan(1,hr,3,0)
DailyUsage = Settings.CellUsageToday'Access the desired value from the Status table and store it in the variable
CellSignal = Settings.CellRSSI'Received signal strength indication
NextScan
EndProg
```

The usage values available to retrieve from the Settings table and their descriptions follow:

| Field Name                                                                   | Data Type         | Description                                                                                                                                                                                         | CRBasic Syntax                                                                                                                                      | Notes                                                                                                                                                                                                                           |
| ---------------------------------------------------------------------------- | ----------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| CellRSSI Or CellRSRP                                                         | Long, Read Only   | Specifies the signal strength of the modem in -dBm. For 3G, this is RSSI (Received Signal Strength Indicator). For 4G, this is RSRP (Reference Signal Received Power)                               | Variable = Settings.CellRSSI Or Variable = Settings.CellRSRP                                                                                        | Excellent: -70dBm or less (3G), -90dBm or less (4G) Good: -70dBm to -85dBm (3G), -90dBm to -105dBm (4G) Fair: -86dBm to -100dBm (3G), -106dBm to -115dBm (4G) Poor: >-100dBm (3G), >-115dBm (4G)                                |
| CellECIO Or CellRSRQ                                                         | Float, Read Only  | Specifies the signal quality of the modem. For 3G, this is ECIO (Energy to Interference Ratio). For 4G, this is RSRQ (Reference Signal Received Quality).                                           | Variable = Settings.CellECIO Or Variable = Settings.CellRSRQ                                                                                        | Excellent: 0 to -6 (3G), >-9 (4G) Good: -7 to -10 (3G), -9 to -12 (4G) Fair to Poor: -11 to -20 (3G), -13 or less (4G)                                                                                                          |
| CellInfo                                                                     | String, Read Only | Cellular diagnostic information. Reports the PPP state, IMEI, IMSI, and ICCID for the datalogger.                                                                                                   | Variable = Settings.CellInfo                                                                                                                        |
| CellStatus                                                                   | String, Read Only | Cellular network/state information. Reports the cellular IP address, default gateway, DNS, network state, cell carrier, connection type, phone number associated with the SIM card, and data usage. | Variable = Settings.CellStatus                                                                                                                      |
| CellState                                                                    | String, Read Only | Specifies the current state of communication with the cell modem and with the network.                                                                                                              | Variable = Settings.CellState                                                                                                                       | Example states include: Power off Powering up Powered up SIM authorized Setting baud rate Waiting for baud rate Baud rate set Baud rate failure Dialing Dialed PPP negotiation Network ready PPP closing PPP paused PPP dropped |
| CellUsageToday or CellUsageYesterday or CellUsageMonth or CellUsageLastMonth | Long, Read Only   | Data usage in KB                                                                                                                                                                                    | Variable = Settings.CellUsageToday Variable = Settings.CellUsageYesterday Variable = Settings.CellUsageMonth Variable = Settings.CellUsageLastMonth |
