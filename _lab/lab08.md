---
layout: lab
num: lab08
ready: draft
desc: "Used Car Lot"
assigned: 2024-03-01 23:59:59.59-7
due: 2024-03-11 23:59:59.59-7
---

In this lab, you'll have the opportunity to practice:

* Defining classes in Python
* Overloading the `==`, `<`, and `>` operators in a Python class
* Implementing and applying the Binary Search Tree (BST) data structure
* Writing functions that ensure Car objects are in sorted order
* Testing your functionality with pytest

**Note: It is important that you start this lab early so you can utilize our office hours to seek assistance / ask clarifying questions during the weekdays before the deadline if needed.**

# Introduction

The goal for this lab is to write a program that will manage cars for a used car dealership. All cars have a make, model, year, and price, which can be used to determine the value of cars in relation to each other. All cars will be managed by a Binary Search Tree (BST) where the nodes are sorted by make, then model in lexicographical order. Within each make/model node, the `Car` objects will be added to a Python List based on insertion order.

In order to manage the cars for this lab, you will define `Car`, `CarInventoryNode` and `CarInventory` classes that organize the Cars in a BST data structure. Cars with the same make/model will be located in the same node, and appended to a list based on insertion order.

You will also write pytests in `testFile.py` illustrating your behavior works correctly. This lab writeup will provide some test cases for clarity, but the Gradescope autograder will run different tests shown here.

# Instructions

You will need to create four files:
* `Car.py` - Defines a Car class. This class will assume all Cars have a `make`(str), `model`(str), `year`(int), and a `price`(int).
* `CarInventoryNode.py` - Defines a BST Node class containing all fields for a BST Node and a Python List collection of Car objects.
* `CarInventory.py` - Defines a CarInventory (BST) class that is an ordered collection of a Dealership's Cars. You can adapt the BST implementation shown in the textbook supporting the specifications in this lab.
* `testFile.py` - This file will contain your pytest functions that tests the overall correctness of your class definitions.

There will be no starter code for this assignment, but rather class descriptions and required methods are defined in the specification below.

You should organize your lab work in its own directory. This way all files for a lab are located in a single folder. Also, this will be easy to import various files into your code using the `import / from` technique shown in lecture. 

# Car.py

The `Car.py` file will contain the definition of a `Car` class. The `Car` class will hold information about the cars (`make`, `model`, `year`, and `price`). We will define the `Car` attributes as follows:

* `make` - string value representing the brand of the car (eg. `Nissan`, `Tesla`, `Toyota`, `Ferrari`). Your program must store this attribute in uppercase characters
* `model` - string value representing the model of the car (eg. `Electra`, `Model3`, `Prius`, `Portofino`). Your program must store this attribute in uppercase characters
* `year` - integer value representing the year of the car (eg. 2010, 2000, 1963)
* `price` - integer value representing the price value of the car (eg. 20000, 30000, 25000)

You will write a constructor that allows the user to construct a `Car` object by passing in values for the `make`, `model`, `year`, and `price`.

* `__init__(self, make, model, year, price)`

In addition to the constructor of the `Car` class, the following methods are required to be implemented:
* `__gt__(self, rhs)` - comparator operator that allows us to check if a `Car` object is greater than another `Car` object. `Car` objects are first organized by the lexicographical/alphabetical order of their `make` attribute. If the `make` attribute is the same, then they'll be determined by the lexicographical/alphabetical order of their `model` attribute. If the `model` attribute is the same, then they are organized by the year (from least-to-greatest). If the year is the same, then the they are organized by their price (from least-to-greatest). For example, if both `Car2` and `Car1` have the same `make` and `model`, then `Car2 > Car1` if `Car2` is newer; if `Car2` and `Car1` have the same `year` as well, then `Car2 > Car1` if `Car2` is more expensive.
* `__lt__(self, rhs)` - comparator operator that allows us to check that a `Car` object is less than another `Car` object via the operation `Car1 < Car2` according to the specifications above.
* `__eq__(self, rhs)` - comparator operator that allows us to check that a `Car` object is equivalent to another `Car` (both cars have the same `make`, `model`, `year`, and `price`) via the operation `Car1 == Car2`.
* `__str__(self)` - overload method that returns the details of a car via the operation `str(Car1)`. The string representation should fit the format: `"Make: [make], Model: [model], Year: [year], Price: $[price]"`. 

For example:

```
c = Car("Honda", "CRV", 2007, 8000)
print(c)
```
**Output:**
```
Make: HONDA, Model: CRV, Year: 2007, Price: $8000
```

