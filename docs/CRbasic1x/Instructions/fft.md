# FFT (Fast Fourier Transform)

The FFT instruction performs a Fast Fourier Transform on a time series of measurements stored in an array.

## Syntax

FFT(SrcArray,DataType,N,SampleInterval,Units,Option)

Following is sample code using the FFT instruction.

```
'Program Declarations
Const SIZE_FFT = 16
Const PI = 3.141592654
Const CYCLESperT = 2
Const AMPLITUDE = 3
Const DC = 7
Const OPT_FFT = 0
Dim I
Public x( SIZE_FFT ), y( SIZE_FFT +2)

'Define DataTables Amp, AmpPhase, Power, PSD, FFT, IFFT
DataTable( Amp, 1, 1 )
FFT( x, fp2, SIZE_FFT, 10, msec, 1 )
EndTable

DataTable( AmpPhase, 1, 1 )
FFT( x, fp2, SIZE_FFT, 10, msec, 2 )
EndTable

DataTable( power, 1, 1 )
FFT( x, fp2, SIZE_FFT, 10, msec, 3 )
EndTable

DataTable( PSD, 1, 1)
FFT( x, fp2, SIZE_FFT, 10, msec, 4 )
EndTable

DataTable( FFT, 1, 1 )
FFT( x, IEEE4, SIZE_FFT, 10, msec, 0 )
EndTable

DataTable( IFFT, 1, 1 )'inverse FFT
FFT( y, IEEE4, SIZE_FFT, 10, msec, 5 )
EndTable

'Main program
BeginProg
Scan( 50, msec, 0, SIZE_FFT )
I = I + 1
X(I) = DC + Sin(PI / 8 + 2 * PI * CYCLESperT * I / SIZE_FFT) * AMPLITUDE + Sin(PI / 2 + PI * I)
NextScan
CallTable( Amp )
CallTable( AmpPhase )
CallTable( Power )
CallTable( PSD )
CallTable( FFT )
For I = 1 To SIZE_FFT +2'get result back into y( )
y( I ) = FFT.x_fft( I,1 )
next I
CallTable(IFFT)'inverse, result is the same as x( )
EndProg
```

## Remarks

An inverse FFT can also be performed with this instruction, generating a time series from the results of an FFT.

## Parameters

# SrcArray (Source Array)

The name of the Source Array for FFT. Right-click the parameter to display a list of defined variables.

Type: Name

# DataType

A code to select the data storage format. Right-click the parameter to display a list.

| Name  | Description                                                                                        |
| ----- | -------------------------------------------------------------------------------------------------- |
| IEEE4 | IEEE four-byte floating point (IEEE 754 std). IEEE4 can also be specified using the keyword FLOAT. |
| FP2   | Campbell Scientifictwo-byte floating point.                                                        |
| IEEE8 | IEEE eight-byte floating point (double)                                                            |

Additional data types are available. However, not all data types are suitable for all output instructions, so they should be used with care.

| Name    | Description                                                        |
| ------- | ------------------------------------------------------------------ |
| String  | ASCII string; size defined by program                              |
| Boolean | 0 = False; -1 = True                                               |
| BOOL8   | 1-byte Boolean value                                               |
| Long    | 32-bit long integer, ranging from -2,147,483,648 to +2,147,483,647 |
| NSEC    | 8-byte time stamp format                                           |
| UINT1   | One-byte unsigned integer                                          |
| UINT2   | Two-byte unsigned integer                                          |
| UINT4   | Four-byte unsigned integer                                         |

**UINT1 **: Holds an 8-bit unsigned integer (a number in the range of 0 - 32,767, where NAN values are stored as 0). The program should be written to check for values outside the range. This data type is an efficient means of storing 8-bit integers since it requires only one byte of memory in a data table. Floating point values are converted to UINT1 as if the INT function were used.

** UINT2 **: Holds a 16-bit unsigned integer (a number in the range of 0 - 65535, where NAN values are stored as 0). The program should be written to check for values outside the range. This data type is an efficient means of storing 16-bit integers since it requires only two bytes of memory in a data table. Floating point values are converted to UINT2 as if the INT function were used.

** UINT4 **: Holds a 32-bit unsigned integer (a number in the range of 0 to 4,294,967,295).

** Long **: Sets the output to a 32-bit long integer, ranging from -2,147,483,648 to +2,147,483,647 (31 bits plus the sign bit). There are two possible reasons a user would use this format: (1) speed, since the OS can do math on integers faster than with floats, and (2) resolution, since the Long has 31 bits compared to the 24-bits in the IEEE4. However, in most instances it is not suitable for output since any fractional portion of the value is lost.

** BOOL8 **: Used to store variables that hold bits (0 or 1) of information. These values are shown in LoggerNet or other datalogger software as an array of eight Boolean values (each element in the array represents one bit). This data type uses less space than normal 32-bit values. Any reps stored must be divisible by two, since an odd number of bytes cannot be stored in a data table. When converting from a Long or a Float to a BOOL8, only the least significant 8 bits are used.

