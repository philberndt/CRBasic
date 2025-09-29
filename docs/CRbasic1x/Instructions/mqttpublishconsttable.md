# MQTTPublishConstTable (Edit the ConstTable via MQTT)

MQTTPublishConstTable can be used to edit ConstTable values by publishing a JSON object to MQTT.

## Syntax

```
MQTTPublishConstTable
```

(QoS)

```
'MQTTPublishConstTable Example program

Const PublishQoS = 1

ConstTable (myConstTableTest,0)
MQTTPublishConstTable(PublishQoS)
Const myString As String = "123"
Const myDouble As Double = 987.654321
EndConstTable

Public BatteryV, TempC

DataTable (TenSec,True,-1 )
DataInterval (0,10,Sec, 10)
Sample (1,TempC,FP2)
Sample (1,BatteryV, FP2)
EndTable

'Main Program
BeginProg
Scan (1,sec,1,0)
Battery (BatteryV)
PanelTemp (TempC,15000)
CallTable (TenSec)
NextScan
EndProg
```

## Remarks

The table values will be published on the following topic:

{MQTT Base Topic}/editConst

To edit the ConstTable via MQTT, publish the new ConstTable values in a JSON object. The JSON keys and the values must be a string. The string values will be converted to the appropriate types by the datalogger. Only the values of the ConstTable to be changed are required in the JSON object. The JSON object must contain the cmd key and editConst value.

JSON format:

```
{
cmd : EditConst ,
key1 : value1 ,
key2 : value2
}
```

To change values in the ConstTable, publish the above JSON object to the following topic:

{MQTT Base Topic}/CC/editConst

**NOTE:** The maximum MQTT packet size is 4800 bytes.

# QoS

Publish Quality of Service. Supports levels 0 and 1 per MQTT v3.1.1. Level 2 is not supported.
