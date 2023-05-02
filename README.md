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

In object-oriented programming, a one-to-many relationship refers to a relationship between two or more objects in which one object can be associated with multiple objects of another type. For instance, a teacher can have multiple students in a class, or a university can have many departments.


To represent this relationship, we can use various techniques such as association, aggregation, or composition. Association refers to a relationship where two objects are related, but not in a way that one object owns the other. For example, a library has an association with books as it stores them, but it doesn't own them. Aggregation is a relationship where one object is a part of another object, and the first object can exist independently. For example, a car can have many wheels, but each wheel can also be used in other cars. Composition is a relationship where one object is made up of another object, and the first object cannot exist without the second object. For example, a house is composed of rooms, and each room is part of the house.


In this lesson, we will discuss how we can use these techniques to create one-to-many relationships.

***

## Association

Association is a weak relationship between otherwise unrelated objects, where the objects have their own lifetime and there is no parent object.

### One way one-to-many relationship

Lets look at an example of a one way one-to-many relationship.
The example will use a `Shopper` and `GroceryItem` class. The `Shopper` class will have a list attribute to store the related `GroceryItem` objects.

In this case it does not make sense for the `GroceryItem` to know about the `Shopper`.

Here's an example implementation:

```py
class GroceryItem:
    def __init__(self, name, price):
        self.name = name
        self.price = price

class Shopper:
    def __init__(self, name):
        self.name = name
        self.grocery_items = []


```

In this example, the `Shopper` class has a `grocery_items` attribute, which is a list of `GroceryItem` objects. The `add_grocery_item` method allows adding an item to the shopper's grocery list, establishing a one-to-many relationship between `Shopper` and `GroceryItem`. Here's how you can use these classes:

```py
# Create a shopper and some grocery items
shopper = Shopper("Alice")
item1 = GroceryItem("apple", 1)
item2 = GroceryItem("orange", 2)

# Add the grocery items to the shopper's list
shopper.grocery_items.append(item1)
shopper.grocery_items.append(item2)

# Print the shopper's grocery list
for item in shopper.grocery_items:
    print(item.name, item.price)

# => apple 1
# => orange 2
```

### Two way one-to-many relationship

Lets look at an example of a two way one-to-many relationship. In this example we will use a `Teacher` and
`Student` class.
The reason we want to make this a two way relationship is because the `Student` class may need to know who the `Teacher` is.
In the previous example the `GroceryItem` has no reason to know who the shopper associated with it is.

For this example we will also add type checks to the setters and methods and raise a `TypeError` if
the types are incorrect. In the setters for the attributes we can use `isinstance` to make sure the user is not passing in a string or number when the attribute expects an object.  

```py
class Student:
    def __init__(self, name, age):
        self.name = name
        self.age = age
        # teacher is protected because it is not a part of the constructor
        self._teacher = None

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
        # students is protected because it is not a part of the constructor

        self._students = []

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
The difference in this example is that when we add the student to the teacher in the `add_student` method we also set the students teacher to the teacher object using `self`.

***

## Aggregation

Aggregation is a form of association in which each object has its own life cycle, but there exists an ownership as well. Similarly to object inheritance we can call this a parent child relationship but instead of the attributes the parent contains the instance of a child. We will use a `Car` and `Engine` class for this example.

```py
class Car:
    def __init__(self, engine):
        self.engine = engine

class Engine:
    def __init__(self, cylinders, fuelType):
        self.cylinders = cylinders
        self.fuelType = fuelType
```

The `Car` class takes in an engine object instead of creating the `Engine` object. This allows
the `Engine` object to have its own lifecycle.

```py
four_cylinder_engine = Engine(4, 'regular')
acura_tlx = Car(engine)
```

If the `Car` is deleted the engine object won't be deleted. We can use the engine object in a different `Car` object.

***

## Composition

Composition is a "part-of" relationship, where both entities are interdependent on each other. For instance, a `CPU` is a part of a `Computer`, and both are dependent on each other

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
