---
layout: lab
num: lab09
ready: draft
desc: "Used Car Lot - Part Two"
assigned: 2024-03-08 23:59:59.59-7
due: 2024-03-18 23:59:59.59-7
---

In this lab, you'll have the opportunity to practice:

* Modifying classes in Python
* Further implementing Binary Search Tree (BST) data structure supporting removal functionality
* Testing your functionality with pytest

**Notes:**
* **There will be NO 24-hour late submission window for this lab.**
* This lab will be dependent on your previous lab. Certain tests from the previous lab will be autograded in this week's lab. It is important that you start this lab early so you can utilize our office hours to seek assistance / ask clarifying questions during the weekdays before the deadline if needed!

# Introduction

The goal for this lab is to take your **existing** Used Car Lot program in **Lab08** that will manage cars for a second-hand car dealership, and support removing Cars from the lot. As a reminder, all Cars have a `make`, `model`, `year`, and `price`, which can be used to determine the value of cars in relation to each other. All Cars will be managed with a Binary Search Tree (BST) where the BST nodes are sorted by `make`, then `model`.

In order to remove the cars for this lab, you will define a `remove_car` method in the `CarInventory` class that will remove Cars with the same `make`/`model`/`year`/`price` from a `CarInventoryNode`'s cars list. After removing a Car and no cars exist in the `CarInventoryNode`'s cars list, you will then need to remove the node from the BST while preserving the BST property.

You will also write pytests in `testFile.py` illustrating your behavior works correctly. This lab writeup will provide some test cases for clarity, but the Gradescope autograder will run different tests shown here. It's important to thoroughly test your program with various cases!

# Instructions

You will need to copy over all your files from Lab08 and modify two files:
* `CarInventory.py` - Defines a CarInventory (BST) class that is an ordered collection of a Dealership's Cars. You will be adding to your existing `CarInventory` class. 
* `testFile.py` - This file will contain your pytest functions that tests the overall correctness of your class definitions.

Your starter code for this assignment will be your program from Lab08, and you'll have to add the additional specifications defined below.

You should organize your lab work in its own directory. This way all files for a lab are located in a single folder. Also, this will be easy to import various files into your code using the `import / from` technique shown in lecture. 

# CarInventory.py

The `CarInventory.py` file will contain the definition of a `CarInventory` class. This will keep track of the cars a dealership has, implemented as a BST. The `CarInventory` will create `CarInventoryNode` objects using `Car` objects based on their `make` and `model`. `Car` objects with the same make and model will be appended to a list based on insertion order within the `CarInventoryNode` object. For further specifications regarding existing requirements, reference the Lab08 page.

In addition to the methods created before, the following methods are required to be implemented:

* `get_successor(self, make, model)` - attempts to finds the `CarInventoryNode` with the `make` and `model`, and returns the CarInventoryNode with the next greatest value (using the same heirarchy of `make`, then `model`). Returns `None` if there is no `CarInventoryNode` with the specified `make` and `model`, or if the `CarInventoryNode` is the maximum and has no successor. **Note, this includes the successor of any `CarInventoryNode` in the BST if it exists, not just the successor used for BST maintenance.**

  How Successors are determined - 
    * The BST is primarily ordered by the car's make and then by model.
    * For a given car make and model, the successor is the CarInventoryNode that comes next in the order of the tree. This could be:
        * In the simplest case, if the node has a right child, the successor is the leftmost node in the right subtree.
        * If the node has no right child, the successor is one of its ancestors, specifically the closest ancestor for which the given node would be in its left subtree.

* `remove_car(self, make, model, year, price)` - attempts to find the `Car` with the specified `make`, `model`, `year`, and `price`, and removes it the `CarInventoryNode`'s cars list. If the list is empty after removing the `Car`, remove the `CarInventoryNode` from the BST entirely. Returns `True` if the `Car` was successfully removed, and `False` if the `Car` is not present in the `CarInventory`. If there are duplicate cars within a `CarInventoryNode`'s car list that matches the specifications, you will just remove the first matching `Car` object in the cars list.

**A note if you have implemented `CarInventoryNode` comparators:** If you have implemented `CarInventoryNode` comparators in last week's lab, in your `__eq__` comparator overload, before you check for the `make` and the `model`, you should check if the right-hand-side is `None`. If it is `None`, you should return `False`. This is because of a quirk about how Python handles comparators between overloaded comparators and `None`.

# Examples

Given an example BST:

