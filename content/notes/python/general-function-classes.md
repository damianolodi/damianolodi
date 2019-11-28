---
title: Function and Classes
linktitle: Functions and Classes
toc: false
type: docs
date: "2019-11-27T00:00:00+01:00"
draft: false
menu:
  python:
    parent: Basic Concepts
    weight: 3

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 3
---

## Functions

```python
def func_name(var1, var2, var3=default_value):
    <code>
    return something1, something2, ...
```

**Recursion** is allowed


### Lambda Functions (sistema)

Small anonymous functions can be created with the [lambda](https://docs.python.org/2/reference/expressions.html#lambda) keyword.
This function returns the sum of its two arguments

```py
lambda a, b: a+b

lambda val: val > 1000000
```

**Lambda functions can be used wherever function objects are required.** They are syntactically restricted to a single expression. Semantically, they are just syntactic sugar for a normal function definition. Like nested function definitions, lambda functions can reference variables from the containing scope

---
## Classes

Classes are used to define **objects** in python. Every object has **properties** and associated **methods** (functons).

### Class definition

```python
class Person:
    def __init__(self, name, age): #(self, var1, var2)
        self.name = name #self.property_name = var1
        self.age = age

    def my_func(self):
        print("Hello my name is " + self.name)
```

- `__init__()` is **needed in all classes definitions.**
    - it is always executed when the class is initialized;
    - it is used to **assign values to object properties**, or to execute operations that are necessary to do when the object is being created.
- `my_func` is a **method** of the class.
- `self.property_name` &rarr; **access properties values** inside the class definition.
    - The `self` parameter is a reference to the current instance of the class.

{{% alert note %}}
Class names start with **capital letters** for convention.
{{% /alert %}}

### Class manipulation

- `new_object = My_class(property_name1, property_name2, ...)` &rarr; **create** an object.
- `new_object.property_name1` &rarr; **access** an attribute.
- `new_object.property_name1 = new_value` &rarr; **modify** an attribute:
    - if the attribute does not exists, it will be created;
    - `setattr(ClassName, attr_name, attr_value)` &rarr; as before, create a new attribute for _ClassName_.
- `new_object.method()` &rarr; **apply** a method.
- `del new_object.property_name1` &rarr; **delete** an object attribute.

### Inheritance

Inheritance allows one to define a class that acquire all the methods and properties from another class.

The new class is called **child class**, while the one from properties are inherited is called **parent class**.

```python
class Student(Person): # child class definition
    def __init__(self, fname, lname, year):
        #add properties etc.
        self.graduationyear = year

    def welcome(self):
        print("Welcome", self.fname, self.lname, "to the class of", self.graduationyear)
```

When `__init__()` is added, the _child class_ will **no longer inherit** the parent's `__init__()` function.

To keep the inheritance of the parent's `__init__()` function, add a call to the parent's `__init__()` function.

```python
class Student(Person):
    def __init__(self, fname, lname, year):
        Person.__init__(self, fname, lname)
        self.graduationyear = year

    def welcome(self):
        print("Welcome", self.fname, self.lname, "to the class of", self.graduationyear)
```

If a new method in the _child class_ is defined with the **same name** as a function in the parent class, _the parent method will be overridden._

