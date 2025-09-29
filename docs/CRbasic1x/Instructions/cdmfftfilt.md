# CDM_FFTFilt

The SPECTRUM module is a 3- or 9-channel analog input module for the datalogger. It includes programmable anti-alias filtering and DC excitation. The CDM_VoltFilt() instruction is used to pass individual voltage measurements for each channel on a SPECTRUM to the datalogger CPU at the scan interval. The CDM_FFTFilt instruction passes an entire spectrum for each channel to the datalogger's CPU at the scan interval.

## Syntax

CDM_FFTFilt( Module, Addr, Dest, Reps, Range, Chan, FiltOpt, Excitation, Mult, FSampleRate, FFTLen, TSwindow, SpectOpt, Fref, Sbin, ILow, IHigh )

```
'CR1000X Series
'Example program for CDM_FFTFilt
'Note that for Destination array, the dimensions of the required array
'are determined by the number of Reps, the SpectumOption selected, and the number
'of values to be returned as defined by ILow and IHigh.
'Note that constants must be used when specifying the array.
'An error will be returned when the program is compiled if variables are used to
'define the array.

Const ADDR = 1
Const NUM_REPS = 2
Const FFT_LEN as Long = 4096
Const FSample as long = 10000
const num_fft_vals = ((FFT_LEN/2) + 1)
const num_vals = num_fft_vals

Public fft_max(NUM_REPS,2)
Public freq_at_max(NUM_REPS)
Dim Dest(NUM_REPS,num_vals)'Note that destination array
Public Count as Long
Dim i as Long

#if 1
DataTable (SpectData, 1, -1)
FFTSample(Dest(1,1),IEEE4)
FFTSample(Dest(2,1),IEEE4)
EndTable
#endif

#if 0
DataTable (TSData, 1, -1)
Sample(FFT_LEN,Dest(1,FFT_LEN/2 + 2),IEEE4)
EndTable
#endif

'Main Program
BeginProg
Scan(500,msec,50,0)

'CDM_FFTFilt(Module,Addr,Dest,reps,Range,Chan,FiltOpt,Excite,M,FSampleRate,FFTLen,TSwindow,SpectOpt,Fref,Sbin,Ihow,IHigh)
CDM_FFTFilt(SPECTRUM103,ADDR,Dest,NUM_REPS,mv5000,2,4,2,1.0,FSample,FFT_LEN,0,1,0,0,0,FFT_LEN/2)
CallTable SpectData
' CallTable TSData
for i = 1 to NUM_REPS
MaxSpa(fft_max(i,1),num_fft_vals,Dest(i,1))
freq_at_max(i) = (fft_max(i,2) * fsample) / FFT_LEN
Next i
Count += 1

NextScan
EndProg
```

## Remarks

The CDM_FFTFilt instruction passes an entire spectrum for each channel to the datalogger CPU at the scan interval.

The FFT operation provides spectra from "seamless" time-series snapshots, if the scan interval is set to the FFT length divided by the time-series sample rate. Slower spectrum rates can be selected by increasing the scan interval above this quotient.

## Parameters

# Module (Module)

Specifies the type of module to configure. Valid options are SPECTRUM103 or SPECTRUM109.

Type: Constant

# Addr (Address)

Specifies the CPI or EPI bus and the CPI address of the module. The valid CPI address range is 1 through 120. If no bus is specified, then CPI_BusA is used by default.

| Description       |
| ----------------- |
| CPI_BusA + 1..120 |
| CPI_BusB + 1..120 |
| EPI_Bus + 1..120  |

**NOTE:** The two EPI buses are linked internally, so there is no A and B designation for the EPI ports.

Type: Constant

# Dest (Destination)

An array in which to store the results of the function. The dimensions of the required array are determined by the number of Reps, the SpectOption selected, and the number of values to be returned as defined by ILow and IHigh. Note that constants must be used when specifying the array. An error will be returned when the program is compiled if variables are used to define the array.

Type: Variable array

# Reps (Repetitions)

Specifies the number of times to repeat the calculation. When Reps is greater than one, calculations are made on subsequent channels. The results from multiple reps are placed one after the other in the Dest array.

Type: Constant integer (or expression that evaluates as a constant).

