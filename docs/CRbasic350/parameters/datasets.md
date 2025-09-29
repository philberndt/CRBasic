# DataSets (Data Sets)

The array element that contains the first set's first data point that will be compared to the CoreSet for calculating the covariances. If multiple covariances are to be performed, the data sets have to be loaded consecutively into one array.

This argument must be dimensioned to at least the value of NumOfCov multiplied by SizeOfSets. For example, if each set of data has 100 elements (SizeOfSets) and there are 4 sets of data (NumOfCov) that are to be compared against the CoreSet, then the DataSets array would need to be dimensioned to 400 (4 X 100) and the DataSets argument would be set to DataSets(1).

Type: Array
