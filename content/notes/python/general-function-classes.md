---
title: Function and Classes
linktitle: Functions and Classes
toc: true
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

# Typical structure of a recursive function
def recursive_function(var1, ...):
    if <condition>:
        return <base_value>
    recursive_function(modified_var_1, ...)
```

### Passing Parameters to Functions
One can pass a variable number of parameters to a function using `*args` and `**kwargs`.

- `*args` is used to pass a non-keyworded variable-length argument list to functions;
- `**kwargs` is used to pass **keyworded**, _variable-length argument dictionary to a function._

In both cases, the names `args` and `kwargs` are used for conventions, so the important thing to notice is the use of asterisks in the definition of the function.

```py
def add_numbers (*args):
    x = 0
    for item in args:
        x = x + item
    return x
```

{{% alert note %}}
When calling or defining a function, one should order the arguments in the following way:

- formal positional arguments
- `*args`
- keyword arguments
- `**kwargs`
{{% /alert %}}

`args` and `kwargs` can be used also in function calls.

```py
args = (value_1, value_2, value_3)
kwargs = { "key_1": value_1,
           "key_2": value_2 }

my_function(*args, **kwargs)
```

---
## Lambda Functions

When a function is defined, Python does two things:

1. it creates a new function object;
2. it assigns a name to that object.

Sometimes one needs to use _anonymous_ functions, e.g. to pass a function to another one as a parameter. In Python this is possible and those functions are called **lambda functions.**

```py
# example taken from the newsletter of Reuven Lerner
mylist.sort(key=lambda x: len(str(x)))

# Equivalent of ...
def str_len(x):
    return len(str(x))

mylist.sort(key=str_len)

# GENERAL SYNTAX FOR CREATION OF A LAMBDA FUNCTION
lambda arg1, arg2, ...: <single_expression>
```

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

