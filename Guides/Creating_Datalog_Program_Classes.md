# Creating_Datalog_Program_Classes
---
# Table of Contents
- [Part 1 - What is a datalog program](#Part-1---What-is-a-datalog-program)
- [Part 2 - Modify the Parser](#Part-2---Modify-the-Parser)
- [Part 3 - Update scheme](#Part-3---Update-scheme)

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


`TODO:  write DatalogProgram, Rule, and Parameter classes. Your classes should include an empty constructor, setter, getter, adder, and toString() methods.`

**Hint: Use your IDE to generate the setters, getters, and constructor**

**Hint: DatalogProgram should have addFact, addScheme, addRule, addQuery, addDomainItem methods.**

**[For details on how to use sets please reference c++.com] (https://www.cplusplus.com/reference/set/set/)**


Here are some example classes to see what this should look like:

Bee has a name and an age
Bee hive has a set of Bees and a location

~~~c++
class Bee {
private:
	string name;
	unsigned int age;

public:
	// empty constructor
	Bee() {} 
	
	// getters
	string getName() {
		return name;
	}

	unsigned int getAge() {
		return age;
	}

	// setters
	void setName(string newName) {
		name = newName;
	}

	void setAge(unsgined int newAge) {
		age = newAge;
	}
	
	// no adder needed b/c this class does not have a container
	
	// toString
	string toString() {
		stringstream out;
		out << name << ":" << age;
		return out.str();
	}
};


class Beehive {
private:
	set<Bee> bees;
	string location;

public:
	// empty constructor
	Beehive() {}

	// getters
	string getLocation() {
		return location;
	}
	
	set<Bee> getBees() {
		return Bees
	}
	
	// setters
	void setLocation(string newLocation) {
		location = newLocation;
	}
	
	void setBees(set<Bee> newBees) {
		bees = newBees;
	}
	
	// adder
	void addBee(Bee bee) {
		bees.insert(bee);
	}
	
	// toString
	string toString() {
		stringstream out;
		out << location << " hive" << endl;
		for (Bee currentBee : bees) {
			out << "  " << currentBee.toString() << endl;
		}
		return out.str();
	}
};

~~~

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
[Top](#Creating_Datalog_Program_Classes)