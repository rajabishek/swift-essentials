# Arrays
Arrays are one of the most powerful data structures in the swift programming language. It is data structure that is used to hold ordered collection of data. Arrays in swift are implemented as generic collections. Every value that is stored in an array is called as an element and the position of the element in the array starting from 0 is called the index of the array. An array can have same elements repeated in multiple index positions.

An array in swift is declared as follows.
```swift
var data: Array<String> //declare data as an array of strings - Longer form of type annotation
var names: [String] //declare names as an array of strings - Shorter form of type annotation

var array = ["Raj Abishek", "Sailesh Dev"] //type inference

var strings: [String] = [] //Initialise as an empty array
var anotherArray = [String]() //Initialise as an empty array
```

The statement `var simple = []` will not compile because there is no way for swift to infer the data type of the simple array. What is th data type of simple array ? Is it an array of Int, or array of String, or array of Double ? We can assign the empty array literal `[]` only to already types variable arrays.

