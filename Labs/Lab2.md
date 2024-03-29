# Lab2
---
# Table of Contents
- [Part 0 - Recap](#Part-0---Recap)
- [Part 1 - Parser class and helper functions](#Part-1---Parser-class-and-helper-functions)
- [Part 2 - Parsing](#Part-2---Parsing)
- [Conclusion](#Conclusion)
- [TODO for the project](#TODO-for-the-project)
- [What is a datalog program](#What-is-a-datalog-program)


---

Make sure to read the Project 2 Guide (found in Learning Suite) — and the specs — so that you have the big picture for the entire Parser.

### Part 0 - Recap
1. Thus far we have taken a .txt file and turned it into a vector of "Tokens"
2. This is an important step in a programming languages evaluation. Labs 1 and 2 focus of defining a syntax for our programming language. 
3. Here we will use a grammar to verify if the Tokens created represent a valid Datalog program. 
4. The output for the project will either be `Success! [toString() of our datalog program object]` or `Failure! [token we failed on]`
5. If you haven't done it yet write "getters" for the `Token.h` class
6. Keep up the good work!

---
### Part 1 - Parser class and helper functions

1. Make a `Parser` class (Parser.h)
The `Parser` is given a `vector` of `Tokens` that are typically provided by the `Lexer`.(Don't forget to `#include <vector>`) (Don't forget to `#include "Token.h"`)

```c++
class Parser {
private:
    vector<Token> tokens;
    unsigned int currTokenIndex = 0;
public:
    Parser(const vector<Token>& tokens) : tokens(tokens) { }
};
```

2. Add some helper functions to the `Parser` class, these should be public functions so we can test them in main. 

The support functions will make the parsing routines simpler and easier to write. 

The `currToken()` function should return the current token being looked at. (Hint: Use `currTokenIndex` to identify which token from the tokens vector to return)

The `currTokenType()` function should returns the type of the current `Token` being looked at.(Hint: Use `currToken()` to get the current token being looked at, then call `Token::getType()` to get the curr token type)

The `advanceToken()` function should move to the next `Token` in `tokens`. 

The `throwError()` function is called when the Parser finds an error. You may want to add other support functions in addition to these. Notice how `throwError()` includes the keyword "throw". When this line is reached computation will be halted and your code will return to the nearest "catch block". If no "catch block" is available it will halt and report an error. If you haven't used exceptions before in C++, see this tutorial:[https://cplusplus.com/doc/tutorial/exceptions/](https://cplusplus.com/doc/tutorial/exceptions/).

```c++
Token currToken() const {
    // TODO: add code for this helper function
}
TokenType currTokenType() const {
    // TODO: add code for this helper function
}
void advanceToken() {
    // TODO: add code for this helper function
}
void throwError() {
    throw currToken();
}
```

3. Test support functions (main.cpp). Notice here how the `catch` block is in the `main` function and the "protected code" is surrounded by a `try` block.

~~~c++
int main() {
    vector<Token> tokens = {
        Token(ID,"Ned",2),
        Token(LEFT_PAREN,"(",2),
        Token(RIGHT_PAREN,")",2)
    };
    try {
        Parser p = Parser(tokens);
        cout << tokenTypeToString(p.currTokenType()) << endl;
        p.advanceToken();
        cout << tokenTypeToString(p.currTokenType()) << endl;
        p.advanceToken();
        cout << tokenTypeToString(p.currTokenType()) << endl;
        p.throwError();
    }
    catch(Token errorToken) {
        cout << errorToken.toString();
    }
}
~~~

`TODO: Take a screenshot of this output (s1)`

4. Add `checkFor()` function to Parser class (Parser.h) this function will take an expected type and if it matches the current type it should advance to the next token. If it fails it will throw an error.

~~~c++
void checkFor(TokenType expectedType) {
    // this cout segement should be removed for the final project output
    cout << "Token at index " << currTokenIndex;
    cout << " was type: " << tokenTypeToString(currTokenType());
    cout << " expected: " << tokenTypeToString(expectedType) << endl;
    
    if (currTokenType() == expectedType) {
        // TODO: think about what should happen if the Parser matches an expected Token
    } else {
        // TODO: think about what should happen if the Parser matches an UN-expected Token
    }
}
~~~

5. Test `checkFor` function (main.cpp)

~~~c++
int main() {
    vector<Token> tokens = {
        Token(ID,"Ned",2),
        Token(LEFT_PAREN,"(",2),
        Token(RIGHT_PAREN,")",2)
    };
    
    try {
        Parser parser = Parser(tokens);
        parser.checkFor(ID);
        parser.checkFor(LEFT_PAREN);
        parser.checkFor(ID); // intentional error
        parser.checkFor(RIGHT_PAREN);
    }
    catch(Token errorToken) {
        cout << errorToken.toString();
    }
}
~~~

`TODO: Take a screenshot of this output (s2). Hint: if nothing happens or you get stuck in an infinite loop make sure to finish the TODO's in the checkFor function.`

6. Move helper functions (`checkFor()`, `advanceToken()`, `throwError()`, `currToken()`, `currTokenType()`) to private
---
### Part 2 - Parsing

1. Here is the grammar rule for `idList` from the Project 2a description. Write a parsing function for `idList` in the `Parser` class (Parser.h)

*Grammar Rule:*

`idList -> COMMA ID idList | lambda`

*Parsing Function:*

~~~c++
// Consider having a comment that tells you what the parsing rule is:
// idList -> COMMA ID idList | lambda
void parseIdList() {
    if (currTokenType() == COMMA) {
        checkFor(COMMA);
        checkFor(ID);
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
        Token(RIGHT_PAREN,")",2)
    };

    try {
        Parser parser = Parser(tokens);
        parser();
        cout << "Success!";
    }
    catch(Token errorToken) {
        cout << "Failure!" << endl << "" << errorToken.toString(); 
    }
}
~~~

3. Test 'idList' with bad input (main.cpp)

`TODO: Write a test case that fails by changing the tokens in the "tokens" vector. Take a screenshot of your code. (s3)`

4. Write the parsing function for the following grammar rule:

*Grammar Rule:*

`scheme -> ID LEFT_PAREN ID idList RIGHT_PAREN`

Hint: you do not need to wrap the function in an if statement because there is only one case for this particular grammar rule

`TODO: Take a screenshot of your code for the scheme parsing function (s4)`

5. Your code should pass the following test case

~~~c++
int main() {
    vector<Token> tokens = {
        Token(ID,"Ned",2),
        Token(LEFT_PAREN,"(",2),
        Token(ID,"Ted",2),
        Token(COMMA,",",2),
        Token(ID,"Zed",2),
        Token(RIGHT_PAREN,")",2)
    };
    
    try {
        Parser parser = Parser(tokens);
        parser.scheme();
        cout << "Success!";
    }
    catch(Token errorToken) {
        cout << "Failure!" << endl << "" << errorToken.toString(); 
    }

}
~~~

---
### Conclusion
1. Submit your screenshots in a .zip folder on learning suite
2. Leave any feedback in notes of your lab submission

---
### TODO for the project

 **Not Required for the lab**
1. Write and test parsing functions for the remaining grammar rules
(Hint: this lab only covered terminals. For non-terminals in the grammar call the function associated with the production)
2. Test your code for all the cases, make sure that success and failure are called appropriately for all tests.
3. Write DatalogProgram, Predicate, Rule, and Parameter classes [guide](/Guides/Creating_Datalog_Program_Classes.md)
4. Modify `main` to call the `run` function in the try-catch section
5. Import your code from project 1 and pass the vector of tokens that code creates into the Parser
6. Good Luck!



[Top](#Lab2)