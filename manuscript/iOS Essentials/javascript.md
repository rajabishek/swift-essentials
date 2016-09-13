## Introduction
- Javascript in interpreted language
- Javascript is case sensitive
- Semicolons are not compulsory to end statements in javascript. It is compulsory if you have to write multiple statements on a single line.
- Javascript is whitespace insensitive


## Javascript Functions
- Extra argumnets passed in function call are ignored
- Arguments that are not passed in function call are treated as undefined


- Using Number(..) (a built-in function) as shown is an explicit coercion from any other type to the number type.
- If you use the == loose-equals operator to make the comparison "99.99" == 99.99, JavaScript will convert the left-hand side "99.99" to its number equivalent 99.99.
- The console.log(number) command has to implicitly coerce that number value to a string to print it out.
- number.toFixed() lets us specify how many decimal places we’d like the number rounded to, and it produces the string as necessary.
- ES6 includes a new way to declare constants, by using const instead of var. If you tried to assign any different value to constant after that first declaration, your program would reject the change (and in strict mode, fail with an error—see “Strict Mode”).
- A block is defined by wrapping one or more statements inside a curly-brace pair { .. }
```js
// a general block
{
amount = amount * 2; console.log( amount ); // 199.98
}
```
This kind of standalone { .. } general block is valid, but isn’t as commonly seen in JS programs.  Typically, blocks are attached to some other control statement, such as an if statement or a loop. A block statement does not need a semicolon (;) to conclude it.

- The if statement expects a boolean, but if you pass it something that’s not already boolean, coercion will occur.
- JavaScript defines a list of specific values that are considered “falsy” because when coerced to a boolean, they become false—these include values like 0 and "". Any other value not on the “falsy” list is automatically “truthy”—when coerced to a boolean they become true.
- A function is generally a named section of code that can be “called” by name, and the code inside it will be run each time.
- In JavaScript, each function gets its own scope. Scope is basically a collection of variables as well as the rules for how those variables are accessed by name. Only code inside that function can access that function’s scoped variables.
- If one scope is nested inside another, code inside the innermost scope can access variables from either scope.
```js
function outer() { vara=1;
	function inner() { varb=2;
    	// we can access both `a` and `b` here
		console.log( a + b ); // 3 
	}
	inner();
    // we can only access `a` here
	console.log( a ); // 1 
}
outer();
```
- Code in one scope can access variables of either that scope or any scope outside of it.
- 