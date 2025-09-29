# DesiredState

| Logic      | Description                                                                                                                                                                                                                                                                                                          |
| ---------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| True or 1  | Attempts to connect MQTT over an available IP connection.                                                                                                                                                                                                                                                            |
| False or 0 | Loads the job queue with a disconnect request. The disconnect request will follow any active jobs in the queue. This allows the datalogger to complete the publishing of data before closing the connection. Since the disconnect request is in a buffered queue, the disconnect will not always happen immediately. |

Type: Constant or Variable
