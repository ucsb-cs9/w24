---
num: "Lecture 5"
desc: "Pytest, Operator Overloading, Inheritance"
ready: false
lecture_date: 2024-01-23 14:00:00.00-7:00
---

[Slides folder]({{ site.slides_url }})

[Lecture recording](https://youtu.be/M14BcV1pOWY)

# Plan for today

* What is operator overloading? What are the benefits of it?
* What is the difference between the shallow and deep equality?

* Why write tests for our code? 
* What are the benefits of pytest?
* How to write and run tests in pytest?

* What is inheritance in Python? What are the benefits of it?

# Operator Overloading

* We can define our own behavior for common operators in our classes
	* What does it mean if two student objects are equal (we defined it to mean perm numbers are equal)?
	* Or what does it mean to add (+) two students together?
	* Python allows us to define the functionality for operators!


**What are operators?**

- Arithmetic operators
- Assignment operators
- Comparison operators
- Logical operators
- Identity operators
- Membership operators
- Bitwise operators

We can re-define them to work appropriately with our custom classes, similar to how we overloaded the `__eq__` method, which controls the `==` operator.

Looked at the difference between `__str__` and `__repr__` methods.

## Defining `__str__` 

* When printing our defined objects, we may get something unusual. For example:

```
from Student import Student

s1 = Student("Gaucho", 1234567)
s2 = Student("Jane", 5555555)
print(s1) <Student.Student object at 0x7fd5380d8e80>
```

* All objects can be printed, but Python wouldn’t know what to print for user-defined objects like Student
* So it just prints the memory address (the `0x...`) of where the object exists in memory
* In order to provide our own meaning of what Python should display when printing an object like Student, we will need to define a special `__str__` method in our Student class:

```
def __str__(self):
	''' returns a string representation of a student '''
	return "Student name: {}, perm: {}".format(self.name, self.perm) 
```

* Python will now use the return value of the `__str__` method when determining what to display in the print statement
	* Now the `print(s1)` statement outputs `Student name: Gaucho, perm: 1234567`



## Overriding the '+' operator

* What would it mean to add (+) two students together?
* Maybe we can return a list collection ... could be useful ... maybe?

```
def __add__(self, rhs):
	''' Takes two students and returns a list containing these two students '''
    return [self, rhs]
```
```
x = s1 + s2 # returns a list of s1 + s2
print(type(x)) # list type

for i in x:
	print(i)

# Output of for loop
# Student name: Gaucho, perm: 1234567
# Student name: Jane, perm: 5555555
```

## Overloading the '<=' and '>=' operator

* Example:

```
# <=
def __le__(self, rhs):
	''' Takes two students and returns True if the
		student is less than or equal to the other
		student based on the lexicographical order
		of the name '''
	return self.name.upper() <= rhs.name.upper()

# >=
def __ge__(self, rhs):
	''' Takes two students and returns True if the
		student is greater than or equal to the other
		student based on the lexicographical order
		of the name '''
	return self.name.upper() >= rhs.name.upper()

# >
# def __gt__

# <
# def __lt__
```
```
print(s1 <= s2) # True
print(s1 >= s2) # False
print(s1 == s2) # False
print(s1 < s2) # ERROR, we didn’t define the __lt__ method
```

* Good article on this as well as a list of common operators we can overload: <https://thepythonguru.com/python-operator-overloading/>



## Checking what's been defined for an object

Check what's been defined for an object using a built-in function `dir()` (check `help(dir)`).

A student asked: _how do we know if a default method or an operator has been overloaded?_

using `__dict__` - if the default method is listed in that dictionary, then it has been overloaded; otherwise, the default method is used.

A question for the class: 
_Can we create an instance of an object without a constructor?_


---

# Testing

* Write your own test code
    - Test-driven development (TDD) - write the tests first
    - Account for plausible input values, edge cases, various scenarios
* Why use the pytest module?
    - Helps organize assert statements
    - Provides a report on which tests failed



# Pytest
* Pytest is a framework that allows developers to write tests to check the correctness of their code
* Gradescope actually uses pytest to check the "correct" answer with students’ submissions
* We can write functions that start with `test_`, and the body of the function can contain assert statements (as seen in lab00)
	* Pytest will run each of these functions are report which tests passed and which tests failed
* Try and install Pytest on your computer (will use this in our examples)
Installation Guide: <https://docs.pytest.org/en/stable/getting-started.html>
* Windows Installation Guide (created by previous Learning Assistants): [Python and Pytest Installation Guide for Windows](https://drive.google.com/file/d/1nPCwIA8cBAkiJ-kOKZFjkOskD94jmWYn/view)

* Example

Write a function `getMax(a,b,c,d)` that takes 4 int values and returns the largest

* Let's write our our tests first (TDD!)

```
# testFile.py

# imports the getMax function from getMax_func.py
from lecture import getMax 

def test_getMax1_position():
    assert getMax(1,2,3,4) == 4
    assert getMax(1,2,4,3) == 4
    assert getMax(1,4,2,3) == 4

def test_getMax2_same():
    assert getMax(5,5,5,5) == 5
    assert getMax(-5,-5,-5,-5) == 5
    # etc.

def test_getMax3():
    assert getMax(-5,-10,-12,-100) == -5
    assert getMax(-100, 1, 100, 0) == 100
    # etc.
```

* Now let’s write the function (buggy code!):

```
# getMax_func.py

def getMax(a, b, c, d):
	currMax = 0
	if a >= b and a >= c and a >= d:
		currMax = a
	if b >= a and b >= c and b >= d:
		currMax = b
	if c >= a and c >= b and c >= d:
		currMax = c
	else:
		currMax = d
```

Command to run pytest on testFile.py:
* Navigate to the folder containing `getMax_func.py` and `testFile.py`
* (On mac terminal): `python3 -m pytest testFile.py`
    * Note: replace `python3` with `python` on Windows.

# Inheritance

* Avoids code duplication (similar to the idea of functions)
* Allows us to extend the existing code 

* Start with the **base class** (also referred to as a **parent class** or a **super class**)
* The classes that inherit from the base class, are referred to as the **child** / **derived** / **sub-class**.

Looking at the Inheritance Hierarchy of the classes that you'll write for lab02,
- the parent class is a `Drink`
- the child classes are `Tea` and `Juice`, since tea and juice **IS-A** drink

What about `DrinkOrder`? 
- it is not a drink
- the order **contains** various drinks, hence it is a **container class**

