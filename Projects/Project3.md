# Project3

UNDER CONSTRUCTION Currently hosted on learning suite

---
# Table of Contents

---
## General Project Requirements

1.  Projects are to be completed by each student individually (not by groups of students)
2.  Projects must be passed-off (over Zoom or Gather) with a TA or by the pass-off server to be given credit
3.  All output should be sent to standard output, not a file
4.  Even though we provide the test cases, the definition of your project working is that it passes all tests on the `pass-off machine`
    -   If the test cases pass on your machine but not on the pass-off machine you should start by doing the following:
        -   Test your code and the official test cases on a Linux machine, where you should be able to replicate the error
        -   Look at how you initialize object. IDEs perform default initialization, but building from commandline with g++ on Linux does not

---
## Resources
1.  READ THE FOLLOWING: [Project 3 Guide](https://learningsuite.byu.edu/plugins/Upload/fileDownload.php?fileId=680dbbb2-Blpa-2gOT-PzAP-p4d57580db0c)
2.  You may also find the [TA slides](https://docs.google.com/presentation/d/1G2zYXPkVIvsY9fuSAidVYV4a1gfJW5L9nfvwSMq-Dqc/edit?usp=sharing) and [help video](https://learningsuite.byu.edu/plugins/Upload/fileDownload.php?fileId=d5492d6a-1ZMv-LGR3-CUYY-qi49e767c84b) useful!

Write an interpreter that uses relational database operations to evaluate the queries in a Datalog Program. For this project, use only the facts to evaluate the queries. (The evaluation of rules will be added in the next project.)

---
## Example Input and Output

---
### Example Input
~~~
Schemes:
  SK(A,B)
Facts:
  SK('a','c').
  SK('b','c').
  SK('b','b').
  SK('b','c').
Rules:

Queries:
  SK(X,'c')?
  SK('b','c')?
  SK(X,X)?
  SK(X,Y)?
  SK('c','c')?
~~~

---
### Example Output
~~~
SK(X,'c')? Yes(2)
  X='a'
  X='b'
SK('b','c')? Yes(1)
SK(X,X)? Yes(1)
  X='b'
SK(X,Y)? Yes(3)
  X='a', Y='c'
  X='b', Y='b'
  X='b', Y='c'
SK('c','c')? No
~~~

---
## The Database Classes

The solution for this project has two main parts, (1) a simple relational database and (2) an interpreter that uses the database to evaluate queries.

A relational database is a collection of relations. A relation has a name, a header, and a set of tuples. A header is a list of attribute names. A tuple is a list of attribute values. Relations are used as operands in relational operations such as select, project, and rename.

In this project a Datalog program is represented by a database. Each scheme in the program defines a relation in the database. Each fact in the program defines a tuple in a relation.

Write classes that implement a simple relational database. Your design must include at least the following classes: Database, Relation, Header, and Tuple. Provide functions in the Relation class for each of the relational operations (select, project, and rename). Each of these functions operates on an existing relation and returns a new Relation (the result of the operation).

The Relation class must use a set data type to hold the tuples in the relation.

Test your database classes before combining them with the query interpreter described in the next section.

Decouple the database classes from the Datalog classes. For example, don't have Tuples that contain Parameters.

---
## Datalog Program Evaluation

Use the relational database classes to evaluate the Datalog program using these major steps:

1.  Evaluate the schemes.
2.  Evaluate the facts.
3.  Evaluate the queries.

---
### Evaluating Schemes

Start with an empty Database. For each scheme in the Datalog program, add an empty Relation to the Database. Use the scheme name as the name of the relation and the attribute list from the scheme as the header of the relation.

---
### Evaluating Facts

For each fact in the Datalog program, add a Tuple to a Relation. Use the predicate name from the fact to determine the Relation to which the Tuple should be added. Use the values listed in the fact to provide the values for the Tuple.

---
### Evaluating Rules

This will be done in `Project 4`. 

---
### Evaluating Queries

For each query in the Datalog program, use a sequence of select, project, and rename operations on the Database to evaluate the query. Evaluate the queries in the order given in the input.

1.  Get the Relation from the Database with the same name as the predicate name in the query.
2.  Use one or more select operations to select the tuples from the Relation that match the query. Iterate over the parameters of the query: If the parameter is a constant, select the tuples from the Relation that have the same value as the constant in the same position as the constant. If the parameter is a variable and the same variable name appears later in the query, select the tuples from the Relation that have the same value in both positions where the variable name appears.
3.  After selecting the matching tuples, use the project operation to keep only the columns from the Relation that correspond to the positions of the variables in the query. Make sure that each variable name appears only once in the resulting relation. If the same name appears more than once, keep the first column where the name appears and remove any later columns where the same name appears. (This makes a difference when there are other columns in between the ones with the same name.)
4.  After projecting, use the rename operation to rename the header of the Relation to the names of the variables found in the query.

The operations must be done in the order described above: any selects, followed by a project, followed by a rename.

---
### Query Evaluation Output

For each query, output the query and a space. If the relation resulting from evaluating the query is empty, output 'No'. If the resulting relation is not empty, output 'Yes(n)' where n is the number of tuples in the resulting relation.

If there are variables in the query, output the tuples from the resulting relation.

Output each tuple on a separate line as a comma-space-separated list of pairs. Each pair has the form N='V', where N is the attribute name from the header and V is the value from the tuple. Output the name-value pairs in the same order as the variable names appear in the query. Indent the output of each tuple by two spaces.

Output the tuples in sorted order. Sort the tuples alphabetically based on the values in the tuples. Sort first by the value in the first position and if needed up to the value in the nth position.

All output should be sent to standard output, not a file.

---
### Assumptions

You may assume the following about the Datalog program:

1.  All schemes, facts, rules, and queries with the same name will have the same number of parameters.
2.  No two attributes in the same scheme will have the same name.
3.  No two schemes in the scheme list will have the same name.
4.  Every relation referenced in a fact, rule, or query will have been defined in the scheme section.

---
### Implementation Requirements

1.  You are required to use a relational database approach in solving the problem in this project. You must create tuples in relations and evaluate queries using select, project, and rename operations.
2.  Your solution must include the following classes: Database, Relation, Header, and Tuple.
3.  The Relation class must use a set data type to hold the tuples in the relation.
4.  The relational operations (select, project, and rename) must be implemented as functions in the Relation class.
5.  Queries must be evaluated using relational operations (select, project, and rename).
6.  The relational operators must not produce relations with duplicate tuples or duplicate names in the header.

---
### Implementation Suggestions

1.  **How does the select operation work?**
    
    It is useful to have two select operations.
    
    1.  One version of select finds all the tuples that have a constant value in a certain column.
        
        This select function could take two parameters, a position and a value. The given position in a tuple would need to have a value equal to the given value for that tuple to be included in the result.
        
    2.  Another version of select finds all the tuples that have the same value in two different columns (it doesn't matter what the value is, as long as both columns have the same value).
        
        This select function could take two parameters that are both positions. The two given positions in a tuple would need to have equal values for that tuple to be included in the result.
        
    
    The select operation does not change the header of the relation. The header of the relation resulting from the select is the same as the header of the original relation.
    
2.  **How does the project operation work?**
    
    The project operation changes the number and order of columns in a relation. The resulting relation may have either the same number or fewer columns. Project changes the header and all the tuples in the relation.
    
    The parameter to the project function could be a list of the positions of the columns that should be included in the result.
    
3.  **Why does the project need to be able to change the order of the columns?**
    
    The project function needs to be able to change the order of the columns in a relation to support evaluating rules in the next project. Changing the order of the columns is not needed for evaluating the queries in this project.
    
4.  **How does the rename operation work?**
    
    The rename operation changes the header of the relation. The resulting relation has the same tuples as the original.
    
5.  **Should rename change one attribute at a time, or replace the entire list of attributes at once?**
    
    Replacing the entire list of attributes is easier and avoids issues with name conflicts.
    
6.  **What should select, project, and rename return?**
    
    These functions should create a new relation that holds the header and tuples resulting from the operation. They should return this new relation. They should not modify the original relation. The new relation will have the same name as the original, but the header and tuples will change depending on the operation.
    
7.  **When a query has two variables with different names, do the values given to the variables need to be different?**
    
    No. Two variables with different names can be given the same value.
    
    Consider the query from the example Datalog program:
    
    SK(X,Y)?
    
    One of the answers for this query is:
    
    X='b', Y='b'