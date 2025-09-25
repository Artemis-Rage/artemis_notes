---
layout: page
title: "Introduction to Classes in Python"
---
## What is a Class?

Think of a **class** as a blueprint or a template for creating things. For example, you could have a blueprint for a car. The blueprint itself isn't a car, but it describes what a car is: it has wheels, an engine, a color, and it can do things like `drive()` or `honk()`.

In programming, a class is a blueprint for creating **objects**. An object is a specific instance of a class. So, if `Car` is our class (the blueprint), a red Ferrari and a blue Mini Cooper would be two different objects created from that class.

### Creating Our First Class: `Animal`

Let's create a simple blueprint for an animal. Our `Animal` class will define what all animals have in common.

In Python, we use the `class` keyword. We'll also create a special function called `__init__`. This function is the "initializer" (or "constructor") that runs whenever we create a new object from the class. It's where we set up the object's initial properties (called **attributes**).

```python
class Animal:
    # This is the initializer method
    def __init__(self, name, species):
        print(f"Creating a new animal named {name}!")
        self.name = name
        self.species = species

    # This is a regular method (a function inside a class)
    def make_sound(self):
        print("?!?")

# --- Let's use our blueprint! ---

# Create an object (an instance) of the Animal class
lion = Animal("Leo", "Lion")
elephant = Animal("Dumbo", "Elephant")

# Access the attributes of our objects
print(f"{lion.name} is a {lion.species}.")         # Output: Leo is a Lion.
print(f"{elephant.name} is a {elephant.species}.") # Output: Dumbo is an Elephant.

# Call the methods of our objects
lion.make_sound()       # Output: ?!?
elephant.make_sound()   # Output: ?!?
```

## What are Subclasses? (Inheritance)

Now, what if we want to create more specific types of animals, like a `Dog` or a `Cat`? A dog is an animal, but it also has its own unique behaviors, like barking.

This is where **inheritance** comes in. We can create a new class (a **subclass** or **child class**) that *inherits* all the attributes and methods from an existing class (the **superclass** or **parent class**). This allows us to reuse code and build upon existing blueprints.

### Creating Subclasses: `Dog` and `Cat`

Let's make a `Dog` class that inherits from `Animal`. It will automatically get the `__init__` method and the `name` and `species` attributes. We can also give it a more specific `make_sound` method. This is called **method overriding**.

```python
# The Animal class is our parent class
class Animal:
    def __init__(self, name, species):
        self.name = name
        self.species = species

    def make_sound(self):
        print("Some generic animal sound")

# The Dog class is a subclass of Animal
# We show this by putting Animal in parentheses: class Dog(Animal):
class Dog(Animal):
    # We are overriding the parent's make_sound method
    def make_sound(self):
        print("Woof!")
        
    # We can also add methods that only Dogs have
    def wag_tail(self):
        print(f"{self.name} wags its tail happily.")

# The Cat class is another subclass of Animal
class Cat(Animal):
    def __init__(self, name):
        # We can also customize the __init__ method.
        # super().__init__() calls the parent class's __init__ method.
        # We'll set the species to "Cat" automatically.
        super().__init__(name, "Cat")

    # Override the make_sound method for cats
    def make_sound(self):
        print("Meow!")

# --- Let's create objects from our subclasses! ---

# my_dog is an instance of the Dog class
my_dog = Dog("Buddy", "Golden Retriever")

# my_cat is an instance of the Cat class
my_cat = Cat("Whiskers")


# Access attributes (inherited from Animal)
print(f"{my_dog.name} is a {my_dog.species}.")   # Output: Buddy is a Golden Retriever.
print(f"{my_cat.name} is a {my_cat.species}.")   # Output: Whiskers is a Cat.

# Call the overridden methods
my_dog.make_sound()   # Output: Woof!
my_cat.make_sound()   # Output: Meow!

# Call a method unique to the Dog class
my_dog.wag_tail()     # Output: Buddy wags its tail happily.
```

### Summary

* **Class**: A blueprint for creating objects.
* **Object**: An instance of a class, with its own attributes and methods.
* **Inheritance**: The process where a new class (subclass) takes on the attributes and methods of an existing class (superclass).
* **Method Overriding**: When a subclass provides its own specific implementation of a method that is already defined in its parent class.
