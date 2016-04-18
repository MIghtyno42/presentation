# Introduction

PEP 380 --Syntax for Delegating to a Subgenerator


# The purpose of Generators in Python

Return returns the entire output at once. Yield, which is typically used by generators, yields only one iteration at a time

# Weakness with Yield and Generators
A drawback to yield is that when yield is used in a function, it can 
only yield back to one caller

# Proposal
''''python
yield from expr
''''



# Proposal (cont.)

''''python
RESULT = yield from EXPR 
''''

# Comparisons

''''python
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
''''

# Syntax

With the new syntax, we can now move around the code with yield in it to a greater degree, making it easier for us to reuse it

#Optimization

Delegating to subgenerators also helps to optimize in recursive calls

# Similarities to Class

Small-Step Semantics

# Counter-points

The proposal, PEP 380, is accepted but disagreed with due to its unusual way of using yield to get outputs


# Eleventh

# 12

# 13

# 14

# 15

# 16

# 17

# 18

# 19

# 20

