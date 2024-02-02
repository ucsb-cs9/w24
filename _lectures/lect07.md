---
num: "Lecture 7"
desc: "Algorithm Analysis, Recursion"
ready: true
lecture_date: 2024-01-30 14:00:00.00-7:00
---

[Slides folder]({{ site.slides_url }})

[Recorded video](https://www.loom.com/share/4290fd7115234fd8a398ffd4154ffb19?sid=04b9a33a-abe4-4388-8e85-ae1614443141)


<!--Recorded Lecture: [#](#)-->

# Plan for today

* Discuss Algorithm Analysis
* Work through algorithm examples
* Review Recursion

# Course-related announcements
* Requests for extensions
* Course weekly pattern 
* Today's office hour slot

# Algorithm Analysis

The recording shows the details of what we discussed.

Additionally, these notes from Prof. Wang might be helpful as well:

# Algorithm Analysis

* There are many ways we can try to estimate an algorithm
* For example, we can benchmark the algorithm by calculating the time it takes for something to run
* We can do this in Python using some code:

```
import time

def f1(n):
	l = []
	for i in range(n):
		l.insert(0,i)
	return

def f2(n):
	l = []
	for i in range(n):
		l.append(i)
	return

print("starting f1")
start = time.time()
f1(200000)
end = time.time()
print("time elapsed: ", end - start, "seconds")

print("starting f2")
start = time.time()
f2(200000)
end = time.time()
print("time elapsed: ", end - start, "seconds")
```

* We’ll get to why the time difference between adding to the list in the beginning and adding to the end differ in time soon enough :)
* This way of measuring algorithms has some problems like:
	* Underlying hardware (fast / slow CPU, amount of memory, disk size / speed, network speed, etc.)
	* How busy the computer is, how the OS is managing other programs
	* How big n is (if n is small, time is almost the same)

## Asymptotic Behavior

* We want to analyze approximately how fast an algorithm runs when the size of the input approaches infinity
* So instead of calculating the raw time of how fast the algorithm runs on our computers, we can approximate the number of instructions the algorithm will take with respect to the size of the input
	* Steps can include things like assigning values to variables, evaluating boolean expressions, arithmetic operations, etc.
* Let’s try this with a simple algorithm:

```
for i in range(10):
	print(i)
```

* Counting expressions in this case:
	* Assignment: i = 0, i = 1, i = 2, etc. (10 steps)
	* print() (10 steps)
	* Algorithm takes 20 steps

* The algorithm runs in **constant time**, since there isn’t a variable input and will always take the same number of instructions to run.
* Let’s change our problem taking in a variable input size:

```
def f(n):
	for i in range(n):
		print(i)
```

* Now the number of instructions in this algorithm is dependent on the value of n
* So let’s try to express the number of instructions as a polynomial with respect to n
	* We can denote the number of instructions with respect to n as T(n)
* T(n) = n assignment statements + n print statements
* T(n) = 2n

## Order of magnitude function (Big-O)
* Since we have no idea how large n will be, we want to assume the **worst-case** scenario when analyzing our algorithms
	* And in this case, the worst-case is when n approaches infinite
* Since n approaches infinite, we can ignore lower order terms and coefficients
	* Does the 3 really matter when we have 1,000,000,000,000,000,000 + 3 ?
	* See: <http://science.slc.edu/~jmarshall/courses/2002/spring/cs50/BigO/index.html> for an example of how lower-order terms converge when n approaches infinite
* So we can express the above algorithm above by dropping all lower order terms, constants, and coefficients to **O(n)**
* **Note:** constant time algorithms are denoted as **O(1)**
* Another (slightly) more complex example:

```
def f(n)
	x = 0
	for i in range(n):
		for j in range(n):
			x = i + j
```

* Initialize x = 0 is 1 instruction
* i assignment statements (n instructions)
* j assignment statements (n<sup>2</sup> instructions)
* i + j computations (n<sup>2</sup> instructions)
* x reassignment statements (n<sup>2</sup> instructions)
	* T(n) = 1 + 3n<sup>2</sup> + n
* Drop all lower order terms, coefficients, and constants and we get **O(n<sup>2</sup>)**
* Another example:

```
def f(n):
    for i in range(n):
        return i
```

* Note that in this example, i gets assigned to 0.
* But then i is immediately returned before re-assigning i to 1.
* So this algorithm is **O(1)**.

---


# Recursion

* Recursion is when a function contains a call to itself
* Recursive solutions are useful when the result is dependent on the result of sub-parts of the problem

## The three laws of recursion

1. A recursive algorithm must have a base case
2. A recursive algorithm must change its state and move toward the base case
3. A recursive algorithm must call itself, recursively

# Code editor: Thonny

* https://thonny.org/
* Helps with visualizing recursion
* Allows setting debug points and walking through the code

