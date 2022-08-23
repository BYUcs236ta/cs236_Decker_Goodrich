# HW02_Grammars
---
### Question 1

Give each possible string described by the following grammar. This is called the “Language” of
the grammar. S is the start symbol. (Recall that a language is a subset of V\*, where V is the
alphabet.)

 >S -> a | aTb | aTbTc
 >
 >T -> x | xy | xyz 

---
### Question 2

Give a grammar for the Time of Day language, which produces strings such as: 

`12:36 pm 1:59 am 4:00 pm 2:45 am` 

It also should not produce times like: 

`01:00 pm 99999:00 am 13:99 pm 10:60 pm`

Note that there are no leading zeros. Also, am and pm are lowercase and have no periods.

Time statements are formatted in many ways, but we want your grammar to only produce the above time format and not military time or any other time format.

If you want you may use BNF notation

Please give good mnemonic names for concepts such as \<Time of Day\>, which is to be the start symbol, and \<Hours\> for digits that are hour digits

Additionally you may use a notation shorthand for lists such as:

`01 | 02 | 03 | ... | 21 | 22 | 23`

