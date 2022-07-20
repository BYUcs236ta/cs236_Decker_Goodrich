# Lab4
# UNDER CONSTRUCTION

---
# Table of Contents
- [Part 0 - Recap](#Part-0---Recap)
- [Natural Join Algorithm](#Natural-Join-Algorithm)
	- [Natural Join Signature](#Natural-Join-Signature)
	- [Example Tables](#Example-Tables)
	- [Step 1 - Calculate header overlap](#Step-1---Calculate-header-overlap)
		- [Overlap Pseudocode](#Overlap-Pseudocode)
	- [Step 2 - Create new Header](#Step-2---Create-new-Header)
		- [Combine Header Pseudocode](#Combine-Header-Pseudocode)
	- [Step 3 - Find Tuples](#Step-3---Find-Tuples)
		- [Tuple Section Pseudocode](#Tuple-Section-Pseudocode)
- [Test Case](#Test-Case)
- [Conclusion](#Conclusion)
- [TODO for the project](#TODO-for-the-project)
- [Alternate Nat Join Pseudocode](#Alternate-Nat-Join-Pseudocode)
	- [Example:](#Example:)

---
### Part 0 - Recap
- In projects 1-3 we have converted a text file to a `vector<Token>` then parsed it to check syntax and build the representation (`DatalogProgram`), and finally converted that representation into a Database and started executing the program
- In the previous lab we evaluated the Schemes, Facts, and Queries 
- Next step is to add the `Rule` evaluation to the `Interpreter`
- Rule evaluation will use Natural Join as one of it's major steps
- Keep up the good work!

---
### Natural Join Algorithm

For this lab we will implement the natural join algorithm. This will be a member function of `Relation.h`. We will use this method to join together the body predicates of each rule.

#### Natural Join Signature
~~~c++
Relation* naturalJoin(Relation* other) {
	// give clearer names to the this and other relations
	Relation* r1 = this; 
	Relation* r2 = other;

	Relation* output = new Relation();

	// set name of output relation

	// calculate header overlap of 'this' and 'other' relations
	
	// combine headers -- will be the header for 'output'
	
	// combine tuples -- will be the tuples for 'output'
	return output;
}
~~~

For the name I recommend using something like:

~~~c++
output->setName(r1->getName() + " |x| " + r2->getName());
~~~


#### Example Tables

snap
| StudentID (0)   | Name (1)            | Address (2)              | PhoneNumber (3)      |
| ------- | ---------------- | ------------------ | ---------- |
| '12345' | 'C.      Brown'  | '12    Apple St.'  | '555-1234' |
| '22222' | 'P.       Patty' | '56    Grape Blvd' | '555-9999' |
| '33333' | 'Snoopy'         | '12    Apple  St.' | '555-1234' |

csg
| Course (0)  | StudentID (1)  | Grade (2) |
| ------- | -------- | ------- |
| 'CS101' | '12345' | 'A'     |
| 'CS101' | '22222'  | 'B'     |
| 'CS101' | '33333'  | 'C'     |
| 'EE200' | '12345'  | 'B+'    |
| 'EE200' | '22222'  | 'B'     |

We will calculate `snap |x| csg`

#### Step 1 - Calculate header overlap

`snap` and `csg` overlap on index pair `(0, 1)` -- Hint: you interpret this as index 0 of `snap`'s header is the same as index 1 of `csg`'s header

`csg` has unique attribute names at columns `[0, 2]` -- Hint: this is a list of indices in `csg's` header that are unique. They need to be included in the `output` relations header.

##### Overlap Pseudocode
Note: What data structures should be used for the `overlap` and `uniqueCols` below? There are multiple options that work. You get to choose (see the hints).

~~~
let Header h1 be r1's header
let Header h2 be r2's header

initialize overlap (what type should this be? maybe a map or 2 vectors?)
initialize uniqueCols (what type should this be? maybe a vector or set)

for index2 = 0 to h2.size():
	found = false
	
	for index1 = 0 to h1.size():
		if (h1[index1] == h2[index2]):
			found = true
			add index1, index2 to your overlap structure
		end if
	end index1 for loop
		
	if (not found):
		add index2 to uniqueCols
	end if
end index2 for loop
~~~

#### Step 2 - Create new Header
From the above example, here is the new header:
```
newHeader = {StudentID, Name, Address, PhoneNumber, Course, Grade}
```
Hint: note that the `newHeader` is the entire header of `snap` and then just the "unique" columns from `csg`. This works every time.

##### Combine Header Pseudocode

This step is best done as a separate function, which you can then test to make sure it is working.
~~~
// don't forget to pass by refrence to avoid making unecessary copies
function combineHeaders (Header h1, Header h2, uniqueCols):
	let newHeader be a new empty header
	
	copy all values from h1 into newHeader
	
	for i in uniqueCols:
		copy h2[i] into newHeader
	
	return newHeader
~~~

#### Step 3 - Find Tuples that can join

For every possible pair of tuples check if they can be joined (you might consider making this a separate function as well).

Examples:
```
t1 = ('12345', 'C. Brown', '12 Apple St.', '555-1234') 

cannot join with

t2 = ('CS101', '22222', 'B')

because:
t1.at(0) == t2.at(1)
'12345' == '22222'
FALSE
```

```
t1 = ('12345', 'C. Brown', '12 Apple St.', '555-1234') 

can join with

t2 = ('CS101', '12345', 'A')

because:
t1.at(0) == t2.at(1)
'12345' == '12345'
TRUE
```

##### Tuple Section Pseudocode

In `naturalJoin()`
```
for each tuple t1 from r1:
	for each tuple t2 from r2:

// Join tables r1 and r2
	for every t1 in r1 and t2 in r2
		// t1 is a tuple from r1
		// t2 is a tuple from r2
		if isJoinable(t1, t2, overlap):
			newTuple = combineTuples(t1, t2, uniqueCols)
			add new tuple into the output relation*

* consider making seperate functions for ifJoinable(t1, t2, overlap) and combineTuples(t1, t2, uniqueCols)
```

Let's think about the line: `for every t1 in r1 and t2 in r2`. The "for every" sounds like the "for all" quantifier. We are using the same quantifier for two variables, the "for all." How do we usually represent that in code? Hint: a `for` loop. But we have nested quantifiers. How do we represent that in code? By iterating over every element (t1) in a table (r1) and then for each t1 we need to iterate over every singe element (t2) in the second table (r2).

It may have been a while since you have thought about Big-O notation. This about the nested quantifiers and the code construct needed, a `for` loop. Can you determine what the Big-O runtime of this algorithm should be? Include a text file in your submission with your answer.


Helper functions:
~~~
function isJoinable (Tuple t1, Tuple t2, overlap):

	for each pair (index1, index2) from overlap:
		if (t1[index1] != t2[index2]):
			return FALSE
			
	return TRUE
~~~

~~~
function combineTuples (Tuple t1, Tuple t2, uniqueCols):
	let newTuple be a new empty tuple
	
	copy all values from t1 into newTuple
	
	for i in uniqueCols:
		copy t2[i] into newTuple
	
	return newTuple
~~~

`Take a screenshot of your natural join function, if you used helper functions take additional screenshots of each function. These are the only screenshots required for the lab` (make sure to also include your Big-O analysis).

---
### Test Case
```c++
int main() {  
    Tuple snap_t1;  
    snap_t1.push_back("33333");  
    snap_t1.push_back("Snoopy");  
    snap_t1.push_back("12 Apple St.");  
    snap_t1.push_back("555-1234");  
  
    Tuple snap_t2;  
    snap_t2.push_back("12345");  
    snap_t2.push_back("C. Brown");  
    snap_t2.push_back("12 Apple St.");  
    snap_t2.push_back("555-1234");  
  
    Tuple snap_t3;  
    snap_t3.push_back("22222");  
    snap_t3.push_back("P. Patty");  
    snap_t3.push_back("56 Grape Blvd");  
    snap_t3.push_back("555-9999");  
  
  
    Tuple csg_t1;  
    csg_t1.push_back("cs101");  
    csg_t1.push_back("12345");  
    csg_t1.push_back("A");  
  
    Tuple csg_t2;  
    csg_t2.push_back("cs101");  
    csg_t2.push_back("22222");  
    csg_t2.push_back("B");  
  
    Tuple csg_t3;  
    csg_t3.push_back("cs101");  
    csg_t3.push_back("33333");  
    csg_t3.push_back("C");  
  
    Tuple csg_t4;  
    csg_t4.push_back("EE200");  
    csg_t4.push_back("12345");  
    csg_t4.push_back("B+");  
  
    Tuple csg_t5;  
    csg_t5.push_back("EE200");  
    csg_t5.push_back("22222");  
    csg_t5.push_back("B");  
  
    Header h1;  
    h1.push_back("StudentID");  
    h1.push_back("Name");  
    h1.push_back("Address");  
    h1.push_back("Phone");  
  
    Header h2;  
    h2.push_back("Course");  
    h2.push_back("StudentID");  
    h2.push_back("Grade");  
  
    Relation r1;  
    r1.setName("snap");  
    r1.setHeader(h1);  
    r1.addTuple(snap_t1);  
    r1.addTuple(snap_t2);  
    r1.addTuple(snap_t3);  
  
    Relation r2;  
    r2.setName("csg");  
    r2.setHeader(h2);  
    r2.addTuple(csg_t1);  
    r2.addTuple(csg_t2);  
    r2.addTuple(csg_t3);  
    r2.addTuple(csg_t4);  
    r2.addTuple(csg_t5);  
  
    cout << r1.natJoin(&r2)->toString() << endl << endl;  
}
```

```c++
StudentID=12345, Name=C. Brown, Address=12 Apple St., Phone=555-1234, Course=EE200, Grade=B+
StudentID=12345, Name=C. Brown, Address=12 Apple St., Phone=555-1234, Course=cs101, Grade=A
StudentID=22222, Name=P. Patty, Address=56 Grape Blvd, Phone=555-9999, Course=EE200, Grade=B
StudentID=22222, Name=P. Patty, Address=56 Grape Blvd, Phone=555-9999, Course=cs101, Grade=B
StudentID=33333, Name=Snoopy, Address=12 Apple St., Phone=555-1234, Course=cs101, Grade=C
// Note: the order of your tuples and columns may be different!
```

---
### Conclusion

Once you get the natural join function working it will make the rest of this project much easier. But how do you now whether or not it is working? You should come up with test cases (like the one above) that test out your implementation. Think about how the natural join works. What happens if there are no columns to join? This is a good test case. What happens if all columns join? This is another good test case. We've already provided a test case where one column joins. You should also consider testing where multiple columns join. 

---
### TODO for the project 
**(NOT REQUIRED FOR THE LAB)**

[Top](#Lab4)
