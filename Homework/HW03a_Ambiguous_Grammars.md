# HW03a_Ambiguous_Grammars
---
### Question 1
Sometimes an expression can have two or more kinds of balanced parentheses.
For example Java expressions can have both round and square parentheses and both must be
balanced; that is, every `(` must match `)`, and every `[` must match `]`.
Write a grammar for strings of balanced parentheses of these two types.

For example `( [ ] ( [ ( ) ] ) )` is in the language but `[ ( ] )` is not.

---
### Question 2

Demonstrate that the following grammar is ambiguous. Create your own ambiguous case. (`<S>` is the start symbol.)

```
<S> -> b <A>
<S> -> b <A> e <A>
<A> -> <S>
<A> -> s
```

---
### Question 3

The following grammar for a fictitious operator '$' is ambiguous.
```
<number> ::= 0|1|2|3
<expression> ::= <number>
<expression> ::= <expression>$<expression>
```

Demonstrate the ambiguity by creating two parse trees for the expression 2 $ 3 $ 0.


---
### Question 4

Fix the grammar in Problem #3 so that it is not ambiguous. Is your grammar left associative or right associative?
