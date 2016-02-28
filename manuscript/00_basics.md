# The Basics
Swift is a powerful and intuitive programming language for iOS, OS X, tvOS, and watchOS that is safe fast and interactive. Writing Swift code is interactive and fun, the syntax is concise yet expressive, and apps run lightning-fast. It seems to me Swift is designed to lure web developers to build apps. The syntax of Swift would be more familiar to web developers. If you have some programming experience with Javascript (or other scripting languages), it would be easier for you to pick up Swift rather than Objective-C.

# Data type
Like any other language Swift uses variable to store and refrence values by an identifying name. Swift also makes extensive use of variable whose values cannot be changed. These are know as constants and much more powerful that constants in C.

Swift provides its own versions of all fundamental C and Objective-C types, including `Int` for integers, `Double` and `Float` for floating point values, `Bool` for boolean values, and `String` for textual data. These are some of the primitive dataypes that are present in swift. Apart from theses swift provides three powerul collection types `Array`,` Set` and `Dictionary`. 

Swift also introduces certain advanced types such a tuples. Tuple enables us to create and pass around a group of values. For example we can use this to return multiple values from a function as a single compound value by creating a tuple.

Swift also introduces optional types, which can handle the absense of a value. These are called as optionals. Optionals say either there is a value, and it equals x or there isn't a value at all. They are the heart of may of Swift's most powerful features.

Swift is a type safe language, which means that it helps you to clear about the type of values your code can work with. If a part of your code expects a `Int`, type safety prevent you from pass it a value of another datatype by mistake. This can help developers catch and fix error as early as possible in the development process itself.

# Variables & Constants
Variables and constants are just a name given to a piece of data that is stored in memory. The difference between them is that, the value of a constant cannot change once it is set, whereas a variable can be set to a different value in the future. Three importants things are to be remebered with respect to variable and constants in Swift. Variables are decalred with the `var` keyword and constants are declared with the `let` keyword.

```swift
let language: String = 'Swift'
var name: String = 'Raj Abishek'
```
In this example `language` is declared as a constant and is given a value Swift but `name` is declared a variable and hence we can change its value if we want.

```swift
language = 'Objective-C'
```

If we try to change a value of a constant Swift would throw a compile time error. There are 3 important things that one needs to remember when it come to variable and constants in Swift. Once you have declared a constant or a variable of a certain type, you can't redeclare it again with the same name. You can neither change it to store values of a different type. Nor can you change a variable to a constant or a constant to a variable. Violating any of these 3 principles gives a compile time error.








 

