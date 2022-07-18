# HW09_Predicate_Logic
---
### Question 1

Translate the following into predicate calculus (a symbolic “function based” representation of the statement). For each answer state your assumed domain.

>Example:
>	"All people who are eating ice-cream are happy"
>Answer:
>	f(x) returns true if the person x is eating ice-cream
>	h(x) returns true if the person x is happy
>	Domain: all people
>	$\forall x [f(x) \rightarrow h(x)]$

a. “Anyone who was an ancient Roman and tried to kill Caesar was not loyal to Caesar.”

b. “All cats which are calico, are female.”

c. “Some Texans have never left the state of Texas.

---
### Question 2

A universe contains the three individuals a, b, and c. For these individuals, a predicate Q(x, y) is defined, and its truth values are given by the following table:

|  x\\y   | a   | b   | c   |
| --- | --- | --- | --- |
| a   | T   | F   | T   |
| b   | F   | T   | F   |
| c   | F   | T   | T   |

Write each of the following expressions without quantifiers (i.e. convert them to expressions with ANDs and ORs or both) and then evaluate the expression.

a. $\forall w Q(w, b)$

b. $\forall r Q(r, r)$

c. $\forall w \exists r Q(w, r)$

---
### Question 3

Algebraically transform: $\lnot \forall x ((P(x) \land Q(y)) \rightarrow \exists z R(z))$

Into: $\exists x \forall z (P(x) \land Q(y) \land \lnot R(z))$

Justify each step with one or more laws.
