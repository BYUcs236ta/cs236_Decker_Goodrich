# HW01a_FSA_RegEx
---
### Question 1

Draw a finite state automaton (FSA) for a string representing a single time of day. Your FSA should accept times such as: 

`12:36 pm 1:59 am 4:00 pm 2:45 am` 

It also should reject times like: 

`01:00 pm 99999:00 am 13:99 pm 10:60 pm`

Note that there are no leading zeros. Also, am and pm are lowercase and have no periods. A terminal error state is not necessary. 

To make the task easier, you can make up any grouping you want. For example, you may wish to let:

\<digit\> represent any digit

\<1\> represent the digit 1

\<0-2\> represent any digit 0 through 2

\<space\> represent a space.

Time statements are formatted in many ways, but we want your FSA to only accept the above time format and not military time or any other time format.

A useful site for drawing them digitally: [https://www.madebyevan.com/fsm/](https://www.madebyevan.com/fsm/)

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