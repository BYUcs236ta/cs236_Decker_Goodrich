# Project4

UNDER CONSTRUCTION Currently hosted on learning suite

---
# Table of Contents

- [General Project Requirements](#General-Project-Requirements)
- [Specific Project Requirement](#Specific-Project-Requirement)
- [Resources](#Resources)
- [Example Input and Output](#Example-Input-and-Output)
	- [Example Input](#Example-Input)
	- [Example Output](#Example-Output)
- [New Database Operations](#New-Database-Operations)
	- [Join](#Join)
	- [Union](#Union)
	- [Project operation](#Project-operation)
- [Evaluating Rules](#Evaluating-Rules)
- [The Fixed-Point Algorithm](#The-Fixed-Point-Algorithm)
- [Output Format](#Output-Format)
- [Rule Evaluation Output](#Rule-Evaluation-Output)
- [Query Evaluation Output](#Query-Evaluation-Output)
- [Implementation Requirements](#Implementation-Requirements)
- [Assumptions](#Assumptions)
- [FAQ](#FAQ)

---
## General Project Requirements

1.  Projects are to be completed by each student individually (not by groups of students)
2.  Projects must be passed-off (over Zoom or Gather) with a TA or by the pass-off server to be given credit
3.  All output should be sent to standard output, not a file
4.  Even though we provide the test cases, the definition of your project working is that it passes all tests on the `pass-off machine`
    -   If the test cases pass on your machine but not on the pass-off machine you should start by doing the following:
        -   Test your code and the official test cases on a Linux machine, where you should be able to replicate the error
        -   Look at how you initialize object. IDEs perform default initialization, but building from command-line with g++ on Linux does not

---
## Specific Project Requirement
1.  Your code will need to run in 60 seconds or less to pass the tests. If you have trouble meeting this requirement, see the subpage on how to speed up code.

---
## Resources
1. Read this first before starting any of the projects: [General Project Guide](/Guides/General-Project-Guide.md) 
2. READ THE FOLLOWING: [Project 4 Guide](/Guides/Project4-Guide.pdf)
3.  [Evaluating Rules Project 4](/Guides/Evaluating_Rules_Project_4.pdf)
4.  You may also find the [TA slides](https://docs.google.com/presentation/d/17Bz8ypKJMGWqW7uJKU26v6S4pc6exdOpbKAxA6j2ru8/edit?usp=sharing) and [help video](https://learningsuite.byu.edu/plugins/Upload/fileDownload.php?fileId=10ea88eb-b8QI-bHcV-hVJO-wB09b8e5ce10) useful!
5. [Test Cases](/Guides/Code_Testing.md)
6. [Project_Pass_Off_Instructions](/Guides/Project_Pass_Off_Instructions.md)

---
## Example Input and Output

---
### Example Input

~~~
Schemes:
  snap(S,N,A,P)
  csg(C,S,G)
  cn(C,N)
  ncg(N,C,G)

Facts:
  snap('12345','C. Brown','12 Apple St.','555-1234').
  snap('22222','P. Patty','56 Grape Blvd','555-9999').
  snap('33333','Snoopy','12 Apple St.','555-1234').
  csg('CS101','12345','A').
  csg('CS101','22222','B').
  csg('CS101','33333','C').
  csg('EE200','12345','B+').
  csg('EE200','22222','B').

Rules:
  cn(c,n) :- snap(S,n,A,P),csg(c,S,G).
  ncg(n,c,g) :- snap(S,n,A,P),csg(c,S,g).

Queries:
  cn('CS101',Name)?
  ncg('Snoopy',Course,Grade)?
~~~

---
### Example Output

~~~
Rule Evaluation
cn(c,n) :- snap(S,n,A,P),csg(c,S,G).
  C='CS101', N='C. Brown'
  C='CS101', N='P. Patty'
  C='CS101', N='Snoopy'
  C='EE200', N='C. Brown'
  C='EE200', N='P. Patty'
ncg(n,c,g) :- snap(S,n,A,P),csg(c,S,g).
  N='C. Brown', C='CS101', G='A'
  N='C. Brown', C='EE200', G='B+'
  N='P. Patty', C='CS101', G='B'
  N='P. Patty', C='EE200', G='B'
  N='Snoopy', C='CS101', G='C'
cn(c,n) :- snap(S,n,A,P),csg(c,S,G).
ncg(n,c,g) :- snap(S,n,A,P),csg(c,S,g).

Schemes populated after 2 passes through the Rules.

Query Evaluation
cn('CS101',Name)? Yes(3)
  Name='C. Brown'
  Name='P. Patty'
  Name='Snoopy'
ncg('Snoopy',Course,Grade)? Yes(1)
  Course='CS101', Grade='C'
~~~

---
## New Database Operations

Add join and union operations to the Relation class from the last project.

---
### Join

The following pseudo-code describes one way to compute the join of relations r1 and r2.

~~~
make the header h for the result relation
	    (combine r1's header with r2's header)

	make a new empty relation r using header h

	for each tuple t1 in r1
	    for each tuple t2 in r2

		if t1 and t2 can join
		    join t1 and t2 to make tuple t
		    add tuple t to relation r
		end if

	    end for
	end for
~~~

Note that the following operations used in the join should be decomposed into separate routines.

1.  combining r1's header with r2's header
2.  testing t1 and t2 to see if they can join
3.  joining t1 and t2

Routines b. and c. need information about which columns between the two relations should overlap; this should be calculated once per join operation.

Join must be able to join two relations, no matter if they have common attribute names or not.

---
### Union

This is the same as the set union operation given in class. This should produce a new relation that contains all tuples from both r1 and r2.

---
### Project operation

The "project" operation from the last project may need to be improved to support evaluating rules. The "project" operation needs to be able to change the order of the columns in a relation to support evaluating rules.

Test the new operations before using them to evaluate rules as described in the next section.

---
## Evaluating Rules

Add rule evaluation to the query interpreter from the last project. The major steps of the interpreter are:

1.  Process the schemes (same as the last project)
2.  Process the facts (same as the last project)
3.  Evaluate the rules (new code)
4.  Evaluate the queries (same as the last project)

Evaluate each rule using relational algebra operations as follows:

1.  **Evaluate the predicates on the right-hand side of the rule:**
    
    For each predicate on the right-hand side of a rule, evaluate the predicate in the same way you evaluated the queries in the last project (using select, project, and rename operations). Each predicate should produce a single relation as an intermediate result. If there are n predicates on the right-hand side of a rule, there should be n intermediate results.
    
2.  **Join the relations that result:**
    
    If there are two or more predicates on the right-hand side of a rule, join the intermediate results to form the single result for Step 2. Thus, if p1, p2, and p3 are the intermediate results from Step 1, join them (p1 |x| p2 |x| p3) into a single relation.
    
    If there is a single predicate on the right hand side of the rule, use the single intermediate result from Step 1 as the result for Step 2.
    
3.  **Project the columns that appear in the head predicate:**
    
    The predicates in the body of a rule may have variables that are not used in the head of the rule. The variables in the head may also appear in a different order than those in the body. Use a project operation on the result from Step 2 to remove the columns that don't appear in the head of the rule and to reorder the columns to match the order in the head.
    
4.  **Rename the relation to make it union-compatible:**
    
    Rename the relation that results from Step 3 to make it union compatible with the relation that matches the head of the rule. Rename each attribute in the result from Step 3 to the attribute name found in the corresponding position in the relation that matches the head of the rule.
    
5.  **Union with the relation in the database:**
    
    Union the result from Step 4 with the relation in the database whose name matches the name of the head of the rule.
    

Evaluate the rules in the order they are given in the input file.

---
## The Fixed-Point Algorithm

Use a fixed-point algorithm to repeatedly evaluate the rules. If an iteration over the rules changes the database by adding at least one new tuple to at least one relation in the database, the algorithm evaluates the rules again. If an iteration over the rules does not add a new tuple to any relation in the database, the fixed-point algorithm terminates.

An easy way to tell if any tuples were added to the database is to count the number of tuples in the database both before and after evaluating the rules. If the two counts are different, something changed, and the rules need to be evaluated again. However, this is computationally inefficient. A more efficient way is to use the return value of set.insert, which includes a boolean that is true if the inserted element was new to the set.

---
## Output Format

The output for this project is composed of the following parts:

1.  Rule evaluation output
2.  Query evaluation output (this is the same as last project and your code for queries should not change)

All output should be sent to standard output, not a file.

---
## Rule Evaluation Output

Output a line with the text 'Rule Evaluation' followed by the output from evaluating each rule.

For each rule that is evaluated, output the rule itself, followed by the tuples generated by the rule. Output the rule in the same form that it appears in a Datalog program. Output each tuple in the same form used by the query output in the previous project. Output the tuples after the last step of the evaluation of the rule, after the attributes have been renamed to be union compatible with the relation that matches the head of the rule. Only output the tuples that don't already exist in the result relation.

After outputting all the evaluated rules, output a blank line followed by a line with the string,

~~~
Schemes populated after n passes through the Rules.
~~~

where n is the number of times the fixed-point algorithm repeated the rule evaluation.

Finally, output a blank line followed by the query evaluation output.

Evaluate the rules in the order they are given in the input file. If you evaluate the rules in a different order, the number n may not match the expected output.

---
## Query Evaluation Output

Output a line with the text 'Query Evaluation' followed by the output from evaluating each query.

The output for each query is the same as the query output from the last project.

---
## Implementation Requirements

1.  You are required to use a relational database approach in solving the problem in this project. You must create tuples in relations and evaluate rules and queries using select, project, rename, and join operations.
2.  Your solution must include the following classes: Database, Relation, Header, and Tuple.
3.  The Relation class must use a set data type to hold the tuples in the relation.
4.  The select, project, rename, union and join operations must be implemented as functions in the Relation class.
5.  Rules and queries must be evaluated using relational operations (select, project, rename, join, and union).
6.  The relational operators must not produce relations with duplicate tuples or duplicate names in the header.
7.  The program must run to completion with a normal exit status for any input. Do not terminate with a non-zero exit status for any input, including inputs that have errors.

---
## Assumptions
You may assume the following about the datalog input:

1.  The head of every rule will only contain variable names. No strings will be given in the head of any rule.
2.  No two variable names in a rule head are the same. Each variable in a rule head is unique in that rule head.
3.  Every variable name in the head of a rule will appear in at least one predicate in the body (right-hand side) of the rule.

---
## FAQ

1.  **When should a rule update the associated relation?**
    
    When a rule is evaluated, any tuples generated by the rule should be added to the associated relation immediately.
    
    For example, suppose there are two rules, R1 and R2, associated with relation A. Suppose R1 is evaluated first, followed by R2. When R1 is evaluated, any new tuples it generates are added to A. Then R2 is evaluated using the updated relation A that contains new tuples from R1, and any new tuples R2 generates are added to A.
    
2.  **When should the algorithm that repeatedly evaluates rules terminate?**
    
    The algorithm should evaluate the rules, repeatedly, until the number of tuples in all the relations in the database does not change. At this point, the rules do not generate any new tuples.
    
3.  **What order should be followed in evaluating rules?**
    
    Evaluate the rules in the order they are given in the input file.
    
4.  **Why is the code taking a long time to run?**
    Are you running your code in Visual Studio? Visual Studio usually runs code in 'debug' mode. Debug mode may slow down the code significantly. Try running the code outside of Visual Studio (from the command line), or configure Visual Studio to run in release mode. (Microsoft explains how to configure release mode at this link: [https://docs.microsoft.com/en-us/visualstudio/debugger/how-to-set-debug-and-release-configurations](https://docs.microsoft.com/en-us/visualstudio/debugger/how-to-set-debug-and-release-configurations) )
    
    Other things that may impact running time include:
    
    1. The computer on which the code is running. (Run the code on the lab computers for the best approximation of how it will run on the TA computers.)
    
    2. Using a vector to store the tuples in a relation instead of a set. (Sorting vectors and searching vectors for duplicates are relatively slow operations.
    3. See this guide for improving [speed issues](/Guides/Speeding_Up_Code.md)

[Top](#Project4)