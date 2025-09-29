# AllowSleep (Allow Sleep Mode)

Optional parameter that allows the datalogger to go into sleep mode during periods of inactivity.

- **0 ** or absent = do not allow sleep mode

- **1 ** or <>**0 **= allow low-power sleep mode during periods of inactivity

** Caution:**After entering sleep mode, incoming data will wake up the datalogger, but it is possible that the first couple of bytes will be lost or corrupted during the wakeup period.

Type: Constant
