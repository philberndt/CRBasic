# MQTTPublishMeta (Notify Data Logger Which Topic to Publish On)

MQTTPublishMeta uses the Table parameter to notify the data logger which topic to publish on.

## Syntax

MQTTPublishMeta( Table,Field,Key,Value)

```
' MQTT Example program
Const CSIJSON = 1
Const GEOJSON = 2
'Const publishQoS = 0 'Publishes and does not get ack from broker
Const publishQoS = 1'Waits for ack from broker and retries
Public PubMeta as Boolean
Public PubPublic As Boolean

ConstTable (myConstTableTest,0)
MQTTPublishConstTable (publishQoS)
Const myString As String = "123"
Const myDouble As Double = 987.654321
Const publishQoS_1 = 1
Const subscribeQoS_1 = 1
EndConstTable

Public batteryVolt, crTemp
Public Array(6) As Long
Alias Array(1) = my_array(2)
Alias Array(5) = my_scaler
Dim i as Long
Public lon = NaN
Public lat = NaN
Public alt = NaN
Public outstat As Boolean
Public lastfile As String * 128
Public filecount As Long
Public fhandle as long
Public num_read as long
Dim GEO_JSON As String * 2000

DataTable (OneMinute,True,-1 )
' TableFile("USR:OneMin",&H407, 1, 0,60, Sec, outstat,lastfile)
TableFile("USR:OneMin",&H20, 1, 0,60, Sec, outstat,lastfile)
DataInterval (0,60,Sec, 10)
Sample (1,crTemp,FP2)
Sample (1,batteryVolt, FP2)
Sample (2,Array, Long)
'MQTTPUblishTable(MQTT QoS, Number of records to publish, Interval to publish, format)
MQTTPublishTable(publishQoS, 0, 60, Sec, GEOJSON, lon, lat, alt)
EndTable

'Main Program
BeginProg
SetStatus ("USRDriveSize",7500)
For i = 1 To 6
Array(i) = i
Next i

Scan (1,sec,1,0)
Battery (batteryVolt)
PanelTemp (crTemp,15000)

for i = 1 to 6
Array(i) += i
Next i

If pubmeta then
MQttPublishMeta("OneMinute", "crTemp","DoorOpen","-1")
MQttPublishMeta("OneMinute", "","test","5")
MQttPublishMeta("", "","Test","12")
PubMeta = false
Endif

If pubpublic then
MQttPublishPublicTable(1)
pubpublic = false
EndIf

CallTable(OneMinute)

If outstat then
fhandle = fileopen(lastfile,"r",0)
If fhandle then
num_read = FileReadLine(fhandle,GEO_JSON,2000)
fileclose(fhandle)
Endif
filecount += 1
Endif
NextScan
EndProg
```

## Remarks

The table, if present, notifies the data logger which topic to publish on. If the table parameter is left empty (""), the notification will be published on the "general" metadata topic, which applies to the entire system. This is the same topic used for "static" system information, except "/prog" will be added to the topic so the cloud knows the incoming data was sent by program control and can be parsed differently than the "general" data. The field parameter, if specified, indicates that the published information applies to only the specified field. Otherwise, the field name will be absent from the published topic, and the notification will apply to the entire table.

## Parameters

# Table

The table, if present, notifies the data logger which topic to publish on. If the table parameter is left empty (""), the notification will be published on the "general" metadata topic, which applies to the entire system.

# Field

The field parameter, if specified, indicates that the published information applies to only the specified field. Otherwise, the fieldname will be absent from the published topic and the notification will apply to the table.

# Key

The name or identifier used to access the corresponding values within a JSON (JavaScript Object Notation) object. A JSON object is a collection of key-value pairs enclosed in curly braces {}. For example:

```
{
key1 : value1 ,
key2 : value2
```

}

```

# Value


The value associated with a corresponding JSON key within a JSON object. For example:

```

{
key1 : value1 ,
key2 : value2

```
}
```
