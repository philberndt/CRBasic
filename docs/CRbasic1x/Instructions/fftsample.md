# FFTSample

The FFTSample instruction stores the current spectral components of a source signal.

## Syntax

```
FFTSample
```

(Source,DataType)

```
'The following program shows the use of the FFTSample instruction within a DataTable declaration.
```

ConstCPI_ADDR =CPI_BUS+ 1

```
Const NUM_REPS = 3
```

Const FFT_LEN as Long = 4096

```
Const FSample as long = 10000
```

Const num_fft_vals = ((FFT_LEN/2) + 1)

```
Const num_vals = num_fft_vals
```

Dim Spectrum1(NUM_REPS,num_vals)

```
DataTable (SpectData, 1, -1)
```

FFTSample(Spectrum1(1,1),IEEE4)

```
EndTable
```

BeginProg

```
'Allocate sufficient data logger memory for real-time measurements; in this example
```

'the scan buffer is 50

```
Scan(500,msec,50,0)
```

CDM_FFTFilt(SPECTRUM103,CPI_ADDR,Spectrum1,NUM_REPS,mv5000,1,4,3,1.0,FSample,FFT_LEN,0,1,0,0,0,FFT_LEN/2)

```
CallTable SpectData
```

NextScan

```
EndProg
```

## Parameters

# Source

The name of the Variable that is the input for the instruction. Right-click the parameter to display a list of defined variables.

Type: Variable

# DataType

A code to select the data storage format. IEEE4 is the only valid data type for FFTSample().

| Name  | Description                                   |
| ----- | --------------------------------------------- |
| IEEE4 | IEEE four-byte floating point (IEEE 754 std). |

Type: Constant
