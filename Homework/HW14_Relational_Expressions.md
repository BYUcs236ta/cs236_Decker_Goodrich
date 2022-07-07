# HW14_Relational_Expressions
---
### Question 1

Use the relations below to evaluate the relational expressions or explain why the expression is invalid.

a. $$\pi_{E} S |{\times}| U$$
b. $$Q |{\times}| R$$
c. $$(\sigma_{B < 2} Q) |{\times}| R |{\times}| (\rho_{C \leftarrow B} U)$$

---
### Relations

##### Relation Q
| A   | B   |
| --- | --- |
| 5   | 1   |
| 6   | 1   |
| 4   | 2   |
| 3   | 4   |

##### Relation R
| B   | C   |
| --- | --- |
| 1   | 4   |
| 2   | 4   |
| 2   | 5   |
| 3   | 6   |
| 3   | 9   |

##### Relation S
| C   | D   | E   |
| --- | --- | --- |
| 4   | 1   | 1   |
| 4   | 2   | 1   |
| 3   | 3   | 2   |
| 2   | 4   | 2    |

##### Relation U
| C   | D   |
| --- | --- |
| 1   | 2   |
| 2   | 4    |

---
### Question 2
Using the database below for each query give a relational algebra expression to answer the query

a. List the names of students whose phone number is `555-1234`. 

b. Find the names and corresponding course numbers of all students who have a class in the `Turing Aud.` 

c. Find the name and phone number of students taking any of the immediate prerequisites of `CS120`.

---
### Database

##### SNAP
| StudentID | Name        | Address        | Phone    |
| --------- | ----------- | -------------- | -------- |
| 12345     | C. Brown    | 12 Apple St.   | 555-1234 |
| 67890     | L. Van Pelt | 34 Pear Ave.   | 555-5678 |
| 22222     | P. Patty    | 56 Grape Blvd. | 555-9999 |
| 33333     | Snoopy      | 12 Apple St.   | 55-1234         |

##### CR
| Course | Room        |
| ------ | ----------- |
| CS101  | Turing Aud. |
| EE200  | 25 Ohm Hall |
| PH100  | Newton Lab            |

##### CDH
| Course | Day | Hour |
| ------ | --- | ---- |
| CS101  | M   | 9AM  |
| CS101  | W   | 9AM  |
| CS101  | F   | 9AM  |
| EE200  | Tu  | 10AM |
| EE200  | W   | 1PM  |
| EE200  | Th  | 10AM |
| PH100  | Tu  | 11AM     |

##### CSG 
| Course | StudentID | Grade |
| ------ | --------- | ----- |
| CS101  | 12345     | A     |
| CS101  | 67890     | B     |
| EE200  | 12345     | C     |
| EE200  | 22222     | B+    |
| EE200  | 33333     | B     |
| CS101  | 33333     | A-    |
| PH100  | 67890     | C-    | 

##### CP
| Course | Prerequisite |
| ------ | ------------ |
| CS101  | CS100        |
| EE200  | EE005        |
| EE200  | CS100        |
| CS120  | CS101        |
| CS121  | CS120        |
| CS205  | CS101        |
| CS206  | CS121        |
| CS206  | CS205             |

