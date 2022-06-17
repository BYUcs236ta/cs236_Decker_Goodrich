# Lab3
---
### Part 0: Recap
1. Thus far we have taken a .txt file and turned it into a vector of "Tokens". We then took that vector and verified that the code is valid. We then used those tokens to create a DatalogProgram object.
2. This is a really cool project. After you finish with project 3 you will have a complete programming language (We will add more functionality in projects 4 and 5). A programming language needs 2 things projects 1, 2a, 2b defined the syntax and projects 3, 4, and 5 will define the meaning
3. You are amazing!

---
### Part 1: What is a relation
1. A relation is comprised of 3 parts.
	1. Name
	2. Header
	3. Set\<Tuple\>

2. Make 3 files: Tuple.h, Relation.h, and Header.h

3. Lets start with Header. A header is a list of values. Each value defines a column in the table.
~~~c++

class Header {

private:
  vector<string> attributes;

public:
  Header() { }
  Header(vector<string> attributes) : attributes(attributes) { }

  unsigned size() {
    return attributes.size();
  }

  string at(int index) {
    return attributes.at(index);
  }

  void push_back(string attribute) {
  	attributes.push_back(attribute);
  }

  // TODO: write a toString() method for testing

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

  unsigned size() const {
    return values.size();
  }

  string at(int index) const {
    return values.at(index);
  }

	
  void push_back(string attribute) {
  	attributes.push_back(attribute);
  }
  
  //You must define this to allow tuples to be put into a set
  bool operator<(const Tuple t) const {
    return values < t.values;
  }

  // TODO: add more delegation functions as needed

};
~~~

5. Write a toString() method for tuples `Tuple::toString()`
~~~c++
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

6. Add error checking to tuple toString() method. Write your own error message. Add this code snippet to the start of Tuple::toString(). 
~~~c++
if (size() != header.size())
	throw "<CUSTOM ERROR MESSAGE HERE>";
~~~

### Part 2: Relation Class
1) Lets make a class that can contain those 3 parts we talked about.
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
2) Write a getter and setter for name and header.
3) Put the following addTuple method into your relation class.
```c++
bool addTuple(Tuple t) {
	// this method adds the tuple into the set 
	// returns true if the tuple was unique. 
	// returns false if the tuple was already in the set it 
	return tuples.insert(t).second;
}
```
4) Add the following toString method to the Relation class `Relation::toString()`
```c++
string toString() {
	stringstream out;
	for (Tuple t : tuples) {
		if (t.size() > 0)
			out << t.toString() << endl;
	}
	return out.str();
}
```
6) Write a test case in main that reproduces the following Relation. Call toString() on the relation 

Name: Snap
| S       | N         | A              | P          |
| ------- | --------- | -------------- | ---------- |
| '12345' | 'Charlie' | '12 Apple St.' | '555-1234' |
| '67890' | 'Lucy'    | '34 Pear Ave.' | '555-5678' |
| '33333' | 'Snoopy'  | '12 Apple St.' | '555-1234' |
 `TODO take a screenshot of your test case in main and the output. (s1)`

---
### Part 3: Database
1) Create `Database.h` 
```c++
class Database {
private:
	
}
```



---
### Conclusion
1. Submit your screenshots in a .zip folder on learning suite
2. Leave any feedback in the feedback section of the lab quiz

---
### TODO for the project 
##### (NOT REQUIRED FOR THE LAB)
1.  Import your code from project 2b
3. Good Luck!





