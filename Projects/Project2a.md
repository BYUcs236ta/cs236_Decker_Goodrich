# Project2a

UNDER CONSTRUCTION Currently hosted on learning suite

---
# Table of Contents
- [General Project Requirements](#General-Project-Requirements)
- [Example Input and Output](#Example-Input-and-Output)
	- [Example Input Successful](#Example-Input-Successful)
	- [Example Output Successful](#Example-Output-Successful)
	- [Example Input Failure](#Example-Input-Failure)
	- [Example Output Failure](#Example-Output-Failure)
- [Testing](#Testing)
- [Design](#Design)
- [The Datalog Grammar](#The-Datalog-Grammar)
- [Output Format](#Output-Format)
- [Implementation Requirements](#Implementation-Requirements)
- [Implementation Suggestions](#Implementation-Suggestions)

---
## General Project Requirements

1.  Projects are to be completed by each student individually (not by groups of students)
2.  Projects must be passed-off (over Zoom or in person) with a TA or by the pass-off server to be given credit
3.  All output should be sent to standard output, not a file
4.  Even though we provide the test cases, the definition of your project working is that it passes all tests on the _pass-off machine_
    -   If the test cases pass on your machine but not on the pass-off machine you should start by doing the following:
        -   Test your code and the official test cases on a Linux machine, where you should be able to replicate the error
        -   Look at how you initialize object. IDEs perform default initialization, but building from commandline with g++ on Linux does not

Write a parser that reads a datalog program from a text file and verifies that it is a valid datalog program.

---
## Example Input and Output
---
### Example Input Successful

~~~
Schemes:
  snap(S,N,A,P)
  HasSameAddress(X,Y)

Facts:
  snap('12345','C. Brown','12 Apple','555-1234').
  snap('33333','Snoopy','12 Apple','555-1234').

Rules:
  HasSameAddress(X,Y) :- snap(A,X,B,C),snap(D,Y,B,E).

Queries:
  HasSameAddress('Snoopy',Who)?
~~~

---
### Example Output Successful

~~~
Success!
~~~

---
### Example Input Failure

~~~
Schemes:
    snap(S,N,A,P) 
    NameHasID(N,S)
 
Facts:
    snap('12345','C. Brown','12 Apple','555-1234').
    snap('67890','L. Van Pelt','34 Pear','555-5678').
 
Rules:
    NameHasID(N,S) :- snap(S,N,A,P)?
 
Queries:
    NameHasID('Snoopy',Id)?
~~~

---
### Example Output Failure

~~~
Failure!
  (Q_MARK,"?",10)
~~~

---
## Testing

Here are some ideas for tests.

1.  A program with extra tokens at the end of the file.
2.  A program that uses the wrong punctuation.
3.  A program that has missing punctuation.
4.  A program with empty fact or rule lists.
5.  A program with empty scheme or query lists.
6.  A program with schemes, facts, rules, and queries, but in the wrong order.

---
## Design

You will build a datalog parser. That parser is responsible for validating if a given list of tokens matches the datalog grammar. Do do this you MUST use a recursive decent parser.

---
## The Datalog Grammar

The Datalog Grammar defines valid datalog programs. Use the grammar to write a recursive-descent parser for datalog programs. Use the scanner from the previous project to provide input tokens for the parser.

The non-terminals in the grammar begin with a lower-case letter. Terminal symbols in the grammar are written in all upper-case letters. The word 'lambda' in the grammar represents the empty string.

Note that comments do not appear in the grammar because comments should be ignored. An easy way to ignore comments is to modify the scanner/tokenizer to stop outputting comments.
~~~
datalogProgram	->	SCHEMES COLON scheme schemeList FACTS COLON factList RULES COLON ruleList QUERIES COLON query queryList EOF

schemeList	->	scheme schemeList | lambda
factList	->	fact factList | lambda
ruleList	->	rule ruleList | lambda
queryList	->	query queryList | lambda

scheme   	-> 	ID LEFT_PAREN ID idList RIGHT_PAREN
fact    	->	ID LEFT_PAREN STRING stringList RIGHT_PAREN PERIOD
rule    	->	headPredicate COLON_DASH predicate predicateList PERIOD
query	        ->      predicate Q_MARK

headPredicate	->	ID LEFT_PAREN ID idList RIGHT_PAREN
predicate	->	ID LEFT_PAREN parameter parameterList RIGHT_PAREN
	
predicateList	->	COMMA predicate predicateList | lambda
parameterList	-> 	COMMA parameter parameterList | lambda
stringList	-> 	COMMA STRING stringList | lambda
idList  	-> 	COMMA ID idList | lambda
parameter	->	STRING | ID
~~~

---
## Output Format

If the parse is successful, output 'Success!' If the parse is unsuccessful, output 'Failure!' followed by the offending token. Output the type, value, and line number of the token surrounded by parentheses and indented by 2 spaces on a new line of output. Note that the parser stops after encountering the first offending token.

All output should be sent to standard output, not a file.

---
## Implementation Requirements

1.  You must implement a deterministic top-down parser that chooses the rule to expand based on the current token.
2.  You must implement the parser using the recursive-descent approach.
3.  You must use the lexer/scanner/tokenizer from the previous project to read tokens from the input.

## Implementation Suggestions

1.  **What's a good way to handle syntax errors?**
    
    A good approach is to use _exceptions_ to handle syntax errors. Throw an exception when the parser detects a syntax error. Catch the exception at the top level of the parser to report either success or failure. (Don't return a boolean from each parsing routine to indicate a syntax error; that would mean that each return value must be checked, resulting in far more complicated code.)

[Top](#Project2a)