---
layout: default
title: Math is Fun!
parent: BIOS823
nav_order: 1
---

# Math is Fun! Project Euler
I provide three solutions to 
questions from [Project Euler](https://github.com/delashu/do_parallel) (math problems that require computing and finesse). 
I aim to discuss my workflow, issues faced, and proposed solution to each problem. All computing is done in Python 3.7.8.    


### 1. Prime Power Triples
[This question](https://projecteuler.net/problem=87) states, *"How many numbers below fifty million can be expressed as the sum of a prime square, prime cube, and prime fourth power?"*   
My first idea was to use numpy to generate a large set of prime numbers. Unfortunately, numpy does not currently have a built-in function that generates the first X prime numbers. Thus the first objective was to generate prime numbers. Below is a snippet of code that does this indefinitely:  
```python
pnum = 2 #set the first prime number
while True: 
    #iterate through 2 and the next square root integer
    for i in range(2, int(math.sqrt(pnum)+1)):
        if pnum%i==0:
            break
    #generate the next prime by adding to pnum
    pnum+=1
```


Because an indefinite generation of primes is useless, the next objective was to find the upper limit of **A** in which **A**^2 + **B**^3 + **C**^4 < 50,000,000.  
In order to find this value of A, we set B and C to the lowest possible prime, 2 and solve for A.  
```python
A = math.ceil(math.sqrt(50000000 - (2**3 + 2**4)))
```

The output of A is found to be 7072.  
7072 represents the greatest possible integer number that can be used in the inequality **A**^2 + **B**^3 + **C**^4 < 50,000,000. 
I thought to generate all possible combinations of three primes from a list of all primes from 2 to the upper limit of 7072. Each list would then be used to compute the sum of **A**^2 + **B**^3 + **C**^4.    

```python
list(combinations(list_of_primes,3))
```

This generation crashed the kernel on my jupyter notebook. Generating all possible combinations of three manually would not be a viable solution.  
Instead I thought to create all posible combinations through a nested for loop. I aimed to loop through the full list of primes three times in this fashion: 

```python
for a in list_of_primes: 
    for b in list_of_primes:
        for c in list_of_primes: 
            my_sum = sum(a**2+b**3+c**4)
```
This too crashed my kernel. This solution would not work.  

Given that I solved the upper limit of A, I then thought to solve the upper limit of **B** and **C** in a similar fashion. Perhaps this would lighten the load of my for loop: 
```python
B= math.ceil((50000000 - (2**2 + 2**4) )**(1/3))
C= math.ceil((50000000 - (2**2 + 2**4) )**(1/4))
```

I then used these limits in my for loop as such: 
```python
for p_one in primes_one:
    for p_two in primes_two: 
        for p_three in primes_three:
            psum = p_one**2 + p_two**3 + p_three**4
```

Bringing all of this together is my below function. The function requires the import of *numpy*, *math*, and *itertools*. The function allows for the user to set the upper limit of the sum to calculate. The default sum is 50,000,000, but the user can set this limit.     

```python
import numpy as np
import math 
from itertools import product

def prime_powers(upper_limit = 50000000):
    """
    Input: upper limit integer sum of a prime square, prime cube, and prime fourth power. Default is fifty million    

    Output: Unique numbers that can be expressed as the sum (less than fifty million) of a prime square, prime cube, and prime fourth power. 

    Behavior: This function takes the user input (default of fifty million) and first creates three upper limits of prime numbers to loop through. 
    Once the three limits are set, the function calculates the sum of a prime square, prime cube, and prime fourth power. If the sum is unique, the function adds to a counter. 
    """    
    #set the first prime
    pnum = 2
    #create an empty list for which the primes will be stored: 
    primes = []
    #create three upper limits to iterate through
    limit_one = math.ceil(math.sqrt(upper_limit - (2**3 + 2**4)))
    limit_two= math.ceil((upper_limit - (2**2 + 2**4) )**(1/3))
    limit_three= math.ceil((upper_limit - (2**2 + 2**4) )**(1/4))
    #while loop to create primes based on the three upper limits. 
    while True: 
        for i in range(2, int(math.sqrt(pnum)+1)):
            if pnum%i==0:
                break
        else:
            primes.append(pnum)
            #ensure that the primes we are making do not exceed the first limit
            if pnum>limit_one:
                break
        pnum+=1
    #first list to iterate through
    primes_one = primes
    #second list to iterate through
    primes_two = primes[:limit_two]
    #third list to iterate through
    primes_three = primes[:limit_three]
    #create empty set to update that stores the UNIQUE sums
    out_list=set()
    #loop through each limit
    for p_one in primes_one:
        for p_two in primes_two: 
            for p_three in primes_three:
                psum = p_one**2 + p_two**3 + p_three**4
                if psum > 50000000:
                    break
                else:
                    out_list.update([psum])
    return out_list

```
My proposed solution to this problem is **1097343**. Thus 1097343 unique numbers below fifty million can be expressed as the sum of a prime square, prime cube, and prime fourth power.  