```python
bst = CarInventory()

car1 = Car("Mazda", "CX-5", 2022, 25000)
car2 = Car("Tesla", "Model3", 2018, 50000)
car3 = Car("BMW", "X5", 2022, 60000)
car4 = Car("BMW", "X5", 2020, 58000)
car5 = Car("Audi", "A3", 2021, 25000)

bst.add_car(car1)
bst.add_car(car2)
bst.add_car(car3)
bst.add_car(car4)
bst.add_car(car5)

#                                  Mazda,CX-5,[Car(Mazda,CX-5,2022,25000)]
#                                 /                                       \
#           BMW,X5,[Car(BMW,X5,2022,60000),Car(BMW,X5,2020,58000)]    Tesla,Model3,[Car(Tesla, Model3,2018,50000)]
#                   /
#  Audi,A3,[Car(Audi,A3,2021,25000)]
```

### Inorder Traversal

Using the `CarInventory` after the `add_car` methods above, an example of the `inorder()` string format for removal is given below after removing the following Car:

```python

bst.remove_car("BMW", "X5", 2020, 58000)

#                                  Mazda,CX-5,[Car(Mazda,CX-5,2022,25000)]
#                                 /                                       \
#           BMW,X5,[Car(BMW,X5,2022,60000)]    Tesla,Model3,[Car(Tesla,Model3,2018,50000)]
#                   /
#  Audi,A3,[Car(Audi,A3,2021,25000)]

assert bst.inorder() == \
"""\
Make: AUDI, Model: A3, Year: 2021, Price: $25000
Make: BMW, Model: X5, Year: 2022, Price: $60000
Make: MAZDA, Model: CX-5, Year: 2022, Price: $25000
Make: TESLA, Model: MODEL3, Year: 2018, Price: $50000
"""
```

and if we then remove the following car, the `CarInventoryNode` will be removed from the BST. The `CarInventory` and `inorder()` string format is given below in this case:

```python
bst.remove_car("BMW", "X5", 2022, 60000)

#                                  Mazda,CX-5,[Car(Mazda,CX-5,2022,25000)]
#                                 /                                       \
#           Audi,A3,[Car(Audi,A3,2021,25000)]    Tesla,Model3,[Car(Tesla,Model3,2018,50000)]


assert bst.inorder() == \
"""\
Make: AUDI, Model: A3, Year: 2021, Price: $25000
Make: MAZDA, Model: CX-5, Year: 2022, Price: $25000
Make: TESLA, Model: MODEL3, Year: 2018, Price: $50000
"""
```

### Preorder Traversal

Using the `CarInventory` after the `add_car` methods above, an example of the `preorder()` string format is given below after removing the following Cars:

```python

bst.removeCar("BMW", "X5", 2020, 58000)

#                                  Mazda,CX-5,[Car(Mazda,CX-5,2022,25000)]
#                                 /                                       \
#           BMW,X5,[Car(BMW,X5,2022,60000)]    Tesla,Model3,[Car(Tesla,Model3,2018,50000)]
#                   /
#  Audi,A3,[Car(Audi,A3,2021,25000)]

assert bst.preorder() == \
"""\
Make: MAZDA, Model: CX-5, Year: 2022, Price: $25000
Make: BMW, Model: X5, Year: 2022, Price: $60000
Make: AUDI, Model: A3, Year: 2021, Price: $25000
Make: TESLA, Model: MODEL3, Year: 2018, Price: $50000
"""

bst.remove_car("BMW", "X5", 2022, 60000)

#                                  Mazda,CX-5,[Car(Mazda,CX-5,2022,25000)]
#                                 /                                       \
#           Audi,A3,[Car(Audi,A3,2021,25000)]    Tesla,Model3,[Car(Tesla,Model3,2018,50000)]


assert bst.preorder() == \
"""\
Make: MAZDA, Model: CX-5, Year: 2022, Price: $25000
Make: AUDI, Model: A3, Year: 2021, Price: $25000
Make: TESLA, Model: MODEL3, Year: 2018, Price: $50000
"""
```

### Postorder Traversal

Using the `CarInventory` after the `add_car` methods above, an example of the `postorder()` string format is given below after removing the following Cars:

