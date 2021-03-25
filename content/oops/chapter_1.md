---
title: "Classes and Instances(Objects)"
menutitle: "Classes and Objects"
date: 2020-09-24T03:07:30+05:30
draft: true
weight: 1
---



#### Prerequisite : **Must know the concepts of oops in theory**

```python
class Employee:
    pass


emp1 = Employee()
emp2 = Employee()

print("===== OBJECTS =====")
print(emp1)
print(emp2)

emp1.first_name = "Corey"
emp2.last_name = "Schafer"

print("===== OBJECT ATTRIBUTES =====")
print(emp1.first_name)
print(emp2.last_name)

```



```foo
OUTPUT :

===== OBJECTS =====
<__main__.Employee instance at 0x7f7c631996c8>
<__main__.Employee instance at 0x7f7c63199710>
===== OBJECT ATTRIBUTES =====
Corey
Schafer
```



1.  This is a sample class definition and its 2 objects.
2.  Classes have functions and variables, and each class object has its own copy of functions and variables.
3.  We can specify specific custom attribute(variable) to attach with an object like done in the above example and python allows it.



#### Example 2

```python
class Employee:
    def __init__(self, first_name, last_name, salary):
        self.first = first_name
        self.last = last_name
        self.pay = salary
        self.email = "{}.{}@company.com".format(first_name, last_name)

    def full_name(self):
        return "{} {}".format(self.first,self.last)

emp1 = Employee("Harvey", "Specter", "1000000")
emp2 = Employee("Donna", "Paulson", "1000000")

print("===== OBJECT ATTRIBUTES =====")
print(emp1.first)
print(emp2.last)
print(emp1.full_name())
print(emp2.full_name())
```



```foo
OUTPUT :

===== OBJECT ATTRIBUTES =====
Harvey
Paulson
Harvey Specter
Donna Paulson
```



1. \_\_init\_\_ is the constructor here. It always receives first parameter as the instance which is called by keyword self.

2. Parameters of constructor and instance variables are named differently on purpose to show the difference of which is which. So basically constructor is just a function which is accepting parameters and assigning the values to the instance variables.

3. To access instance variables or functions in the class anywhere use it with self.

   `self.instance_variable`         AND         `self.function_name()`

4. In fact every object function in class will receive first parameter is self which is mandatory.

5. We can also call object functions by class name and then pass the object as an parameter explicitly like this.

   `Employee.full_name(emp1)`

