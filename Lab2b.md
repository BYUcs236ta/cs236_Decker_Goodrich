# Lab2b
---
# Table of Contents
- [Part 0 - Recap](#Part-0---Recap)
- [Part 1 - What is a datalog program](#Part-1---What-is-a-datalog-program)
- [Part 2 - Modify the Parser](#Part-2---Modify-the-Parser)
- [Part 3 - Update scheme](#Part-3---Update-scheme)
- [Conclusion](#Conclusion)
- [TODO for the project](#TODO-for-the-project)

---
### Part 0 - Recap
1. Thus far we have taken a .txt file and turned it into a vector of "Tokens". We then took that vector and verified that the Datalog file is valid Datalog syntax.
2. This is an important step in a programming languages evaluation. Labs 1, 2a, 2b focus of defining a syntax for our programming language. 
3. Here we will create an internal representation of the program (to be used in the later Projects). This will be done using a `DatalogProgram` class.
4. The output for the project will either be `Success! [the toString() of your datalogProgram object]` or `Failure! [token we failed on]`
5. This lab will walk through the needed data-structures and have you create them.
6. You can do this!

---
### Part 1 - What is a datalog program

1. A Datalog Program is composed of 5 parts. 
	1. Schemes
	2. Facts
	3. Rules
	4. Queries
	5. Domain

We will represent them using the following 4 classes: `DatalogProgram`, `Rule`, `Predicate`, `Parameter`

Here is how those map onto each other:

Schemes, facts, and queries are `Predicates`

Rules are represented by our `Rule` class

A Datalog Program has member variables:

```c++
vector<Predicate> schemes;
vector<Predicate> facts;
vector<Rule> rules;
vector<Predicate> queries;
set<string> domain;
```
	
A Predicate has member variables:

```c++
vector<Parameter> parameters;
string name;
```

Example:

```
snap(A, B, C)
->
name: snap
Parameters: {A, B, C}
```

A Parameter has:

```c++
string value;
```


A Rule has:

```
Predicate head;
vector<Predicate> body;
```
	
Example:

```
snap(A, B, C) :- apple(e, f, g), orange(A, B, C, f)
->
head: snap(A, B, C)
body: {apple(e, f, g), orange(A, B, C, f)}
```


`TODO:  write DatalogProgram, Rule, and Parameter classes. Your classes must include an empty constructor, setter, getter, adder, and toString() methods. Take a screenshot of each class (s1, s2, s3). Parameter will not have an adder`

**Hint: Use your IDE to generate the setters, getters, and constructor**

**Hint: DatalogProgram should have addFact, addScheme, addRule, addQuery, addDomainItem methods.**

**[For details on how to use sets please reference c++.com] (https://www.cplusplus.com/reference/set/set/)**

To get you started here is `Predicate`

```c++
class Predicate {
private:
	vector<Parameter> parameters;
	string name;
public:
	Predicate() {}
	// setters
	void setName(string newName) { 
		name = newName; 
	}
	void setParameters(vector<Parameter> newParameters) {
		parameters = newParameters;
	}
	
	// getters
	string getName() { 
		return name; 
	}
	vector<Parameter> getParameters() {
		return parameters;
	}
	
	// adder
	void addParameter(Parameter parameter) {
		parameters.push_back(parameter);
	}

	//helper function
	void addParameter(string parameterValue) {
		Parameter parameter;
		parameter.setValue(parameterValue);
		parameters.push_back(parameter);
	}
	// toString
	string toString() {
		string sep = "";
		stringstream out;
		out << name << "(";
		for (Parameter currParam : parameters) {
			out << sep << currParam.toString();
			sep = ",";
		}
		out << ")";
		return out.str();
	}
};
```

2. Write a test case in `main` that when you call `toString()` on a `DatalogProgram` object you get the following (you need to create the `DatalogProgram` object and call its member functions to store the required information):

```
Schemes(2):
  snap(S,N,A,P)
  HasSameAddress(X,Y)
Facts(1):
  snap('12345','C. Brown','12 Apple','555-1234').
Rules(1):
  HasSameAddress(X,Y) :- snap(S,N,A,P),snap(S,N,A,P).
Queries(1):
  HasSameAddress('Snoopy',Who)?
Domain(1):
  '12 Apple'
```

You will need to "hard-code" values for the test case.

To get you started here is the beginning of `main.cpp`

```c++
int main() {
	DatalogProgram program;
	
	Predicate snapScheme;
	snapScheme.setName("snap");
	snapScheme.addParameter("S");
	snapScheme.addParameter("N");
	snapScheme.addParameter("A");
	snapScheme.addParameter("P");
	
	program.addScheme(snapScheme);
	
	// ...
	
	cout << program.toString();
	return 0;
}
```

`TODO: Take a screenshot of main (s4)`

---
### Part 2 - Modify the Parser

1. Add helper functions into your `Parser`, you may consider writing your own custom error messages.

~~~c++
string getPrevTokenContents() {
  if (currTokenIndex < 0) throw "CUSTOM ERROR MESSAGE HERE";
  return tokens.at(currTokenIndex - 1).getContents();
}

string getCurrTokenContents() {
  if (currTokenIndex >= tokens.size()) throw "CUSTOM ERROR MESSAGE HERE";
  return tokens.at(currTokenIndex).getContents();
}
~~~

2. In main you should have code that looks like:

~~~c++
try {
  ...
}
catch(Token errorToken) {
  ...
}
~~~

3. Add in a catch block for strings:

~~~c++
try {
  ...
}
catch(Token errorToken) {
  ...
}
catch(const char* errorMsg) {
  cout << errorMsg;
}
~~~

4. Add a private `DatalogProgram` member to `Parser`

~~~c++
private:
  DatalogProgram program;
~~~

5. Modify `Parser::run` to return a `DatalogProgram`:

~~~c++
DatalogProgram run() {
  ...
  return program;
}
~~~

---
### Part 3 - Update scheme
1. Your scheme function should look like the following:
~~~c++
void scheme() {
	match(ID);
	match(LEFT_PAREN);
	match(ID);
	idList();
	match(RIGHT_PAREN);
}
~~~

2. Add in code to create and add a scheme to datalog program:
~~~c++
void scheme() {
	Predicate newScheme;
	
	match(ID);
	match(LEFT_PAREN);
	match(ID);
	idList();
	match(RIGHT_PAREN);
	
	program.addScheme(newScheme);
	
}
~~~

3. A scheme looks like the following `snap(s, N, a, P)`. Notice how the first ID is the "name" of the scheme

4. Update scheme to save the "name" into the scheme. We will do this using the `getPrevTokenContents()` helper function we added 

~~~c++
void scheme() {
	Predicate newScheme;

	match(ID); // in our example this would be "snap"
	newScheme.setName(getPrevTokenContents());

	match(LEFT_PAREN);
	match(ID); // in our example this would be "s"
	idList(); // in our example this would be the list ["N", "a", "P"]
	match(RIGHT_PAREN);
	
	program.addScheme(newScheme);
	
}
~~~

5. Do the same for the second ID. The second ID represents the first parameter.

~~~c++
void scheme() {
	Predicate newScheme;

	match(ID); // in our example this would be "snap"
	newScheme.setName(getPrevTokenContents());

	match(LEFT_PAREN);
	match(ID); // in our example this would be "s"
	
	Parameter firstParameter;
	firstParameter.setValue(getPrevTokenContents()) // make a new parameter
	
	newScheme.addParameter(firstParameter); // add our new parameter into our scheme
	
	idList(); // in our example this would be the list ["N", "a", "P"]
	
	match(RIGHT_PAREN);
	
	program.addScheme(newScheme); // add scheme to our program
	
}
~~~

6. Think about how you would extract the data from the `idList()` function. Do you need to save it in a member variable of the class? Should you change the `idList()` function to return something? If so what would you change it too?

---
### Conclusion
1. Submit your screenshots in a .zip folder on learning suite
2. Leave any feedback in the feedback section of the lab quiz

---
### TODO for the project 

**NOT REQUIRED FOR THE LAB**
1. Add code to produce the datalog program into your parser. This will necessitate changing your existing functions for the various productions.
	1. For this step start with schemes, test, the go onto facts, test, then queries, test, then finally rules. 
	2. For things like idList think about what type that function will have, figure out how to pass the value of this thing from this function to where it needs to be stored
2. Good Luck!

[Top](#Lab2b)