```python

bst.remove_car("BMW", "X5", 2020, 58000)

#                                  Mazda,CX-5,[Car(Mazda,CX-5,2022,25000)]
#                                 /                                       \
#           BMW,X5,[Car(BMW,X5,2022,60000)]    Tesla,Model3,[Car(Tesla,Model3,2018,50000)]
#                   /
#  Audi,A3,[Car(Audi,A3,2021,25000)]

assert bst.postorder() == \
"""\
Make: AUDI, Model: A3, Year: 2021, Price: $25000
Make: BMW, Model: X5, Year: 2022, Price: $60000
Make: TESLA, Model: MODEL3, Year: 2018, Price: $50000
Make: MAZDA, Model: CX-5, Year: 2022, Price: $25000
"""

bst.remove_car("BMW", "X5", 2022, 60000)

#                                  Mazda,CX-5,[Car(Mazda,CX-5,2022,25000)]
#                                 /                                       \
#           Audi,A3,[Car(Audi,A3,2021,25000)]    Tesla,Model3,[Car(Tesla,Model3,2018,50000)]


assert bst.postorder() == \
"""\
Make: AUDI, Model: A3, Year: 2021, Price: $25000
Make: TESLA, Model: MODEL3, Year: 2018, Price: $50000
Make: MAZDA, Model: CX-5, Year: 2022, Price: $25000
"""
```

These are just a few simple examples illustrating the functionality of removing a Car from the `CarInventory` cars list, and removing the `CarInventoryNode` from the `CarInventory`. Gradescope will thoroughly test various cases. As always, it's important to thoroughly test your own code with various possible cases.

As part of the debugging process, we are providing you two functions to visually represent the BST to ensure it looks similar to your thought. Make use of these carefully through multiple steps of your methods - For e.g., you can print out the visual representation of your subtree in each step of the recursion so that you can validate your theoretical assumption in action. If you want to call this method inside your call, you can do this by running `show_tree(self.root)` or `show_tree(node)` or `show_tree(car_inventory_object.root)`. Below is the code snippet for the functions and the corresponding logic to see it in action - 

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
    # Displaying the inventory before removing a node
    show_tree(inventory.root)
    # Remving a car
    inventory.remove_car("Toyota", "Camry", 2020, 25000)
    # Displaying the inventory after removing a node (root node):
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


Showing Tree Representation of Children under Node - Make: TOYOTA, Model: COROLLA

|----(Level 0) TOYOTA : COROLLA (*)
      |----(Level 2) HONDA : CIVIC (LR)
   |----(Level 1) HONDA : ACCORD (L)
      |----(Level 2) FORD : FUSION (LL)
         |----(Level 3) CHEVROLET : MALIBU (LLL)

End of the car inventory. 


==================================================
```


Other than the required methods, feel free to implement any helper methods that you think are useful in your implementation. The automated tests will test only your implementation of the required methods and certain methods from last week by creating a `CarInventory` containing various `Cars` with different `make`, `model`, `year`, and `price` attributes. The `remove_car()` and `add_car()` methods will be run, with `get_car()`, `get_successor()`, `inorder()`, `preorder()`, and `postorder()` being used to verify that the `CarInventory` is fully functional. You should be sure that Lab08 is working correctly, and write tests to confirm your program for this lab is working properly.

# testFile.py

This file should test all of the new methods in `CarInventory.py` using pytest. Think of and create your own various scenarios and edge cases when testing your code according to the given descriptions. For the `get_successor` method, your tests should test the general case and the case used for BST maintenance. For the `remove_car` method, you tests should cover Cases 1, 2, and 3 (as discussed in lecture) at a minimum, as well as only removing a Car from the `CarInventoryNode`'s cars list without removing the `CarInventoryNode`. Even though Gradescope will not use this file when running automated tests (Gradescope will use other tests), it is important to provide this file with various test cases (testing is important!!).

**A note about Gradescope tests:** Gradescope will use your functions to correctly check the state of your `Car`s and `CarInventory` with many scenarios. In order to test if everything is in the correct state, these tests use your CarInventory's `preorder` / `inorder` / `postorder` traversals and `add_car` methods, as well as getting the string representation of your `Car`s and `CarInventoryNode`s to run other tests. It is important to ensure your `preorder` / `inorder` / `postorder` traversals, your various string representations, and `CarInventory`'s `add_car` methods work correctly first or else many of the other tests may not pass. Here, examples of various test cases could be different tree structures. **You may re-use the examples from Lab08, and write various cases for `remove_car()` in addition to others.**

Of course, feel free to reach out / post questions on Piazza as they come up!

# Submission

Once you're done with writing your class definitions and tests, submit the following files to Gradescope's Lab09 assignment:

* `Car.py` 
* `CarInventoryNode.py`
* `CarInventory.py`
* `testFile.py`

There will be various unit tests Gradescope will run to ensure your code is working correctly based on the specifications given in this lab.

If the tests don't pass, you may get some error message that may or may not be obvious at this point. Don't worry - take a minute to think about what may have caused the error. Try writing more test cases to see if you're able to reproduce the problem. If you're still not sure why you're getting the error, feel free to ask your TAs or Learning Assistants.


