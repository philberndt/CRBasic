# Option

A bit field parameter used to specify how the I2C transaction starts and ends.

| Option | Description                                                                                                            |
| ------ | ---------------------------------------------------------------------------------------------------------------------- |
| B0     | Stop. Send a stop condition at the end of the transaction                                                              |
| B1     | Start. Send a start condition at the beginning of the transaction. The address will be sent following the start.       |
| B2     | Restart. Send a restart condition at the beginning of the transaction. The address will be sent following the restart. |

If Start or Restart are asserted the address will be sent prior to any data. If the stop is not set, it is assumed that another I2C instruction will occur, and the bus is left in control of the controller.

When all of the bytes are read and a stop is specified, the controller will send a NACK on the last byte. This is in accordance with the I2C specification and allows slave devices to reset their control state machines to a known state.

Examples of the Option parameter include:

- &H02 = Start with no stop at the end

- &H03 = Start, then Stop at the end

- &H05 = Restart, then Stop at the end

Type: Constant
