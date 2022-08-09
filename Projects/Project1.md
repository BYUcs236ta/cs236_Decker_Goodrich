# Project1

UNDER CONSTRUCTION Currently hosted on learning suite

---
# Table of Contents
- [General Project Requirements](#General-Project-Requirements)
- [Special Requirements](#Special-Requirements)
- [Recourses](#Recourses)
- [Example Input and Output](#Example-Input-and-Output)
	- [Example Input 1](#Example-Input-1)
	- [Example Output 1](#Example-Output-1)
	- [Example Input 2](#Example-Input-2)
	- [Example Output 2](#Example-Output-2)
- [Testing](#Testing)
- [Design](#Design)
- [White Space](#White-Space)
- [Output Format](#Output-Format)
- [Input Errors](#Input-Errors)
- [Token Types](#Token-Types)
	- [Table](#Table)
	- [Identifiers](#Identifiers)
	- [Strings](#Strings)
	- [Comments](#Comments)
		- [Single Line](#Single-Line)
		- [Block Comments](#Block-Comments)
	- [Undefined Tokens](#Undefined-Tokens)
		- [Undefined Characters](#Undefined-Characters)
		- [Undefined Strings](#Undefined-Strings)
		- [Undefined Comments](#Undefined-Comments)

---
## General Project Requirements

1.  Projects are to be completed by each student individually (not by groups of students)
2.  Projects must be passed-off (over Zoom or Gather) with a TA or by the pass-off server to be given credit
3.  All output should be sent to standard output, not a file
4.  Input is provided in the form of a .txt file whose name is provided as the first (and only) argument when running your code.
5.  Even though we provide the test cases, the definition of your project working is that it passes all tests on the `pass-off machine`
	1. If the test cases pass on your machine but not on the pass-off machine you should start by doing the following:
		1. Test your code and the official test cases on a Linux machine, where you should be able to replicate the error
		2. Look at how you initialize object. IDEs perform default initialization, but building from command-line with g++ on Linux does not

---
## Special Requirements

1. You may **not** use a regular expression library for this project.
2. You **must** use the Parallel and Max approach which applies our study of Finite-State Automata

---
## Recourses
1.  Video: Parallel and Max, [Applying Finite State Automata](https://www.dropbox.com/s/z04uim5gbah2o9g/Combining%20FSAs%20in%20Project%201.mp4?dl=0)
2.  Please Read:  [Project 1 Guide](/Guides/Project1-Guide.pdf)
3.  Here is a resource about [Using Pointers](/Guides/UsingPointers.pdf)
4.  [TA Help Slides](https://docs.google.com/presentation/d/1zDi6YQHHFZyIrt9Wm-WIxvWNizmpyOKIjHn1_tWDfUs/edit#slide=id.g1376e9015ce_0_100)
5. [Test Cases](Code_Testing.md)
6. [Project_Pass_Off_Instructions](/Guides/Project_Pass_Off_Instructions.md)

Write a `Lexer` that reads a sequence of characters from a text file, identifies the `datalog language` tokens found in the file, and outputs each token.

---
## Example Input and Output
---
### Example Input 1
```
Queries:
   marriedTo ('Bea' , 'Zed')?

Rules:
   marriedTo( X,Y ) :- marriedTo(Y,X) .
```

---
### Example Output 1
```
(QUERIES,"Queries",2)
(COLON,":",2)
(ID,"marriedTo",3)
(LEFT_PAREN,"(",3)
(STRING,"'Bea'",3)
(COMMA,",",3)
(STRING,"'Zed'",3)
(RIGHT_PAREN,")",3)
(Q_MARK,"?",3)
(RULES,"Rules",5)
(COLON,":",5)
(ID,"marriedTo",6)
(LEFT_PAREN,"(",6)
(ID,"X",6)
(COMMA,",",6)
(ID,"Y",6)
(RIGHT_PAREN,")",6)
(COLON_DASH,":-",6)
(ID,"marriedTo",6)
(LEFT_PAREN,"(",6)
(ID,"Y",6)
(COMMA,",",6)
(ID,"X",6)
(RIGHT_PAREN,")",6)
(PERIOD,".",6)
(EOF,"",8)
Total Tokens = 26
```

---
### Example Input 2
```
,
'a string'
# a comment
Schemes
FactsRules
::-
```

---
### Example Output 2
```
(COMMA,",",1)
(STRING,"'a string'",2)
(COMMENT,"# a comment",3)
(SCHEMES,"Schemes",4)
(ID,"FactsRules",5)
(COLON,":",6)
(COLON_DASH,":-",6)
(EOF,"",7)
Total Tokens = 8
```

---
## Testing
Here are some ideas for tests:

1.  An empty input file.
2.  A colon immediately followed by another token (no space between the colon and the next token).
3.  An identifier that contains a number.
4.  An identifier that contains a keyword.
5.  An empty string (nothing between the quotes '').
6.  An unterminated string.

---
## Design
You will build a datalog parser in the next project. The datalog parser will read tokens from the datalog lexer. The `lexer` should be designed such that the parser is able to easily get the tokens from it.

---
## White Space

White space is a sequence of space, tab, or newline characters. Your `lexer` should always skip over white space between tokens. White space is not completely ignored because it is sometimes needed to separate tokens. For the C++ language, an easy way to recognize white space characters is to use the '`isspace()`' function in the \<cctype\> library.

---
## Output Format
The expected output is a list of the tokens found in the input file followed by a count of the number of tokens found. The tokens are output one token per line.

Each line has the form:

~~~
(type,"value",line)
~~~

The 'type' must be one of the types listed in the table. The 'value' is the actual input text that forms the token. The 'line' is the line number where the token is found. Notice there are no spaces on either side of the commas separating the token's elements.

The last line of output has the form:

~~~
Total Tokens = N
~~~

where 'N' is the number of tokens found. There is no newline at the end of this line.

All output should be sent to standard output (`cout`), not a file.


---
## Input Errors

When the input contains errors, output tokens with the type UNDEFINED.

Undefined tokens are:
	1.  A single character that cannot be the first character of a valid token.
	2.  A string that is not terminated.
	3.  A comment that is not terminated.

Example Input:

~~~
Facts:
$
Rules:
~~~

Example Output:

~~~
(FACTS,"Facts",1)
(COLON,":",1)
(UNDEFINED,"$",2)
(RULES,"Rules",3)
(COLON,":",3)
(EOF,"",4)
Total Tokens = 6
~~~

---
## Token Types

### List
~~~
	COMMA, PERIOD, Q_MARK, LEFT_PAREN, RIGHT_PAREN,
	COLON, COLON_DASH,
	MULTIPLY, ADD,
	SCHEMES, FACTS, RULES, QUERIES,
	ID, STRING, COMMENT, UNDEFINED,
	EOF
~~~

### Table

The following table describes the types of tokens your `Lexer` must recognize.

| Token Type  | Description                | Examples |
| ----------- | -------------------------- | -------- |
| COMMA       | The ',' character          | ,        |
| PERIOD      | The '.' character          | .        |
| Q_MARK      | The '?' character          | ?        |
| LEFT_PAREN  | The '(' character          | (        |
| RIGHT_PAREN | The ')' character          | )        |
| COLON       | The ':' character          | :        |
| COLON_DASH  | The string ":-"            | :-       |
| MULTIPLY    | The '\*' character         | \*       |
| ADD         | The '+' character          | +        |
| SCHEMES     | The string "Schemes"       | Schemes  |
| FACTS       | The string "Facts"         | Facts    |
| RULES       | The string "Rules"         | Rules    |
| QUERIES     | The string "Queries"       | Queries  |
| ID          | [See below](#Identifiers)                  |          |
| STRING      | [See below](#Strings)                  |          |
| COMMENT     | [See below](#Comments)                  |          |
| UNDEFINED   | [See below](#Undefined-Tokens)                  |          |
| EOF         | The end of the input file. |          |

---
### Identifiers

TokenType: ID

An identifier is a letter followed by zero or more letters or digits, and is not a keyword (Schemes, Facts, Rules, Queries).  

Note that for the input "1stPerson" the scanner would find two tokens: an 'undefined' token made from the character "1" and an 'identifier' token made from the characters "stPerson".

Examples:
| Valid       | Invalid   |
| ----------- | --------- |
| Identifier1 | 1stPerson |
| Person      | Schemes          |


---
### Strings

TokenType: STRING

A string is a sequence of characters enclosed in single quotes. White space (space, tab, etc.) is not skipped when inside a string. Two adjacent single quotes within a string denote an apostrophe. The line number for a string token is the line where the string begins. If a string is not terminated (end of file is encountered before the end of the string), the token becomes an undefined token.  
  
The 'value' of a token printed to the output is the sequence of input characters that form the token. For a string token this means that two adjacent single quotes in the input are printed as two adjacent single quotes in the output. (In other words, don't convert two adjacent single quotes in a string to just one apostrophe in the output.)

~~~
'This is a string'  
  
'' -- (The empty string)  
  
'This isn''t two strings'
~~~

---
### Comments

TokenType: COMMENT

There are 2 varieties, you may consider handling each case as its own automaton


#### Single Line
A line comment starts with a hash character (#) and ends at the end of the line or end of the file. (You may consider handling single-line and block comments as separate automata). A single line comment does NOT start with the characters "#|".

```
# This is a comment
# This is a comment that ends in eof
```

#### Block Comments
A block comment starts with #| and ends with |#. Block comments may cover multiple lines. Block comments can be empty and multiple comments can appear on the same line. The line number for a comment token is the line where the comment begins. If a block comment is not terminated (end of file is encountered before the end of the comment), then this is an undefined token that includes all of the text starting at the "#|" all the way to the end of file.

Examples:

~~~
#||#  
  
#| This is a  
multiline comment |#
~~~

---
### Undefined Tokens

TokenType: UNDEFINED

There are 3 varieties, you may consider handling each case as its own automaton.

#### Undefined Characters
Any character not tokenized as a string, keyword, identifier, symbol, or white space is undefined.

Examples:

~~~
$&^ (Three undefined tokens)
~~~

#### Undefined Strings
Any non-terminating string is undefined. If you reach EOF before finding the end of the string the opening of the string to the EOF is considered a single UNDEFINED Token.

Examples:

~~~
' a string that does not end

' '' '' a another string that does not end
~~~

#### Undefined Comments
Any non-terminating Block-Comment is undefined. If you reach EOF before finding the end of the Block-Comment the opening of the Block-Comment to the EOF is considered a single UNDEFINED Token.

Examples:

~~~
#| a comment that does not end

#|||||||||

#| This is an illegal block comment  
because it ends with end of file
~~~

[Top](#Project1)
