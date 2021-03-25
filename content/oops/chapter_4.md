---
title: "Inheritance"
date: 2020-09-24T03:20:36+05:30
draft: true
weight: 4
---





```python
class Employee:

    raise_amount = 1.04

    def __init__(self, first_name, last_name, salary):
        self.first = first_name
        self.last = last_name
        self.pay = salary
        self.email = "{}.{}@suits.com".format(first_name, last_name)

    def full_name(self):
        return "{} {}".format(self.first,self.last)

    def apply_raise(self):
        self.pay = int(Employee.raise_amount * self.pay)

class Developer(Employee):
    pass

dev_1 = Developer('Corey','Schafer',100000)
dev_2 = Developer('Brad','Traversy',100000)

print(help(Developer))

# print(dev_1.email)
# print(dev_2.email)
```



```foo
OUTPUT : 

Help on class Developer in module __main__:

class Developer(Employee)
 |  Method resolution order:
 |      Developer
 |      Employee
 |      builtins.object
 |
 |  Methods inherited from Employee:
 |
 |  __init__(self, first_name, last_name, salary)
 |      Initialize self.  See help(type(self)) for accurate signature.
 |
 |  apply_raise(self)
 |
 |  full_name(self)
 |
 |  ----------------------------------------------------------------------
 |  Data descriptors inherited from Employee:
 |
 |  __dict__
 |      dictionary for instance variables (if defined)
 |
 |  __weakref__
 |      list of weak references to the object (if defined)
 |
 |  ----------------------------------------------------------------------
 |  Data and other attributes inherited from Employee:
 |
 |  raise_amount = 1.04


```



1.  In the above example we made a developer class and inherited that from Employee class and by default we inherit all the methods and data of that class.
2.  In this we didn't make anything in the developer class and only inherited that class from the employee class and made the two objects of developer class.
3.  These two objects can access all the attributes of the employee class.
4.  There's a function called help by which we can see that what are the things a class has inherited from its parent class as shown the example above.
5.  __When we make the object of a class it looks for the init method. When it doesn't find that it check that method in its parent class that is employee in this case. This sequence of checking is decided by__ __Method Resolution Order__ as shown in this example. So it defines the list of places where python searches for attributes and methods.



#### A complete Example

```python
class Employee:

    raise_amount = 1.04

    def __init__(self, first_name, last_name, salary):
        self.first = first_name
        self.last = last_name
        self.pay = salary
        self.email = "{}.{}@suits.com".format(first_name, last_name)

    def full_name(self):
        return "{} {}".format(self.first, self.last)

    def apply_raise(self):
        self.pay = int(Employee.raise_amount * self.pay)


class Developer(Employee):
    raise_amount = 1.10

    def __init__(self, first_name, last_name, salary, prog_lang):
        super().__init__(first_name, last_name, salary)
        # Employee.__init__(self,first_name,last_name,salary)
        self.prog_lang = prog_lang

class Manager(Employee):

    def __init__(self,first_name,last_name,salary,employees=None):
        super().__init__(first_name,last_name,salary)
        if employees is None:
            self.employees = []
        else:
            self.employees = employees

    def add_emp(self,emp):
        if emp not in self.employees:
            self.employees.append(emp)

    def remove_emp(self,emp):
        if emp in self.employees:
            self.employees.remove(emp)

    def print_emps(self):
        for emp in self.employees:
            print("==>> {}".format(emp.full_name()))



dev_1 = Developer("Corey", "Schafer", 100000, "Python")
dev_2 = Developer("Brad", "Traversy", 100000, "Golang")

mgr_1 = Manager('John','Doe',1000000,[dev_1])

mgr_1.print_emps()
mgr_1.add_emp(dev_2)
print('=============')
mgr_1.print_emps()
mgr_1.remove_emp(dev_1)
print('=============')
mgr_1.print_emps()

```



```foo
OUTPUT :

==>> Corey Schafer
=============
==>> Corey Schafer
==>> Brad Traversy
=============
==>> Brad Traversy

```

1. There are two ways to call the method of parent class. One by using super and other by class name both are shown in above example.

2. __Don't pass mutable data types as default values of the arguments in a function__

3. There are two more methods :

   `isinstance(Object,Class)`     and `issubclass(class1,class2)`

   isinstance will return true if object belongs to a class or its parent class.

   issubclass will return true if class 2 is the parent of class 1



