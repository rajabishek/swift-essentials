# The Basics
Swift is a powerful and intuitive programming language for iOS, OS X, tvOS, and watchOS that is safe fast and interactive. Writing Swift code is interactive and fun, the syntax is concise yet expressive, and apps run lightning-fast. It seems to me Swift is designed to lure web developers to build apps. The syntax of Swift would be more familiar to web developers. If you have some programming experience with Javascript (or other scripting languages), it would be easier for you to pick up Swift rather than Objective-C.

# Data Types
Like any other language Swift uses variable to store and refrence values by an identifying name. Swift also makes extensive use of variable whose values cannot be changed. These are know as constants and they are much more powerful that constants in C. Infact the entire Swift community encourages developers to use constants extensively where ever possible.

Swift provides its own versions of all fundamental C and Objective-C types, including `Int` for integers, `Double` and `Float` for floating point values, `Bool` for boolean values, and `String` for textual data. These are some of the primitive dataypes that are present in swift. Apart from theses swift also provides three powerul collection types `Array`,` Set` and `Dictionary`. 

Swift also introduces certain advanced types such a tuples. Tuple enables us to create and pass around a group of values. For example we can use this to return multiple values from a function as a single compound value.

Swift also introduces optional types, which can handle the absense of a value. These are called as optionals. Optionals say either there is a value, and it equals x or there isn't a value at all. They are the heart of many of Swift's most powerful features.

Swift is a type safe language, which means that it helps you to be clear about the type of values your code can work with. If a part of your code expects a `Int`, type safety prevent you from passing it a value of another datatype by mistake. This can help developers catch and fix error as early as possible in the development process itself.

# Variables & Constants
Variables and constants are just a name given to a piece of data that is stored in memory. The difference between them is that, the value of a constant cannot be changed once it is set, whereas a variable can be set to a different value in the future. Variables are declared with the `var` keyword and constants are declared with the `let` keyword.
```swift
let language: String = "Swift"
var name: String = "Raj Abishek"
```

In this example `language` is declared as a constant and is given a value Swift but `name` is declared a variable and hence we can change its value if we want.
```swift
language = "Objective-C"
```

There are 3 important things that one needs to remember when it come to variable and constants in Swift. Once you have declared a constant or a variable of a certain type, you can't redeclare it again with the same name. You can neither change it to store values of a different type. Nor can you change a variable to a constant or a constant to a variable. Violating any of these above 3 principles gives a compile time error.

# Type Inference & Type Annotation
We could have declared the `language` and `name` without providing the datatype also. Ok I know what you're thinking "Wait Raj, you just told me Swift is a type safe language, that means every variable or constant should have a definite type and chnaging data types shouldn't be possible". Yes you are right, but Swift has something called a `Type Inference` using which swift can infer or understand the type of variable or constant based on the initial value that you provide.
```swift
let language = "Swift"
var name = "Raj Abishek"
```

Since we have given the intial values Swift is smart enough to understand that both `language` and `name` are of datatype `String`. During compile time it can deduce the type of a particular expression automatically, simply by examining the nature of the values you provide. Because of this wondeful feature, Swift requires far fewer type declarations that languages such a C or Objective-C.

You should also note that if you want to explicity specify the type of a variable it is no harm. This is called a `Type Annotation`.
```swift
let language: String = "Swift"
var name: String = "Raj Abishek"
```

A quick thing to note here is that if you do not provide intial values while declaring a variable then type annotation is compulsory as shown below.
```swift
let language: String
var name: String
```






 

