1. The question is incomplete.
2. If its not passed by reference then it would pass by value. If the argument is passed by value, its copy constructor would call itself to copy the actual parameter to formal parameter. This process would go on until the system runs out of memory.
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
```
