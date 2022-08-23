# HW06_Truth_Tables_CNF
---
### Symbol Reference
- $\lnot$ logical not (sometimes written as !)
- $\lor$ logical or
- $\land$ logical and
- $\rightarrow$ implication (if A then B)
- $\leftrightarrow$ biconditional (if and only if)

---
### Question 1

Construct the truth table for each of the following expressions. Indicate for each expression which of the following fits: 
- tautology (Always true)
- contradiction (cannot be true)
- contingent (meaning that it is neither a tautology or a contradiction)

 a. 

$$(P \land (P \rightarrow Q)) \land \lnot Q$$
  
 b.
 
 $$(P \rightarrow Q) \leftrightarrow (\lnot P \lor Q)$$
 
 c.
 
$$(Q \land (P \rightarrow Q)) \rightarrow P$$

---
### Question 2

Reduce the expression 

$$Q \lor \lnot((P \rightarrow Q) \land P)$$

to T (meaning true). You must justify every step with the law (or laws) you use for the step. You can find the laws in the textbook or in the Formal Logic Document (in slack pinned to the #homework channel).

---
### Question 3

Algebraically find the conjunctive normal form of the following expression:

$$P \rightarrow ((Q \land R) \leftrightarrow S)$$

Justify every step with the law (or laws) you use for the step.

Hints

Biconditional Equivalence:

$$A \leftrightarrow B \equiv ((A \rightarrow B) \land (B \rightarrow A))$$

CDE (Conditional Disjunctive Equivalence):
 
$$A \rightarrow B \equiv \lnot A \lor B$$
