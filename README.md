# Swift-Tips-and-Tricks

### High level functions

```swift
/*
 Sorted, to sort and return the array
 */
let unsortedUnumbers = [1,6,3,2]
print(unsortedUnumbers.sorted())

/*
 Sort, sort the same array reference, make sure it's var
 */

var unsortNumbers = [1,6,3,2]
unsortNumbers.sort()
print(unsortNumbers)

/*
 Sort by where we can compare the object to return bool result, update the same array.
 */
var arrayForSortBy = [1,6,3,2]
arrayForSortBy.sort { (n1, n2) -> Bool in
    return n1 > n2
}
print(arrayForSortBy)

/*
Sort by where we can compare the object to return bool result, and return the array
*/
let result = arrayForSortBy.sorted { (n1, n2) -> Bool in
    return n1 < n2
}
print(result)

/*
 Closure syntax
 */
let sortSignature1 = arrayForSortBy.sorted { $0 > $1 }
print(sortSignature1)

let sortSignature2 = arrayForSortBy.sorted (by: >)
print(sortSignature2)

/*
 Map
 */
let numbersForMap = [3,1,6]
let resultForMap = numbersForMap.map { (n1) -> String in
    return "\(n1)"
}
print(resultForMap)

let closureSyntaxResultForMap = numbersForMap.map { "\($0)" }
print(closureSyntaxResultForMap)

/*
 Filter
 */
let numbersForFilter = [3,1,6]
let filterResult = numbersForFilter.filter { (n1) -> Bool in
    return n1 >= 3
}
print(filterResult)

let closureSyntaxFilterResult = numbersForFilter.filter { $0 > 3 }
print(closureSyntaxFilterResult)

/*
 Reduce
 */

let numbersForReduce = [3,1,6]
let resultForReduce = numbersForReduce.reduce(0) { (result, n1) -> Int in
    return result + n1
}
print(resultForReduce)

let closureSyntaxReduceResult = numbersForReduce.reduce(0) { $0 + $1 }
print(closureSyntaxReduceResult)```
