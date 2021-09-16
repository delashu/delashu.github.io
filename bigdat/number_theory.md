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
**Objective**: *Write a function to generate an arbitrarily large expansion of a mathematical expression like π.*  
I used the [sympy library](https://docs.sympy.org/latest/index.html) to compute mathematical expressions such as π and e.  
In our problem, we are interested in finding the decimal expansion of 17π. I thus allow for the user to input a multiplier (such as 17) to expressions such as e and π.  
Below is the function:  

```python
import math
from sympy import pi, E, N
def expand(digits = 10, mathex = "pi", multiplier = 1):
    """
    INPUT: digits - The number of digits past the decimal the user is interested in 
           mathex - The mathematical expression the user is intersted in 
           multliplier - Multipliers to the mathematical expression
    BEHAVIOR: intakes desired math expression and outputs a string 
                representation for later processing
    OUTPUT: A string representation of digits past the decimal
    """
    if mathex == "pi":
        myval = str(N(multiplier * pi, digits))
        return myval
    if mathex == "e":
        myval = str(N(multiplier * E, digits))
        return myval
```

### Sub-Problem 2  
**Objective**: *Write a function to check if a number is prime.*  
Python, numpy, and other popular python math libraries do not have a built-in function that checks if an integer is a prime number. One ancient algorithm that identifies a prime that can be implemented in python is [The Sieve of Eratosthenes.](https://en.wikipedia.org/wiki/Sieve_of_Eratosthenes) The algorithm iteratively finds multiples of integers to an integer upper limit starting with 2. All multiples of 2 starting from 2 to the upper integer limit are marked as **not** prime. Then, all multiples of 3 to the upper limit are marked as **not** prime. The next number that has not yet been marked, 
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

While implementing this function to larger integer values, I realized the computation was taking too long. Iterating through the ranges of each value and finding existing multiples is an ineffecient method to define a prime number. I later came up with another solution to shorten computation time. The general idea of the function is to iterate through 2 to the ceiling of the square root of the user input integer, and attempting to find a divisor that results in no remainder. The function returns True or False.  

```python
def is_prime(user_num = 1):
    """
    INPUT: Integer number to check if prime
    BEHAVIOR:check if the user input is a prime number
        by iteratively checking the ceiling of square roots
    OUTPUT: string that shows "prime" or "not prime"
    """
    if user_num > 1 and isinstance(user_num, int) == True:  
        #if the number is greater than 1 AND it is a whole integer
        #then we iterate
        #from the input number to the cieling of the square root
        for num in range(2, int(math.sqrt(user_num)+1)):
            #if we find a whole number divisor, then the user input
            #is not a FALSE
            if (user_num % num) == 0:
                return False
        return True
    #if the number input is less than one, 
    #then we have a NON prime immediately
    return False
```



### Sub-Problem 3  
**Objective**: *Write a function to generate sliding windows of a specified width from a long iterable (e.g. a string representation of a number)* 

I wrote a function that takes in a string object, and returns a list of integers. Each integer within the list has the length specified by the user's window width.  
For example if the user inputs a string of "12345" and a window width of 2, the output will be a list that contains the following integers:    
[12, 23, 34, 45]  

I ensure that integers that start with 0 are removed from the list, as they will not have the specified length of interest.   
Below is the code that implements these operations:  
```python
def sliding(my_string = "8309735", window=3):
    """
    INPUT: string representation of a long integer. 
        sliding window length

    BEHAVIOR:user inputs an integer and the function 
    will generate sliding windows of that input 
    length from a string representation of a number (iterable)
    
    OUTPUT: list of integers. 
    each integer is of specified sliding window length
    """
    #intialize main empty list that will hold all sub-lists
    all_list = []
    my_string = str(my_string.split('.')[-1])
    #store the length of the string
    total_len = len(my_string)
    #create for loop to iterate through 
    #all possible sliding windows of the string
    for i in range(0,(total_len - window+1)):
        #create a substring that stores a window
        substr = my_string[i:i+window]
        #If the substring starts with 0, then we go to the next iteration of the loop
        #we want integers with entire length. starting with 0 will break this
        if substr[0] == '0':
            continue
        #append the string to the main list
        all_list.append(int(substr))
    #return the main list that now contains all sublist
    return all_list
```


### All Together Now!  
Now that I have defined three helper functions, I bring each of them together into one function to solve the puzzle.  
The function first creates a string representation of the decimal expansion of the user specified math equation (ie: 17π) and user specified length (10).  
The string is then used to create a list of integers, each with the user specified window length.  
Finally, the function iterates through the list of integers and returns the first prime number it finds.  

```python 
def digit_prime(pdigits = 10, pmathex = "pi", pmultiplier = 1, pwindow=3):
    """
    INPUT: 
        pdigits - The number of digits past the decimal the user is interested in
        pmathex - The mathematical expression the user is intersted in 
        pmultliplier - Multipliers to the mathematical expression
    
    BEHAVIOR: 
        uses the helper functions 'expand', 'sliding' and 'is_prime' to 
        determine the first prime in the decimal expansion of user input's window length 
        from the user input's mathematical expression
    
    OUTPUT: 
        First prime digit of user specified length from 
        decimal expansion of user input mathematical expression
    """
    #create string with 'expand' function
    my_expression = expand(digits = pdigits, mathex = pmathex, multiplier = pmultiplier)
    #create list of integers with length of user specified window
    slide_list = sliding(my_string = my_expression, window = pwindow)
    #iterate through the list and check if the value is prime
    for i in slide_list:
        if is_prime(i) == True:
            #if prime, we end the loop by returning the integer
            return(i)
```

  
I run the final function to solve the puzzle:  
```python 
print(digit_prime(pdigits = 120, pmathex = "pi", pmultiplier = 17, pwindow=10))
#output: 8649375157
```

<span style="color:red">**My final solution to find the first 10-digit prime in the decimal expansion of 17π is 8649375157**  </span>

</br>  
</br>  

### Unit Testing

Each helper function and the final function has been unit tested.  
Readers can look find the raw .py function files [here](https://github.com/delashu/pysolve_notebooks/blob/main/num_theory/num_theory.py)  and the unit test .py file [here](https://github.com/delashu/pysolve_notebooks/blob/main/num_theory/num_theory_test.py).  

Below is a suite of unit tests for sub function and final function that can be found in the above links. The final unit-test checks if the function correctly finds the first 10-digit prime in the expansion e (*answer*: 7427466391) :  

```python 
#import the num_theory functions from num_theory.py
from num_theory import * 
import math
from sympy import pi, E, N

"""
this is a python file for unit testing of the python file 'num_theory.py'
"""

"""
Testing 'expand() function'
"""
def test_expand_1():
    assert expand(digits = 20, mathex = "pi", multiplier = 1) == "3.1415926535897932385"

def test_expand_2():
    assert expand(digits = 12, mathex = "pi", multiplier = 17) == "53.4070751110"
    
def test_expand_3():
    assert expand(digits = 25, mathex = "e", multiplier = 1) == "2.718281828459045235360287"

    
    
"""
Testing 'is_prime() function'
"""

def test_is_prime_1():
    assert is_prime(user_num = 1) == False
    
def test_is_prime_2():
    assert is_prime(user_num = -7) == False
    
def test_is_prime_3():
    assert is_prime(user_num = 2.4) == False

def test_is_prime_4():
    assert is_prime(user_num = 4) == False

def test_is_prime_5():
    assert is_prime(user_num = 97) == True

    
"""
Testing 'sliding() function'
"""

def test_sliding_1():
    assert sliding(my_string = '200.89420', window = 2) == [89, 94, 42, 20]
    
def test_sliding_2():
    assert sliding(my_string = '69.0131094', window = 3) == [131, 310, 109]

    
"""
Testing 'digit_prime() function'
"""
def test_digit_prime_1():
    assert digit_prime(pdigits = 120, pmathex = "e", pmultiplier = 1, pwindow=10) == 7427466391
    
```


The unit tests can be run by executing the following in the terminal:  

```console 
pytest
``` 

Running the pytest in the terminal yields the following result: 

```console
==== 11 passed in 1.96s ====
```
