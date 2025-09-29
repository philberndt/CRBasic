# Timeout

The Timeout parameter is the number of seconds the interface will stay powered on after the last activity is detected on the Interface. The Timeout parameter is optional and if the state is non-zero, the interface will be powered on and remain on for the time specified by the Timeout. The Timeout is refreshed when activity is detected, thus keeping the interface on while communications are active. The interface will be powered off the number of seconds specified by Timeout after the last detected activity.

**NOTE:** Cell interfaces often experience frequent network traffic, which typically prevents them from shutting down. If shutting down a cell interface is desired, setting a short timeout (minimum of one second), in the optional timeout parameter can be used to prompt a shutdown.

Type: Constant
