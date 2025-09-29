# Wi-Fi Settings

The following table demonstrates the proper syntax for some common Wi-Fi settings that may be set programmatically using the [SetSetting](setstatussetsetting.md) instruction. These settings may also be set usingDevice Configuration Utility

**Note:** Configuration tool used to set up dataloggers and peripherals, and to configure PakBus settings before those devices are deployed in the field and/or added to networks.

. For additional information on Wi-Fi settings, see the datalogger manual.

| Setting          | Fieldname Type    | Example Syntax                                    | Notes                                                                                                                                      |
| ---------------- | ----------------- | ------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| IPAddressWIFI    | String            | SetSetting( IPAddressWIFI", 1.1.1.1 )             |
| IPMaskEthWIFI    | String            | SetSetting( IPMaskEthWIFI", 255.255.255.255 )     |
| IPGatewayWIFI    | String            | SetSetting( IPGatewayWIFI", 1.1.1.1 )             |
| WIFIChannel      | Long              | SetSetting("WIFIChannel",10)                      | Applies only when configured to create a network                                                                                           |
| WIFIConfig       | Long              | SetSetting("WIFIconfig",1)                        | 0 = Join a network 1 = Create a network 4 = Disable                                                                                        |
| WIFIEapMethod    | Long              | SetSetting("WIFIEapMethod",10)                    | 2 = TTLS 4 = PEA                                                                                                                           |
| WIFIEapPassword  | String            | SetSetting("WIFIEapPassword", PW )                | Used for Enterprise Security-enabled network                                                                                               |
| WIFINetworks     | String, Read Only | _ Variable _= Settings.WIFINetworks               | Lists networks available in area                                                                                                           |
| WIFIPassword     | String            | SetSetting("WIFIPassword", PW )                   | Used for WPA or WPA2 security enabled network; 8 to 63 characters                                                                          |
| WIFIPowerMode    | Long              | SetSetting("WIFIPowerMode",1)                     | 0 = disable power savings during high throughput 1 = disable power savings during any communication 2 = power savings enabled at all times |
| WIFISSID         | String            | SetSetting("WIFISSID", SSID )                     | 31 character maximum                                                                                                                       |
| WIFIStatus       | String; Read Only | _ Variable_= Settings.WIFIStatus                  | String must be sized large enough to hold desired information                                                                              |
| WIFITxPowerLevel | Long              | SetSetting("WIFITxPowerLevel",10)                 | 0 = Low (7 +/- 1 dBm) 1 = Medium (10 +/- 1 dBm) 2 = High (15 +/- 2 dBm)                                                                    |
| WL AND omainName | String            | SetSetting("WL AND omainName", linktodevice.com ) | Applies only if configured to join a network                                                                                               |
