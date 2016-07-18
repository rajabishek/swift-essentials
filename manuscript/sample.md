# Arrays
Arrays are one of the most powerful data structures in the Swift programming language. An array stores values of the same type in an ordered list. The same value can appear in an array multiple times at different positions. Arrays in Swift are implemented as generic collections. Every value that is stored in an array is called as an element and the position of the element in the array (0 based) is called the index of the element. If you do not want to have repeated elements then you should have a look at the `Set` data structure. Sets are unordered collection of unique values.

An array in Swift is declared as follows.
```swift
var data: Array<String> //declare data as an array of strings - Longer form of type annotation
var names: [String] //declare names as an array of strings - Shorter form of type annotation

var array = ["Macbook", "iMac"] //type inference

var strings: [String] = [] //Initialise as an empty array
var anotherArray = [String]() //Initialise as an empty array
```

he statement `var simple = []` will not compile because there is no way for Swift to infer the data type of the simple array. What is the data type of simple array ? Is it an array of `Int`, or array of `String`, or array of `Double` ?

It is important to note that as shown above if the context already provides type information, such as a function argument or an already typed variable or constant you can create an empty array with an empty array literal, which is written as `[]`.