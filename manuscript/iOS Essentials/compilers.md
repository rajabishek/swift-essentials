## Ambiguous grammars
Derivation is for a particular string from a particular grammar. If you are expanding the leftmost variable first in the derivation its called as the left most derivation. If you are expanding the rightmost variable first in the derivation its called as the right most derivation. For a grammar if we take any string and if it has more than one left most derivation, or more that one right most derivation or more than one parse tree then the grammar is said to be ambiguous. Ambiguous in the sense if we have 2 parse trees then parsers will get confused on which one to generate and which one is right. 

The parse trees may have the same yield when it comes to strings, but if you feed it numbers the evaluation is completely different, one may not follow the bod mass rule and therefore that cannot be used at all. If a parse tree evaluation is correct for a grammar, then by looking at the grammar itself we can tell about the precedence of the operators and about the associativity of the operators. The operators that are present at the most deepest level of the tree are of higher precedence and the operators that are force the tree to grow in only one direction in left(left associative)or right(right associative).\

Almost all parsers except one will not allow or cannot work with ambiguous grammars. So if we have an ambiguous grammar we have to convert it to unambiguous grammar first.Its important to understand that, there are no algorithms for finding whether a grammar is ambiguous or not, you have to do hit and trail way by taking a string and checking whether it has more than two parse trees or not. Some of the ambiguous grammar can be converted to unambiguous grammars. And there are no algorithms for converting, you have to find the root cause by seeing the production rules that cause ambiguity(disambiguating rules) in the grammar and work on them.

A grammar, if the production that generates the operator is left recursive the that operator is left associative. Similarly in a grammar, if the production that generates the operator is right recursive the that operator is right associative. The precedence for operator is introduced by the concept of levels. Defining the correct associativity and precedence for grammar avoids ambiguity in the grammar.

## Removing ambiguity
Lets say we have a production rule such as E -> E + E | id, when we have such a rule the grammar allows us to grow the tree in both the direction, i.e the associativity of + operator is nor defined. If + operator is left associative then the tree should be allowed to grow only in the left direction. The rules most be E -> E + id | id, because we want the left subtree to be evaluated first if there are two + operators, thats what left associative means right. There if we need the operator to be left associative then ensure that the grammar is left recursive i.e the left most symbol in RHS is equal to LHS.

Secondly when there are 2 different operators, the highest precedence operator must be at the deepest level in the ensuring that only after evaluating that we come and evaluate the lower precedence operators. Therefore to solve the problem define the grammar to be of different levels.
```
E -> E + T | T (we are defining the E -> T production because the string may not have any + at all)
T -> T * F | F (we are defining the T -> F production because the string may not have any * at all)
F -> id
```
If you observe closely, the * is in the next level of +. Once we have reached a T in the tree we can never reach a +, thus making sure all the Ts are evaluated first. If you see the production rules + and * both are left associative and once we reach the 2nd production we can't generate any + at all, this makes sure that in the tree all expressions having * are evaluated first.
```
E -> E + T | T
T -> T * F | F
F -> G ^ F | G
G -> id
```
See the above rules the ^ power operator is right associative and the * and + operators are left associative and the precedence is ^ > * > +.
```
E -> E * F | F + E | F
F -> F - F | id
```
In the above grammar + and * are defined at the same level, therefore they have the same precedence. - is farther away from the start symbol there fore it has higher precedence than * and + operators. The precedence is - > + = *. The + operator is defined as right recursive and therefore making it right associative. The * operator is defined as left recursive and therefore making it left associative. For - the associativity in undefined cause its defines as both left ans right recursive thereby making the grammar ambiguous.

## Removing left recursion
Not all parsers are comfortable with left recursive grammars, especially the top down parsers have problems working with left recursive grammars. Therefore we have to convert left recursive grammars right recursive grammars.
```
A -> Aa | b //The grammar it generates is ba*
```
We want the same language to be generate by a right recursive grammar. So we can do the following.
```
A -> ba* // we want this to happen

A -> bG
G -> aG | e ( e is epsilon here)
```
As you can see above we have come up with a right recursive grammar for the same language generated by left recursive grammar.

