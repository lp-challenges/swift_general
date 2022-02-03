# swift_general
* Type_Safety:  we are required to define the types of the values we are going to store in a variable. We will get an error if we attempt to assign a value to a variable that is of the wrong type
* Type_Inference: allows us to omit the variable type when the variable is defined with an initial value. The compiler will infer the type based on that initial value. 
* Tuples: group multiple values into a single compound type. These values are not required to be of the same type.
* Enumerations: data type that enables us to group related types together and use them in a type-safe manner. Enumerations are not tied to integer values. Enumerations can also have associated values. Associated values allow us to store additional information, along with member values. This additional information can vary each time we use the member. It can also be of any type, and the types can be different for each member.
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
* **Optional binding**: to find out whether an optional contains a value, and if so, to make that value available as temporary. Can be used with if and while statements to check and extract some value.
* **Optional chaining**: is a process for querying and calling properties, methods, and subscripts that could be nil. If the optional contains a value,the call succeeds; if the optional is nil, the call returns nil. Multiple queries can be chained together, the entire chain fails if any link in the chain is nil.

## Collections
Multiple items into a single unit. The data stored in a Swift collection must be of the same type
* Arrays: store data in an ordered collection
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


## Classes vs. Structures
* Type: A structure is a value type, while a class is a reference type
* Inheritance: A structure cannot inherit from other types, while a class can
* Deinitializers: Structures cannot have custom deinitializers, while a class can

*Structures are value types. When we pass instances of a structure within our application, we pass a copy of the structure and not the original structure. Classes are reference types; therefore, when we pass an instance of a class within our application, a reference to the original instance is passed.

## Access controls
Access controls enable us to hide implementation details and expose the interfaces. We can assign specific access levels to properties, methods, and initializers 
* Open: Accessible from any import. Can be inherited and overridden out of the module.
* Public: Accessible from any import. Can be inherited and overridden **ONLY INSIDE** the module.
* Internal: It is the default. Can be used inside the module.
* Fileprivate: Allow access only within the same source file.
* Private: Only inside the class and extension.

## Key-path expressions as functions (swift 5.2)
```
let employee1 = EmployeeStruct(firstName: "Jon", lastName: "Hoffman", salaryYear: 90000)
let employee2 = EmployeeStruct(firstName: "Kailey", lastName: "Hoffman", salaryYear: 32000)
let employee3 = EmployeeStruct(firstName: "Kara", lastName: "Hoffman", salaryYear: 28000)
let employeeCollection = [employee1, employee2, employee3]

employeeCollection.map(\.firstName)
```
## Polymorphism
Polymorphism is a single interface to multiple types.<br>
Ability to interact with multiple types in a uniform manner.

## Protocol-Oriented Design
* protocol inheritance:  is where one protocol can inherit the requirements from one or more additional protocols. This is similar to class inheritance in OOP, but insteadof inheriting functionality, we are inheriting requirements. We can also inherit requirements from multiple protocols, whereas a class in Swift can have only one superclass. 
* protocol composition: allows types to conform to more than one protocol. This is one of the many advantages that protocol-oriented design has over object-oriented design. With object-oriented design, a class can have only one superclass. This
can lead to very large, monolithic superclasses. Having a single type that conforms to multiple protocols is called protocol composition
* protocol extensions: allow to provide method and property implementations to conforming types. They also allow us to provide common implementations to all the conforming types, eliminating the need to provide an implementation in each individual type or the need to create a class hierarchy

