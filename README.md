# One-to-many Object Relationships

## Learning Goals

- Describe one-to-many relationships.
- Explain why one-to-many relationships are important.

***

## Key Vocab

- **Class**: a bundle of data and functionality. Can be copied and modified to
accomplish a wide variety of programming tasks.
- **Object**: the more common name for an instance. The two can usually be used
interchangeably.
- **Object-Oriented Programming**: programming that is oriented around data
(made mobile and changeable in **objects**) rather than functionality. Python
is an object-oriented programming language.
- **Function**: a series of steps that create, transform, and move data.
- **Method**: a function that is defined inside of a class.

***

## Introduction

A one-to-many relationship refers to a relationship between two or more objects. One object can be associated with multiple objects of another type. This relationship is represented using association, aggregation, or composition. In this lesson we will talk about association, aggregation, and composition and how we can use them to create one-to-many relationships. In this lesson we will talk about these relationships using examples of real world relationships. One to many relationships are not as common in the real world as many-to-many relationships.

***

## Association

Association is a weak relationship between otherwise unrelated objects, where the objects have their own lifetime and there is no parent object.

Lets look at an example of a one way one-to-many relationship.

The example will use a Shopper and GroceryItem class. The Shopper class will have a list attribute to store the related GroceryItem objects. Here's an example implementation:

```py
class GroceryItem:
    def __init__(self, name, price):
        self._name = name
        self._price = price

    @property
    def name(self):
        return self._name

    @name.setter
    def name(self, value):
        self._name = value

    @property
    def price(self):
        return self._price

    @price.setter
    def price(self, value):
        self._price = value

class Shopper:
    def __init__(self, name):
        self._name = name
        self._grocery_items = []

    @property
    def name(self):
        return self._name

    @name.setter
    def name(self, value):
        self._name = value

    @property
    def grocery_items(self):
        return self._grocery_items

    def add_grocery_item(self, item):
        self._grocery_items.append(item)
```

In this example, the Shopper class has a `grocery_items` attribute, which is a list of `GroceryItem` objects. The `add_grocery_item` method allows adding an item to the shopper's grocery list, establishing a one-to-many relationship between `Shopper` and `GroceryItem`. Here's how you can use these classes:

```py
# Create a shopper and some grocery items
shopper = Shopper("Alice")
item1 = GroceryItem("apple", 1)
item2 = GroceryItem("orange", 2)

# Add the grocery items to the shopper's list
shopper.add_grocery_item(item1)
shopper.add_grocery_item(item2)

# Print the shopper's grocery list
for item in shopper.grocery_items:
    print(item.name, item.price)
```

Lets look at an example of a two way one-to-many relationship. In this example we will use a `Teacher` and
`Student` class.
The reason we want to do this is because the `Student` class may need to know who the `Teacher` is.
In the previous example the `GroceryItem` has no reason to know who the shopper associated with it is.

For this example we will also add type checks to the setters and methods and return `TypeError` if
the types are incorrect. In the setters for the attributes we can use `isinstance` to make sure the user is not
passing in a string when the attribute expects an integer or certain object.  

```py
class Student:
    def __init__(self, name, age):
        self.name = name
        self.age = age
        self._teacher = None

    @property
    def name(self):
        return self._name

    @name.setter
    def name(self, value):
        if not isinstance(value, str):
            raise TypeError("Name must be a string")
        self._name = value

    @property
    def age(self):
        return self._age

    @age.setter
    def age(self, value):
        if not isinstance(value, int):
            raise TypeError("Age must be an integer")
        self._age = value

    @property
    def teacher(self):
        return self._teacher

    @teacher.setter
    def teacher(self, value):
        if not isinstance(value, Teacher):
            raise TypeError("Teacher must be an instance of Teacher class")
        self._teacher = value

class Teacher:
    def __init__(self, name):
        self.name = name
        self._students = []

    @property
    def name(self):
        return self._name

    @name.setter
    def name(self, value):
        if not isinstance(value, str):
            raise TypeError("Name must be a string")
        self._name = value

    @property
    def students(self):
        return self._students

    def add_student(self, student):
        if not isinstance(student, Student):
            raise TypeError("Student must be an instance of Student class")
        self._students.append(student)
        student.teacher = self
```

In this example a teacher can have multiple students and a student needs to know who their teacher is.
The difference in this example is that when we add the student to the teacher we also set the students teacher to
the teacher object using `self`.

## Aggregation

Aggregation is a form of association in which each object has its own life cycle, but there exists an ownership as well. This is typically a parent/child relationship. We will use a `Car` and `Engine` class.

```py
class Car:
    def __init__(self, engine):
        self.engine = engine

class Engine:
    def __init__(self, cylinders, fuelType):
        self.cylinders = cylinders
        self.fuelType = fuelType
```

The engine class does not need to know about the car.

We can create an engine object and pass it into the Car when we initialize it.

```py
four_cylinder_engine = Engine(4, 'regular')
acura_tlx = Car(engine)
```

If the car is deleted the engine object won't be deleted.

## Composition

Composition is a "part-of" relationship, where both entities are interdependent on each other. For instance, an CPU is a part of a computer, and both are dependent on each other

```py

class CPU:
  def __init__(self, cpu_type):
    self.cpu_type = cpu_type

class Computer:
  def __init__(self, cpu_type):
    self.CPU = CPU(cpu_type)

```

This example represents a one-to-many relationship between computer and CPU because a computer can have many different types of CPUs. This is composition because the CPU is created within the Computer class.

***

## Conclusion

In conclusion, one-to-many relationships in object-oriented programming are a fundamental concept that allows for the efficient organization of relationships between entities. By designing classes and objects that represent real-world entities and their relationships, we can create scalable and reusable code structures.

***

## Resources

- [Python classes](https://docs.python.org/3/tutorial/classes.html)