# Range (Voltage Range)

The Range parameter is the expected voltage range of the input from the sensor. An alphanumeric or the numeric code can be entered:

| Alphanumeric | Description |
| ------------ | ----------- |
| mV10000      | 10000 mV    |
| mV5000       | 5000 mV     |
| mV1000       | 1000 mV     |
| mV200        | 200 mV      |

The Spectrum normally replaces out-of-range measurements with NaN. However, out-of-range measurements can be replaced by the analog-to-digital converter saturation value by adding 1000 to the code selected for the FiltOption parameter.

Type: Constant

# Chan (Channel)

The number of the channel on which to make the first measurement. If the Reps parameter is greater than 1, additional measurements will be made on sequential channels.

Type: Constant

# FiltOption (Filter Sample Ratio)

Used to specify the sample ratio which is the sample rate divided by the frequency at the top of the passband of the low pass filter.

The FiltOption parameter is a constant designating the sample ratio for the anti-aliasing filter that will be applied prior to performing the FFT. The sample ratio determines the top of the pass-band (FPASS) and the beginning of the stop-band (FSTOP) of the anti-aliasing filter relative to the sample rate (FSAMPLE). The sample rate is the inverse of the scan interval in the CRBasic program. FiltOption must be the same for all channels of a single Spectrum Filter Module. The Granite Spectrum supports the following sample ratios, pass-bands, and stop-bands:

| Numeric Code | Sampling Ratio | FPASS       | FSTOP           |
| ------------ | -------------- | ----------- | --------------- |
| 4            | 4              | FSAMPLE /4  | FSAMPLE /2      |
| 20           | 20             | FSAMPLE /20 | FSAMPLE /3.3333 |

Type: Constant

# Excitation (Excitation)

Used to set the continuous DC output level for the excitation channel(s). If the Reps parameter is greater than 1, the same excitation level will be used for each channel. A numeric code is entered:

| Code | Output Level             |
| ---- | ------------------------ |
| 0    | Disabled (No Excitation) |
| 1    | Constant 10 mA           |
| 2    | Constant 5 V             |
| 3    | Constant 10 V            |

Type: Constant

# Multiplier (Multiplier)

Factor by which to scale the results of the measurement. Multiplier provides the reference value for the decibel (dB) spectral option.

Type: Constant, Variable, Array, or Expression

# FSampleRate (Filter Sample Rate)

A constant used to designate the sample rate (in samples per seconds) at which the filter board will collect time series data before performing the FFT. FSampleRate must be the same for all channels of a SPECTRUM module. There are over 700 valid sample rates for the module. The most commonly used sample rates are as follows:

| Sample Rates |
| ------------ |
| 10000        |
| 5000         |
| 2000         |
| 1000         |
| 500          |
| 200          |
| 100          |
| 50           |
| 20           |
| 10           |
| 5            |
| 2            |
| 1            |

Right-click this parameter to display a pick-list of the above rates, or enter a desired sample rate if it does not appear on this list. If you attempt to send a program with a sample rate not supported by the Spectrum module, an error message will be returned with the next higher and next lower valid rates. Adjust your program to reflect one of these rates.

Type: Constant

# FFTLen (FFT Length)

A constant that designates the length of the time series snapshot on which to perform the FFT. If FFTLen/FSample equals the scan rate, consecutive time series snapshots that are processed into the spectra will be seamless. If FFTLen/FSample is greater than the sample rate, the time series snapshots will have gaps between them. FFTLen/FSample cannot be less than the scan rate. FFTLen must be the same for all channels of a Spectrum module. Valid entries are:

| FFT Length |
| ---------- |
| 65536      |
| 32768      |
| 16384      |
| 8192       |
| 4096       |
| 2048       |
| 1024       |
| 512        |
| 256        |
| 128        |
| 64         |
| 32         |

The FFT throughput for transforms of 2048 points or less is much higher than the throughput for transforms longer than 2048 points.

Type: Constant

# TSWindow (Time Series Window)

A constant that designates whether a window function (also known as taper or apodization) should be applied to the time series snapshot before performing the FFT. Typical window functions give more weight to the middle of the time series while tapering the ends to avoid spectral leakage caused by a non-integral number of periods of a repetitive signal in the snapshot. A numeric code is entered.

