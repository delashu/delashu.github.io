---
layout: default
title: Math is Fun!
parent: BIOS823
nav_order: 1
---

# Math is Fun! Project Euler
I provide three solutions to 
questions from [Project Euler](https://projecteuler.net) (math problems that require computing and finesse). 
I aim to discuss my workflow, issues faced, and proposed solution to each problem. All computing is done in Python 3.7.8.    
Note that this is a blog post that provides steps of each solution. **To access the jupyter notebook where the solutions were processed and completed, visit [here.](https://github.com/delashu/pysolve_notebooks/blob/main/math.ipynb)**

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
In order to find this value of A, we set B and C to the lowest possible prime, 2, and solve for A.  
```python
A = math.ceil(math.sqrt(50000000 - (2**3 + 2**4)))
```

The output of A is found to be 7072.  
7072 represents the greatest possible integer number that can be used in the inequality **A**^2 + **B**^3 + **C**^4 < 50,000,000 where A, B, and C are prime numbers. 
I thought to generate all possible combinations of three primes from a list of all primes from 2 to the upper limit of 7072. Each list would then be used to compute the sum of **A**^2 + **B**^3 + **C**^4.    

```python
list(combinations(list_of_primes,3))
```

This generation crashed the kernel on my jupyter notebook. Generating all possible combinations of three primes manually would not be a viable solution.  
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
The calculations showed that **B** = 369 and **C** = 85. 

I then used these limits in my for loop as such, where *primes_one* is a list of the first 7072 prime numbers, *primes_two* is a list of the first 369 prime numbers, and *primes_three* is a list of the first 85 prime numbers: 
```python
for p_one in primes_one:
    for p_two in primes_two: 
        for p_three in primes_three:
            psum = p_one**2 + p_two**3 + p_three**4
```

Bringing all of this together is my below function. The function requires the import of *numpy*, *math*, and *itertools*. The function allows for the user to set the upper limit of the sum to calculate. The default sum is 50,000,000.     

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
    return len(out_list)

```
My proposed solution to this problem is **1097343**.  
```python
 print(prime_powers(upper_limit = 50000000))
 #output: 1097343
```

Thus 1097343 unique numbers below fifty million can be expressed as the sum of a prime square, prime cube, and prime fourth power.  

<br />  

### 2. Permuted Multiples    
[This question](https://projecteuler.net/problem=52) asks to *"find the smallest positive integer, x, such that 2x, 3x, 4x, 5x, and 6x, contain the same digits."*   
I decided to use a while loop and try integers one by one until the algorithm found the smallest positive integer, x, such that 2x, 3x, 4x, 5x, and 6x, contain the same digits.  

The basic idea of the while loop was to use the current integer value, multiply it by itself and then multiply it by 2.  
At this step, the algorithm would check if the length of the list and the unique values within the list were the same. Below is a code snippet that does this:  
```python
a = my_integer * 1
b = my_integer *2 
 if len(a)==len(b) and set(a)==set(b):
     continue
```

In the above example, *a* is the integer multiplied by 1.  
*b* is the integer multipied by 2.  
I wrote the while loop such that it would re-assign the value *a* to be the next multiplication (integer multiplied by 3).  
If the length or unique values do not match, then the while loop discontinues the current integer and iterates onto the next integer and the loop starts over at a multiplication of 1.  
```python
a = my_integer * 1
b = my_integer *2 
 if len(a)==len(b) and set(a)==set(b):
     continue
else:
    my_integer+=1
```

Below is the function in its entirety:  
```python
def permuted_multiples(my_input=2): 
    """
    This function solves the Permuted Multiples problem. 
    Input: default of integer value of 2
    Behavior: use the user input value as the base integer value. Multiply this base input by 1,2,3,4,5, and 6 sequentially. 
    If at any step, the number of integers or the unique values in the product is NOT the same, then stop the multiplications, and start over with user integer+1. 
    Once the product of the input and 1,2,3,4,5, and 6 have the same numbers and unique values, capture the integer. 
    Output: The smallest integer,x in which 2x, 3x, 4x, 5x, and 6x, contain the same digits. 
    
    """
    #assign the user input value to a new variable
    itr=my_input
    #create an indicator variable for the While loop
    ind="no"
    #while the indicator variable is "no":
    while ind=="no":
        #iterate through 6 - 1
        for i in reversed(range(1,7)):
            #multiply the input by the iteration number
            vals=itr*i
            #strip the digits from the product and place into a list
            strvals = list(map(int, str(vals)))
            #reassign the list to list, "a"
            a = strvals
            #If we are at the start of the for loop:
            if i==max(range(1,7)): 
                #assign the list to, "b" for comparison purposes
                b=strvals
            else:
                #check if the length of the lists and the unique value within the lists are the same
                if len(a)==len(b) and set(a)==set(b):
                    #if the values are the same AND we are at the end of the loop, we've solved the problem
                    if i==min(range(1,7)):
                        #capture the number
                        return(itr)
                        #break the while loop
                        ind ="yes"
                        break
                    else: 
                        #if we are not at the end of the loop, keep going
                        continue
                else:
                    #length of the lists or the sets of the lists are NOT the same 
                    #thus, we move onto the next possible integer
                    itr+=1
                    #reset the i value to the max of range(1,7)
                    i=6
                    #break the inner for loop
                    break
```

Here is the function in use and my solution: 

```python
permuted_multiples(my_input=2)
#output: 142857
```

Thus, the smallest positive integer, x, such that 2x, 3x, 4x, 5x, and 6x, contain the same digits is **142857**.   



<br />  

### 3. Sum Square Difference    
[This question](https://projecteuler.net/problem=6) asks to find the difference between the sum of the squares of the first one hundred natural numbers and the square of the sum. Compared to the first two questions that were previously solved, this problem was relatively straight forward.  
I immediately thought to create two lists: 
1. List One: Sum of the squares of the natural numbers 
2. List Two: Square of the sum of the natural numbers  

Both rely on user input as such:  
```python
#create a list of natural numbers with user input    
my_list = list(range(1,user_input+1))
#sum the squares of each of the numbers in the list
sum_a = sum(map(lambda x:x*x,my_list))
#square the sums of all of the numbers in the list
sum_b = sum(my_list) **2
```

We then simply find the difference of the sum of list (1) and list (2).  
```python
#find the difference in the two lists
sum_diff = sum_b - sum_a
```

All together, the function looks like the following:  
```python
def ssd(my_num):
    """
    This function solves the Sum Square Difference Problem. 
    Inputs: Integer 
    Behavior: Sums of the squares of the first X (user input) natural numbers. 
    Then squares the sum of  the first X (user input) natural numbers. 
    Finally take the difference between the two sums.
    Output: Integer solution to the Sum Square Difference problem.    
    """
    #create a list of natural numbers with user input    
    my_list = list(range(1,my_num+1))
    #sum the squares of each of the numbers in the list
    sum_a = sum(map(lambda x:x*x,my_list))
    #square the sums of all of the numbers in the list
    sum_b = sum(my_list) **2
    #find the difference in the two lists
    sum_diff = sum_b - sum_a
    #return the difference
    return sum_diff
```

Here is an example of the function where the user is interested in the difference between the sum of the squares of the first one hundred natural numbers and the square of the sum:

```python
print(ssd(my_num=100))
#output: 25164150
```
My propsed solution to this problem is **25164150**. 