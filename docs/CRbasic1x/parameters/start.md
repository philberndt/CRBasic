# Start

Specifies the 16 bit address of the first register that will be requested or acted upon. The Start parameter should be entered as a number 1 to 65535. The data address, or offset, in the Modbus frame sent to the server will be equal to "Start-1". For example, to access the extended holding register commonly referred to as 40001, use a Function code of 3 and a Start parameter of 1. Note, setting the Start parameter directly to a value of 40001 will not start access at the first holding register, it will start access 40000 registers into the holding register table; this is a common mistake when using ModbusClient.

Type: Variable of Constant
