---
layout: lab
num: lab07
ready: true
desc: "Soup or Salad?"
assigned: 2024-02-23 23:59:59.59-7
due: 2024-03-4 23:59:59.59-7
---

# Learning Goals

In this lab, we will utilize many concepts covered in the course so far including:

* Inheritance and Polymorphism
* Implementing and applying the Heap data structure as a priority queue
* Testing your functionality with pytest

**Note: It is important that you try and start this lab early so you can utilize our office hours to seek assistance / ask clarifying questions during the weekdays before the deadline if needed!**

# Introduction

<img style="float: right; margin: 5px;" alt="Meme Caption for the astonished koala: My face when I am eating a salad and everyone else is eating a pizza" src="https://i.imgflip.com/39o454.jpg" />
The goal for this lab is to write a program that will manage incoming salad orders. All salad orders have an associated time representing when the customer expects to have their salad ready (it's possible for a customer to call ahead and schedule a later pickup time). All salad orders will be managed by a **MinHeap** where the next order to prepare is the one that is the earliest compared to the other orders.

In order to manage salad orders for this lab, you will design various Salad classes (`Salad`, `CustomSalad`, and `SpecialtySalad` that utilizes inheritance / polymorphism), a `SaladOrder` class representing a collection of Salads a customer wants to place in a single order, and an `OrderQueue` class that organizes the Salad orders in a MinHeap data structure.

You will also write pytests in `testFile.py` illustrating your behavior works correctly. This lab writeup will provide some test cases for clarity, but the Gradescope autograder will run different tests shown here.

# Instructions

You will need to create six files:
* `Salad.py` - Defines a Salad class representing commonality for all Salads. For simplicity, this class will assume all Salads have a `size` and `price`.
* `CustomSalad.py` - Defines a child class of Salad. This class should inherit all fields / methods from the Salad class, but also introduces the concepts of toppings a customer can order (represented as a list of strings).
* `SpecialtySalad.py` - Defines a child class of Salad. This class should inherit all fields / methods from the Salad class. Specialty salads are defined by a name attribute and all have a set price depending on the salad size.
* `SaladOrder.py` - Defines a class that is a collection of salad objects the customer wants to order. The total price for the order can be derived from each individual salad price. This class will also have an expected time of when the customer would like their salads ready for pickup. More details on this later in the writeup.
* `OrderQueue.py` - Defines a MinHeap to organize and process Salad Orders according to their expected time of pickup. You can adapt the Heap implementation shown in the textbook supporting the specifications in this lab.
* `testFile.py` - This file will contain your pytest functions that tests the overall correctness of your class definitions.

There will be no starter code for this assignment, but rather class descriptions and required methods are defined in the specification below.

You should organize your lab work in its own directory. This way all files for a lab are located in a single folder. Also, this will be easy to import various files into your code using the `import / from` technique shown in lecture. 

# Salad.py

The `Salad.py` file will contain the definition of a Salad base class. We will define the Salad attributes as follows:

* `price` - float that represents the price of a salad. Since the price will be defined by what type of salad is ordered, we can simply create the price field and initialize it to 0.0
* `size` - str that represents the size of the salad being ordered. For simplicity, we can have three valid salad sizes and label these with `"S"` for small, `"M"` for medium, and `"L"` for large

You should write a constructor that allows the user to construct a Salad object by passing in values for the size. Your constructor should also create a price attribute and set it to 0.0.

* `__init__(self, size)`

Your Salad class definition should also support the "getter" and "setter" methods for the price and size. Since this will be a base class for other Salad types, anything we write here can be inherited by its child classes.

* `get_price(self)`
* `set_price(self, price)`
* `get_size(self)`
* `set_size(self, size)`

# CustomSalad.py and SpecialtySalad.py

`Salad` objects can be two different types. Both of these types of salads inherit from the Salad class:
1. `CustomSalad` that allows the customers to add additional toppings of their choosing
2. `SpecialtySalad` that has already been pre-configured and has a fixed price based on its size

## CustomSalad.py
Your `CustomSalad` class definition will be defined in `CustomSalad.py`. The `CustomSalad` class will contain a constructor that takes in the size of the Salad, and should use this size to call our base class' constructor. In addition to the size, it will initialize a toppings list represented as a Python List.

The price of a CustomSalad is defined by two things:
1. the size of the salad
2. the number of toppings the salad will have (assuming no toppings, it is a simple salad). CustomSalads will have the following fixed prices based on its size:

* Small (S): $8.75
* Medium (M): $10.75
* Large (L): $12.75

The size of the salad also dictates the amount each additional topping will cost based on the following definition:

* Small (S): + $1.75 per additional topping
* Medium (M): + $2.50 per additional topping
* Large (L): + $3.00 per additional topping

Since we now know that the price of a salad should be based on the size, the `CustomSalad` constructor should determine the base price of the salad and set it appropriately (remember, no need to write it in this class if we already have the method in `Salad.py`).

* `__init__(self, size)`

There are two more methods this class should support:

* `add_topping(self, topping)` - this method will add to the toppings list and update its price appropriately. The topping parameter is represented as a `str` type
* `get_salad_details(self)` - this method will construct and return a string containing the details of the `CustomSalad` object including the size, toppings, and price of the `CustomSalad`. An example (with escape characters shown for formatting) is given below. When constructing your string, please follow the **EXACT** format since this is what Gradescope will expect.

`CustomSalad` without toppings example:

```python
cs1 = CustomSalad("S")

assert cs1.get_salad_details() == \
"""\
CUSTOM SALAD
Size: S
Toppings:
Price: $8.75
"""
```

`CustomSalad` with a list of toppings example (note that each topping will be indented with a tab):

```python
cs1 = CustomSalad("L")
cs1.add_topping("chicken")
cs1.add_topping("cucumbers")

assert cs1.get_salad_details() == \
"""\
CUSTOM SALAD
Size: L
Toppings:
\t+ chicken
\t+ cucumbers
Price: $18.75
"""
```

## SpecialtySalad.py

<img style="float: right; margin: 5px;" alt="Meme Caption for the angry Gordon Ramsey yelling: How the hell do you manage to burn a salad?" src="https://i.imgflip.com/2qbmkm.jpg" />


A `SpecialtySalad` class definition will exist in `SpecialtySalad.py`. Similar to a `CustomSalad` object, the class constructor will take in a size as well as the name for the specialty salad. 

* `__init__(self, size, name)`

Also similar to the `CustomSalad` class, `SpecialtySalad` will use the size to set its price appropriately. The price of a `SpecialtySalad` is defined as follows:

* Small (S): $12.50
* Medium (M): $14.50
* Large (L): $16.50

Unlike custom salads, specialty salads do not have a list of toppings associated with it, but do have a unique name that will be displayed when getting details for this salad. This class should also have its own `get_salad_details` method described below:

* `get_salad_details(self)` - this method will construct and return a string containing the details of the `SpecialtySalad` object including the size and name of the `SpecialtySalad`. An example (with escape characters shown for formatting) is given below. When constructing your string, please follow the **EXACT** format since this is what Gradescope will expect.

A sample output test for `get_salad_details()`:

```python
ss1 = SpecialtySalad("S", "Cobb")
assert ss1.get_salad_details() == \
"""\
SPECIALTY SALAD
Size: S
Name: Cobb
Price: $12.50
"""
```

# SaladOrder.py

The `SaladOrder` class will be defined in `SaladOrder.py`. This class will keep track of various salads for a single order. The `SaladOrder` class will have the following attributes:

* `salads` - a Python List containing all the salads that the single order contains. This can be initially set to an empty list.
* `time` - an int representing the expected time the order will be picked up. This field will be used in determining the *priority of orders*. So it is possible for an early order to be deprioritized based on when the salad is expected to be ready.

The constructor for a `SaladOrder` will take in the expected time that order should be ready:

* `__init__(self, time)`

The time format will be stored as an `int` in a 24-hour time format. For example, given a `hour:minute:second` format, the corresponding int values would be:

* 12:00:00 AM --> 0
* 12:08:00 AM --> 800
* 8:00:05 AM --> 80005
* 9:25:32 AM --> 92532
* 1:14:23 PM --> 131423
* 10:56:59 PM --> 225659

In addition to the constructor, getters for the time attribute, the ability to add Salad objects to the order, as well as a method to construct a string representing the order details will need to be implemented:

* `get_time(self)`
* `add_salad(self, salad)` - will add the Salad object to the end of the Python List
* `info(self)` - constructs and returns a string containing the time of the order, all information for each salad in the order, as well as the total order price. Since we're storing various Salad objects in this class, we can utilize polymorphism and simply call the `get_salad_details()` method on the Salad objects when constructing the string for our entire order, as well as `get_price()` to compute the `SaladOrder` total price

An example of the `info()` string format is given below:

```python
cs1 = CustomSalad("S")
cs1.add_topping("chicken")
cs1.add_topping("cucumber")
ss1 = SpecialtySalad("S", "Cobb")
order = SaladOrder(123000) #12:30:00PM
order.add_salad(cs1)
order.add_salad(ss1)

assert order.info() == \
"""\
***
Order Time: 123000
CUSTOM SALAD
Size: S
Toppings:
\t+ chicken
\t+ cucumber
Price: $12.25

----
SPECIALTY SALAD
Size: S
Name: Cobb
Price: $12.50

----
TOTAL ORDER PRICE: $24.75
***
"""
```

# OrderQueue.py

<img style="float: right; margin: 5px; width: 300px" alt="Meme Caption for the curious chicken: What went bad first - the chicken or the egg salad?" src="https://i.imgflip.com/1megy8.jpg" />

The `OrderQueue` class will be defined in `OrderQueue.py`. This priority queue is implemented as a MinHeap data structure. The `OrderQueue` will manage `SaladOrder` objects based on their `time` attribute.

* `__init__(self)` - the constructor for the `OrderQueue` will simply initialize the Python List representing the MinHeap.

In addition to the construction of the MinHeap in this class, two methods are required to be implemented:

* `add_order(self, saladOrder)` - a `saladOrder` object will be stored in the MinHeap *prioritized by its time attribute* (lower value means higher priority)
* `process_next_order(self)` - this removes the root node from the MinHeap (and restructures the MinHeap), and returns a string containing the root value's salad order description. If the `OrderQueue` is empty, then it should return an empty string.

The automated tests will create various salad orders with different time attributes. It will then call `process_next_order()` one at a time and check the removed SaladOrder is in the right priority by checking their expected `info()` string. You should write similar tests to confirm the MinHeap state is in the correct order.

# testFile.py

<img style="float: left; margin: 5px; width: 300px" alt="Meme Caption for the green unimpressed cat: I do not eat salad" src="https://i.imgflip.com/2kifto.jpg" />

This file should test all of your classes using pytest. Think of various scenarios and edge cases when testing your code according to the given descriptions. You should test every class' method functionality (except for getters / setters). Even though Gradescope will not use this file when running automated tests (there are separate tests defined for this), it is important to provide this file with various test cases (testing is important!!).

Of course, feel free to reach out / post questions on Piazza as they come up!

# Submission

Once you're done with writing your class definitions and tests, submit the following files to Gradescope's Lab07 assignment:

* `Salad.py` 
* `CustomSalad.py`
* `SpecialtySalad.py`
* `SaladOrder.py`
* `OrderQueue.py`
* `testFile.py`

There will be various unit tests Gradescope will run to ensure your code is working correctly based on the specifications given in this lab.

If the tests don't pass, you may get some error message that may or may not be obvious at this point. Don't worry - if the tests didn't pass, take a minute to think about what may have caused the error. If your tests didn't pass and you're still not sure why you're getting the error, feel free to ask your TAs or Learning Assistants.

# Troubleshooting

TBA

