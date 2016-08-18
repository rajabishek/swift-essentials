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