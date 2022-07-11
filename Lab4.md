# Lab4
---
# Table of Contents

---
### Part 0 - Recap
- In projects 1-3 we have converted a text file -> vector\<Token\> -> DatalogProgram -> Database
- Next step is to add the `Rule` evaluation to the `Interpreter`
- Keep up the good work!


---
### Part 1 - Natural Join Algorithm

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