## Removing non determinism using left factoring
```
A -> ab | ac | ad
```
The above is a non deterministic grammar, because lets say we have a string ad now first character it will read is a, on seeing this it will choose the 1st production. Then it finds by using that is that 2nd character is b. So it back tracks and tries using the 2nd production. The problem here is that instead of seeing the whole string at once we are just seeing the 1st character and making a decision for choosing which production to use, if we can post pone the decision process we can avoid the non determinism here. Most top down parsers are not comfortable with non deterministic grammars. So lets eliminate this problem, by defining a deterministic grammar without changing the language.
```
A -> aB
B -> b|c|d
```
The technique use here is called as left factoring. As you can see above, lets say we have a string ad now first character it will read is a, on seeing this it will choose the 1st production and its the right choice so no backtracking, then finds the 2nd character to be b and therefore uses the 3rd production. As you can see there is no backtracking here. This is a deterministic grammar. Its an important thing to note here that eliminating non determinism not eliminate ambiguity, a deterministic grammar can still be ambiguous.

## Parsers
The top down parsers without backtracking, the grammar must not be left recursive and it should not have non determinism. Any parser can work only with unambiguous grammars, expect one which is the operator precedence parser. Operator precedence parser can also handle ambiguous grammars because we define the operator precedence parsing table, in which we tell about the precedence and associativity of the operators thereby making it possible for the parser to work with it. See the parser needs to know about the precedence and associativity of the operators, if its present in the grammar itself it can identify it, but if it is not present then we can tell the parse about it and help it in the parsing, this is what we do with operator precedence parsers. Top down parser think about when to use a production rules and bottom up parsers think about when to reduce an expression. Top down parsers follow left most derivation and bottom up parsers follow reverse of right most derivation.

## First and Follow
First of a variable is if you start deriving from the variable and generate all the strings from it the what are the terminal that you see first. For example from a variable A you are able to generate string { aax, badb, ccaa, aama, ccc, bca} then {a,b,c} is the first of A. Whenever you get any epsilon you substitute that and look for the first of the next variable.
```
S -> AB
A -> a | e (e is epsilon)
B -> b
```
Now since A can generate epsilon when you look for first of S substituting e in S -> AB gives S -> B, therefore first of S is equal to first of B. Another things to note here is that the 2nd production rule generates epsilon therefore the first of A is {a,e}. So epsilon can be in the first of a variable if the variable directly generates it.

Follow of a variable is, in the process of deriving a string the set of terminal that follow a variable. Its the set of terminals that could follow a variable in the process of deriving a string. We are always going to start deriving the string from S$, this is the step from where we will start the derivation process. Therefore $ will always be in the follow of the starting symbol. Every string is actually formed using the right hand side of the productions so you can look at that to tell about the follow of a variable. To find the follow of a variable see the right hand side of all the productions, if a variable is there anywhere in the right hand side of a production then follow will be the first of the next variable following it.
```
S -> ABCD
A -> b
B -> c | e
C -> d | e
D -> m | e
```
In the above grammar to find the follow of A see the right hand side of all the productions yes in the first production there is A on the right hand side. Now follow of A is first of BCD. First of BCD is first of B. First of B is c and epsilon therefore c will be in the follow of A. Now substitute epsilon, in BCD which gives CD therefore first of CD is first of C, which is d and epsilon. Therefore d will be in the follow of A, now substituting epsilon gives D, therefore first of D is m and epsilon, therefore follow of A will have m now substituting give epsilon. Since we are getting epsilon follow of A is follow of the left hand side (S -> ABCD), i.e follow of S. Follow of S is $, therefore $ will be in the follow of A. Therefore follow of A is {c,d,m,$}. 

Also if a variable is at the right end of the production and there is nothing after it, then follow of that variable is follow of the left hand side. Whenever while finding the first of the string that follows the variable(BCD) if the string ends up being epsilon after continuous substitution then follow of the variable is the follow of the left hand side.