# CarInventoryNode.py

The `CarInventoryNode.py` file will contain the definition of a `CarInventoryNode` class, which is the node for a BST.

The `CarInventoryNode` class should have the following attributes:

* `make` - string value that represents the make of the car; just like in the `Car` class, your program must store this attribute in uppercase characters.
* `model` - string value that represents the model of the car; just like in the `Car` class, your program must store this attribute in uppercase characters.
* `cars` - a Python List that contains `Car` objects that should have the same `make` and `model`. `Car` objects will be organized in insertion order (most recently inserted `Car` will exist at the end of the Python List).
* `parent` - a reference to the parent of a `CarInventoryNode`, `None` if it has no parent (it is the root).
* `left` - a reference to the left child of a `CarInventoryNode`, `None` if it has no left child.
* `right` - a reference to the right child of a `CarInventoryNode`, `None` if it has no right child.

The `CarInventoryNode` class should define the following methods:

* `__init__(self, car)` - the constructor for the `CarInventoryNode` will take in a `Car` object, and initialize all attributes in the `CarInventoryNode`. The constructor should also append the parameter `Car` object from the parameter into the list attribute `cars`.

In addition to the construction of the BST in this class, the following methods are required to be implemented:
* `get_make(self)` - returns a string containing the `make` of a `CarInventoryNode`.
* `get_model(self)` - returns a string containing the `model` of a `CarInventoryNode`.
* `get_parent(self)` - returns the `parent` Node of the `CarInventoryNode`. If the parent does not exist, return `None`.
* `set_parent(self, parent)` - sets the `parent` Node of the `CarInventoryNode`.
* `get_left(self)` - returns the left child of the `CarInventoryNode`. If the left child does not exist, return `None`.
* `set_left(self, left)` - sets the left child of the `CarInventoryNode`.
* `get_right(self)` - returns the right child of the `CarInventoryNode`. If the right child does not exist, return `None`.
* `set_right(self, right)` - sets the right child of the `CarInventoryNode`.
* `__str__(self)` - overload the string operator to allow us to get the details of all cars in the `CarInventoryNode` (eg, `str(CarNode1)`). The string representation should contain all the `Car` objects in this `CarInventoryNode` in insertion order. Make use of the `Car` class' `__str__` method, and separate each car with a newline character (`\n`) (including the last `Car` object in the cars Python List).

For example:

```
car1 = Car("Dodge", "dart", 2015, 6000)
car2 = Car("dodge", "DaRt", 2003, 5000)
carNode = CarInventoryNode(car1)
carNode.cars.append(car2)
print(carNode)
```
**Output:** (note the extra newline)
```
Make: DODGE, Model: DART, Year: 2015, Price: $6000
Make: DODGE, Model: DART, Year: 2003, Price: $5000

```

Some tips for the implementation:

`CarInventoryNode`s are ordered by `make` and `model` attributes. You *can* implement comparators (`<`, `==`, `>`) for `CarInventoryNode` to help with navigating through the `CarInventory`, but this is not required.

# CarInventory.py

The `CarInventory.py` file will contain the definition of a `CarInventory` class. This will manage the `CarInventoryNode`s and keep track of all the cars a dealership has. The `CarInventory` is implemented as a BST. The `CarInventory` will create and manage `CarInventoryNode` objects based on a car's `make` and `model` attributes. When storing `Car` objects in the `CarInventory`, `Car` objects with the same `make` and `model` will be appended to a Python List based on insertion order within the `CarInventoryNode` object.

* `__init__(self)` - the constructor for the `CarInventory` will simply initialize the empty BST. You should have an attribute called `root` to represent the root node of the `CarInventory` BST.

In addition to the construction of the BST in this class, the following methods are required to be implemented:

* `add_car(self, car)` - adds a `Car` object to the BST. If a `CarInventoryNode` with the same `make` and `model` exists, then append the car to the end of its car list.
* `does_car_exist(self, car)` - searches for a `Car` object in the `CarInventory` by traversing to the appropriate `CarInventoryNode` (if it exists), and checking the `cars` Python List to see if any `Car` object is equal to the parameter `car`. This method returns `True` if it does, and returns `False` otherwise (i.e. no `Car` object with the same `make`, `model`, `year`, and `price` exists).
* `inorder(self)` - returns a string with the in-order traversal of the BST. Printing the in-order traversal should help check that the cars are in the correct order in the BST
* `preorder(self)` - returns a string with the pre-order traversal of the BST. BSTs with the same structure should always have the same pre-order traversal, so this can be used to verify that everything was inserted correctly
* `postorder(self)` - returns a string with the post-order traversal of the BST.
* `get_best_car(self, make, model)` - returns the `Car` with the newest year - and if multiple, then highest price - given the make and model. If the make and model doesn't exist, then return None.
* `get_worst_car(self, make, model)` - returns the car with the oldest year - and if multiple, then lowest price - given the make and model. If the make and model doesn't exist, then return None.
* `get_total_inventory_price(self)` - returns an integer the total price of all the cars in the dealership. 