| Code | Function (formulas given below)                         |
| ---- | ------------------------------------------------------- |
| 0    | None                                                    |
| 1    | Hanning                                                 |
| 2    | Hamming                                                 |
| 3    | Blackman-Harris                                         |
| 4nn  | Kaiser-Bessel, where nn is beta (see below) 5 < nn < 16 |

The SPECTRUM applies the selected window function by multiplying each point of the original time series by the corresponding point of the window function. Because this windowing process removes some of the original signal variation, the SPECTRUM uses the following procedure to correct the resulting spectra.

The SPECTRUM first computes the mean and standard deviation of the original time series for use in additional processing. Next, the SPECTRUM subtracts the mean from each point of the original time series, and then multiplies the mean-subtracted time series by the selected window function. The SPECTRUM then computes the standard deviation of this windowed time series. The SPECTRUM then computes the FFT of the windowed time series, and multiplies each ac component of the complex spectrum by the ratio of the standard deviations of the time series computed before and after the window function was applied. The SPECTRUM then sets the dc component of the spectrum to the mean of the original time series, normalized for the FFT length.

The SPECTRUM computes the **Hanning** window function from

N is the length of the original time series (FFTLen).

The SPECTRUM computes the **Hamming** window function from

The SPECTRUM computes the **Blackman** window function from

The **Kaiser-Bessel** window function allows users to trade spectral leakage for spectral resolution by selecting the parameter beta. Larger betas apply more taper to the ends of the time series, improving (decreasing) the spectral leakage at the expense of spectral resolution. The following table gives the maximum out-of-band spectral leakage and the spectral resolution, expressed as amplitude full-width half-of-maximum (FWHM), as a function of beta. A beta of 12 gives a good match of spectral leakage to the SPECTRUM spectral noise floor with only a minor decrease in spectral resolution.

| Beta | Max Spectral Leakage | Spectral Resolution (Amplitude FWHM) |
| ---- | -------------------- | ------------------------------------ |
| 8    | -63 dB               | 2.25 bins                            |
| 10   | -74 dB               | 2.50 bins                            |
| 12   | -95 dB               | 2.75 bins                            |
| 14   | -110 dB              | 3.00 bins                            |

Type: Constant

# SpectOption (Spectrum Output Option)

A constant designating the output option for the computed spectrum. ILow and IHigh determine the number of values or pairs of values returned by the instruction. A numeric code is entered for this parameter. The SPECTRUM supports the options given below. (Options 0 through 4 are the same as for the FFT instruction. Options 6 and 7 are new for the FFTFilt instruction.) SpectOption also allows users to return the time series from which each spectrum is derived and to apply predefined weighting functions to spectra (see below).

