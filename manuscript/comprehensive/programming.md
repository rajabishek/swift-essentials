3. Classes
4. There is no question at all.
5. Standard input(cin) stream object.
6. There are no options.
7. The question is incomplete.
8. Functions can be used to reuse code.
9. When we write the definition of the function before it is called.
10. The inline functions are a C++ enhancement feature to increase the execution time of a program. Functions can be instructed to compiler to make them inline so that compiler can replace those function definition wherever those are being called. Compiler replaces the definition of inline functions at compile time instead of referring function definition at runtime. To make any function as inline, start its definitions with the keyword **inline**.
11. Think of a reference as an alias. When you invoke something on a reference, you're really invoking it on the object to which the reference refers.
```c++
int i;
int& j = i; // j is an alias to i

j = 5; // same as i = 5


//int i is a value and the argument passed is passed by value.
//i will be a copy of x. Thus setting i to 5 has no effect on x, because it's the copy of x being changed.
void foo(int i)
{
    i = 5;
}
int x = 2;
foo(x);

//foo(x) no longer makes a copy of x; i is x.
//So if we say foo(x), inside the function i = 5; is exactly the same as x = 5;, and x changes.
void foo(int& i) // i is an alias for a variable
{
    i = 5;
}
int x = 2;
foo(x);
```
12. The parameter is converted to **int * a**. Remember that an array name in internally a pointer to the array.
13. There are no options for the questions.
14. x takes the default value of 3, therefore y and z take default values. Therefore answer is: 2.
15. The return type of a function. For resolving a overloaded function call the compiler does not check the return type, only the function signature is used(The no. of parameters, and type of parameters).
16. Different function signatures for C++ to resolve the function call.
17. There are no options.
18. The instance of the class and the member of the class.
19. The question is confusing.
20. This is not at all a question.
21. No options.
22. No options.
23. Not able to search.
24. A non constant pointer to constant data.
25. Construction always starts from the base class. Then it comes the turn for member fields. They are initialized in the order they are declared. At the last the class itself is constructed. The order of destructor is exactly reverse.
26. Class A can access private variables of class B, because in class B declaration we told it that A is your friend.
28. s1 is the calling object. Behind the scenes its operator overloading at work s1.<(s2)
30.
31.
32. We can use the getAge() and getCost() public methods to read the fields.
33. The member access operator, the ternary conditional operator, the scope resolution operator, the sizeof operator, pointer to member operator cannot be overloaded.
34. 2012
35.  &tPtr[3]
36. The left operand is an int.
37. No options.
38. There is no question.
39. No options.