### where statement
Is used to filter things, like a conditional. <br>
It is always filtering something: `Get me all of X where X.x = y` <br>
Provides a constraint of the data type you want to work with. <br>
It is flexible and can be used in unrelated places:
* Iterations  
```
for animal in animals where animal is SeaAnimal {
    print("Only Sea Animal: \(index)")
}

for name in names where name.hasPrefix("Shubham") {
    print(name)
}
```
* Extensions
```
extension Animal where Self is SeaAnimal
extension Array where Element : Comparable
```
* Generics
```
func genericFunction<S where S: StringLiteralConvertible>(string: S){
    print(string)
}
```
* Conditionals
```
while let arr = mutableArray where arr.count < 5 {
    mutableArray?.append(0) // [0,0,0,0,0]
}

if let str = string where str == "checkmate" {
    print("game over") // match
} else {
    print("let's play")
}

guard let str = string where str != "checkmate" else {
    fatalError("game over") // match
}

var value = (1,2)
switch value {
    case let (x, y) where x == 1:
        // match 1
    break
    case let (x, y) where x / 5 == 1:
        // not-match
    break
    default:
    break
}
```
* Do Catch
```
do {
    try errorProne()
} catch ResponseError.HTTP(let code) where code >= 400 && code % 2 == 0 {
    print("Bad Request") // match
} catch ResponseError.HTTP(let code) where code >= 500 && code < 600  {
    print("Internal Server Error")
}
```

To me, the code base in a project that uses protocol-oriented design is much safer, easier to read, and easier to maintain.

## Generics
Generics allow us to write very flexible and reusable code that avoids duplication. With a type-safe language, such as Swift, we often need to write functions, classes, and structures that are valid for multiple types. Without generics, we need to write separate functions for each type we wish to support; however, with generics, we can write one generic function to provide the functionality for multiple types.

### Conditionally adding extensions with generics
```
extension List where T: Numeric {
    func sum () -> T {
       items.reduce (0, +)
    }
}
```

### Conditionally adding functions
```
extension List {
    func sum () -> T where T: Numeric {
       items.reduce (0, +)
    }
}
```
### Conditional conformance
```
extension List: Equatable where T: Equatable {
    static func ==(l1:List, l2:List) -> Bool {
        if l1.items.count != l2.items.count {
            return false
        }
        for (e1, e2) in zip(l1.items, l2.items) {
            if e1 != e2 {
                return false
} }
return true }
}
```

### Generic subscripts
```
subscript<T: Hashable>(item: T) -> Int {
    return item.hashValue
}

subscript<T>(key: String) -> T? {
    return dictionary[key] as? T
}
```

### Associated types
An associated type declares a placeholder name that can be used instead of a type within a protocol. The actual type to be used is not specified until the protocol is adopted.
#### Associated types vs. Generics
A protocol is an abstract set of requirements — a checklist that a concrete type must fulfill in order to say it conforms to the protocol. Traditionally one thinks of that checklist of being behaviors: methods or properties implemented by the concrete type. Associated types are a way of naming the things that are involved in such a checklist, and thereby expanding the definition while keeping it open-ended as to how a conforming type implements conformance.

When you see:
```
protocol SimpleSetType {
    associatedtype Element
    func insert(_ element: Element)
    func contains(_ element: Element) -> Bool
    // ...
}
```
What that means is that, for a type to claim conformance to SimpleSetType, not only must that type contain insert(_:) and contains(_:) functions, those two functions must take the same type of parameter as each other. But it doesn't matter what the type of that parameter is.

You can implement this protocol with a generic or non-generic type:

```
class BagOfBytes: SimpleSetType {
    func insert(_ byte: UInt8) { /*...*/ }
    func contains(_ byte: UInt8) -> Bool { /*...*/ }
}

struct SetOfEquatables<T: Equatable>: SimpleSetType {
    func insert(_ item: T) { /*...*/ }
    func contains(_ item: T) -> Bool { /*...*/ }
}
```
Notice that nowhere does BagOfBytes or SetOfEquatables define the connection between SimpleSetType.Element and the type used as the parameter for their two methods — the compiler automagically works out that those types are associated with the right methods, so they meet the protocol's requirement for an associated type.

On Generic Type Parameters
Where associated types expand your vocabulary for creating abstract checklists, generic type parameters restrict the implementation of a concrete type. When you have a generic class like this:

