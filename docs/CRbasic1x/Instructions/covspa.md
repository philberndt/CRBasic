# CovSpa (Spatial Covariance)

The CovSpa instruction computes the spatial covariance on multiple sets of measurements that are loaded into an array.

## Syntax

CovSpa(Dest,NumOfCov,SizeOfSets,CoreSet,DataSets)

This example program takes 128 voltage measurements on5consecutive channels, and calculates the FFT for each of the channels. It then retrieves the FFT results using the GetRecord instruction and performs a Spatial Covariance on the first4channels against the last channel.

```
'CR1000X Series

Dim Sig1(128)
Dim Sig2(128)
Dim Sig3(128)
Dim Sig4(128)
Dim Sig5(128)
Dim Sets(645)
Public CoVarVal(4)

DataTable(PSDFFT,True,-1)
CardOut(0,0)
FFT(Sig1(),IEEE4,128,20,uSec,4)'Perform FFT on Sig1()
FFT(Sig2(),IEEE4,128,20,uSec,4)'Perform FFT on Sig2()
FFT(Sig3(),IEEE4,128,20,uSec,4)'Perform FFT on Sig3()
FFT(Sig4(),IEEE4,128,20,uSec,4)'Perform FFT on Sig4()
FFT(Sig5(),IEEE4,128,20,uSec,4)'Perform FFT on Sig5()
EndTable

BeginProg
Scan(3500,mSec,0,1)'Main Scan, 1 scan
'Measure Each Channel 128 times repeatedly
VoltSe(Sig1(),128,mv5000C,-5,-1,0,15000,1,0.0)
VoltSE(Sig2(),128,mv5000C,-6,-1,0,15000,1,0.0)
VoltSe(Sig3(),128,mv5000C,-7,-1,0,15000,1,0.0)
VoltSE(Sig4(),128,mv5000C,-8,-1,0,15000,1,0.0)
VoltSE(Sig5(),128,mv5000C,-9,-1,0,15000,1,0.0)

CallTable(PSDFFT)'Table runs FFTs on the measurements
NextScan
GetRecord(Sets,PSDFFT,1)'Retrieve the FFT results
CovSpa(CoVarVal(1),4,129,Sets(517),Sets(1))'Perform Spatial Covariances
EndProg
```

## Remarks

This instruction allows the user to perform spatial covariances on multiple sets of data against one core set of data. An individual covariance comparison is performed for each of the multiple sets against the base set.

Covariance is defined as:

n is the number of measurements in each measurement set whose covariance is to be calculated. J and K refer to the source array element number.

Both Source and Dest must be aFloat

**Note:** Four-byte floating-point data type. Default datalogger data type for Public or Dim variables. Same format as IEEE4.

data type (for example,Strings

**Note:** A data type used when declaring a variable consisting of alphanumeric characters.

orLongs

**Note:** Data type used when declaring a variable as an integer.

will not work).

If aNAN

**Note:** Not a number. A data word indicating a measurement or processing error. Voltage overrange, SDI-12 sensor error, and undefined mathematical results can produce NAN.

is returned by the datalogger it is not included in the spatial covariance.

## Parameters

# Dest (Destination Variable)

A variable in which to store the results of the instruction. The array must be dimensioned to at least the value of NumOfCov.

# NumOfCov (Number of Covariances)

The number of Covariances to be calculated. If four data sets are to be compared against a fifth set, this argument would be set to four.

Type: Constant (or expression that evaluates as a constant)

# SizeOfSets (Size of Data Sets to Compare)

The size of the data sets that are to be compared.

Type: Constant (or expression that evaluates as a constant)

# CoreSets (Core Data Set)

The array element that holds the core data set's first value. This core data set will be compared to each of the other sets of data independently for calculating the covariances.

Type: Array

# DataSets (Data Sets)

The array element that contains the first set's first data point that will be compared to the CoreSet for calculating the covariances. If multiple covariances are to be performed, the data sets have to be loaded consecutively into one array.

This argument must be dimensioned to at least the value of NumOfCov multiplied by SizeOfSets. For example, if each set of data has 100 elements (SizeOfSets) and there are 4 sets of data (NumOfCov) that are to be compared against the CoreSet, then the DataSets array would need to be dimensioned to 400 (4 X 100) and the DataSets argument would be set to DataSets(1).

Type: Array

If the CovSpa instruction is SpaCov(Dest(1),4,100,Core(1),DataSets(1)) then there will be four covariance relationship values calculated and sent to the Dest array. Dest will need to be dimensioned as a one dimensional, four element array.

**NOTE:** This instruction useshigh precision math

**Note:** A normal single precision float has 24 bits of mantissa. With high precision, a 32 bit extension of the mantissa is saved and used internally, resulting in 56 bits of precision. Instructions that use high precision are Average, AvgRun, AvgSpa, CovSpa, MovePrecise, RMSSpa, StdDev, StdDevSpa, TotalRun, and Totalize.

. A normal single precision float has 24 bits of mantissa. With high precision, a 32 bit extension of the mantissa is saved and used internally, resulting in 56 bits of precision. Instructions that use high precision are [AddPrecise](addprecise.md),[Average](average.md),[AvgRun](avgrun.md),[AvgSpa](avgspa.md),[CovSpa](#),[MinRun](minrun.md),[MaxRun](maxrun.md),[MovePrecise](moveprecise.md),[RMSSpa](rmsspa.md),[StdDev](stddev.md),[StdDevRun](stddevrun.md),[StdDevSpa](stddevspa.md),[TotalRun](totalrun.md), and [Totalize](totalize.md).
