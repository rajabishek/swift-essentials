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

# Arrays Properties

`count` is a property that gives the number of elements that the array holds currently.
`description` is a property that gives a textual representation of the array as a string.
`endIndex` is the successor of last valid subscript argument. If the array has 4 elements then it means that the last index in the array is 3. There `endIndex` is 4 which is one more than the last index. For an empty array `endIndex`. You can think of `endIndex` as the index position from which you will start inserting the next element you append to the end of the array.
`startIndex` is always zero. Even if the array is empty then `startIndex` property is zero.
`isEmpty` property returns a boolean whether the array is empty or not. If the array is empty is retuns true otherwise false.

```swift
let array: ["iPhone", "iPad", "Macbook"]
print("The number of elements in the array is: \(array.count)")
print("The array contents are \(array.description)");
print("The next position for insert is: \(array.endIndex)")
print("The start index for array is: \(array.startIndex)")
```

# Arrays Methods
In swift we cannot use the subscript syntax to append elements to the end of the array. We can only use the subscript syntax to get an element from the array or update an existing element from the array. Whenever an new element is added in an array or whenever an existing element is removed from an array the elements in the array are automatically reordered, i.e if we remove the first element then 2nd becomes 1st, the 3rd becomes 2nd and so on. Similarly if we remove the 3rd element from the array then the 4th element becomes 3rd, 5th becomes 4th and so on. 

```swift
let array: ["iPhone", "iPad", "Macbook"]
print("The first element in the array is: \(array[0])")
print("The second element in the array is: \(array[1])")
array[2] = "Macbook Pro"
print("The third element in the array is: \(array[2])")
``` 
The above statements are perfectly valid because whatever is placed within the square brackets only ranges from 0 to array.count - 1, anything supplied outside this range will result in a run time error. We cannot append an element to the end of the array using the subscript syntax, ie statement `array[array.count] = newvalue` will result in a run time error.

To append elements to the end of the array we use the `append(_)` method.
```swift
var array: ["iPhone", "iPad", "Macbook Pro"] //array is decalared as var to make it mutable
array.append("iMac")
array.append("Macbook Air")
print(array.count) //Prints 5
```

But the `append` method can only insert elements at the end of the array. To insert an element in between we have to use the `insert(_:atIndex:)` method.
```swift
var array: ["iPhone", "iPad", "Macbook Pro"]
array.insert("iMac", atIndex: 0)
array.insert("Macbook Air", atIndex: array.count)
```
The `atIndex` parameter of the insert method can be supplied with an integer argument ranging from 0 to array.count. Anything exeding this range will result in an run time error. Thus we can use this function to also insert an element to the end of the array by doing `array.insert(value, atIndex: array.count)`. If the array is empty then we can pass only 0 to the`atIndex` parameter of the method.

To remove an element from an array at a specific index we can use the `removeFromIndex(_)` method.
The argument supplied to this method is an integer that can range from 0 to array.count - 1.
If index outside of this range is supplied it gives a runtime error. The method removes the element from the array at the given index and returns the removed element. If the array is empty then any argument supplied to the method will result in a run time error.

```
var array: ["iPhone", "iPad", "Macbook Pro"]
let name = array.removeAtIndex(0)
print("The removed element is: \(name)") // The removed element is: iPhone
print(array.count) //Prints 2
```

To remove the last element from the array we can do `array.removeAtIndex(array.count - 1)` but we have another useful method for this. It is the `removeLast()` method. It removes the last element in array and returns it, if the array is empty then a call to this method results in a run time error.

```swift
var array: ["iPhone", "iPad", "Macbook Pro"]
let last = array.removeLast()
print("The last element that was removed is: \(last)")
```