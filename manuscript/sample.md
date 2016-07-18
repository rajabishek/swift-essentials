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

It is important to note that as shown above if the context already provides type information, such as a function argument or an already typed variable or constant you can create an empty array with an empty array literal, which is written as `[]`

## Array Properties

- `count` is a property that gives the number of elements that the array holds currently.
- `description` is a property that gives a textual representation of the array as a string.
- `endIndex` is the successor of last valid subscript argument. If the array has 4 elements then it means that the last index in the array is 3. There `endIndex` is 4 which is one more than the last index. For an empty array `endIndex` is 0. You can think of `endIndex` as the index position from which you will start inserting the next element you append to the end of the array.
- `startIndex` is always zero. Even if the array is empty then `startIndex` property is zero.
- `isEmpty` property returns a boolean indicating whether the array is empty or not. If the array is empty is retuns true otherwise false.
- `first` gets the first element from the array, if array is empty then returns nil, therefore the return type is essentially an optional.
- `last` gets the last element from the array, if array is empty then returns nil, therefore the return type is essentially an optional.

```swift
let array = ["iPhone", "iPad", "Macbook"]
print("The number of elements in the array is: \(array.count)")
print("The array contents are \(array.description)");
print("The next position for insert is: \(array.endIndex)")
print("The start index for array is: \(array.startIndex)")
```
