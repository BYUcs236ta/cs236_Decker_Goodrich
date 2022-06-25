# HW01a
---
### Question 1

Draw a finite state automaton (FSA) for a string representing a single time of day. Your FSA should accept times such as: 

`12:36 pm 1:59 am 4:00 pm 2:45 am 

It also should reject times like: 

`01:00 pm 99999:00 am 13:99 pm 10:60 pm`

---
### Question 2

Give a regular expression for the FSA described in Question 1. 

Here are the symbols we would like you to use:
>() - parenthesis 
>
>\* - Kleene Star 
>
 >$\cup$ or | - Union
 >
 >[] - brackets ([2-5] is a shorthand for (2 | 3 | 4 | 5)) 
 
 Test your regex [here](https://regexr.com/)