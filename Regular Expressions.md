# Regular Expressions

A regular expression is a way to search through a string for multiple different tasks in a very advanced way. 

## Format:
A regular expression begins an ends with a '/'. Another thing that may exist in the regular expression are flags, the flags come after the '/' and following are the flags
### Flags:
```
g -> global
i -> case insenistive
m -> multiline
s -> single line (dotall)
u -> unicode
y -> sticky
```
An example of a regular expression without a flag:
```re
/cat/
```
This RE would search for the word `cat` and then return to us the first occurance of that word.
With the global flag:
```re
/cat/g
```
This would return all the occurances of the word cat in a provided string.
We can also use multiple flags together. Consider the following example:
```re
/The/ig
```
What this would do is it would look for the word `The` globally and it would actually ignore the case of the word and would look for all occurances of `The`, `the`, `tHe`, `THe` etc.

## Special Characters:
### +:
The `+` symbol is really powerful in RE. What this allows us to do is match atleast one or more of the preceding token. For Example:
```re
alll test # Consider this string
/al+/g
```
What this regular expression will do is look for text `al` and then check if there are any other `l` in front of it and it will extract all the `l` and combine it with `al` and return it