# Lab 1
---
### Part 0: Walter's Best Practice
1. This is advice, please take it as such
2. Stay organized
3. Keep consistent style as there are no style requirements in this course it is up to you to enforce that for yourself
4. Make friends and talk to them about the class
5. Read through and fully understand the provided code. If you have questions please ask a TA!
6. For smaller files try to keep your code in only the header files (.h) instead of splitting them between a .h and .cpp
7. Please only use unsigned ints or size_t in this course. None of the labs require negative numbers and this will improve consistency.
8. Write a toString for every class (Except maybe the automaton classes). This will be helpful during testing and debugging.
---
### Part 1: Tokens
1. Make a "TokenType" enum (Token.h). You will need to add more to this list later but 
```
enum TokenType {
  COMMA, COLON, COLON_DASH, ...
};
```

2. Make a "Token" class (Token.h). A token is comprised of 3 things (Type, Contents, LineNum)
```
class Token {
 private:
  TokenType type;
  String contents;
  unsigned int line;
};
```

3. Create a constructor (Token.h)
__TODO: Write the constructor yourself. Take a screenshot of the constructor you wrote and turn it in (s1)__ Feel free to use your IDE to generate it. Your constructor should initialize the "type", "contents", and "line" variables using its arguments.

4. Write "toString" function for the "Token" class
~~~
  string toString() const {
    stringstream out;
    out << "(" << type << "," << "\"" << value << "\"" << "," << line << ")";
    return out.str();
  }
~~~

5. Create and print a 'Token' in the 'main' function (main.cpp) with the following values
type=COMMA
value=","
line=42
__TODO: Take a screenshot of the code you wrote to test this and the resulted print (s2)__

6. Notice how the output has a number in place of the COMMA enum-type. We need to fix this. Add the following function to "Token.h". 
```
string typeName(TokenType type) const {
  switch (type) {
  case COMMA:
    return "COMMA";
  ...
  }
}
```
__TODO: Add cases for COLON and COLON_DASH types, take a screenshot (s3)__

---
### Part 2: Automata
1. Here we will create an Automaton.h file. This will be the base class for all of our Automata.

```
#include <string>

class Automaton
{
protected:
    //This tracks where in the input we are
    unsigned int currCharIndex = 0;
	
    //This tracks the number of newLines we have read
    unsigned int newLinesRead = 0;
	
	//This tracks the total number of characters consumbed
	//This is different from currCharIndex to let you "peek" at the next input without consiming it
    unsigned int numCharRead = 0;
    std::string input = "";

    // All children of this class must define this function
    virtual void s0() = 0;

    // Helper functions
    void next()
    {
        if (curr() __ '\n')
            newLinesRead++;
        numCharRead++;
        currCharIndex++;
    }

    char curr()
    {
        return input.at(currCharIndex);
    }

    bool match(char c)
    {
        return (curr() __ c);
    }

    //Call this function to check if you have reached the end of file
    bool endOfFile()
    {
        return (currCharIndex >= input.size());
    }

    //this is the error state call it when the token is invalid
    void sError()
    {
        numCharRead = 0;
    }

public:
    Automaton() {}

    unsigned int run(std::string input)
    {
        this->input = input;
        currCharIndex = 0;
        newLinesRead = 0;
        numCharRead = 0;
        s0();
        return numCharRead;
    }

    unsigned int getNewLines()
    {
        return newLinesRead;
    }
};
```
<add more here>

---
### Part 3: Parser