# Swift-Tips-and-Tricks

```swift
/*
 1. Sorted, to sort and return the array
 */
let unsortedUnumbers = [1,6,3,2]
print(unsortedUnumbers.sorted())

/*
 2. Sort, sort the same array reference, make sure it's var
 */

var unsortNumbers = [1,6,3,2]
unsortNumbers.sort()
print(unsortNumbers)

/*
 3. Sort by where we can compare the object to return bool result, update the same array.
 */
var arrayForSortBy = [1,6,3,2]
arrayForSortBy.sort { (n1, n2) -> Bool in
    return n1 > n2
}
print(arrayForSortBy)

/*
4. Sort by where we can compare the object to return bool result, and return the array
*/
let result = arrayForSortBy.sorted { (n1, n2) -> Bool in
    return n1 < n2
}
print(result)

/*
 5. Closure syntax
 */
let sortSignature1 = arrayForSortBy.sorted { $0 > $1 }
print(sortSignature1)

let sortSignature2 = arrayForSortBy.sorted (by: >)
print(sortSignature2)

/*
 6. Map
 */
let numbersForMap = [3,1,6]
let resultForMap = numbersForMap.map { (n1) -> String in
    return "\(n1)"
}
print(resultForMap)

let closureSyntaxResultForMap = numbersForMap.map { "\($0)" }
print(closureSyntaxResultForMap)

/*
 7. Filter
 */
let numbersForFilter = [3,1,6]
let filterResult = numbersForFilter.filter { (n1) -> Bool in
    return n1 >= 3
}
print(filterResult)

let closureSyntaxFilterResult = numbersForFilter.filter { $0 > 3 }
print(closureSyntaxFilterResult)

/*
 8. Reduce
 */

let numbersForReduce = [3,1,6]
let resultForReduce = numbersForReduce.reduce(0) { (result, n1) -> Int in
    return result + n1
}
print(resultForReduce)

let closureSyntaxReduceResult = numbersForReduce.reduce(0) { $0 + $1 }
print(closureSyntaxReduceResult)

/*
 9. Defining function as operator.
 */

let binaryRange =    { (0...1).contains($0) }
print(binaryRange(1))
print(binaryRange(2))

/*
 10. Difference between Self and self
 Self refers to the type of the current "thing" inside of a protocol
 */

protocol ProtocolName {

}

extension ProtocolName where Self : UIView {
    func addShadow() {

    }
}

/*
 11. Associated type, powerful way of making protocols generic
 */

protocol DataHolder {
    associatedtype T
    var items: [T] { get set }
    mutating func addItem(_ newItem: T)
}

extension DataHolder {
    mutating func addItem(_ newItem: T) {
        items.append(newItem)
    }
}

class NumberHolder : DataHolder {
    var items: [Int] = []
}

var numbers = NumberHolder()
numbers.addItem(0)
print(numbers.items)

class StringHolder : DataHolder {
    var items: [String] = []
}

var strings = StringHolder()
strings.addItem("One")
print(strings.items)

/*
 struct define a custom init without loosing the complier generated one
 */
struct Person {
    let name: String
}

extension Person    {
    init() {
        name = "Default name"
    }
}

let p1 = Person(name: "Name")
let p2 = Person()

print(p1)
print(p2)```
