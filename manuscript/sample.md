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

There are 3 important things that one needs to remember when it come to variable and constants in Swift. Once you have declared a constant or a variable of a certain type, you can't redeclare it again with the same name. You can neither change it to store values of a different type. Nor can you change a variable to a constant or a constant to a variable. Violating any of these above 3 principles gives a compile time error.

# Type Inference & Type Annotation
We could have declared the `language` and `name` without providing the datatype also. Ok I know what you're thinking "Wait Raj, you just told me Swift is a type safe language, that means every variable or constant should have a definite type and changing data types shouldn't be possible". Yes you are right, but Swift has something called as `Type Inference` using which swift can infer or understand the type of variable/constant based on the initial value that you provide.
```swift
let language = "Swift"
var name = "Raj Abishek"
```

Since we have given the intial values Swift is smart enough to understand that both `language` and `name` are of datatype `String`. During compile time it can deduce the type of a particular expression automatically, simply by examining the nature of the values you provide. Because of this wondeful feature, Swift requires far fewer type declarations than languages such a C or Objective-C.

You should also note that if you want to explicity specify the type of a variable it is no harm in doing so. This is called as `Type Annotation`.
```swift
let language: String = "Swift"
var name: String = "Raj Abishek"
```

A quick thing to note here is that if you do not provide intial values while declaring a variable or a constant then type annotation is compulsory as shown below.
```swift
let language: String
var name: String
```

You can declare multiple contants/variables in the same line, separate by commas as shown below.
```swift
var i = 0, j = 0, k = 0
```

You can define multiple related variables of the same type on a single line, separated by commas, with a single type annotation after the final variable name.
```swift
var name, othername, anotherName: Double
```

# Comments
Use comments to include nonexecutable text in your code, as a note or reminder to yourself. Comments are ignored by the Swift compiler when your code is compiled. 
```swift
// This is a comment
```

Swift also has multiline comments and unlike C it can have nested multiline comments allowing developers to quickly commenting out a piece of code.
```swift
/* 
	This is the outer multiline comment
/*
	This is the nested multiline comment
*/
*/
```

# Semicolons
Unlike many other languages, Swift does not require you to write a semicolon (;) after each statement in your code, although you can do so if you wish. However, semicolons are required if you want to write multiple separate statements on a single line.
```swift
let language = 'Swift'; print(language)
//prints "Swift"
```

# Type Aliases
Type aliases define an alternative name for an existing type. You define type aliases with the typealias keyword.

Type aliases are useful when you want to refer to an existing type by a name that is contextually more appropriate, such as when working with data of a specific size from an external source.
```swift
typealias AudioSample = UInt16
```

Once you define a type alias, you can use the alias anywhere you might use the original name.
```swift
var maxAmplitudeFound = AudioSample.min
// maxAmplitudeFound is now 0
```

Here, AudioSample is defined as an alias for UInt16. Because it is an alias, the call to AudioSample.min actually calls UInt16.min, which provides an initial value of 0 for the maxAmplitudeFound variable.

# Tuples
Tuples group multiple values into a single compound value. The values within a tuple can be of any type and do not have to be of the same type as each other.
```swift
let http404Error = (404, "Not Found")
// http404Error is of type (Int, String), and equals (404, "Not Found")”
```