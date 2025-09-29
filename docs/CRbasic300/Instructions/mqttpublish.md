# MQTTPublish

Publish to MQTT

**NOTE:** See [MQTT](https://help.campbellsci.com/CR300/Content/shared/Communication/mqtt/mqtt.htm) for more information on the use of MQTT inCampbell Scientificdata loggers.

## Syntax

- Result =\*MQTTPublish( TopicString,Payload, QoS,Retain)

```
'Declare Variables
Public Counter
```

Public Result
Public Flag As Boolean
Public Payload As String = "TestString"

'Main Program
BeginProg
Scan (1,Sec,0,0)

If Flag Then
Counter +=1
'MQTTPublish(TopicString,Payload,QoS,Retain)
Result =MQTTPublish("TestTopic",Payload,0,0)
Flag = false
EndIf

NextScan
EndProg

```

## Remarks


The MQTTPublish function must be called from within a Scan. If a publish QoS of 1 is used, this function will wait on a response or timeout before continuing to run CRBasic. **NOTE:** Beginning with OS11.02the payload parameter now accepts elements from a string array and the return value can be stored in an array.


### MQTT topic structure


The topic structure is user defined. This instruction does not inherit the base topic from the ** MQTTBaseTopic**datalogger setting.


### Return Value (Result)


The function returns 0 if the MQTTPublish is successful. It will return an error if the MQTTPublish fails. The return value can be stored in an array.

The following error codes are possible:

| Error Code | Description |
| --- | --- |
| 0 | No error |
| -1 | No MQTT connection |
| -2 | No Topic |
| -3 | Wildcard in topic string. |
| -4 | Bad Data Table payload pointer |
| -5 | Bad memory pointer (system error) |
| -101 | No new data to publish in table |


## Parameters



# TopicString


A string that defines the payload. Format conforms to MQTT v3.1.1.


# PayLoad


The payload for the publish function. This is a user-formatted message of type string. The payload parameter can accept elements from a string array.

** NOTE:**The maximum MQTT packet size is 4800 bytes.


# QoS


Publish Quality of Service. Supports levels 0 and 1 per MQTT v3.1.1. Level 2 is not supported.


# Retain


Broker retains value. Not supported by Amazon Web Services; must be 0.
```
