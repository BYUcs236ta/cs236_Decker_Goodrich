# Lab2b
---
### Part 0: Recap
1. Thus far we have taken a .txt file and turned it into a vector of "Tokens". We then took that vector and verified that the code is valid.
2. This is an important step in a programming languages evaluation. Labs 1, 2a, 2b focus of defining a syntax for our programming language. 
3. Here we will use create an internal representation of the program. This will be done using a "DatalogProgram" class.
4. The output for the project will either be `Success! [the toString() of your datalogProgram object]` or `Failure! [token we failed on]`
5. This lab will walk through the needed data-structures and have you create them.
6. You can do this!

---
### Part 1: What is a datalog program?

1. A Datalog Program is composed of 5 parts. 
	1. Schemes
	2. Facts
	3. Rules
	4. Queries
	5. Domain

We will represent them using the following 4 classes

DatalogProgram
Rule
Predicate
Parameter

Here is how those map onto each other:

A schemes, facts, and queries are Predicates
A rules are represented by our Rule class
```
A Datalog Program has:
	vector<Predicate> schemes
	vector<Predicate> facts
	vector<Rule> rules
	vector<Predicate> queries
	set<string> domain
```
	
```
A Predicate has:
	vector<Parameter> Parameters
	string name
	
Example:
snap(A, B, C)
->
name: snap
Parameters: {A, B, C}
```

```
A Parameter has:
	string value
```

```
A Rule has:
	Predicate head
	vector<Predicate> body
	
Example
snap(A, B, C) :- apple(e, f, g), orange(A, B, C, f)
->
head: snap(A, B, C)
body: {apple(e, f, g), orange(A, B, C, f)}
```


A slide you may find helpful: 
![](assets/images/project-2-data-structures.png)

`TODO:  write DatalogProgram, Rule, and Parameter classes. Your classes must include an empty constructor, setter, getter, adder, and toString() methods. Take 3 screenshots of your code(s1, s2, s3)`

##### Hint: Use your IDE to generate the setters, getters, and constructor
##### Hint: DatalogProgram should have addFact, addScheme, addRule, addQuery, addDomainItem methods.

##### [For details on how to use sets please refrence c++.com] (https://www.cplusplus.com/reference/set/set/)

To get you started here is Predicate
```c++
class Predicate {
private:
	Predicate() {}
	vector<Parameter> parameters;
	string name;
public:
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
	
	// helper string adder
	void addParameter(String paramValue) {
		Parameter param;
		param.setValue(paramValue);
		addParameter(param);
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

2. Write a test case in main that when you call toString() on the DatalogProgram you get the following:
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

To get you started here is the beginning of main.cpp
```
int main() {
	DatalogProgram program;
	
	Scheme s1;
	s1.setName("snap");
	s1.addParameter("S");
	s1.addParameter("N");
	s1.addParameter("A");
	s1.addParameter("P");
	
	program.addScheme(s1);
	
	...
	
	cout << program.toString();
	return 0;
}
```
`TODO: Take a screenshot of main (s4)`

---
### Conclusion
1. Submit your screenshots in a .zip folder on learning suite
2. Leave any feedback in the feedback section of the lab quiz

---
### TODO for the project 
##### (NOT REQUIRED FOR THE LAB)
1.  Import your code from project 2a
2. Add code to produce the datalog program into your parser. This will necessitate changing your existing functions for the various productions
	1. For this step start with schemes, test, the go onto facts, test, then queries, test, then finally rules. 
3. Good Luck!