Given an example BST:

```python
bst = CarInventory()

car1 = Car("Nissan", "Leaf", 2018, 18000)
car2 = Car("Tesla", "Model3", 2018, 50000)
car3 = Car("Mercedes", "Sprinter", 2022, 40000)
car4 = Car("Mercedes", "Sprinter", 2014, 25000)
car5 = Car("Ford", "Ranger", 2021, 25000)

bst.add_car(car1)
bst.add_car(car2)
bst.add_car(car3)
bst.add_car(car4)
bst.add_car(car5)
```

An example of the traversal functions is given below:
```python
assert bst.get_best_car("Nissan", "Leaf") == car1
assert bst.get_best_car("Mercedes", "Sprinter") == car3
assert bst.get_best_car("Honda", "Accord") == None

assert bst.get_worst_car("Nissan", "Leaf") == car1
assert bst.get_worst_car("Mercedes", "Sprinter") == car4
assert bst.get_best_car("Honda", "Accord") == None

assert bst.get_total_inventory_price() == 158000
```

An example of the `inorder()` string format is given below:

```python
assert bst.inorder() == \
"""\
Make: FORD, Model: RANGER, Year: 2021, Price: $25000
Make: MERCEDES, Model: SPRINTER, Year: 2022, Price: $40000
Make: MERCEDES, Model: SPRINTER, Year: 2014, Price: $25000
Make: NISSAN, Model: LEAF, Year: 2018, Price: $18000
Make: TESLA, Model: MODEL3, Year: 2018, Price: $50000
"""
```

An example of the `preorder()` string format is given below:

```python
assert bst.preorder() == \
"""\
Make: NISSAN, Model: LEAF, Year: 2018, Price: $18000
Make: MERCEDES, Model: SPRINTER, Year: 2022, Price: $40000
Make: MERCEDES, Model: SPRINTER, Year: 2014, Price: $25000
Make: FORD, Model: RANGER, Year: 2021, Price: $25000
Make: TESLA, Model: MODEL3, Year: 2018, Price: $50000
"""
```

An example of the `postorder()` string format is given below:
```python
assert bst.postorder() == \
"""\
Make: FORD, Model: RANGER, Year: 2021, Price: $25000
Make: MERCEDES, Model: SPRINTER, Year: 2022, Price: $40000
Make: MERCEDES, Model: SPRINTER, Year: 2014, Price: $25000
Make: TESLA, Model: MODEL3, Year: 2018, Price: $50000
Make: NISSAN, Model: LEAF, Year: 2018, Price: $18000
"""
```

As part of the debugging process, we are providing you two functions to visually represent the BST to ensure it looks similar to your thought. Make use of these carefully through multiple steps of your methods - For e.g. in `inorder`, you can print out the visual representation of your subtree in each step of the recursion so that you can validate your theoretical assumption in action. If you want to call this method inside your call, you can do this by running `show_tree(self.root)` or `show_tree(node)` or `show_tree(car_inventory_object.root)`. Below is the code snippet for the functions and the corresponding logic to see it in action - 

