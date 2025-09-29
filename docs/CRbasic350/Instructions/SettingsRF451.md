# CR350-RF452 Settings

The table below demonstrates the proper syntax for some common datalogger settings that may be set or read programmatically. These settings may also be set using the Device Configuration Utility, or with Tablename.Fieldname syntax (e.g., Settings.Fieldname = setting value). Detailed descriptions for each setting are provided in Device Configuration Utility Help.

| Setting             | Fieldname Type    | Example Syntax                      | Notes                                                                                                  |
| ------------------- | ----------------- | ----------------------------------- | ------------------------------------------------------------------------------------------------------ |
| RadioDataRate       | Long              | SetSetting("RadioDataRate",3)       | 3 = Normal, 115 Kbps 2 = High, 153 Kbps                                                                |
| RadioEnabled        | Long              | SetSetting(RadioEnabled,1)          | 0 = False 1 = True                                                                                     |
| RadioFreqKey        | Long              | SetSetting(RadioFreqKey,11)         | Min = 0 Max = 14                                                                                       |
| RadioFreqRepeat     | Long              | SetSetting("RadioFreqRepeat",0)     | 0 = Use Master Freq Key 1 = Use Repeater Freq Key                                                      |
| RadioHopSize        | Long              | SetSetting("RadioHopSize",50)       | 50 = Min 112 = Max                                                                                     |
| RadioHopVersion     | Long              | SetSetting("RadioHopVersion",0)     | 0 = Standard 1 = Australia 2 = Table 2 3 = Table 3 4 = New Zealand 5 = Notch 6 = Brazil                |
| RadioLowPwr         | Long              | SetSetting("RadioLowPwr",0)         | Min = 0 Max = 31                                                                                       |
| RadioMaxPacket      | Long              | SetSetting("RadioMaxPacket",5)      | Min = 0 Max = 9                                                                                        |
| RadioMinPacket      | Long              | SetSetting("RadioMinPacket",5)      | Min = 0 Max = 9                                                                                        |
| RadioModOS          | String; Read Only | Variable = Settings.RadioModOS      | RF451 Firmware Version                                                                                 |
| RadioNetID          | Long              | SetSetting("RadioNetID",1)          | Min = 0 Max = 4095                                                                                     |
| RadioOpMode         | Long              | SetSetting(RadioOpMode,2)           | Point to MultiPoint Access Point = 2 Point to Multipoint End Point= 3 Point to Multipoint Repeater = 7 |
| RadioPacketRepeat   | Long              | SetSetting("RadioPacketRepeat",0)   | Min = 0 Max = 9                                                                                        |
| RadioRepeaters      | Long              | SetSetting("RadioRepeaters",0)      | 0 = no repeaters used 1 = repeaters used                                                               |
| RadioRetryTimeout   | Long              | SetSetting("RadioRetryTimeout",8)   | 8 = Min 255 = Max                                                                                      |
| RadioRxSubID        | Long              | SetSetting("RadioRxSubID",15)       | Min = 0 Max = 15                                                                                       |
| RadioEndPointRepeat | Long              | SetSetting("RadioEndPointRepeat",0) | 0 = Normal 1 = Enable EndPoint/Repeater                                                                |
| RadioEndPointRetry  | Long              | SetSetting("RadioEndPointRetry",0)  | Min = 0 Max = 9                                                                                        |
| RadioTxPwr          | Long              | SetSetting("RadioTxPwr",1)          | 0 = min 10= max                                                                                        |
| RadioTxSubID        | Long              | SetSetting("RadioTxSubID",15)       | Min = 0 Max = 15                                                                                       |
