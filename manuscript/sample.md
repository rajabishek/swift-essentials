# The Basics
Swift is a powerful and intuitive programming language for iOS, OS X, tvOS, and watchOS that is safe fast and interactive. Writing Swift code is interactive and fun, the syntax is concise yet expressive, and apps run lightning-fast. It seems to me that Swift is designed to lure web developers to build apps. The syntax of Swift would be more familiar to web developers. If you have some programming experience with Javascript (or other scripting languages), it would be easier for you to pick up Swift rather than Objective-C.

# Data Types
Like any other language Swift uses variable to store and reference values by an identifying name. Swift also makes extensive use of variables whose values cannot be changed. These are know as constants and they are much more powerful that constants in C. Infact the entire Swift community encourages developers to use constants extensively where ever possible.

Swift provides its own versions of all fundamental C and Objective-C types, including `Int` for integers, `Double` and `Float` for floating point values, `Bool` for boolean values, and `String` for textual data. These are some of the primitive dataypes that are present in Swift. Apart from these Swift also provides three powerul collection types `Array`,` Set` and `Dictionary`.

Swift also introduces certain advanced types such a tuples. Tuple enables us to create and pass around a group of values. For example we can use this to return multiple values from a function as a single compound value. Lets say we have a function that returns the status of a http request, now using tuples we could return both the status code and the message as a single compound value from the function.

Swift also introduces optional types, which can handle the absense of a value. These are called as optionals. Optionals say either there is a value, and it equals x or there isn't a value at all. They are the heart of many of Swift's most powerful features.

Swift is a type safe language, which means that it helps you to be clear about the type of values your code can work with. If a part of your code expects an `Int`, type safety prevent you from passing it a value of another datatype by mistake. This can help developers catch and fix error as early as possible in the development process itself.

# Variables & Constants
Variables and constants are just a name given to a piece of data that is stored in memory. The difference between them is that, the value of a constant cannot be changed once it is set, whereas a variable can be set to a different value in the future. Variables are declared with the `var` keyword and constants are declared with the `let` keyword.
```swift
let language: String = "Swift"
var name: String = "Raj Abishek"

In this example `language` is declared as a constant and is given a value Swift but `name` is declared a variable and hence we can change its value if we want. You should also note that in Swift a semicolon is not neccessary. You need to use semicolon only when you need to write multiple statements on the same line.
```swift
language = "Objective-C"
```