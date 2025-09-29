# FFTSpaInit (Spatial Fast Fourier Transform)

This is an optional parameter. FFTSpaInit is a variable array that will be initialized at compile time with the frequency axis of the FFT, from DC to the Nyquist frequency, in units of Hertz. The array is not updated at run time. The array can be dimensioned to hold less than the N/2+1 frequencies, or if it is dimensioned greater than N/2+1, the remainder will be initialized to 0.

Type: Variable (optional parameter)
