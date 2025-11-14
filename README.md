# Count

A Pine Script library of functions for counting the number of times (frequency) that elements occur in an array or matirx.

## Usage

[View on TradingView](https://www.tradingview.com/script/NCvGg3Ef-Count/)

Import the Count library.

```pine
import joebaus/count/1 as c
```

Create an array or matrix that is a `float`, `int`, `string`, or `bool` type to count elements from, then call the count function on the array or matrix.

```pine
floatArray = array.from(1.00, 1.50, 1.25, 1.00, 0.75, 1.25, 1.75, 1.25)
countMap = floatArray.count() // Alternatively: countMap = c.count(floatArray)
```

The "count map" will return a map with keys for each unique element in the array or matrix, and with respective values representing the number of times the unique element was counted. The keys will be the same type as the array or matrix counted. The values will always be an `int` type.

```pine
array<float> mapKeys = countFloat.keys() // Returns unique keys [1.00, 1.50, 1.25, 0.75, 1.75]
array<int> mapValues = countFloat.values() // Returns counts [2, 1, 2, 1, 1]
```

If an array is in ascending or descending order, then the keys of the map will also generate in the same order.

```pine
intArray = array.from(2, 2, 2, 3, 4, 4, 4, 4, 4, 6, 6) // Ascending order
map<int, int> countMap = intArray.count() // Creates a "count map" of all unique elements
array<int> mapKeys = countMap.keys() // Returns [2, 3, 4, 6] // Ascending order
array<int> mapValues = countMap.values() // Returns count [3, 1, 5, 2]
```

Include a value to get the count of only that value in an array or matrix.

```pine
floatMatrix = matrix.new<float>(3, 3, 0.0)
floatMatrix.set(0, 0, 1.0), floatMatrix.set(1, 0, 1.0), floatMatrix.set(2, 0, 1.0)
floatMatrix.set(0, 1, 1.5), floatMatrix.set(1, 1, 2.0), floatMatrix.set(2, 1, 2.5)
floatMatrix.set(0, 2, 1.0), floatMatrix.set(1, 2, 2.5), floatMatrix.set(2, 2, 1.5)

int countFloatMatrix = floatMatrix.count(1.0) // Counts all 1.0 elements, returns 5
// Alternatively: int countFloatMatrix = c.count(floatMatrix, 1.0)
```

The string method of the count function is a special case because it has a `regex` parameter instead of a `value` parameter. Enabling the use of a regular expression like `'bull*'` to count all matching occurrences in a string array.

```pine
stringArray = array.from('bullish', 'bull', 'bullish', 'bear', 'bull', 'bearish', 'bearish')
int countString = stringArray.count('bullish') // Returns 2
int countStringRegex = stringArray.count('bull*') // Returns 4
```

To count multiple values, use an array of values. This will return a map like `id.count()`, but only return a count map for the elements in the paramaterized array..

```pine
countArray = array.from(1.0, 2.5)
map<float, int> countMap = floatMatrix.count(countArray)
array<float> mapKeys = countMap.keys() // Returns keys [1.0, 2.5]
array<int> mapValues = countMap.values() // Returns counts [5, 2]
```

Multiple regex patterns or strings can be counted as well.

```pine
matrix<string> stringMatrix = matrix.new<string>(3, 3, '')
stringMatrix.set(0, 0, 'a'), stringMatrix.set(1, 0, 'a'), stringMatrix.set(2, 0, 'a')
stringMatrix.set(0, 1, 'b'), stringMatrix.set(1, 1, 'c'), stringMatrix.set(2, 1, 'd')
stringMatrix.set(0, 2, 'a'), stringMatrix.set(1, 2, 'd'), stringMatrix.set(2, 2, 'b')

// Count the number of times the regex patterns `'^(a|c)$'` and `'^(b|d)$'` occur
array<string> regexes = array.from('^(a|c)$', '^(b|d)$')
map<string, int> countMap = stringMatrix.count(regexes)
array<string> mapKeys = countMap.keys() // Returns ['^(a|c)$', '^(b|d)$']
array<int> mapValues = countMap.values() // Returns [5, 4]
```

An optional comparison operator can be specified to count the number of times an equality was satisfied for `float`, `int`, and `bool` methods of `count()`.

```pine
intArray = array.from(2, 2, 2, 3, 4, 4, 4, 4, 4, 6, 6)

// Count the number of times an element is greater than 4
countInt = intArray.count(4, '>') // Returns 2
```

When passing an array of values to count and a comparison operator, the operator will apply to each value.

```pine
intArray = array.from(2, 2, 2, 3, 4, 4, 4, 4, 4, 6, 6)
values = array.from(3, 4)

// Count the number of times and element is greater than 3 and 4
map<int, int> countMap = intArray.count(values, '>')
array<int> mapKeys = countMap.keys() // Returns [3, 4]
array<int> mapValues = countMap.values() // Returns [7, 2]
```

Multiple comparison operators can be applied when counting multiple values.

```pine
matrix<int> intMatrix = matrix.new<int>(3, 3, 0)
intMatrix.set(0, 0, 2), intMatrix.set(1, 0, 3), intMatrix.set(2, 0, 5)
intMatrix.set(0, 1, 2), intMatrix.set(1, 1, 4), intMatrix.set(2, 1, 2)
intMatrix.set(0, 2, 5), intMatrix.set(1, 2, 2), intMatrix.set(2, 2, 3)

values = array.from(3, 4)
comparisons = array.from('<', '>')

// Count the number of times an element is less than 3 and greater than 4
map<int, int> countMap = intMatrix.count(values, comparisons)
array<int> mapKeys = countMap.keys() // Returns [3, 4]
array<int> mapValues = countMap.values() // Returns [4, 2]
```