** NSEC**: An 8-byte time stamp format (4 bytes of seconds since 1990, 4 bytes of nanoseconds into the second) used when the variable being sampled is the result of the [RealTime](realtime.md) instruction or when the variable is a Long storing time since 1990 (for instance, when using TableName.TimeStamp). If the variable array is dimensioned to 7, the values stored will be year, month, day of year, hour, minutes, seconds, and milliseconds. The variable array must be declared as a Float or Long. If the variable array is dimensioned to two and declared as a Long, the instruction assumes that the first element holds seconds since 1990 and the second element holds microseconds into the second. If the source is a single variable dimensioned as a Long, the instruction assumes that the variable holds seconds since 1990 and the microseconds into the second is 0. In this instance, the value stored is a standard datalogger timestamp rather than the number of seconds since January 1990.

Type: Constant (or expression that evaluates as a constant)

# N (Number of Points in Time Series)

The number of points in the time series (must be a power of 2).

Type: Name

# SampleInterval (Sample Interval)

In normal operation (non-burst mode), this is the datalogger scan rate, or the time series data interval.

If the measurement instruction that created the time-series data used burst mode (that is, the channel was entered as a negative number), theCR1000Xburst measurement samplingfrequencyis determined by the fN1(first notch frequency) parameterwhich can go up to a maximum of 31250 Hz (minimum sample interval of 32 uS). The SettlingTime parameter is used to delay once prior to beginning the burst. The total time prior to beginning the burst is the SettlingTime plus the ADC flush time (which is 450 uS). The sample interval resolution is 1/31250 Hz. The specified notch frequency will use the nearest multiple of (1/31250 Hz) to get as close to the specified frequency as possible.

Type: Constant (or expression that evaluates as a constant)

For the FFT instruction, the exact sampling interval can be calculated as Tsample = 1.085069 \* INT(SettleUSEC/1.085069) + 0.5), where SettleUSEC is the sample interval entered in the settling time parameter of the source instruction.

# Units

The units for the interval parameter. An alphabetical code is entered. Right-click the parameter to display a list.

| Alphanumeric Code | Description  |
| ----------------- | ------------ |
| usec              | microseconds |
| msec              | milliseconds |
| sec               | seconds      |
| min               | minutes      |

Type: Constant

# Option

Used to select the processing option for FFT. Right-click the parameter to display a list.

| Option | Result                                                                                                                                                                                                                                                                                                                                |
| ------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0      | FFT. The output is (N/2)+1 complex data points, i.e., the real and imaginary parts of the FFT. The first pair is the DC pair; the last pair is the Nyquist pair. Zero is seen for their imaginary component.                                                                                                                          |
| 1      | Amplitude spectrum. The output is (N/2)+1 magnitudes. With Acos( ωt ); A is magnitude.                                                                                                                                                                                                                                                |
| 2      | Amplitude and Phase Spectrum. The output is (N/2)+1 pairs of magnitude and phase; with Acos(ωt - φ); A is amplitude, φ is phase ( - π, π ).                                                                                                                                                                                           |
| 3      | Power Spectrum. The output is (N/2)+1 values normalized to give a power spectrum. With Acos(ωt - φ), the power is A2/ 2. The summation of the (N/2)+1 values yields the total power in the time series signal.                                                                                                                        |
| 4      | Power Spectral Density ( PSD ). The output is (N/2)+1 values normalized to give a power spectral density ( power per hertz ). The Power Spectrum multiplied by T = N \* tau yields the PSD. The integral of the PSD over a given bandwidth yields the total power in that band. Note that the bandwidth of each value is 1 / T hertz. |
| 5      | Inverse FFT. The input is (N/2)+1 complex numbers, organized as in the output of option 0, which is assumed to be the transform of some real time series. The output is the time series (N values) whose FFT would result in the input array.                                                                                         |

If 100 is added to the option code (100, 101, 102, etc.), no FFT will be performed on an array of data that was processed by an [FFTSpa](fftspa.md) instruction. The data will be stored to the data table with the appropriate description fields in the data file/header/table definitions so that it can be appropriately processed/graphed by the software.

Type: Constant

Normalization Details

Complex FFT result I

i = 1 .. N / 2: ai \* cos( wi \* t ) + bi \* sin( wi \* t ).

wi =2 π ( I - 1 ) / T.

φi =atan2( bi, ai ) ( 4 quadrant arctan )

Power(1) = ( a12+ b12) / N2( DC )

Power(i) = 2 \* ( ai2+ bi2) / N2( i = 2..N / 2, AC )

PSD(i) = Power( I ) \* T = Power( I ) \* N \* tau

A1 = sqrt( a12+ b12) / N ( DC )

Ai = 2 \* sqrt( ai2+ bi2) / N ( AC )

Notes

- Power is independent of the sampling rate (1/tau) and of the number of samples (N).

- The PSD is proportional to the length of the sampling period (T = N\* tau), since the width of each bin is 1/T.

- The sum of the AC bins (excluding DC) of the Power Spectrum is the Variance (AC Power) of the time series.

- The factor of 2 in the Power(i) calculation is due to the power series being mirrored about the Nyquist frequency N/(2\*T); only half the power is represented in the FFT bins below N/2, with the exception of DC. Hence, DC does not have the factor or 2.

- The Inverse FFT option assumes that the data array input is the transform of a real time series. Filtering is performed by taking an FFT on a data set, zeroing certain frequency bins, and then taking the Inverse FFT. Interpolation is performed by taking an FFT, zero padding the result, and then taking the Inverse FFT of the larger array. The resolution in the time domain is increased by the ratio of the size of the padded FFT to the size of the unpadded FFT. This can be used to increase the resolution of a maximum or minimum, as long as aliasing is avoided.
