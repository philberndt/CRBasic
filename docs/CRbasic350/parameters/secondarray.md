# SecondArray (Second Array)

The name of the array that contains the upper bin limits to compare the second element of the Source array against. The first range is everything below the first element in the SecondArray. The second range is all values greater than or equal to the first element and less than the second element of the SecondArray; and so on. The number of ranges that the second dimension has is determined by the SecondDim argument, so this array must be dimensioned to at least the value of the SecondDim argument. The boundary levels should be loaded into the arrays sequentially from the lowest values to the highest. Because it does not make sense to change the levels while the program is running, the program should be written to load the values into the array once before entering the scan. Right-click the parameter to display a list of defined variables.

Type: Array
