# Lab3
---
# Table of Contents
- [Part 0 - Recap](#Part-0---Recap)
- [Part 1 - What is a relation](#Part-1---What-is-a-relation)
- [Part 2 - Relation Class](#Part-2---Relation-Class)
- [Part 3 - Database](#Part-3---Database)
- [Part 4 - Relational Operations](#Part-4---Relational-Operations)
- [Conclusion](#Conclusion)
- [TODO for the project](#TODO-for-the-project)

---
### Part 0 - Recap
1. Thus far we have taken a .txt file and turned it into a vector of "Tokens". We then took that vector and verified that the code is valid. We then used those tokens to create a `DatalogProgram` object.
2. This is a really cool project. After you finish with project 3 you will have a complete programming language (We will add more functionality in projects 4 and 5). A programming language needs 2 things Syntax and Meaning. Projects 1, 2a, 2b defined the syntax and Projects 3, 4, and 5 will define the meaning.

---
### Part 1 - What is a relation

1. A relation is comprised of 3 parts:
	1. Name
	2. Header
	3. Set\<Tuple\>

2. Make 3 files: Tuple.h, Relation.h, and Header.h

3. Lets start with Header. A header is a list of values. Each value represents a column in the table.

~~~c++

class Header {

private:
  vector<string> attributes;

public:
  Header() { }
  Header(vector<string> attributes) : attributes(attributes) { }

};
~~~

4. Here is Tuple. A tuple represents a single row in a table.

~~~c++
class Tuple {

private:

  vector<string> values;

public:
  Tuple() { }
  Tuple(vector<string> values) : values(values) { }

  //You must define this to allow tuples to be put into a set
  bool operator<(const Tuple t) const {
    return values < t.values;
  }

  // TODO: add more delegation functions as needed

};
~~~

5. Write a toString() method for tuples `Tuple::toString()`. For this to work you must define the following functions in both `Header` and `Tuple`. 

Add helper methods:
~~~c++
// Tuple :
unsigned int size() {
    return values.size();
}
 
string at(unsigned int index) {
    return values.at(index);
}

void push_back(string value) {
    values.push_back(value);
}

// Header :
unsigned int size() {
    return attributes.size();
}
 
string at(unsigned int index) {
    return attributes.at(index);
}

void push_back(string value) {
    attributes.push_back(value);
}


~~~

~~~c++
  // This goes in your tuple class, note that tuple must include Header.h
  string toString(Header header) {
    stringstream out;
    string sep = "";
    for (unsigned i = 0; i < size(); i++) {
      string name = header.at(i);
      string value = at(i);
      out << sep << name << "=" << value;
	  sep = ",";
    }
    return out.str();
  }
~~~

6. Optional: Add error checking to `Tuple::toString()`, `Header::at()`, `Tuple::at()`. Write your own error message.

~~~c++
// for the toString
if (size() != header.size()) {
    throw "CUSTOM ERROR MESSAGE HERE";
}

// for at methods
if (index >= size()) {
    throw "CUSTOM ERROR MESSAGE HERE";
}
~~~

### Part 2 - Relation Class

1. Lets make a class that can contain those 3 parts we talked about.

```c++
#include "Header"
#include <set>
class Relation {
private:
	string name;
	Header header;
	set<Tuple> tuples;
	
public:
	Relation() { }

}
```

2. Write a getter and setter for name and header.

3. Put the following addTuple method into your relation class.

```c++
bool addTuple(Tuple t) {
	// this method adds the tuple into the set 
	// returns true if the tuple was unique. 
	// returns false if the tuple was already in the set it 
	return tuples.insert(t).second;
}
```

4. Add the following `toString` method to the `Relation` class

```c++
string toString() {
    stringstream out;
    for (Tuple t : tuples) {
        if (t.size() > 0) {
	    out << t.toString() << endl;
        }
    }
    return out.str();
}
```

5. Add the `size()` method to your `Relation` class

```c++
unsigned int size() {
    return tuples.size();
}
```

6. Write a test case in main that reproduces the following Relation. Call `toString()` on the relation 

Name: Snap

| S       | N         | A              | P          |
| ------- | --------- | -------------- | ---------- |
| '12345' | 'Charlie' | '12 Apple St.' | '555-1234' |
| '67890' | 'Lucy'    | '34 Pear Ave.' | '555-5678' |
| '33333' | 'Snoopy'  | '12 Apple St.' | '555-1234' |

 `TODO take a screenshot of your test case in main and the output. (s1)`

---
### Part 3 - Database

1. Create `Database.h` 

```c++
#include "Relation.h"
#include <map>
class Database {
private:
	map<string, Relation> database;

public:
	void addRelation(Relation r) {
		database.insert({r.getName(), r});
	}
	
	string toString() {
		stringstream out;
		// TODO: Write a toString method for your database class.
		// Use this for testing
		return out.str();
	}
}
```

2. Run the following code in main:

```c++
#include "Database.h"
int main() {
	Tuple t1;
	t1.push_back("A");
	t1.push_back("B");
	t1.push_back("C");
	
	Tuple t2;
	t1.push_back("1");
	t1.push_back("2");
	t1.push_back("3");

	Header h1;
	h1.push_back("col0");
	h1.push_back("col1");
	h1.push_back("col2");

	Relation r1;
	r1.setName("first");
	r1.setHeader(h1);
	r1.addTuple(t1);
	
	Relation r2;
	r2.setName("second");
	r2.setHeader(h1);
	r2.addTuple(t2);

	Database database;
	database.addRelation(r1);

	cout << database.toString();
}
```

`TODO: take a screenshot of the output of the above code (s2)`

---
### Part 4 - Relational Operations

I am providing some test code for the following functions and the types. You must implement each of the functions and take a screenshot. These methods will go in the relation class. They will also return a relation. 

1) Select

You will need 2 select methods. I provide the definition of one of them. 

```c++
Relation select(unsigned int col, string value) {
	Relation output; // make a new empty relation
	output.setName(this.name); // copy over name
	output.setHeader(this.header); // copy over header

	for (Tuple currTuple : this.tuples) { // loop through each tuple
		if (currTuple.at(col) == value) {
			output.addTuple(currTuple);
		}
	}

	return output;
}

Relation select(unsigned int col1, unsigned int col2) {
	// check to make sure both columns are in bounds
	// write your code here
}
```

`TODO: take a screenshot of your select method (s3)`

The following case is given for you to try on your own to verify your method is working, feel free to add more to it!

Test case:

```c++
int main() {
	Tuple t1;
	t1.push_back("A");
	t1.push_back("B");
	t1.push_back("C");
	
	Tuple t2;
	t1.push_back("1");
	t1.push_back("2");
	t1.push_back("1");

	Header h1;
	h1.push_back("col0");
	h1.push_back("col1");
	h1.push_back("col2");

	Relation r1;
	r1.setName("first");
	r1.setHeader(h1);
	r1.addTuple(t1);
	r1.addTuple(t2);

	cout << r1.select(r1.select(0, "A")).toString() << endl;
	cout << r1.select(r1.select(0, 2)).toString() << endl;
}
```

Output:

```c++
col0="A",col1="B",col2="C"

col0="1",col1="2",col2="1"
```



2. Rename

```c++
Relation rename(vector<string> newAttributes) {
	// Make a new empty relation
	// Make a new Header with newAttributes as its contents
	// copy over the old name
	// copy over all of the existing tuples
	
}
```

`TODO: take a screenshot of your rename method (s4)`

The following case is given for you to try on your own to verify your method is working, feel free to add more to it!

Test case:

```c++
int main() {
	Tuple t1;
	t1.push_back("A");
	t1.push_back("B");
	t1.push_back("C");
	
	Tuple t2;
	t1.push_back("1");
	t1.push_back("2");
	t1.push_back("1");

	Header h1;
	h1.push_back("col0");
	h1.push_back("col1");
	h1.push_back("col2");


	vector<string> h2;
	h2.push_back("colA");
	h2.push_back("colB");
	h2.push_back("colC");

	Relation r1;
	r1.setName("first");
	r1.setHeader(h1);
	r1.addTuple(t1);
	r1.addTuple(t2);

	cout << r1.rename(h2) << endl;
}
```

Output:

```c++
colA="A",colB="B",colC="C"
colA="1",colB="2",colC="1"
```

3. Project 

```c++
Relation project(vector<unsigned int> indiciesToKeep) {
	// Make a new empty relation
	// copy over the old name
	
	// generate the new header
	// keep only the columns in the 
	// vector in the order they are given
	
	// loop through all the tuples
		// for each tuple "re-order" it
	
}
```

Hint: instead of copying the header and they re-organizing it start with an empty tuple/header and add elements one at a time.

`TODO: take a screenshot of your project method (s5)`

The following case is given for you to try on your own to verify your method is working, feel free to add more to it!

Test case:

```c++
int main() {
	Tuple t1;
	t1.push_back("A");
	t1.push_back("B");
	t1.push_back("C");
	
	Tuple t2;
	t1.push_back("1");
	t1.push_back("2");
	t1.push_back("1");

	Header h1;
	h1.push_back("col0");
	h1.push_back("col1");
	h1.push_back("col2");


	vector<unsigned int> colsToKeep;
	h2.push_back(2);
	h2.push_back(0);


	Relation r1;
	r1.setName("first");
	r1.setHeader(h1);
	r1.addTuple(t1);
	r1.addTuple(t2);

	cout << r1.project(colsToKeep) << endl;
	
}
```

Output:

```c++
colC="C",colB="A"
colC="1",colB="1"
```

---
### Conclusion
1. Submit your screenshots in a .zip folder on learning suite
2. Leave any feedback in the feedback section of the lab

---
### TODO for the project 

**(NOT REQUIRED FOR THE LAB)**
1.  Import your code from project 2b
2. Create Interpreter class
3. Add in evalSchemes, evalFacts, evalQueries methods
4. In interpreter::run() call each of those methods
5. Try to keep code as modular as possible
6. Good Luck!





