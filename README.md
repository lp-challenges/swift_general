# swift_general
* Type_Safety:  we are required to define the types of the values we are going to store in a variable. We will get an error if we attempt to assign a value to a variable that is of the wrong type
* Type_Inference: allows us to omit the variable type when the variable is defined with an initial value. The compiler will infer the type based on that initial value. 
* Tuples: group multiple values into a single compound type. These values are not required to be of the same type.
* Enumerations: special data type that enables us to group related types together and use them in a type-safe manner. Enumerations in Swift are not tied to integer values as they are in other languages. In Swift, enumerations can also have associated values. Associated values allow us to store additional information, along with member values. This additional information can vary each time we use the member. It can also be of any type, and the types can be different for each member.
```
enum Product {
    case Book(Double, Int, Int)
    case Puzzle(Double, Int)
}

var masterSwift = Product.Book(49.99, 2017, 310)
var worldPuzzle = Product.Puzzle(9.99, 200)

switch masterSwift {
case .Book(let price, let year, let pages):
    print("Mastering Swift was published in \(year) for the price of \(price) and has \(pages) pages")

case .Puzzle(let price, let pieces):
    print("Mastering Swift is a puzzle with \(pieces) and sells for \(price)")
}

switch worldPuzzle {
case .Book(let price, let year, let pages):
    print("World Puzzle was published in \(year) for the price of \(price) and has \(pages) pages")
case .Puzzle(let price, let pieces):
    print("World Puzzle is a puzzle with \(pieces) and sells for \(price)")
}
```
## Optionals
Optionals are a special type in Swift. 
``` 
var myString1: String?
var myString2: Optional<String>
```
The optional type is an enumeration with two possible values, None and Some(T) where T is the generic associated value of the appropriate type.
Internally, an optional is defined as follows:
```
enum Optional<T> {
    case None    
    case Some(T)
}
```
When we forget to initialize an object or set a value for a variable, we can get unexpected results at runtime with Objective-c.
With optionals, Swift is able to detect problems such as this at compile time and alert us before it becomes a runtime issue.
If we expect a variable or object to always contain a value prior to using it, we will declare the variable as a non-optional.
* Optional binding is the recommended way to unwrap an optional.
* Optional chaining allows us to call properties, methods, and subscripts on an optional that might be nil.

## Collections
Multiple items into a single unit. The data stored in a Swift collection must be of the same type
* Arrays: tore data in an ordered collection
* Dictionaries: unordered collections of key-value pairs
* Sets: unordered collections of unique values

## Variadic parameters
A variadic parameter is one that accepts zero or more values of a specified type.
```
func sayHello(greeting: String, names: String...) {
    for name in names {
        print("\(greeting) \(name)")
    }
}
```

## inout parameters
If we want to change the value of a parameter and we want those changes to persist once the function ends, we need to define the parameter as an inout parameter.
```
func reverse(first: inout String, second: inout String) {
    let tmp = first
    first = second
second = tmp }
```
