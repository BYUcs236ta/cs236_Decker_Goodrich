# HW01b_Project_1_FSA_Diagrams

## Draw the following Finite State Automata (FSA) needed for project 1:

---
### Question 1

 An FSA that recognizes the language consisting of "strings" in Project 1 (see Definition 4 from section 13.3.4 and the description of Project 1 under Course Content). 

```
A string is a sequence of characters enclosed in single quotes. 

White space (space, tab) is not skipped when inside a string. Two adjacent single quotes within a string denote an apostrophe. 

The line number for a string token is the line where the string begins. 

If a string is not terminated (end of file is encountered before the end of the string), the token becomes an undefined token.
```


---
### Question 2

An FSA that recognizes the language consisting of "line comments" in Project 1. 

```
A line comment starts with a hash character (#) and ends at the end of the line or end of the file.

A line comment does not have '|' as it's second character
```

---
### Question 3

An FSA that recognizes the language consisting of "block comments" in Project 1. 

```
A block comment starts with #| and ends with |#. 

Block comments may cover multiple lines. 

Block comments can be empty and multiple comments can appear on the same line. 

The line number for a comment token is the line where the comment begins. 

If a block comment is not terminated (end of file is encountered before the end of the comment), the token becomes an undefined token.
```
