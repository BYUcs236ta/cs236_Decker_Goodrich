# Lab4
# UNDER CONSTRUCTION

---
# Table of Contents

---
### Part 0 - Recap
- In projects 1-3 we have converted a text file -> vector\<Token\> -> DatalogProgram -> Database
- Next step is to add the `Rule` evaluation to the `Interpreter`
- Keep up the good work!


---
### Part 1 - Natural Join Algorithm

#### Example

snap
| StudentID (0)   | Name (1)            | Address (2)              | PhoneNumber (3)      |
| ------- | ---------------- | ------------------ | ---------- |
| '12345' | 'C.      Brown'  | '12    Apple St.'  | '555-1234' |
| '22222' | 'P.       Patty' | '56    Grape Blvd' | '555-9999' |
| '33333' | 'Snoopy'         | '12    Apple  St.' | '555-1234' |

csg
| Course (0)  | StudentName (1)  | Grade (2) |
| ------- | -------- | ------- |
| 'CS101' | '12345' | 'A'     |
| 'CS101' | '22222'  | 'B'     |
| 'CS101' | '33333'  | 'C'     |
| 'EE200' | '12345'  | 'B+'    |
| 'EE200' | '22222'  | 'B'     |

We will calculate snap |x| csg

#### Step 1 - Calculate header overlap

snap and csg overlap on index pair `(0, 1)`
csg has unique attribute names at indices `[0, 2]`


#### Step 2 - Create new Header
newHeader = \[StudentID, Name, Address, PhoneNumber, Course, Grade\]

#### Step 3 - Find Tuples

```
for each tuple t1 from snap:
	for each tuple t2 from csg:
		if t1 and t2 are the same at all locations in our overlap:
			Create a new tuple*
			add the tuple into the output relation*

* consider making seperate functions for ifJoinable(t1, t2, overlapData) and combineTuples(t1, t2, uniqueCols)
```

```
t1 = ('12345', 'C. Brown', '12 Apple St.', '555-1234') 

can join with

t2 = ('CS101', '12345', 'A')

because t1.at(0) == t2.at(1);
```


```
t1 = ('12345', 'C. Brown', '12 Apple St.', '555-1234') 

cannot join with

t2 = ('CS101', '22222', 'B')

because t1.at(0) != t2.at(1)
```


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
```

---
### Conclusion

---
### TODO for the project 
**(NOT REQUIRED FOR THE LAB)**