# Lab2a
---
### Part 0: Recap
1. Thus far we have taken a .txt file and turned it into a vector of "Tokens"
2. This is an important step in a programming languages evaluation. Labs 1, 2a, 2b focus of defining a syntax for our programming language. 
3. Here we will use a grammar to verify if the tokens created represent valid datalog code. 
4. The output for the project will either be `Success!` or `Failure! [token we failed on]`
5.  If you haven't done it yet write getters for the token.h class
6. Keep up the good work!

---
### Part 1: Parser class and helper functions

1. Make a `Parser` class (Parser.h)  
The Parser is given a vector of Tokens that are typically provided by the Scanner.  
(Don't forget to "#include \<vector\>")  
(Don't forget to '#include "Token.h"')
```c++
class Parser {
 private:
  vector<Token> tokens;
 public:
  Parser(const vector<Token>& tokens) : tokens(tokens) { }
};
```

2. Add some support functions to the 'Parser' class (Parser.h)  
The support functions will make the parsing routines simpler and easier to write. The `tokenType` function returns the type of the current Token. The `advanceToken()` function moves to the next Token. The `throwError()` function is called when the Parser finds an error. You may want to add other support functions in addition to these. Notice how `throwError()` includes the keyword "throw". When this line is reached computation will be halted and your code will return to the nearest "catch block". If no "catch block" is available it will halt and report an error.
(Don't forget to '#include \<iostream\>')

```c++
  TokenType tokenType() const {
    return tokens.at(0).getType();
  }
  void advanceToken() {
    tokens.erase(tokens.begin());
  }
  void throwError() {
    throw tokens.at(0);
  }
```

3. Test support functions (main.cpp). Notice here how the catch block is in main and the "protected code" is surrounded by a try block.
~~~c++
int main() {

  vector<Token> tokens = {
    Token(ID,"Ned",2),
    Token(LEFT_PAREN,"(",2),
    Token(RIGHT_PAREN,")",2),
  };
  try {
    Parser p = Parser(tokens);
    cout << p.tokenType() << endl;
    p.advanceToken();
    cout << p.tokenType() << endl;
    p.advanceToken();
    cout << p.tokenType() << endl;
    p.throwError();
  }
  catch(Token errorToken) {
    cout << errorToken.toString()
  }
}
~~~

## __TODO: Take a screenshot of this output (s1)__

4. Add match function to Parser class (Parser.h)
~~~c++
  string match(TokenType t) {
    //the cout can be removed for the final project
    cout << "match: " << t << endl;
    string tokenValue = "DATA UNCAUGHT"
	if (tokenType() == t)
		// TODO
    else
		// TODO
	return tokenValueout;
  }
~~~

5. Test match function (main.cpp)
~~~c++
int main() {

  vector<Token> tokens = {
    Token(ID,"Ned",2),
    Token(LEFT_PAREN,"(",2),
    Token(RIGHT_PAREN,")",2),
  };
  
  try {
    Parser p = Parser(tokens);
    p.match(ID);
    p.match(LEFT_PAREN);
    p.match(ID);         // intentional error
    p.match(RIGHT_PAREN);
  }
  catch(Token errorToken) {
    cout << errorToken.toString()
  }
}
~~~
## __TODO: Take a screenshot of this output (s2)__

---
### Part 2: Parsing

1. Here is the grammar rule for 'idList' from the Project 2a description, write a parsing function for 'idList' in the 'Parser' class (Parser.h)

*Grammar Rule:*
`idList -> COMMA ID idList | lambda`

*Parsing Function:*
~~~c++
  void idList() {
    if (tokenType() __ COMMA) {
      match(COMMA);
      match(ID);
      idList();
    } else {
      // lambda
    }
  }
~~~

2. Test 'idList' with valid input (main.cpp)  
(Compile and test)  
(No errors should be reported.)

~~~c++
int main() {

  vector<Token> tokens = {
    Token(COMMA,",",2),
    Token(ID,"Ted",2),
    Token(COMMA,",",2),
    Token(ID,"Zed",2),
    Token(RIGHT_PAREN,")",2),
  };
  
  try {
    Parser p = Parser(tokens);
    p.idList();
	cout << "Success!";
  }
  catch(Token errorToken) {
    cout << "Failure!" << endl << "  " << errorToken.toString(); 
  }

}
~~~

3. Test idList with bad input (main.cpp)
__TODO: Write a test case that fails by changing the tokens in the "tokens" vector. Take a screenshot of your code. (s3)__

4. Write the parsing function for the following grammar rule:

*Grammar Rule:*
`scheme -> ID LEFT_PAREN ID idList RIGHT_PAREN`

##### Hint: you do not need to wrap the function in an if statement b/c there is only one case for this particular grammar
__TODO: Take a screenshot of your code for the scheme parsing function (s4)__

5. Your code should pass the following test case
~~~c++
int main() {

  vector<Token> tokens = {
    Token(ID,"Ned",2),
    Token(LEFT_PAREN,"(",2),
    Token(ID,"Ted",2),
    Token(COMMA,",",2),
    Token(ID,"Zed",2),
    Token(RIGHT_PAREN,")",2),
  };

  try {
    Parser p = Parser(tokens);
    p.scheme();
	cout << "Success!";
  }
  catch(Token errorToken) {
    cout << "Failure!" << endl << "  " << errorToken.toString(); 
  }

}
~~~

---
### Conclusion
1. Submit your screenshots in a .zip folder on learning suite
2. Leave any feedback in notes of your lab submission.

---
### TODO for the project 
##### (NOT REQUIRED FOR THE LAB)
1.  Write and test parsing functions for the remaining grammar rules.
(Hint: this lab only covered terminals. For non-terminals in the grammar call the function associated with the production.)
2. Write a "run" function for the parser
3. Modify main to call the run function in the try-catch section;
4. Import your code from project 1 and pass the vector of tokens that code creates into the parser
5. Good Luck!
