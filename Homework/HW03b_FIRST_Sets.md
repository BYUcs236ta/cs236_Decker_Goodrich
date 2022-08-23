# HW03b_FIRST_Sets
---
### Question 1

Find the FIRST sets for all of the productions with the following non-terminals (on the left-hand side) from Project 2:
```
rule
query
schemeList
factList
ruleList
queryList
```

Project 2 Grammar (lowercase words represent a single non-terminal, upper case words represent a single terminal, lambda is the empty string)
```
datalogProgram -> SCHEMES COLON scheme schemeList FACTS COLON factList RULES COLON ruleList QUERIES COLON query queryList EOF

schemeList -> scheme schemeList | lambda
factList -> fact factList | lambda
ruleList -> rule ruleList | lambda
queryList -> query queryList | lambda

scheme -> ID LEFT_PAREN ID idList RIGHT_PAREN
fact -> ID LEFT_PAREN STRING stringList RIGHT_PAREN PERIOD
rule -> headPredicate COLON_DASH predicate predicateList PERIOD
query -> predicate Q_MARK

headPredicate -> ID LEFT_PAREN ID idList RIGHT_PAREN
predicate -> ID LEFT_PAREN parameter parameterList RIGHT_PAREN

predicateList -> COMMA predicate predicateList | lambda
parameterList -> COMMA parameter parameterList | lambda
stringList -> COMMA STRING stringList | lambda

idList -> COMMA ID idList | lambda
parameter -> STRING | ID
```