| Code                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Result                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 0                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | Real and Imaginary. Returns the real and imaginary parts of the FFT. Maximum spectrum length = (FFTLen/2+1) pairs.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| 1                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | Amplitude. Returns the amplitude of each spectral component. The amplitude for all components except the dc and Nyquist components are computed from. The dc and Nyquist components are computed from. N is the length of the original time series (FFTLen). The units for the amplitude spectrum are mV. Maximum spectrum length = (FFTLen/2+1) values.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| 2                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | Amplitude and Phase. This option returns the amplitude as described above, plus the phase in radians given by. The phase is between -π and π. Maximum spectrum length = (FFTLen/2+1) pairs.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| 3                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | Power. Returns the power for each of the spectral components. The power is computed fromfor all spectral components except the dc and Nyquist components. The dc component is computed fromand the Nyquist component is computed from. The sum of all the ac components of the power spectrum gives the variance of the original time series. The units for the power spectrum are. Maximum spectrum length = (FFTLen/2+1) values.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| 4                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | Power Spectral Density (PSD). Normalizes the power spectrum by the bandwidth of each spectral component. The PSD is computed fromfor all spectral components except the dc and Nyquist components.is the sample rate of the original time series (FSampleRate parameter). The dc component is computed fromand the Nyquist component is computed from. The integral of the PSD over the ac components gives the variance of the original time series. The units for the PSD are. Maximum spectrum length = (FFTLen/2+1) values.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| 6                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | RMS (root mean square) Amplitude. The RMS amplitude is computed from the square root of the power spectrum for all spectral components, or equivalently, the amplitude spectrum divided by thefor all ac components. The dc component of RMS amplitude spectrum is the same as the dc component of the amplitude spectrum. The units for the RMS amplitude spectrum are mV RMS. Maximum spectrum length = (FFTLen/2+1) values.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| 7                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | Decibels. The decibel (dB) spectrum normalizes the RMS amplitude spectrum according to, whereis the value from the RMS amplitude spectrum andis the RMS amplitude reference level. The inverse of the Multiplier parameter (Multiplier-1) gives. Because the square of the RMS amplitude is equal to power, an equivalent normalization to dB is, whereis the value from the power spectrum, andis the power reference level. The square of the inverse of the Multiplier parameter (Multiplier-2) gives. The Multiplier parameter performs two functions for the dB spectrum option. The first function is to convert the raw signal measurements from mV to the units in which the dB reference is specified. The second function gives the dB reference. For example, a signal from a microphone can be converted to the sound pressure level (SPL) spectra in dB relative to 20 Pascals RMS, by setting the Multiplier towhere k is the microphone calibration in Pascals per mV. The dB spectrum is unitless. Maximum spectrum length = (FFTLen/2+1) values. |
| The original time series data from which the output spectrum was computed can be returned with a special option in the SpecOption parameter. This option is enabled by adding 100 to the option codes above. In this case, the datalogger places the time series data in the array Dest immediately following the spectrum. When this option is enabled, the Dest variable must be dimensioned large enough to hold this additional time series data. If Reps is more than one while this time series option is enabled, then the datalogger places the spectrum for the first channel in Dest, followed by the time series for the first channel. Next, the datalogger places the spectrum for the second channel in Dest, followed by the time series for the second channel, and so on. |

Tye: Constant

# FRef, SBin

The FRef and SBin parameters are used to specify the type of spectral binning that will be used by the FFTFilt instruction.

**No Spectral Binning **- Select no spectral binning by setting FRef equal to 0 and SBin equal to either 0 or 1. The center of each spectral component with no spectral binning is

|     |     |     |
| --- | --- | --- |
|     | for |     |

whereis sample rate of the time series before the FFT (FSampleRate), andis the length of the FFT (FFTLen).is the center frequency of the first spectral component returned by the FFTFilt instruction,is the center frequency of the second spectral component, and so on.

The difference between the center frequencies of adjacent spectral components is, and bandwidth of each spectral component is.

You can select whether the SPECTRUM returns the entire spectrum or only part of the spectrum by setting ILow and IHigh. To return the entire spectrum, set ILow to its minimum value, and IHigh to its maximum value. The minimum ILow is 0, and the maximum IHigh is.

To limit the lower end of the spectrum, first select a minimum frequency of interest,, and then set ILow to

,

whereisrounded to the nearest integer. To limit the upper end of the spectrum, select a maximum frequency of interest,, and then set IHigh to

.

The total number of spectral components (spectral pairs for real and imaginary, or amplitude and phase, spectral options) returned by the SPECTRUM is IHigh - ILow + 1.

** Linear Spectral Binning **- Linear spectral binning combines a fixed number of adjacent spectral components into a single component of the final spectrum. Select linear spectral binning by setting FRef equal to zero and SBin to two or more. The parameter SBin determines the number of bins to combine.

The mathematical operation to combine bins depends on the spectrum type (SpectOption). The SPECTRUM computes binned amplitude, RMS amplitude, and dB spectra by summing the power of adjacent bins into a single bin, and then converting to the desired spectrum type (amplitude, RMS amplitude, or dB) from this summed power. The SPECTRUM computes spectrally binned power spectral density (PSD) functions by averaging adjacent frequency-normalized bins into a single bin, giving a frequency-normalized result. The SPECTRUM does not allow Real and Imaginary, nor Amplitude and Phase, spectral binning. For all spectral options, the dc component is not included in spectral binning: the first bin to be combined is the first ac component. However, the dc component can still be returned as part of the final spectrum.

The center frequency of each spectral component with linear spectral binning is

|     |     |     |
| --- | --- | --- |
|     | for |     |

