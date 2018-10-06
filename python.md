---
description: application of python
---

# Python

## Parsing word and letter in String

### Parsing word

it is for counting distinct word in a String or .txt.

####  guideline

* Split word by space or comma or ... by split function
* Define a set which contain distinct word in String 
*  For each word in the set, setting key is word, and value is the  the number of word appearing in String
* Print dict

#### code 

{% code-tabs %}
{% code-tabs-item title="count word" %}
```python
input_lines="list of lines fron std input to be parsed list word std"
d={}
def wordparser(input_lines):
#word split and put distinct word in set
    input=input_lines.split(" ")
    words=set(input)
#count word
    for word in words:
        d[word]=input.count(word)
    print d
    
    #output
    #{'std': 2, 'be': 1, 'word': 1, 'fron': 1, 'lines': 1, 'of': 1, 'list': 2, 'to': 1, 'input': 1, 'parsed': 1}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

#### addition

1. count function
   1. string.count\(substring, start=…, end=…\)

{% code-tabs %}
{% code-tabs-item title="count function" %}
```python

# Python program to demonstrate the use of 
# count() method  using optional parameters 
  
# string in which occurence will be checked 
string = "geeks for geeks" 
  
# counts the number of times substring occurs in  
# the given string between index 0 and 5 and returns  
# an integer 
print(string.count("geeks", 0, 5))   
print(string.count("geeks", 0, 15)) 

#output:
#1
#2
```
{% endcode-tabs-item %}
{% endcode-tabs %}

### Parsing letter

The function is to count the letter in a sentence.

#### guideline

* for each letter in line, split and put into a list called letters
* defined a set in which the distinct letter are
* for each letter put into dict, in which key is letter and value is the number of appearing time in list
* print dict

{% code-tabs %}
{% code-tabs-item title="parsing letter" %}
```python

input_lines="list of lines fron std input to be parsed list word std"

l2={}
#letter split
    input_lines=input_lines.replace(' ','')
    input_letter=[letter for letter in input_lines]
    letters=set(input_letter)
    for letter in letters:
        l2[letter]=input_letter.count(letter)
    print l2
    
    #output:
   # {'a': 1, 'b': 1, 'e': 3, 'd': 4, 'f': 2, 'i': 4, 'l': 3, 'o': 4, 'n': 3, 'p': 2, 's': 6, 'r': 3, 'u': 1, 't': 6, 'w': 1}
```
{% endcode-tabs-item %}
{% endcode-tabs %}



