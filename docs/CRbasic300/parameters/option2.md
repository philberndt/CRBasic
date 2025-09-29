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

If 100 is added to the option code (100, 101, 102, etc.), no FFT will be performed on an array of data that was processed by an [FFTSpa](../Instructions/fftspa.md) instruction. The data will be stored to the data table with the appropriate description fields in the data file/header/table definitions so that it can be appropriately processed/graphed by the software.

Type: Constant
