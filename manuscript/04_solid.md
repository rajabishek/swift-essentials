
# SOLID

[SOLID](https://en.wikipedia.org/wiki/SOLID_(object-oriented_design)) is a mnemonic acronym introduced by Michael Feathers and Bob Martin that refers to a set of five principles, when applied together, intend to make it more likely that a programmer will create a system that is easy to maintain and extend over time. The five principles are as follows:
- S - [Single Responsibility Principle](https://en.wikipedia.org/wiki/Single_responsibility_principle)
- O - [Open/Closed Principle](https://en.wikipedia.org/wiki/Open/closed_principle)
- L - [Liskov Substitution Principle](https://en.wikipedia.org/wiki/Liskov_substitution_principle)
- I - [Interface Segregation Principle](https://en.wikipedia.org/wiki/Interface_segregation_principle)
- D - [Dependency Inversion Principle](https://en.wikipedia.org/wiki/Dependency_inversion_principle)

Let's dive deeper into each one of these principles and look at some examples. The code sample presented in this article will be written in Swift, however the principle are merely just guidelines that can be applied in any object oriented programming language while working on software to remove code smells.

## Single Responsibility Principle
The single responsibility principle states that every module or class should have responsibility over a single part of the functionality provided by the software. It helps keep classes and methods small and maintainable. In addition to keeping classes small and focused it also makes them easier to understand.

Ok now that we understand that a class should be responsible for only one thing, so how do we identify the responsibility of a class ?

Identifying the responsibilities of a class is pretty simple. Lets say you want to change the way how a particular feature is implemented and now to make that change if you have to make structural changes to the class, then that feature is a responsibility of this class. Therefore is you follow single responsibility principle then any class in your project should have only one reason to change.

A TaxCalculator class will need to change only if you want to change the way in which tax is calculated. For any other reason if you are making a change in the TaxCalculator class then it means that TaxCalculator is responsible for multiple things, which clearly violates the Single Responsibility Principle. Let’s look at an example of some code that isn’t following the principle:

```swift
enum FoodType {
    case Starter
    case MainCourse
    case Desert
}

struct Food {
    var name: String
    var price: Double
    var type: FoodType
}

class BillCalculator {
    
    var items: [Food]

    init(items: [Food]) {
        self.items = items
    }
    
    func calculateAmount() -> Double {
        var price = 0.0
        for food in items {
            price += food.price + calculateTax(food.price)
        }
        return price
    }
    
    func calculateTax(price: Double) -> Double {
        return 0.10 * price
    }
}
```

First lets analyze the responsibilities of this class. If we need to change how the bill is calculated, let say we don't bill the deserts anymore (deserts are free ) then we will have to change the `calculateAmount` method. If we need to change how tax is calculate for a food item then will have to change the `calculateTax` method. So as you can see here the `BillCalculator` class is responsible for multiple things, it is responsible for how the food items are billed and how the tax is calculated for a food item. The fact that we can identify multiple reasons to change signals a violation of the Single Responsibility Principle.

We can do a quick refactor and get our code in compliance with the Single Responsibility Principle. Let’s take a look:

```swift
class TaxCalculator {
    
    static func calculateTax(food: Food) -> Double {
        return 0.10 * food.price
    }
}

class BillCalculator {
    
    var items: [Food]

    init(items: [Food]) {
        self.items = items
    }
    
    func calculateAmount() -> Double {
        var price = 0.0
        for food in items {
            price += food.price + calculateTax(food)
        }
        return price
    }
    
    func calculateTax(food: Food) -> Double {
        return TaxCalculator.calculateTax(food)
    }
}
```

We now have two smaller classes that handle the two specific tasks. We have `TaxCalculator` class that is responsible for calculating the tax amount for a given food item and the `BillCalculator` is responsible for how the food items are billed. Now we will have to change the `TaxCalculator` only if we need to change the way in which tax is calculated for a food item, similarly we will need to change the `BillCalculator` class only if we want to change the way in which food items are billed.

## Open/Closed Principle
In object-oriented programming, the open/closed principle states that classes or methods should be open for extension, but closed for modification i.e, such an entity can allow its behavior to be extended without modifying its source code. Let’s look at an example of some code that isn’t following the principle:
```swift
class ReportManager {
    
    var data: [String]
    
    init(data:[String]) {
        self.data = data
    }
    
    func generateExcelReport() {
        //Generate the report in Excel format
    }
    
    func generatePDFReport() {
        //Generate the report in PDF format
    }
}
```

As you can see in the above code if we have to generate report in HTML format, now we will have to update the `ReportManager` class. This violates the Open/Closed Principle. What we essentially want is the ability to extend the behavior of the system without making modifications to the existing code. This is generally achieved through the use of patterns such as the strategy pattern. Let’s take a look at how we might modify this code to make it open to extension:
```swift
protocol CanGenerateReport {
    func generate(data: [String])
}

class ExcelReportGenerator: CanGenerateReport {
    func generate(data: [String]) {
        //Generate the report in Excel format
    }
}

class PDFReportGenerator: CanGenerateReport {
    func generate(data: [String]) {
        //Generate the report in PDF format
    }
}

class ReportManager {
    
    var data: [String]
    
    init(data:[String]) {
        self.data = data
    }
    
    func generateReport(generator: CanGenerateReport) {
        generator.generate(data)
    }
}
```
With this refactor we’ve made it possible to add new report generators without changing any existing code. To add a new report generator that can generate reports in HTML format we just add another new class called `HTMLReportGenerator` that conforms to the `CanGenerateReport` protocol and write its implementation. We don't even need to touch the `ReportManager` class to bring about this change.
```swift
class HTMLReportGenerator: CanGenerateReport {
    func generate(data: [String]) {
        //Generate the report in HTML format
    }
}
```

## Liskov Substitution Principle
This principle states that if S is a subtype of T, then objects of type T may be replaced with objects of type S without creating any unexpected or incorrect behaviors, i.e we should be able to replace any instances of a parent class with an instance of one of its children. 
In essence if a program module is using a Base class, then the reference to the Base class can be replaced with a Derived class without affecting the functionality of the program module. Let’s look at an example of some code that isn’t following the principle:
```swift
class Rectangle
{
    var width: Int = 0
    var height: Int = 0
    
    func setWidth(width: Int) {
        self.width = width;
    }
    
    func setHeight(height: Int) {
        self.height = height;
    }

    func getArea() -> Int {
        return self.width * self.height;
    }
}

class Square: Rectangle
{
    override func setWidth(width: Int) {
        self.width = width;
        self.height = width;
    }
    
    override func setHeight(height: Int) {
        self.height = height;
        self.width = height;
    }
}
```


## Interface Segregation Principle

This principle is less relevant in dynamic languages. Since loosely typed languages don’t require that types be specified in our code this principle can’t be violated.The principle states that no client should be forced to depend on methods it does not use. Let’s look at an example of some code that isn’t following the principle:
```swift
class ReportManager {
    
    var data: [String]
    
    init(data:[String]) {
        self.data = data
    }
    
    func generateReport() {
        //Generate the report with the data
    }
    
    func uploadReport(data: [String]) {
        // Upload the report with the data
    }
}

class Controller {
    
    func generate(generator: ReportManager) {
        generator.generateReport()
    }
}
```
Here we have a violation of the Interface Segregation Principle. Here the `generate` method of the `Controller` class depends on the `ReportManager` class for its implementation. The `ReportManager` class has two methods one for generating a report and another for uploading a report. Here the `generate` method of the `Controller` class depends on `generateReport` but does not care about `uploadReport`. Let’s take advantage of Swift’s protocols to fix this to adhere to the Interface Segregation Principle.
```swift
protocol CanGenerateReport {
    var data: [String] { get }
    
    func generateReport()
}

class ReportManager: CanGenerateReport {
    
    var data: [String]
    
    init(data:[String]) {
        self.data = data
    }
    
    func generateReport() {
        //Generate the report with the data
    }
    
    func uploadReport(data: [String]) {
        // Upload the report with the data
    }
}

class Controller {
    
    func generate(generator: CanGenerateReport) {
        generator.generateReport()
    }
}
```
Here as you can see now that the `generate` method now depends on the `CanGenerateReport` type. This is an improvement because now we can pass any instance to the `generate` method that adheres to the `CanGenerateReport` protocol. If you really think about it, all that the `generate` method needs is just a instance that knows how to generate a report, it doesn't actually depend on a `ReportManager` that knows to do a lot of other things also. 
We can also pass an instance of the following class to the `generate` method.
```swift
class PDFReportGenerator: CanGenerateReport {
    
    func generateReport() {
        //Generate the report in PDF format with the data
    }
}
```
Therefore interface segregation is all about being minimal on the data type of dependencies that you require to implement a particular piece of functionality.

## Dependency Inversion Principle
The Dependency Inversion Principle states that the  high-level modules (business logic) should not depend on low-level modules (database querying/IO). Often this pattern is used to achieve the Open/Closed Principle that we discussed above. Let’s look at an example following the principle:
```swift
protocol CanGenerateReport {
    func generate(data: [String])
}

class ExcelReportGenerator: CanGenerateReport {
    func generate(data: [String]) {
        //Generate the report in Excel format
    }
}

class PDFReportGenerator: CanGenerateReport {
    func generate(data: [String]) {
        //Generate the report in PDF format
    }
}

class ReportManager {
    
    var data: [String]
    
    init(data:[String]) {
        self.data = data
    }
    
    func generateReport(generator: CanGenerateReport) {
        generator.generate(data)
    }
}

let manager = ReportManager(["Apple Watch","Apple iPhone"]);

manager.generateReport(PDFReportGenerator()); //Generates report in PDF format
```
As you can see, our high-level object, the report manager, does not depend directly on an implementation of a lower-level object, PDF and Excel report generators. All that the `generateReport` method of the `ReportManager` class depends on is an instance that conforms to the `CanGenerateReport` protocol i.e an instance that knows how to generate a report. It can be any report generator (XML, PDF, HTML, Excel). It doesn't depend on the lower level implementation details, it only depends on a higher level abstraction.

The flexibility that we get by following this rule is that we can substitute any specific implementation easily.
```swift
let manager = ReportManager(["Apple Watch","Apple iPhone"]);

//manager.generateReport(PDFReportGenerator()); //Generates report in PDF format
manager.generateReport(ExcelReportGenerator()); //Generates report in Excel format
```
As you can see above we can easily change the report to be generated in Excel format instead of PDF by altering just a single line of code. It becomes easy to invert the dependency on the fly.

Lets say now your company wants you to generate reports in XML format. All that we need to do now is first create a `XMLReportGenerator` class that conforms to the `CanGenerateReport` protocol and write the implementation ( Open/Closed principle ) . Then change the dependency for the `generateReport` method to an instance of `XMLReportGenerator` class ( Dependency Inversion principle).
```swift
class XMLReportGenerator: CanGenerateReport {
    func generate(data: [String]) {
        //Generate the report in XML format
    }
}

let manager = ReportManager(["Apple Watch","Apple iPhone"]);
manager.generateReport(XMLReportGenerator()); //Generates report in XML format
```
If you give it a thought it really makes sense, why should the ReportManager class worry about generating an Excel / PDF / HTML report, all that it cares is that it should have an instance that it can use to generate a report. It should'nt really worry about the lower level implementation details.