class ViewController<V: View> {
    var view: V
}
It doesn't say that there are lots of different ways to make a ViewController (as long as you have a view), it says a ViewController is a real, concrete thing, and it has a view. And furthermore, we don't know exactly what kind of view any given ViewController instance has, but we do know that it must be a View (either a subclass of the View class, or a type implementing the View protocol... we don't say).

Or to put it another way, writing a generic type or function is sort of a shortcut for writing actual code. Take this example:
```
func allEqual<T: Equatable>(a: T, b: T, c: T) {
    return a == b && b == c
}
```
This has the same effect as if you went through all the Equatable types and wrote:

func allEqual(a: Int, b: Int, c: Int) { return a == b && b == c }
func allEqual(a: String, b: String, c: String) { return a == b && b == c }
func allEqual(a: Samophlange, b: Samophlange, c: Samophlange) { return a == b && b == c }
As you can see, we're creating code here, implementing new behavior — much unlike with protocol associated types where we're only describing the requirements for something else to fulfill.

#### Conclusion
Associated types and generic type parameters are very different kinds of tools: associated types are a language of description, and generics are a language of implementation. They have very different purposes, even though their uses sometimes look similar (especially when it comes to subtle-at-first-glance differences like that between an abstract blueprint for collections of any element type, and an actual collection type that can still have any generic element). Because they're very different beasts, they have different syntax.

## Closures
Closures are blocks of code that can be passed around and used throughout our application. <br>
A closure is a strong reference by default in ARC.
* **non-escaping closure:**  When you are passing a closure as the function argument, the closure gets execute with the function’s body and returns the compiler back. As the execution ends, the passed closure goes out of scope and have no more existence in memory.
* **escaping closure:** when the closure is passed as an argument to the function, but is called after the function returns. It is retaines on memory until gets finished. 
    * Variables of function type are implicit escaping
    * typealiases are implicit escaping
    * Optional closures are implicit escaping   

## Memory Management
### value types
when we pass an instance of a structure within our application, such as a parameter of a method, we create a new instance
of the structure in the memory. This new instance of the structure is only valid while the application is in the scope where the structure was created. Once the structure goes out of scope, the new instance of the structure is automatically destroyed, and the memory is released. This makes the memory management of structures very easy and painless. 

### reference types
Classes, are reference types. This means that we allocate memory for the instance of the class only once, which is when it is initially created. When we pass an instance of the class within our application, either as a function argument or by assigning it to a variable, we are really passing a reference to where the instance is stored in memory. Since the instance of a class may be referenced in multiple scopes (unlike a structure), it cannot be automatically destroyed, and memory is not released when it goes out of scope because it may be referenced in another scope. Therefore, Swift needs some form of memory management to track and release the memory used by instances of classes when the class is no longer needed. 


### arc counts
* With ARC, for the most part, memory management in Swift simply works. ARC will automatically track the references to instances of classes, and when an instance is no longer needed (when there are no references pointing to it), ARC will automatically destroy the instance and release the memory.
* ARC counts how many times the instance is referenced; that is, how many active properties, variables, or constants are pointing to the instance of the class. Once the reference count for an instance of a class equals zero (that is, nothing is referencing the instance), the memory is marked for release.

### memory leak 
when memory is reserved for instances that are no longer needed.

### Strong reference cycles
The instances of two classes hold a strong reference to each other, preventing ARC from releasing either instance.

### Unowned and Weak references
The difference between a weak reference and an unowned reference is that the instance that a weak reference refers to can be nil, whereas the instance that an unowned reference is referring to cannot be nil. This means that when we use a weak reference, the property must be an optional property.

# Control flow
## The continue statement
#### The continue statement tells a loop to stop executing the code block and to go to the next iteration of the loop
```
for i in 1...10 {
    if i % 2 == 0 {
        continue
    }
    print("\(i) is odd")
}
```
`1 is odd`
`3 is odd`
`5 is odd`
`7 is odd`
`9 is odd`
## The break statement
#### The break statement immediately ends the execution of a code block within the control flow. 
```
for i in 1...10 {
    if i % 2 == 0 {
        break
    }
    print("\(i) is odd")
}
```
`1 is odd`

## GCD, DispatchQueue and NSOperationQueue
 [Repository LINK](https://github.com/lp-challenges/grand_central_dispatch/blob/main/README.md)
