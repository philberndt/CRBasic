# MeasPEx (Excitation Channels)

The number of sensors to excite with the same excitation terminal before automatically advancing to the next excitation terminal. To excite all sensors with the same excitation terminal, the value of this parameter should be set equal to the value of the Reps parameter.

This parameter in a bridge measurement can be used when the sensors to be measured outnumber the available excitation channels. It allows multiple sensors to be excited with each excitation channel.

For example, assume it is desired to measure eight pressure transducers that have 350 ohm full bridge strain gages to be excited by 5 volts. Each transducer requires 14 mA (5 volts/350 ohms = 14 mA). Since each excitation channel can provide a maximum of 50 mA at 5 volts, a maximum of 3 pressure transducers can be excited per channel (50 mA/14 mA = 3.57). To measure the eight transducers, 8 would be entered for the Reps argument and 3 for the MeasPEx. The first three transducers would all be connected to the first excitation channel, the next 3 to the second excitation channel, and the last 2 to the third excitation channel.

Type: Constant (or expression that evaluates as a constant)
