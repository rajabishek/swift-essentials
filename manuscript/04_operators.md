# Operators in Swift
An operator is a special symbol or phrase that you use to check, change, or combine values. In essence an operator is a symbol or phrase that represents a functionality. For example, `name = "String"` in Swift uses the assignment operator = which has a functionality of storing the string value "Apple" in the `name` variable. 

Many languages also have phrases that are operators, like `typeof` in Javascript, `sizeof` in C,and the type-cast operator `as` in Swift are some of the examples. The values that operators affect are operands.

## Arity
In computer science, the arity of a function or operation is the number of arguments or operands that the function takes. Therefore the operators can be classified based on their arity:
> Unary operators - operate on a single target
> Binary operators - operate on two targets
> Ternary operators - operate on three targets

Unary operators operate on a single target (such as -a). Unary prefix operators appear immediately before their target (such as !b), and unary postfix operators appear immediately after their target (such as c!).

Binary operators operate on two targets (such as 5 * 6) and are infix because they appear in between their two targets.

Ternary operators operate on three targets. Like C, Swift has only one ternary operator, the ternary conditional operator (a ? b : c).

## Fixity
