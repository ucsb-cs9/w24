---
num: "Lecture 3"
desc: "Python Objects and Classes"
ready: true
lecture_date: 2024-01-16 14:00:00.00-7:00
---

[Slides folder]({{ site.slides_url }})

# Plan for today
* What are Python objects and classes? Why do we need them?
* How do we create a new class?
* How to define a method of a class?
* How do we instantiate an object of our class?
* How to create a container class?




# Python Objects and Classes

* Objects are a way for programmers to define their own Python types and create abstractions of things with the programming language.
* Each object may have a certain state that gets modified throughout program execution.
* Object Oriented (OO) programming is the way programs use and manipulate objects to solve problems and model real-world properties
* OO Programming is not REQUIRED
	* It’s more of a way to organize, read, maintain, test, and debug software in a manageable way
* We’ve been using objects already, for example:

```
>>> help(int)
Help on class int in module builtins:

class int(object)
 |  int([x]) -> integer
 |  int(x, base=10) -> integer
 |
 |  Convert a number or string to an integer, or return 0 if no arguments
 |  are given.  If x is a number, return x.__int__().  For floating point
 |  numbers, this truncates towards zero.
...
```

Similarly, for strings, we would be able to store characters, ask about the length of the string, and use built-in **methods**, such as `.upper()` or `.islower()`.


* **Methods** are like functions but are associated with an object
* In this case, Python already defined its own class called `str` that we can use, but sometimes we want to create our own specific objects for the applications we’re trying to build!


## Student Class Example

```
class Student:
```

* When defining methods, the `self` parameter represents the current object we’re calling the method on
* In the example above, `s1` is the variable storing the created Student object.
* When we define the methods, the first parameter is the *instance* of the object stored in `s1`
* We can then use and manipulate the state of the object with these methods using the `self` parameter

## Default Constructor

* We can provide either default values or set values of the object when constructing it through the parameter list.
* In the example above, we can set an empty object without any initial attributes, which may cause an error when trying to use them.
* The constructor below will set `self.name` and `self.perm` to the value `None`, which doesn't require the set methods to set these fields in the object.

```
def __init__(self):
	self.name = None
	self.perm = None

s1 = Student()
s1.info()
```

* We can even go a step further and define the attributes by putting them in the parameters when we create the object

```
def __init__(self, name, perm):
	self.name = name
	self.perm = perm

s1 = Student("Ricardo", 1234567)
s1.printAttributes()
```

## Initializing default values in the constructor

```
def __init__(self, name=None, perm=None):
	self.name = name
	self.perm = perm
```

* In this case, we can pass in parameters or not. If not, then `None` will automatically be assigned to the `self.name` and `self.perm`
* Or we can pick and choose what to set. For example:

```
s1 = Student(perm = 1234567)
s1.printAttributes()

# Student name: None, perm: 1234567
```

* We used the default value of name (None) and then explicitly set the perm (1234567)

## Example of using objects in code

```
s1 = Student("Jane", 1234567)
s2 = Student("Joe", 7654321)
s3 = Student("Jill", 5555555)

studentList = [s1, s2, s3]

for student in studentList:
	student.show()

```

# Container Classes

* Let's continue to expand on our `Student` class
* Classes are also useful to organize / maintain state of a program
* Student objects are useful to organize the attributes of a single student
* But let’s imagine we are trying to write a database of various students
	* The database may be useful to search for students with certain attributes...
	* We will need to add / remove things from our database
	* We can define a class to represent a collection of courses containing students
	* Let’s set up a collection of courses such that a dictionary key is defined by the course number and the value is a list of actual student objects

```
# Courses.py

# from [filename (without .py)] import [component]
from Student import Student 

class Courses:
	''' Class representing a collection of courses. Courses are
		organized by a dictionary where the key is the
		course number and the corresponding value is
		a list of Students of this course '''

	def __init__(self):
		self.db = {}

	def addStudent(self, courseNum, student):
		# If course doesn't exist... 
		if self.db.get(courseNum) == None:
			self.db[courseNum] = [student]
		elif not student in self.db.get(courseNum):
			self.db[courseNum].append(student)

	def showStudentsbyCourse(self, courseNum):

```
