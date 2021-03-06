<p align="center">
  <img width=100% src="https://github.com/buntylm/Swift-Tips-and-Tricks/blob/master/header.png">
</p>

#### Sorted, to sort and return the array
```swift
let unsortedUnumbers = [1,6,3,2]
print(unsortedUnumbers.sorted())
```

#### Sort, sort the same array reference, make sure it's var
```swift
var unsortNumbers = [1,6,3,2]
unsortNumbers.sort()
print(unsortNumbers)
```

#### Sort by where we can compare the object to return bool result, update the same array.
```swift
var arrayForSortBy = [1,6,3,2]
arrayForSortBy.sort { (n1, n2) -> Bool in
    return n1 > n2
}
print(arrayForSortBy)
```

#### Sort by where we can compare the object to return bool result, and return the array
```swift
let result = arrayForSortBy.sorted { (n1, n2) -> Bool in
    return n1 < n2
}
print(result)
```

#### Closure syntax
```swift
let sortSignature1 = arrayForSortBy.sorted { $0 > $1 }
print(sortSignature1)

let sortSignature2 = arrayForSortBy.sorted (by: >)
print(sortSignature2)
```

#### Map
```swift
let numbersForMap = [3,1,6]
let resultForMap = numbersForMap.map { (n1) -> String in
    return "\(n1)"
}
print(resultForMap)

let closureSyntaxResultForMap = numbersForMap.map { "\($0)" }
print(closureSyntaxResultForMap)
```
#### Filter
```swift
let numbersForFilter = [3,1,6]
let filterResult = numbersForFilter.filter { (n1) -> Bool in
    return n1 >= 3
}
print(filterResult)

let closureSyntaxFilterResult = numbersForFilter.filter { $0 > 3 }
print(closureSyntaxFilterResult)
```
#### Reduce
```swift
let numbersForReduce = [3,1,6]
let resultForReduce = numbersForReduce.reduce(0) { (result, n1) -> Int in
    return result + n1
}
print(resultForReduce)

let closureSyntaxReduceResult = numbersForReduce.reduce(0) { $0 + $1 }
print(closureSyntaxReduceResult)
```

#### Defining function as operator
```swift
let binaryRange =    { (0...1).contains($0) }
print(binaryRange(1))
print(binaryRange(2))
```

#### Difference between Self and self. Self refers to the type of the current "thing" inside of a protocol
```swift
protocol ProtocolName {

}

extension ProtocolName where Self : UIView {
    func addShadow() {

    }
}
```

#### Associated type, powerful way of making protocols generic
```swift
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
```

####  struct define a custom init without loosing the complier generated one
```swift
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
print(p2)
```

#### Throttling (process responsible for regulating the rate at which application processing is conducted)
🔗 [For more, read the full blog here at bmnotes.com](https://bmnotes.com/2020/04/23/how-to-perform-throttling-in-swift-debounce/)
```swift
protocol Throttable {
    func perform(with delay: TimeInterval,
                 in queue: DispatchQueue,
                 block completion: @escaping () -> Void) -> () -> Void
}

extension Throttable {
    func perform(with delay: TimeInterval,
                 in queue: DispatchQueue = DispatchQueue.main,
                 block completion: @escaping () -> Void) -> () -> Void {
        
        var workItem: DispatchWorkItem?
        
        return {
            workItem?.cancel()
            workItem = DispatchWorkItem(block: completion)
            queue.asyncAfter(deadline: .now() + delay, execute: workItem!)
        }
    }
}
```

#### How to test a static func
```swift
import Foundation
import AppTrackingTransparency
import PlaygroundSupport

protocol PrivacyManagerBridge {
    static func requestTrackingAuthorization(completionHandler completion: @escaping (ATTrackingManager.AuthorizationStatus) -> Void)
}

extension ATTrackingManager: PrivacyManagerBridge {}

protocol AppPrivacyManagerContract {
    func requestTrackingAuthorization(completionHandler completion: @escaping (ATTrackingManager.AuthorizationStatus) -> Void)
}

class AppPrivacyManager: AppPrivacyManagerContract {
    private var trackable: PrivacyManagerBridge.Type!

    init(trackable: PrivacyManagerBridge.Type = ATTrackingManager.self) {
        self.trackable = trackable
    }

    func requestTrackingAuthorization(completionHandler completion: @escaping (ATTrackingManager.AuthorizationStatus) -> Void) {
        self.trackable.requestTrackingAuthorization(completionHandler: completion)
    }
}

class ATTrackingManagerMock: PrivacyManagerBridge {
    static func requestTrackingAuthorization(completionHandler completion: @escaping (ATTrackingManager.AuthorizationStatus) -> Void) {
        // Return whichever flow you want to test.
        completion(.denied)
    }
}

let privacyManager = AppPrivacyManager(trackable: ATTrackingManagerMock.self)
privacyManager.requestTrackingAuthorization { result in
    // result here
}

PlaygroundPage.current.finishExecution()
```
