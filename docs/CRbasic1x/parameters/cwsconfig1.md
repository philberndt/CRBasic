# CWSConfig (String or Path to Sensor Information)

The use of the CWSConfig parameter is optional and can be left blank. This parameter is a string or the path to a file that contains the sensor name and variable names that the datalogger will use for each sensor. The string or file also determines the order in which each sensor's data will be stored in the datalogger.

CWSConfig string The configuration string contains a sensor ID and sensor name for each sensor, the number of values to include for each sensor, and the fieldnames (in the order they are returned by the sensor) to be used by the datalogger (i.e., Public variables). Each part of the string for a sensor is separated by a space. As an example, a string for one sensor would contain:

SensorID SensorName NumberOfValues Field1Name Field2Name Field3Name

Strings for multiple sensors are separated by a comma. If a sensor returns 10 values, but 4 is specified for the NumberOfValues, then only 4 values will be stored by the datalogger for that sensor.

CWSConfig filename Rather than entering a string for the CWSConfig parameter, a filename can be entered. It is specified as Device:CWSConfig.txt, where Device is CPU, CRD, USB, or USR. If the Network Planner or Wireless Sensor Planner is used to configure the wireless sensors, this file will be created when the "Send a config string to the datalogger" task is chosen (or it can be created manually by the user).

Any sensors in the network not contained in the CWSConfig string will assume their default names when discovered in the network.

Type: Constant String (SensorID SensorName NumberOfValues Field1Name Field2Name )