```python
def show_tree(node):
    """
    * -> Indicates the base node
    L -> Indicates the left child of the base node
    R -> Indicates the right child of the base node
    LR -> Indicates the right child of the left child of the base node
    ..... and so on
    """
    import sys
    from io import StringIO
    old_stdout = sys.stdout
    sys.stdout = StringIO()
    if node is None:
        print("No cars in inventory.")
    else:
        print(f"Showing Tree Representation of Children under Node - Make: {node.get_make()}, Model: {node.get_model()}\n")
        _print_tree(node, 0, "")
        print("\nEnd of the car inventory. \n")
    print("\n" + "="*50 + "\n")
    contents = sys.stdout.getvalue()
    sys.stdout = old_stdout
    print(contents)        

def _print_tree(node, level, position):
    if node is not None:
        _print_tree(node.get_right(), level + 1, position + "R")
        print("   " * level + "|----" + f"(Level {level}) {node.get_make()} : {node.get_model()} ({position if position else '*'})")
        _print_tree(node.get_left(), level + 1, position + "L")

if __name__ == "__main__":
    inventory = CarInventory()
    # Adding some cars
    inventory.add_car(Car("Toyota", "Camry", 2020, 25000))
    inventory.add_car(Car("Honda", "Accord", 2019, 23000))
    inventory.add_car(Car("Toyota", "Corolla", 2018, 20000))
    inventory.add_car(Car("Honda", "Civic", 2021, 22000))
    inventory.add_car(Car("Ford", "Fusion", 2017, 18000))
    inventory.add_car(Car("Chevrolet", "Malibu", 2016, 17000))
    # Displaying the inventory
    show_tree(inventory.root)
```

The output for this should look like this - 

```
Showing Tree Representation of Children under Node - Make: TOYOTA, Model: CAMRY

   |----(Level 1) TOYOTA : COROLLA (R)
|----(Level 0) TOYOTA : CAMRY (*)
      |----(Level 2) HONDA : CIVIC (LR)
   |----(Level 1) HONDA : ACCORD (L)
      |----(Level 2) FORD : FUSION (LL)
         |----(Level 3) CHEVROLET : MALIBU (LLL)

End of the car inventory. 


==================================================

```

Other than the required methods, feel free to implement any helper methods that you think are useful in your implementation (Gradescope will test the required methods). The automated tests will test your implementation of the required methods by creating a `CarInventory` containing various `CarInventoryNode`s containing `Car`s with different `make`, `model`, `year`, and `price` attributes. The `add_car()` method will be run, with `does_car_exist()`, `get_best_car()`, `get_worst_car()`, `inorder()`, `preorder()`, and `postorder()`, etc. being used to verify that the `CarInventory` is fully functional. You should write similar tests to confirm your BST is working properly.

# testFile.py

This file should test all of your classes and required methods using pytest. Think of various scenarios and edge cases when testing your code according to the given descriptions. You should test every class' method functionality (except for getters / setters). Even though Gradescope will not use this file when running automated tests (there are separate tests defined for this), it is important to provide this file with various test cases (testing is important!!).

**A note about Gradescope tests:** Gradescope will use your functions to correctly check the state of your Cars and CarInventory with many scenarios. In order to test if everything is in the correct state, these tests use your CarInventory's `preorder` / `inorder` / `postorder` traversals and `add_car` methods, as well as getting the string representation of your Cars (using your `__str__` overloaded method in `Car`) to run other tests such as CarInventory's `does_car_exist`, `get_best_car`, `get_worst_car`, `get_total_inventory_price`, etc. It is important to ensure your `preorder` / `inorder` / `postorder` traversals, Car's `__str__` method, and `CarInventory`'s `add_car` methods work correctly first or else many of the other tests may not pass.

Of course, feel free to reach out / post questions on Piazza as they come up!

# How to best approach this lab

This lab contains a lot of implementation details, and different parts of the lab intertwine and depend on each other. Here are some suggestions on the order of approaching the lab:

1. Implement the `Car` class, and especially double check that your comparators are working
2. Implement the `CarInventoryNode` class. You should go through and test your `__str__` overloading before moving on
3. Start with `add_car`, and then the BST traversal methods. You should test to see if your `add_car` is working by inserting several Cars into the BST, and using the traversals to verify your results; is a new `CarInventoryNode` being created if the Car didn't exist in the BST, and if the node did already exist, is the Car being inserted to the list of cars in the node?
4. Once you've made sure your `add_car` is working, you can then move on to `does_car_exist`, `get_best_car` and `get_worst_car`.
5. Testing is extremely important to help debug any issues you may experience. Be sure to write thorough tests with various edge cases to make sure your program works as expected.

# Submission

Once you're done with writing your class definitions and tests, submit the following files to Gradescope's Lab08 assignment:

* `Car.py` 
* `CarInventoryNode.py`
* `CarInventory.py`
* `testFile.py`

There will be various unit tests Gradescope will run to ensure your code is working correctly based on the specifications given in this lab.

If the tests don't pass, you may get some error message that may or may not be obvious at this point. Don't worry - if the tests didn't pass, take a minute to think about what may have caused the error. If your tests didn't pass and you're still not sure why you're getting the error, feel free to ask your TAs or Learning Assistants.