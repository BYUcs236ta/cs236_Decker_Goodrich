# Lab5
---
# Table of Contents
- [Part 0 - Recap](#Part-0---Recap)
- [Datalog Code](#Datalog-Code)
- [Part 1 - Dependency Graph](#Part-1---Dependency-Graph)
- [Part 2 - Reverse Dependency Graph](#Part-2---Reverse-Dependency-Graph)
- [Part 3 - Reverse Post Order](#Part-3---Reverse-Post-Order)
- [Part 4 - Strongly Connected Components](#Part-4---Strongly-Connected-Components)
- [Example 1](#Example-1)
	- [Forward](#Forward)
	- [Reverse](#Reverse)
	- [Post-Order](#Post-Order)
	- [Reverse Post-Order](#Reverse-Post-Order)
	- [SCC's](#SCC's)
- [Example 2:](#Example-2:)
	- [Forward](#Forward)
	- [Reverse](#Reverse)
	- [Post-Order](#Post-Order)
	- [Reverse Post-Order](#Reverse-Post-Order)
	- [SCC's](#SCC's)
- [Conclusion](#Conclusion)
- [TODO for the project **(NOT REQUIRED FOR THE LAB)**](#TODO-for-the-project-**(NOT-REQUIRED-FOR-THE-LAB)**)


---
### Part 0 - Recap
1. We have created a programming language from the ground up. In Projects 1, 2a, 2b we defined the syntax, and converted the raw input text to tokens, then into a data structure that represents the program.
2. For Projects 3 and 4 we took that program and interpreted it. We gave that syntax its meaning.
3. For this final project we are going to increase the efficiency of the `Rule Evaluation` it using some graph theory.
4. This lab will be written, it will be graded on correctness, you are welcome to come into the TA lab to check over your work and get it graded on the spot.
5. Some examples are given below the instructions, try doing those first and check your work.

---
### Datalog Code
```datalog
Schemes:
  dog(v,w,x,y,z)
  cat(a,b,c,d,e)
  fish(a,b,c,d,e)
  bird(a,b,c,d,e,f,g,i)
  monkey(a,b,c,d,e)
  spider(a,b,c,d,e)


Facts:
  dog('a','b','c','d','e').
  fish('f','g','h','i','j').
Rules:
  #|R0|# cat(a,b,c,d,e) :- dog(a,b,c,d,e). 
  #|R1|# cat(a,b,c,d,e) :- fish(b,c,d,e,a). 
  #|R2|# fish(a,b,c,d,e) :- cat(b,c,d,e,a). 
  #|R3|# cat(a,b,c,d,e) :- cat(b,a,c,d,e). 

  #|R4|# bird(a,f,b,g,c,h,d,i) :- cat(a,b,c,d,e),fish(f,g,h,i,j). 
  #|R5|# spider(a,b,c,d,e) :- dog(a,b,c,d,e). 
  #|R6|# monkey(a,b,c,d,e) :- bird(a,b,z,y,c,d,e,i),spider(m,n,x,y,z). 
  #|R7|# spider(a,a,c,d,e) :- monkey(a,b,c,d,e),spider(a,w,x,y,z). 
  #|R8|# spider(a,b,c,d,e) :- spider(e,a,b,c,d). 


Queries:
  spider(a,b,c,d,e)?
```

---
## Part 1 - Dependency Graph

Draw the dependency graph and give the adjacency list for the rules of the above input. 

---
## Part 2 - Reverse Dependency Graph

Give the reverse dependency graph and its adjacency list

---
## Part 3 - Reverse Post Order

Give the Post Order after performing a `Depth First Search Forest` on the Reverse Graph. Wherever a choice is to be made choose the node that is at a lower index.

---
## Part 4 - Strongly Connected Components

Give the Strongly connected components after performing a `Depth First Search Forest` on the Forward Graph. Use the Reverse of your Post Order from part 3 as the priority list.

---
### Example 1
```datalog
Rules:

#|0|# Sibling(x,y) :- Sibling(y,x).

#|1|# Ancestor(x,y) :- Ancestor(x,z), Parent(z,y).

#|2|# Ancestor(x,y) :- Parent(x,y).
```

#### Forward

![](/assets/images/Lab5_Forward_SimpleExample.png)

| From | To   |
| ---- | ---- |
| 0    | 0    |
| 1    | 1, 2 |
| 2    |      |

#### Reverse

![](/assets/images/Lab5_ReverseGraph_SimpleExample.png)

| From | To  |
| ---- | --- |
| 0    | 0   |
| 1    | 1   |
| 2    | 1    |

#### Post-Order

>\[0, 1, 2\]

#### Reverse Post-Order

>\[2, 1, 0\]

#### SCC's

1. SCC{2}
2. SCC{1}
3. SCC{0}


---
### Example 2:
```datalog
Rules:

#|0|# Alpha(x, y, z) :- Bravo(a, b, z), Charlie(x, y, c).

#|1|# Bravo(x, y, z) :- Charlie(a, x, z), Alpha(y, a, b).

#|2|# Charlie(x, y, z) :- Delta(z, y, x).

#|3|# Delta(x, y, z) :- Charlie(z, x, y).

#|4|# Delta(x, y, z) :- Echo(y, z, x).
```

#### Forward

![](/assets/images/Lab5_Forward_ComplicatedExample.png)

| From | To   |
| ---- | ---- |
| 0    | 1, 2 |
| 1    | 0, 2 |
| 2    | 3, 4 |
| 3    | 2    |
| 4    |      |

#### Reverse

![](/assets/images/Lab5_RevereGraph_ComplexExample.png)

| From | To      |
| ---- | ------- |
| 0    | 1       |
| 1    | 0       |
| 2    | 0, 1, 3 |
| 3    | 2       |
| 4    | 2       |

#### Post-Order

>\[1, 0, 3, 2, 4\]

#### Reverse Post-Order

>\[4, 2, 3, 0, 1\]

#### SCC's

1. SCC{4}
2. SCC{2, 3}
3. SCC{0, 1}


---
## Conclusion
1. Submit a .png or .pdf of your work to learning suite
2. Leave any feedback in the feedback section of the lab

---
## TODO for the project 

**(NOT REQUIRED FOR THE LAB)**
1.  Write code to perform the above steps
2. Refactor Project 4 code to use this new Algorithm for rule evaluation
