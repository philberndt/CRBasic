# FFTSpa (Spatial Fast Fourier Transform)

The FFTSpa instruction performs a Fast Fourier Transform on a time series of measurements stored in an array and stores the result in an array.

## Syntax

FFTSpa(Dest,N,SrcArray,SampleInterval,Units,Option, [FFTSpaInit] )

Following is sample code using the FFTSpa instruction.

```
Const Sample_Interval =50
Const TS_Samps_For_FFT=4096
Const FFTBins=TS_Samps_For_FFT/2 +1
Public AccelFFTMax(2)
Dim _Accel, Accel(TS_Samps_For_FFT), AccelSpaFFT(TS_Samps_For_FFT+1), i

DataTable(AccelFFT,True,-1)
Sample(2,AccelFFTMax(),Float)
FFT(AccelSpaFFT,Float,TS_Samps_For_FFT,Sample_Interval,mSec,3+100)'Note: Adding 100 peforms no FFT on FFT processed data from FFTSpa.
' FFT(AccelSpaFFT,Float,TS_Samps_For_FFT,Sample_Interval,mSec,3)'<-- Use when data table processes FFT instead of FFTSpa.
EndTable

DataTable(TSeries,True,TS_Samps_For_FFT*100)
Sample(1,_Accel,Float)
EndTable

BeginProg

Scan(Sample_Interval,Sec,10,0)

VoltSe(_Accel,1,mv2500,1,False,200,4000,1.0,0.0)

i=i+1
Accel(i)=_Accel
If i>=TS_Samps_For_FFT Then
FFTSpa(AccelSpaFFT(),TS_Samps_For_FFT,Accel(),Sample_Interval,mSec,3)'FFT type 3 is power spectrum.
MaxSpa(AccelFFTMax(),FFTBins-1,AccelSpaFFT(2))'Skip over 1st bin (DC bin).
CallTable(AccelFFT)
i=0
EndIf

CallTable(TSeries)

NextScan

EndProg
```

## Remarks

An inverse FFT can also be performed with this instruction, generating a time series from the results of an FFT.

The difference in this instruction and [FFT](fft.md) is that the results are stored in an array instead of output to a table.

## Parameters

# Dest (Destination)

The Variable in which to store the results of the instruction. Right-click the parameter to display a list of defined variables.

If this instruction has a Repetitions parameter and it is greater than 1, the results are stored in an array with the variable name. The array must be dimensioned large enough to hold all of the values returned from all of the Reps.

Type: Variable or Array

# N (Number of Points in Time Series)

The number of points in the time series (must be a power of 2).

Type: Name

# SrcArray (Source Array)

The name of the Source Array for FFT. Right-click the parameter to display a list of defined variables.

Type: Name

For the FFTSpa instruction, the SrcArray is the name of the Variable array that contains the input data for the FFTSpa function.

# SampleInterval (Sample Interval)

In normal operation (non-burst mode), this is the datalogger scan rate, or the time series data interval.

If the measurement instruction that created the time-series data used burst mode (that is, the channel was entered as a negative number), theCR300burst measurement samplingrateis determined by the fN1(first notch frequency) parameter.

Type: Constant (or expression that evaluates as a constant)

For the FFTSpa instruction, this is the sampling interval of the time series. Exact sampling interval can be calculated as Tsample = 1.085069 \* INT(SettleUSEC/1.085069) + 0.5), where SettleUSEC is the sample interval entered in the settling time parameter of the source instruction.

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

If 100 is added to the option code (100, 101, 102, etc.), no FFT will be performed on an array of data that was processed by an [FFTSpa](#) instruction. The data will be stored to the data table with the appropriate description fields in the data file/header/table definitions so that it can be appropriately processed/graphed by the software.

Type: Constant

# FFTSpaInit (Spatial Fast Fourier Transform)

This is an optional parameter. FFTSpaInit is a variable array that will be initialized at compile time with the frequency axis of the FFT, from DC to the Nyquist frequency, in units of Hertz. The array is not updated at run time. The array can be dimensioned to hold less than the N/2+1 frequencies, or if it is dimensioned greater than N/2+1, the remainder will be initialized to 0.

Type: Variable (optional parameter)

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
