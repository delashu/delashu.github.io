---
layout: default
title: Number Theory
parent: BIOS823
nav_order: 2
---


# Number Theory & a Google Recruitment Puzzle

The objective of this puzzle is to find the first 10-digit prime in the decimal expansion of 17π. My proposed solution partitions the puzzle into several sub-problems. Each sub-problem is solved with its own function in python. All computing is done in Python 3.7.8.    
Note that this is a blog post that guides the reader through my workflow, ideas, and functions to each solution. **To access the jupyter notebook where the solutions were processed and completed, visit [here.](https://github.com/delashu/pysolve_notebooks/blob/main/number_theory.ipynb)**  


### Sub-Problem 1  
**Objective**: Write a function to generate an arbitrary large expansion of a mathematical expression like π.  

```python
import math
from sympy import * 
def expand(digits = 10, mathex = "pi", multiplier = 1):
    """
    INPUT: 
    BEHAVIOR: 
    OUTPUT:
    """
    if mathex == "pi":
        myval = str(N(multiplier * pi, digits))
        myval = str(myval.split('.')[-1])
        return myval
    if mathex == "e":
        myval = str(N(multiplier * E, digits))
        myval = str(myval.split('.')[-1])
        return myval
```

### Sub-Problem 2  
**Objective**: *Write a function to check if a number is prime.*  
Python, numpy, and other popular python math libraries do not have a built-in function that checks if an integer is a prime number. One ancient algorithm that identifies a prime that can be implemented in python is [The Sieve of Eratosthenes.](https://en.wikipedia.org/wiki/Sieve_of_Eratosthenes). The algorithm iteratively finds multiples of integers to an integer upper limit starting with 2. All multiples of 2 starting from 2 to the upper integer limit are marked as **not** prime. Then, all multiples of 3 to the upper limit are marked as **not** prime. The next number that has not yet been marked, 
5 is then used. All multiples of 3 to the upper limit are marked as **not** prime. This continues until the integer upper limit is reached.  

```python
def is_prime(user_num = 1):
    """
    INPUT: Integer number to check if prime
    BEHAVIOR:check if the user input is a prime number
    OUTPUT: string that shows "prime" or "not prime"
    """
    #if the user inputs 1, we provide not prime and return the string
    if user_num == 1:
        ip = "not prime"
        return ip
    else:
        #else, we will use the sieve of eratosthenes
        for i in range(2, user_num):
            #iterate through the integers from 2 to the upper limit of the user input 
            if user_num % i ==0:
                #if any multiples exist, then this is NOT a prime. 
                ip = "not prime"
                return ip
        #if we go through the entire loop, then we have a prime
        ip = "prime"
        return ip
```


### Sub-Problem 3  
**Objective**: *Write a function to generate sliding windows of a specified width from a long iterable (e.g. a string representation of a number)* 

I wrote a function that takes in a string object, and returns a list of lists. Each sublist within the main list contains a string that has the length of the user-specified window width. The sublist is defined by a sliding window.      
For example if the user inputs a string of "12345" and a window width of 2, the output will be a list that contains the following sublists:    
[12], [23], [34], [45]  
Below is the code that implements these operations:  
```python
#intialize main empty list that will hold all sub-lists
all_lists = []
def sliding(my_string = "8309735", window=3):
    """
    INPUT: string representation of a long integer. 
        sliding window length

    BEHAVIOR:user inputs an integer and the function 
    will generate sliding windows of that input 
    length from a string representation of a number (iterable)
    
    OUTPUT: list of lists. 
    each sublist contains a string of specified sliding window length
    """
    #store the length of the string
    total_len = len(my_string)
    #create for loop to iterate through 
    #all possible sliding windows of the string
    for i in range(0,(total_len - window+1)):
        #create a substring that stores a window
        substr = my_string[i:i+window]
        #store the substring into a list
        sublist = [substr]
        #append the list to the main list
        all_lists.append(sublist)
    #return the main list that now contains all sublists
    return all_lists
```

Prompt: 


Find the first 10-digit prime in the decimal expansion of 17π.

The first 5 digits in the decimal expansion of π are 14159. The first 4-digit prime in the decimal expansion of π are 4159. You are asked to find the first 10-digit prime in the decimal expansion of 17π.

Write unit tests for each of these three functions. You are encouraged, but not required, to try test-driven development.

Now use these helper functions to write the function that you need. Write a unit test for this final function, given that the first 10-digit prime in the expansion e is 7427466391. Finally, solve the given problem.