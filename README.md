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


## Classes vs. Structures
* Type: A structure is a value type, while a class is a reference type
* Inheritance: A structure cannot inherit from other types, while a class can
* Deinitializers: Structures cannot have custom deinitializers, while a class can

*Structures are value types. When we pass instances of a structure within our application, we pass a copy of the structure and not the original structure. Classes are reference types; therefore, when we pass an instance of a class within our application, a reference to the original instance is passed. The functions can change the STRUCUTRE without affecting the original instance of the structure.*

## Access controls
Access controls enable us to hide implementation details and only expose the interfaces we want to expose. We can also assign specific access levels to properties, methods, and initializers 
* Open: Accessible from any module that imports the module they are defined in. Can be inherited and overridden out of the module.
* Public: Accessible from any module that imports the module they are defined in. Can be inherited and overridden **ONLY INSIDE** the module.
* Internal: It is the default. Can be used inside the module.
* Fileprivate: Allow access only within the same source file.
* Private: Only inside the class and its extension.

## Key-path expressions as functions (swift 5.2)
```
let employee1 = EmployeeStruct(firstName: "Jon", lastName: "Hoffman", salaryYear: 90000)
let employee2 = EmployeeStruct(firstName: "Kailey", lastName: "Hoffman", salaryYear: 32000)
let employee3 = EmployeeStruct(firstName: "Kara", lastName: "Hoffman", salaryYear: 28000)
let employeeCollection = [employee1, employee2, employee3]

employeeCollection.map(\.firstName)
```
## Polymorphism
Polymorphism is a single interface to multiple types, gives us the ability to interact with multiple types in a uniform manner.

## Protocol-Oriented Design
* protocol inheritance:  is where one protocol can inherit the requirements from one or more additional protocols. This is similar to class inheritance in OOP, but insteadof inheriting functionality, we are inheriting requirements. We can also inherit requirements from multiple protocols, whereas a class in Swift can have only one superclass. 
* protocol composition: allows types to conform to more than one protocol. This is one of the many advantages that protocol-oriented design has over object-oriented design. With object-oriented design, a class can have only one superclass. This
can lead to very large, monolithic superclasses. Having a single type that conforms to multiple protocols is called protocol composition
* protocol extensions: allow to provide method and property implementations to conforming types. They also allow us to provide common implementations to all the conforming types, eliminating the need to provide an implementation in each individual type or the need to create a class hierarchy

### where statement
```
for (index, animal) in animals.enumerated() where animal is SeaAnimal {
    print("Only Sea Animal: \(index)")
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

## Closures
Closures are blocks of code that can be passed around and used throughout our application.
A closure is a strong reference by default in ARC.

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