## LL(1) parsing table
A top down parser has to decide whenever there are 2 productions for a variable which production to use. LL(1) parsing table helps the LL(1) parser in this process. Variable are going to be the rows and the terminals with $ is going to be the columns. See the table means that if I see the row in the derivation process and get the column which production I have to choose, this depends on whether the first of the row contains the column or not. The procedure to fill this table is take each production one by one. If the production is of the form A -> .. then the entries go in the A row. So we find the first of the right hand side and in those columns we place this production rule. And whenever the variable directly generates epsilon then we are going to place the production also in the follow of the left hand side. And whenever the first of right hand side has epsilon then also we are going to place the production in the follow of the left hand side. 

A LL(1) parser can work only with a grammar if there is a single entry in the table cell. If we get multiple entries in a table cell then the parse cannot deal with that grammar.

## LL(1) parsing
Lets say you want to generate the string (())$, always when you start the bottom of the stack is $ and top of the stack is the start symbol. Initially the pointer points to the first character in the string. Now top of the stack is the row and the pointer is the column in the parsing table. Get the production from the table, and pop the stack and push in the stack such that left most symbol will appear on the top and start constructing the parse tree with S replaced by the production. If the production generates epsilon then we just pop from the stack. If the top of the stack and the input matches then pop the stack and increment the pointer. Finally when the pointer reaches $ and if $ is the only symbol in the stack then the string is accepted.

To check whether a grammar can be used for LL(1) parsing 1st it should not be left recursive. It should also not be non deterministic. But there are some grammars which is not left recursive and deterministic but still can't be used for LL(1) parsing because there are multiple entries in a table cell. Therefore to check whether a grammar can be used for LL(1) parsing just make sure that there aren't multiple entries in a any table cell.

Therefore given a grammar check this for each production. If a variable generates more than one production just make sure that in this case there is no intersection in the first of the right hand sides. If there is only one production that is generated by a variable then its not an issue, but if same variable has multiple production then if there is intersection in the first of right hand side then that grammar cannot be used for LL(1) parsing. The first of the right hand sides of a variable must be mutually exclusive. Or the other case where the grammar cannot be used is, if we have a variable that has more than 1 production and one of the production generates epsilon then first of the right hand side of the non epsilon production and the follow of the variable has an intersection.
```
A -> B | e (where e is epsilon)
``
Here if first of B and follow of A has intersection then this grammar cannot be used for LL(1) parsing.

## LR parsers
All LR parsers have the same parsing algorithm, only the parsing table will change for all of them. Here like first and follow we have concepts called closure and goto. The LR(0) and SLR(1) parsing use what's called as canonical collection of LR(0) items for the construction of the parsing table. And the CLR(1) and LALR(1) parsing use what's called as LR(1) items.

## Canonical collection of LR(0) items
Whenever any grammar is given you add an extra production to the grammar, you augment it. The result is called as augmented grammar. Do this S' -> S, S -> .... 
Any production with a dot in the right hand side is called as an item. Now start with S' -> .S this is the LR(0) item for the first production, now apply the closure. Closure is nothing but, whenever there is a dot to the left of a variable then you have a write all the productions of that variable with dot in the beginning. You keep on applying the closure for every production you get in this process. This set of productions that you get is called closure of S' -> .S

The closure that we got is a state. Now we have to do the goto. Wherever there is a dot we have to move it to the next symbol. For every goto apply the closure and get that state, if that state is already present then just link it. Whenever dot is to the right most side we get whats called as a final item. For final items you can't proceed any further. For the other states repeat the same process by finding the goto for each state and the closure for the goto items.

The final items are those in which the dot is on the right most side. The significance of the final items is that, we have seen everything in the right hand side therefore we can reduce the right hand side to the left variable. This state diagram is called as canonical collection of LR(0) items and using this we are going to construct the parsing table. Name every state with I suffix a number like I0, I1, I2 ... and so on.
