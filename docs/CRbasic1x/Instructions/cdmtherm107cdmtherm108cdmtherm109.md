# CDM_Therm107, CDM_Therm108, CDM_Therm109 (CDM Thermistor Measurements)

The CDM_Therm107, CDM_Therm108, and CDM_Therm109 instructions are used to measure the 107, 108, and 109 thermistors, respectively. For additional information on controller area network peripherals and Campbell modules, see [CPI and CDM](https://www.campbellsci.com/news-cpi-cdm).

## Syntax

```
CDM_Therm107
```

(CDMType,CPIAddress,Dest,Reps,SEChan,ExChan,SettlingTime,fN1,Mult, Offset)

Syntax for all three instructions is the same.

The following program shows an example of measuring two 109 Thermistors using the CDM_Therm109 instruction (the 108 and 107 thermistors are measured identically). A multiplier and offset are used in the example to store the measurements in degrees F.

```
'Declare Variables
Public Temp(2)

'Main Program
BeginProg
Scan (1,min,3,0)
CDM_Therm109( CDM_A108,1,Temp(),2,1,1,0,60,1.8,32)
NextScan
EndProg
```

## Remarks

This instruction makes a half bridge voltage measurement and processes the results using the Steinhart-Hart calculation. The output is temperature in degrees C.

## Parameters

# CDMType (CDM Type)

Used to specify the type ofCDM **Note:** used by the instruction. This is a read-only setting in the device which cannot be changed. Right-click the parameter to display a list of valid CDM types.

Type: Constant

# CPIAddress (CPI Address)

TheCPI

** Note:**CPI is a proprietary interface for communications between Campbell Scientific dataloggers and Campbell Scientific CDM peripheral devices. It consists of a physical layer definition and a data protocol. CDM devices are similar to Campbell Scientific SDM devices in concept, but the use of the CPI bus enables higher data-throughput rates and use of longer cables. CDM devices require more power to operate in general than do SDM devices.

address configured in theCDM ** Note:** module. Valid range is 1 through 120. Note that the CPIAddress cannot be a variable because the datalogger must know all information for a given CDM module at program compile.(e.g., before the program runs).

Type: Constant integer

# Dest (Destination)

The Variable in which to store the results of the instruction. Right-click the parameter to display a list of defined variables.

If this instruction has a Repetitions parameter and it is greater than 1, the results are stored in an array with the variable name. The array must be dimensioned large enough to hold all of the values returned from all of the Reps.

Type: Variable or Array

# Reps (Repetitions)

The number of repetitions for the measurement or instruction.

Type: Constant integer (or expression that evaluates as a constant).

For the CDM_Therm instructions, measurements are made on consecutive channels. If the Reps parameter is greater than 1, the Dest parameter must be a variable array.

# SEChan (Single-Ended Channel)

The single-ended channel to use with this instruction. The number of available channels depends upon the CDM being programmed. If the Reps parameter is greater than 1, the additional measurements will be made on sequential channels. If the SEChan number is entered as a negative value, all Reps will be performed on the same channel (burst measurement). The burst mode sample frequency is controlled by the fN1parameter, which can go up to a maximum of 93750 Hz (minimum sample interval of 10.667 S). The SettlingTime parameter is used to delay once prior to beginning the burst. The total time prior to beginning the burst is the SettlingTime plus the ADC flush time (which is 810 S). The sample interval resolution is 1/93750Hz. The specified notch frequency will use the nearest multiple of (1/93750Hz) to get as close to the specified frequency as possible.

Type: Constant

# ExChan (Excitation Channel)

Enter the excitation channel number (X1, X2...) used to excite the first measurement. The number of available channels depends upon the CDM being programmed. An alphanumeric code is entered.If multiple thermistors are measured with one instruction, all repetitions use the same excitation channel. Right-click within the parameter to display a list of valid options.

Type: Constant

# SettlingTime

The amount of time to delay, in microseconds, after setting up a measurement and before making the measurement on the CDM. Minimum settling time is 100 microseconds; maximum settling time is 100 ms. If 0 is entered, a default of 500 microseconds is used.

Type: Constant

# fN1 (First Notch Frequency)

Determines the lowest frequency that will be eliminated or notched out by the sinc filter. This filter notches out frequencies at integer multiples of fN1by averaging for a time equal to 1/fN1; thus, lower fN1frequencies result in longer measurement times. Any value between 2.5 Hz and 30,000 Hz can be entered, but the value entered will be rounded to the nearest of the frequencies below. The three most common noise filtering options are listed first.

| Option | Description                                                       |
| ------ | ----------------------------------------------------------------- |
| 30000  | Performs a 0.0333 millisecond integration (for fast measurements) |
| 60     | Performs a 16.67 millisecond integration (filters 60 Hz noise)    |
| 50     | Performs a 20 millisecond integration (filters 50 Hz noise)       |
| 15000  | Performs a 0.0667 millisecond integration                         |
| 7500   | Performs a 0.0133 millisecond integration                         |
| 3750   | Performs a 0.2667 millisecond integration                         |
| 2000   | Performs a 0.5 millisecond integration                            |
| 1000   | Performs a 1 millisecond integration                              |
| 500    | Performs a 2 millisecond integration                              |
| 100    | Performs a 10 millisecond integration                             |
| 30     | Performs a 33.33 millisecond integration                          |
| 25     | Performs a 40 millisecond integration                             |
| 15     | Performs a 66.67 millisecond integration                          |
| 10     | Performs a 100 millisecond integration                            |
| 5      | Performs a 200 millisecond integration                            |
| 2.5    | Performs a 400 millisecond integration                            |

Type: Constant

# Mult, Offset (Multiplier and Offset)

Factors by which to scale the raw results of the measurement. Typically used to convert the raw measurement to engineering units or to units other than which is output. For example, the TCDiff instruction measures a thermocouple and outputs temperature in degrees C. A multiplier of 1.8 and an offset of 32 will convert the temperature to degrees F.

For temperature measurements, a multiplier (mult) of 1 and an offset of 0, would output in degrees Celsius. For analog measurements, a multiplier (mult) of 1 and an offset of 0, would output the measured voltage in millivolts divided by the excitation voltage in volts.

If Repetitions of greater than 1 are used for this instruction, Repetitions can also be used for the Multiplier and Offset.[SeeMultipliers, Offsets, and Disable Variables with Repetitionsfor more information.](../Info/multipliersoffsets.md)

Type: Constant, Variable, Array, or Expression
