
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

Ok now that we understand that a class should be responsible for only one thing, so how do we identify the responsiblty of a class ?

Identifying the resposibilites of a class is pretty simple. Lets say you want to change the way how a particular feature is implemented and now to make that change if you have to make structural changes to the class, then that feature is a responsibility of this class. Therefore is you follow single resposibilty principle then any class in your project should have only one reason to change.

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

First lets analyse the responsibilties of this class. If we need to change how the bill is calculated, let say we don't bill the deserts anymore (deserts are free ) then we will have to change the `calculateAmount` method. If we need to change how tax is calculate for a food item then will have to change the `calculateTax` method. So as you can see here the `BillCalculator` class is responsible for multiple things, it is responsible for how the food items are billed and how the tax is calculated for a food item. The fact that we can identify multiple reasons to change signals a violation of the Single Responsibility Principle.

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

We now have two smaller classes that handle the two specific tasks. We have `TaxCalculator` class that is responsible for calculating the tax amount for a given food item and the `BillCalculator` is responsible for how the food items are billed.