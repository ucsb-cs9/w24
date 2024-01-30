---
num: "Lecture 6"
desc: "Pytest, Inheritance"
ready: true
lecture_date: 2024-01-25 14:00:00.00-7:00
---

[Slides folder]({{ site.slides_url }})

[Recorded video](https://www.loom.com/share/e14306389a31470db833eba1752a0e4e)


# Plan for today

* Define a parent class
* Define derived classes
* Write the corresponding test classes and methods
* Run the tests in pytest 


# Inheritance

* Avoids code duplication (similar to the idea of functions)
* Allows us to extend the existing code and use existing methods

* Start with the **base class** (also referred to as a **parent class** or a **super class**)
* The classes that inherit from the base class, are referred to as the **child** / **derived** / **sub-class**.

For pytest:
* a test file's name has to start with `test_` (pytest won't recognize it otherwise)
* the convention in this class is that function names will also start with `test_`
* Need to run pytest on the Command Line (Terminal on Mac or Command Prompt on Windows)

---

* Let's write an `Animal` class and see what inheritance looks like in action:

```
# Animal.py
class Animal:
	''' Animal class type that contains attributes for all animals '''

    def __init__(self, species=None, name=None):
        self.species = species
        self.name = name

    def setName(self, name):
        self.name = name

    def setSpecies(self, species):
        self.species = species

    def info(self):
        return f"Species: {self.species}\nName: {self.name}"

    def getSound(self):
        return "I'm an Animal!!!"
```

* Let's test our Animal class by creating its instances:

```

if __name__ == "__main__":
    	pet = Animal()
	rarebird = Animal("phoenix", "Zarra")
	print(rarebird.info())
	print(rarebird.getSound()) # I’m an Animal!!!
```

## Testing the class and its methods

Once we have a class and its methods, we can generate a test file by saving a copy of the class definition and changing the names of the class and the methods (as is shown in lab02 instructions):

```
from Animal import Animal

class TestAnimal:
    
    def test__init__default(self):
        a1 = Animal()
        assert a1.species == None
        assert a1.name == None
    
    def test__init__initialized(self):
        a1 = Animal("bird", "Gosha")
        assert a1.species == "bird"
        assert a1.name == "Gosha"  

    def test_setSpecies(self):
        pass

    def test_setName(self):
        pass

    def test_getSpecies(self):
        a1 = Animal("horse", "Lala")
        assert a1.getName() == "Lala"

    def test_getName(self):
        pass

    def test_info(self):
        pass

    def test_getSound(self):
        pass
                
```

* Ideally, you need to create the test file as soon as you have all the stubs for the class methods
	* create a variety of tests, thinking of potential edge cases, different ways to instantiate the class, plausible input values
 	* by creating the method stubs first and then writing the tests, you ensure that when you run your tests, they should all FAIL
   	* as you begin implementing the functions, you should see your tests PASS once you have the correct implementation.


## Creating a derived class

* Let's define a `Cow` class that inherits from the `Animal` class:

```
# Cow.py

from Animal import Animal

class Cow(Animal):
    # Available method for the Cow Class 
    def setSound(self, sound):
        self.sound = sound
```

and instantiate a specific cow:
```
mycow = Cow("cow", "Betsy")
print(mycow.info())
mycow.setSound("Moo") # Sets a Cow sound attribute to "Moo"
print(mycow.getSound()) # I’m an Animal!!! (calls the `Animal.getSound` method)
```

* Note that the Cow’s constructor (`__init__`) was inherited from the class `Animal` as well as the `info()` method
* Also note that we didn’t need to define the `getSound()` method since it was inherited from `Animal`
* But in this case, this inherited method `getSound()` may not be what we want.
* So we can redefine its functionality in the Cow class!

```
# in Cow class
def getSound(self):
	return f"{self.sound}!"
```

* We changed the `getSound()` method in the `Cow` class, so in this case our `Cow` class overrode the `getSound()` method of `Animal`
* So now, cow objects will use its own version of `getSound()`, not the version that was inherited from `Animal`, as seen below:

```
mycow = Cow("cow", "Betsy")
mycow.setSound("Moo") # Sets a Cow sound to "Moo"
print(mycow.getSound()) # Moo!
```

* We can still create `Animal` objects, and `Animal` objects will still use its own version of `getSound()`

```
rarebird = Animal("phoenix", "Zarra")
print(rarebird.info())
print(rarebird.getSound()) # I’m an Animal!!!
```

<b>Note:</b> The constructed object type will dictate which method in which class is called.
* It first looks at the <b>constructed object type</b> and checks if there is a method defined in that class. If so, it uses that method.
* If the constructed object doesn’t have a method definition in its class, then it checks the immediate parent(s) it inherited from, and so on ...
* If there is no matching method call, then an error happens.

We can build on the parent class methods and further extend them in the derived class. For example, if we wanted the cow information to include the sound string as well, we can re-use the existing info like so:

```
# in Cow class
def info(self):
    """ Make the default output look like:
        ---
        Species: None
        Name: None
        Sound: None
    """
       return f"---\n{super().info()}\nSound: {self.sound}"
```


### Troubleshooting

If you get a `TypeError: object.__init__() takes exactly one argument (the instance to initialize)` and your `__init__()` constructor looks correct, double-check the indentation of your code: everything that belongs to the class definition needs to be indented underneath the `class` keyword.




