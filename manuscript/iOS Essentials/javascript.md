The Abstract Equality Comparison Algorithm
The comparison x == y, where x and y are values, produces true or false. Such a comparison is performed as follows:
- 
- If one of the variable is undefined and the other is null then return true
- Of one is a number and the other is a string the string is implicitly coerced before comparison is performed
- If one of the variable is a boolean then it is implicitly coerced to a number before comparison is performed