# RF407-Series Settings

The following table demonstrates the proper syntax for some common radio settings that may be set programmatically using the [SetSetting](setstatussetsetting.md) instruction. These settings may also be set usingDevice Configuration Utility

**Note:** Configuration tool used to set up dataloggers and peripherals, and to configure PakBus settings before those devices are deployed in the field and/or added to networks.

. For additional information on RF407-Series settings, see the datalogger manual.

| Setting        | Fieldname Type    | Example Syntax                                             | Notes                                                                                                                                          |
| -------------- | ----------------- | ---------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- | --- | --- | --- | --- | --- | --- | ------------ | ----- | --- | -------- | -------- | --- | --------- | -------- | --- | --------- | --------- | --- | ---------- | --------- | --- | ---------- | --------- | --- |
| RadioOS        | String; Read Only | _ Variable _= Settings.RadioOS                             | RF4xx OS version                                                                                                                               |
| RadioModel     | String; Read Only | _ Variable _= Settings.RadioModel                          | RF407, RF412, or RF422                                                                                                                         |
| RadioRSSI      | RadioRSSI         | _ Variable _= Settings.RadioRSSI                           | Signal strength of the last packet received. Units = dBm; -40 is stronger than -70. It may take several minutes for this value to be returned. |
| RadioRSSIAddr  | Long; Read Only   | _ Variable _= Settings.RadioRSSIAddr                       | Pakbus address that last packet came from. It may take several minutes for this value to be returned.                                          |
| RadioAvailFreq | String; Read Only | _ Variable _= Settings.RadioAvailFreq                      | Returns bitfield of available frequencies                                                                                                      |
| RadioChanMask  | String; Read Only | _ Variable_= Settings.RadioChanMask                        | Bitfield of frequencies; allows channels to be selectively enabled or disabled.                                                                |
| RadioEnable    | Boolean           | SetSetting("RadioEnable",1) or SetSetting("RadioEnable",0) | 1 = True 0 = False                                                                                                                             |
| RadioNetID     | Long              | SetSetting("RadioNetID",10)                                | Min = 0 Max = 32767                                                                                                                            |
| RadioHopSeq    | Long              | SetSetting("RadioHopSeq",0)                                | Min = 0 Max = 7 for RF407 or RF412 Max = 9 for RF422                                                                                           |
| RadioTxLevel   | Long              | SetSetting("RadioTxLevel ",1)                              |                                                                                                                                                |     |     |     | --- | --- |     | RF407, RF412 | RF422 |     | 0 = 5 mW | 0 = 2 mW |     | 1 = 32 mW | 1 = 5 mW |     | 2 = 63 mW | 2 = 10 mW |     | 3 = 125 mW | 3 = 16 mW |     | 4 = 250 mW | 4 = 25 mW |     |
| RadioPwrMode   | Long              | SetSetting("RadioPwrMode",0)                               | 0 = Always on 1 = 0.5 sec 2 = 1 sec 3 = 4 sec                                                                                                  |
| RadioRetries   | Long              | SetSetting("RadioRetries",1)                               | 0 = None 1 = Low 2 = Medium 3 = High                                                                                                           |
