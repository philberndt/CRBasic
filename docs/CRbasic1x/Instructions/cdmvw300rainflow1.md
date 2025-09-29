# CDM_VW300RainFlow (VWIRE or CDM-VW300 Rainflow)

The CDM_VW300RainFlow instruction is used to calculate a rainflow histogram of the measurements from a VWIRE 305, CDM-VW300 or CDM-VW305. For additional information on controller area network peripherals and Campbell modules, see [CPI and CDM](https://www.campbellsci.com/news-cpi-cdm).

## Syntax

CDM_VW300RainFlow(CPIAddress,RF1, RF2, RF3, RF4, RF5, RF6, RF7, RF8)

The following portion of code can be used in the datalogger program to help set up the arrays for the instruction.

```
Const MBINS = 20
Const ABINS = 20

Public RF1(MBINS,ABINS)
Public RF2(MBINS,ABINS)
Public RF3(MBINS,ABINS)
Public RF4(MBINS,ABINS)
Public RF5(MBINS,ABINS)
Public RF6(MBINS,ABINS)
Public RF7(MBINS,ABINS)
Public RF8(MBINS,ABINS)
```

The following is a sampleCR1000Xprogram showing the use of the CDM_VW300Rainflow instruction.

```
'Rainflow Histogram Output Example for the VWIRE 305 or CDM-VW300 series
'50Hz sampled data from one VWIRE 305 or CDM-VW305 device (8 channels)

'Make sure the CPI address given here
'matches what is shown in Device Configuration Utility or DVW Toolbox
'for the intended VWIRE 305 or CDM-VW300 series device
Const CPI_ADDR = 5

Public Freq(8)
Public Diag(8) As Long
Public StaticFreq(8)
Public Therm(8)
Public DynStdDev(8)

'Rainflow : Mean Bins and Amplitude Bins dimensions
Const MBINS = 10
Const ABINS = 10
'Rainflow Histogram Outputs - 2 dimensional arrays
Public RF1(MBINS,ABINS)
Public RF2(MBINS,ABINS)
Public RF3(MBINS,ABINS)
Public RF4(MBINS,ABINS)
Public RF5(MBINS,ABINS)
Public RF6(MBINS,ABINS)
Public RF7(MBINS,ABINS)
Public RF8(MBINS,ABINS)

Dim Enable(8) As Long = { 1, 1, 1, 1, 1, 1, 1, 1}
Dim Max_AMP(8) = { 0.002, 0.002, 0.002, 0.002, 0.002, 0.002, 0.002, 0.002}
Dim F_Low(8) = { 300, 300, 300, 300, 300, 300, 300, 300}
Dim F_High(8) = { 6000, 6000, 6000, 6000, 6000, 6000, 6000, 6000}
Dim OutForm(8) As Long = { 0, 0, 0, 0, 0, 0, 0, 0}
Dim Mult(8) = { 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0, 1.0}
Dim Off(8) = { 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0}
Dim SteinA(8) = { 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0}
Dim SteinB(8) = { 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0}
Dim SteinC(8) = { 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0, 0.0}

'Rainflow Histogram configuration
Dim RF_mean_bins(8) As Long = { MBINS, MBINS, MBINS, MBINS, MBINS, MBINS, MBINS, MBINS}'Mean Bins
Dim RF_amp_bins(8) As Long = { ABINS, ABINS, ABINS, ABINS, ABINS, ABINS, ABINS, ABINS}'Amplitude Bins
Dim RF_Lo_lim(8) = { 400.0, 400.0, 400.0, 400.0, 400.0, 400.0, 400.0, 400.0}'Low Limit
Dim RF_Hi_lim(8) = {4000.0,4000.0,4000.0,4000.0,4000.0,4000.0,4000.0,4000.0}'High Limit
Dim RF_Hyst(8) = { 0.005, 0.005, 0.005, 0.005, 0.005, 0.005, 0.005, 0.005}'Hysteresis

'Output format consists of three digits ABC, that can be either 1 or 0
Dim RF_OutForm(8) As Long = { 110, 110, 110, 110, 110, 110, 110, 110}

'RF Histogram Digit Code Histogram output result
'----------------------- ------------------------
'A = 0 Reset histogram after each output.
'A = 1 Do not reset histogram.
'B = 0 Divide bins by total count.
'B = 1 Output total in each bin.
'C = 0 Open form. Include outside range values in end bins.
'C = 1 Closed form. Exclude values outside range.

CDM_VW300Config(1,CPI_ADDR,0,Enable(),Max_AMP(),F_Low(),F_High(), _
OutForm(),Mult(),Off(), SteinA(),SteinB(),SteinC(), _
RF_mean_bins(),RF_amp_bins(),RF_Lo_lim(), _
RF_Hi_lim(),RF_Hyst(),RF_OutForm())

DataTable (static,true,-1)
Sample (8,StaticFreq(),IEEE4)
Sample (8,Therm(),IEEE4)
Sample (8,DynStdDev(),IEEE4)
EndTable

DataTable (dynamic,true,-1)
Sample (8,Freq(),IEEE4)
Sample (8,Diag(),IEEE4)
EndTable

'Store Rainflow Histogram Output
DataTable (rainflhist,true,-1)
RainflowSample(RF1(),IEEE4)
RainflowSample(RF2(),IEEE4)
RainflowSample(RF3(),IEEE4)
RainflowSample(RF4(),IEEE4)
RainflowSample(RF5(),IEEE4)
RainflowSample(RF6(),IEEE4)
RainflowSample(RF7(),IEEE4)
RainflowSample(RF8(),IEEE4)
EndTable

BeginProg

'50 Hz/20msec scan rate
Scan(20,msec,500,0)

CDM_VW300Dynamic(CPI_ADDR,Freq(),Diag())
CallTable dynamic
If TimeIntoInterval (0,1,Sec) Then
CDM_VW300Static(CPI_ADDR,StaticFreq(),Therm(),DynStdDev())
CallTable static
EndIf

NextScan

SlowSequence
Scan(5,sec,10,0)
'Rainflow Histogram call must be in a slow sequence scan
CDM_VW300Rainflow(CPI_ADDR,RF1(),RF2(),RF3(),RF4(),RF5(),RF6(),RF7(),RF8())
CallTable rainflhist
NextScan

EndProg
```

## Remarks

The CDM_VW300RainFlow instruction is a processing instruction. It must be used in a [SlowSequence](slowsequence.md) scan.To output the results to a data table, use the [RainFlowSample](rainflowsample2.md) instruction.

## Parameters

# CPIAddress (CPI Address)

TheCPI

**Note:** CPI is a proprietary interface for communications between Campbell Scientific dataloggers and Campbell Scientific CDM peripheral devices. It consists of a physical layer definition and a data protocol. CDM devices are similar to Campbell Scientific SDM devices in concept, but the use of the CPI bus enables higher data-throughput rates and use of longer cables. CDM devices require more power to operate in general than do SDM devices.

address configured in theCDM **Note:** module. Valid range is 1 through 120. Note that the CPIAddress cannot be a variable because the datalogger must know all information for a given CDM module at program compile.(e.g., before the program runs).

Type: Constant integer

** NOTE:**UseDevice Configuration Utility

** Note:**Configuration tool used to set up dataloggers and peripherals, and to configure PakBus settings before those devices are deployed in the field and/or added to networks.

, DVW Toolbox software, or the CPIAddModule instruction to configure the CPIAddress within your VWIRE 305 or CDM-VW300 series device.

# RFx (Rainflow Histogram Arrays)

Two-dimensional arrays in which to store the rainflow histogram for each channel measured on the device. The first dimension is the number of Mean bins and the second dimension is the number of amplitude bins. For the CDM-VW300, only two variable arrays are required (RF1 and RF2); for the VWIRE 305 or CDM-VW305 eight variable arrays are required (RF1 RF8).

Type: Two-dimensional variable array
