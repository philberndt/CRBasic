# TimeOut (Wait Time)

An optional parameter that specifies how long the instruction will wait before moving on to the next instruction.

- **Timeout = 0/Omitted:** Default timeout is applied (75 seconds)

- **Timeout > 0:** Specifies the maximum time (in seconds) the instruction will run before moving on.

- **Timeout < 0:** The absolute value acts as a timeout and enables time servo mode. Time servo mode allows the data logger to gradually adjust its clock to sync with the NTP server rather than making abrupt time changes. While this mode improves time tracking, accuracy is influenced by factors such as network latency and server variability.

- **Dependencies:** 1. Execute the instruction every 2 to 5 minutes. 2. Set theNTPMaxMSecparameter to 1 second or less to maintain reliable synchronization.