whereis sample rate of the time series before the FFT (FSampleRate),is the length of the FFT (FFTLen), andis the number of bins to combine (SBin).is the center frequency of the first spectral component returned by the FFTFilt instruction,is the center frequency of the second spectral component, and so on.

The difference between the center frequencies of adjacent spectral components after linear spectral binning is, and bandwidth of each spectral component (except the dc component) is also. The bandwidth of the dc component is.

You can select whether the SPECTRUM returns the entire spectrum or only part of the spectrum by setting ILow and IHigh. To return the entire spectrum, set ILow to its minimum value, and IHigh to its maximum value. The minimum ILow is zero, and the maximum IHigh is thewhere theis the largest integer that is not greater than. To limit the lower end of the spectrum, first select a minimum frequency of interest,, and then set ILow towhereisrounded to the nearest integer. To limit the upper end of the spectrum, select a maximum frequency of interest,, and then set IHigh to.

The total number of spectral components returned by the SPECTRUM is IHigh - ILow + 1.

** Logarithmic Spectral Binning (1/n Octave Analyses)**- Logarithmic spectral binning combines a variable number of adjacent spectral components into a single component of the final spectrum. The number of components combined into a single bin increases logarithmically with frequency. Select logarithmic spectral binning by setting FRef to a non-zero value (see below) and SBin between 1 and 12. The parameter SBin determines the number of spectral components per octave (a factor of two increase in frequency) to report in the final, binned spectrum.

The mathematical operation to combine bins depends on the spectrum type (SpectOption). The SPECTRUM computes binned amplitude, RMS amplitude, and dB spectra by summing the power of adjacent bins into a single bin, and then converting to the desired spectrum type (amplitude, RMS amplitude, or dB) from this summed power. The SPECTRUM computes spectrally binned power spectral density (PSD) functions by averaging adjacent frequency-normalized bins into a single bin, giving a frequency-normalized result. The SPECTRUM does not allow Real and Imaginary, nor Amplitude and Phase, spectral binning. For all spectral options, the dc component is not included in spectral binning: the first bin to be combined is the first ac component. In addition, the dc component is never part of the final spectrum for logarithmic binning.

The center frequency of each spectral component with logarithmic spectral binning is

|     |     |     |
| --- | --- | --- |
|     | for |     |

whereis an arbitrary reference frequency selected by the user (FRef), andis the number bins reported in the final, binned spectrum (SBin). In many acoustic applications, FRef is set to 1 kHz.is the center frequency of the first spectral component returned by the FFTFilt instruction,is the center frequency of the second spectral component, and so on.

The ratio (not the difference) between center frequencies of adjacent spectral components after logarithmic spectral binning is. The absolute bandwidth of each spectral component is not constant, but rather, increases with increasing frequency. The bandwidth of each spectral component, expressed as a percent of the center frequency, is. Many acoustic applications call for 1/3 octave analyses (three points per octave). For this case, the center frequency of a given bin is a factor of about 1.26 greater than the center frequency of the preceding bin. The bandwidth of each bin is about 23 percent of the bin s center frequency.

You can select whether the SPECTRUM returns the entire spectrum or only part of the spectrum by setting ILow and IHigh. To return the entire spectrum, set ILow to its minimum value, and set IHigh to its maximum value. The minimum ILow is

where theis the smallest integer that is not less than. The maximum IHigh is the

wherethe is the largest integer that is not greater than. As an alternative to computing the minimum ILow and maximum IHigh from the equations given above, set ILow a very negative value (like -1000) and set IHigh to a very positive value (like 1000). When the program is compiled an error message will be returned that gives the minimum ILow and maximum IHigh for the current FFT length and reference frequency.

The lower end of the final spectrum may be limited by selecting a minimum frequency of interest,, and then setting ILow according to

,

whereisrounded to the nearest integer. To limit the upper end of the final spectrum, select a maximum frequency of interest,, and then set IHigh to

.

The total number of spectral components returned by the SPECTRUM is IHigh - ILow + 1.

Type: Constant

# ILow, IHigh

The ILow and IHigh parameters are used to determine which part of the spectrum will be returned. Refer to FRef and SBin parameters.

Type: Constant
