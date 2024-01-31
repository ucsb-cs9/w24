---
layout: lab
num: lab03
ready: ready
desc: "Recursion"
assigned: 2024-01-26 23:59:59.59-7
due: 2024-02-05 23:59:59.59-7
---

# Learning Goals

In this lab, you'll practice:

* Writing recursive functions based on a specification.
  - Define a base case (or cases)
  - Design the _next smallest input_ that would trigger a recursive case
  - Determine the actions that need to happen in the recursive case as well as how to reduce the input to get it to trigger the base case
* Practice writing pytests to ensure your recursive functions are correct.

Before you get started, make sure to read up on recursion, specifically **Chapter 4.2**.


# Instructions

For this lab, you will need to create two files:
* `lab03.py` - file containing your solution to all recursive functions you will implement in this lab.
* `testFile.py` - file containing pytest functions testing all recursive functions you will implement in this lab. 
  - **Note:** Gradescope's autograder requires you to submit your `testFile.py` in order for it to run your code (hopefully you're practicing TDD and use your tests to check correctness!). Gradescope will also require you to provide all function definitions (even if incorrect) in `lab03.py` in order to run the tests.

There will be no starter code for this assignment, but rather function descriptions are given in the specification below.

It's recommended that you organize your lab work in its own directory. This way all files for a lab are located in a single folder. Also, this will be easy to import various files into your code using the `import / from` technique shown in lecture.

## Recursive function definitions and specifications

You will write five recursive functions for this lab. Each one is specified below. One example test will be given, but you should write 3 - 5 explicit tests for each function (think of edge cases and various interesting cases when writing your tests!).

**Note: You must write each function _recursively_ in order to receive any credit; you may receive a 0 for your implementation, even if Gradescope's tests pass. For this lab, you may not (and need not) define additional helper functions.**

* `int_division(num, div)` - The parameters `num` and `div` are positive integers (you may assume `num` is >= 0 and `div` is > 0). This recursive function recursively returns the quotient (i.e. `num // div`) without explicitly using the `//` or `/` operators, or a pre-defined function.

```
# Example test
assert int_division(27, 4) == 6
```

* `get_even_ints(int_list)` - The parameter `int_list` is a list containing positive integer values. This recursive function will return a list containing only even values in `int_list` in the order that they appear in `int_list`. If there are no even values in `int_list`, then this function should return an empty list.

```
# Example test
assert get_even_ints([1, 2, 3, 4, 5]) == [2, 4]
```

* `count_vowels(text)` - This recursive function will take a string value (`text`) and return the number of vowels (which for this exercise are 'A','E','I','O','U','a','e','i','o','u') that are found in it.

```
# Example test
assert count_vowels("This Is A String") == 4
```

* `reverse_str(text)` - The parameter `text` is a string. This recursive function will return a string in the reverse order of `text`. Note that the reverse of an empty string is an empty string.

```
# Example test
assert reverse_str("CMPSC9") == "9CSPMC"
```

* `remove_seq(text, seq)` - The parameters `text` and `seq` are strings that contain at least one character. This recursive function will return a string where all occurrences of `seq` are removed in the order it appears in the string `text` (see example test below for an interesting case). **Your solution SHOULD NOT use the string's `replace()` or `find()` methods.**

```
Example test
assert remove_seq("Lolololol", "lol") == "Loo"
# The first "lol" is removed, which reduces the string 
# to: "Loolol". Then the 2nd "lol" is removed, which 
# reduces the string to: "Loo"
```

**Hints**:
Think of what your base case could be, test it - what's the simplest case where you don't need to do any work? 
- use sample short strings to figure out what the algorithm should be
- try strings that contain and do not contain the sequence that needs to be replaced and see what actions need to take place in each case.

Which one of these cases would trigger your base case? 
- `text = 'abc', seq = 'b'`
- `text = 'acb', seq = 'ab'`
- `text = 'ab', seq = 'abc'`


## `testFile.py` pytest

This file will contain unit tests using pytest to test if your functionality is correct. Write your tests first in order to check the correctness of your recursive function. Again, Gradescope requires `testFile.py` to be submitted before running any autograded tests. You should write 3 - 5 tests per function.

**The asserts will be marked 0 if some of the asserts in testFile.py fail.**

Here's the structue for the test file that you can use to get started:

```py
# testFile.py

def test_int_division():
    # from lab03 import int_division
    # uncomment the above import post completion of function in lab03.py
    # write your assert statements here
    pass # remove this after writing asserts

def test_get_even_ints():
    # from lab03 import get_even_ints
    # uncomment the above import post completion of function in lab03.py
    # write your assert statements here
    pass # remove this after writing asserts

def test_count_vowels():
    # from lab03 import count_vowels
    # uncomment the above import post completion of function in lab03.py
    # write your assert statements here
    pass # remove this after writing asserts

def test_reverse_string():
    # from lab03 import reverse_str
    # uncomment the above import post completion of function in lab03.py
    # write your assert statements here
    pass # remove this after writing asserts

def test_remove_seq():
    # from lab03 import remove_seq
    # uncomment the above import post completion of function in lab03.py
    # write your assert statements here
    pass # remove this after writing asserts
```

## Submission

Once you're done with writing your recursive function definitions and tests, submit your `lab03.py` and `testFile.py` files to the `Lab03` assignment on Gradescope. There will be various unit tests Gradescope will run to ensure your code is working correctly based on the specifications given in this lab.

If the tests don't pass, you may get some error message that may or may not be obvious at this point. Don't worry - if the tests didn't pass, take a minute to think about what may have caused the error. If your tests didn't pass and you're still not sure why you're getting the error, feel free to ask your TAs or Learning Assistants.


# Troubleshooting

`RecursionError: maximum recursion depth exceeded while calling a Python object`

If you are getting the error above, check:
- is your base case is correct? do you have all necesary base cases?
- is your recursive function called with an input _that gets closer_ to the base case?
- are all braches of your function returning the correct result? (walk through different example inputs using Python Tutor, if necessary)

---


