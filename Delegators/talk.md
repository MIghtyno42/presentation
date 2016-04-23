#Title Slide

CSCI #3155 Presentation - Python

* Michael Min

* Andrew Orr

* Devon Connor

# Introduction

* Our proposal was PEP 380 --Syntax for Delegating to a Subgenerator


* PEP 380 simply changes the use of the yield keyword in Python

# The purpose of Generators in Python

* ````python 
Return
```` 
	* The keyword that returns the entire output at once. 
* ````python
Yield
````
	* The keyword typically used by generators, which yields only one iteration at a time

# Code Example of yield

````python
def get_primes(number):
	while True:
		if is_prime(number):
			number = yield number
		number += 1

get_primes(2)
````

* This function is turned into a generator that will constantly return numbers in its endless loop, one at a time


# Finite Example of Generators

````python
def get_primes(number):
	for x in range(1,3):
		yield number + 2


print sum(get_primes(3))
````
* In a more explicit example, this function returns 10

# Proposal

* The drawback to this incarnation of yield is that when yield is used in a function, it can 
only yield back to one caller
* The entire premise of PEP 380 is the following grammar: 

````python
			yield from expr
````


# Proposal (cont.)

* When used more formally, the syntax is: 
````python
RESULT = yield from EXPR 
````

#Process
	
* The yield runs until EXPR is depleted of iterations, as usual
* The main change with PEP 380 is that it allows for yield to be used out of the function
````python
RESULT = yield from EXPR 
````

# Comparisons


````python
_i = iter(EXPR) 
try:
    _y = next(_i)
except StopIteration as _e:
    _r = _e.value
else:
    while 1:
        try:
            _s = yield _y
        except GeneratorExit as _e:
            try:
                _m = _i.close
            except AttributeError:
                pass
            else:
                _m()
            raise _e
        except BaseException as _e:
            _x = sys.exc_info()
            try:
                _m = _i.throw
            except AttributeError:
                raise _e
            else:
                try:
                    _y = _m(*_x)
                except StopIteration as _e:
                    _r = _e.value
                    break
        else:
            try:
                if _s is None:
                    _y = next(_i)
                else:
                    _y = _i.send(_s)
            except StopIteration as _e:
                _r = _e.value
                break
RESULT = _r
````


#Further Description of Proposal

* Other than the change with yield being added, no new keywords or symbols are actually added 

#Further Description of Proposal (cont.)

At one point, 

````python
	yield *
````


was used instead of:

````python
	yield from
````

but it was ruled that it looked too similar to yield in:

````python
def count(number):
	for x in range(0,3):
		number = yield number
		number += 1
````

# Syntax

With the new syntax, we can now move around the code with yield in it to a greater degree, making it easier for us to reuse it

# Refactoring

Main purpose to move easily between functions and share data


#Optimization

Delegating to subgenerators also helps to optimize in recursive calls

# Compartmentalization

New syntax allows code to be split up, similar to threads

# Similarities to Class

Small-Step Semantics

# Counter-points

The proposal, PEP 380, is accepted but disagreed with due to its unusual way of using yield to get outputs


# Rejected Automation


 Use of automated next() calls not within scope of project

 
# Rejected alternate return from sub-generator

 Goes against idea of suspendable functions being like other functions


# Resources

* http://www.cosc.canterbury.ac.nz/greg.ewing/python/yield-from/
* https://www.python.org/dev/peps/pep-0380/