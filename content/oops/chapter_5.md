---
title: "Special(Magic_Dunder) Methods"
menutitle: "Magic Methods"
date: 2020-09-24T03:20:38+05:30
weight: 5
---



__Special Methods are those methods generally which start  and end by Dunder(\_\_). They are also called magic methods and they allow us to emulate some builtin behaviour in python and it's also how we implement operator overloading.__

Like for eg how + behaves differently with integers and strings. In case of integers it adds them while in case of string it concatenates them.



Now there are two more special methods which we should always implement while making our own classes.

1.  `__repr__` : It is meant to be an unambiguous representation of the object and should be used for logging and debugging and things like that. It's really meant to be seen by other developers.
2.  `__str__` : It is meant to be more of a readable representation of that object and is meant to be used as a display to the end user.

So in our class we must have `__repr__` implemented because in case we have this method in our class and we don't have `__str__` method in our class and `__str__` is called on our object than `__repr__` will be called as a fallback.

TIP : When creating `__repr__` , try to display something which we can copy back in python code and it would recreate that same object.



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

    def __repr__(self):
        return "Employee('{}','{}',{})".format(self.first,self.last,self.pay)

    def __str__(self):
        return "{} - {}".format(self.full_name(),self.email)

    def __add__(self,other):
        return (self.pay + other.pay)

    def __len__(self):
        return len(self.full_name())


emp1 = Employee('Corey','Schafer',50000)
emp2 = Employee('Brad','Traversy',60000)

print(repr(emp1)) # Same as emp1.__repr__()
print(str(emp2)) # Same as emp2.__str__()

print(emp1+emp2)
print(len(emp1))
```



```foo
OUTPUT :

Employee('Corey','Schafer',50000)
Brad Traversy - Brad.Traversy@suits.com
110000
13
```



In the above example we implemented our own custom \_\_add\_\_ method to add two objects of Employee class and addition is performed on their salaries which are given as output.

We also defined our own custom \_\_len\_\_ method to check the length of the full name of an employee directly by using the object.



Some applications for operator overloading is we can check how objects are compared or how they check for equality.



Here's a link to the list of special methods which we can define in our classes to define own functionality on objects.

https://docs.python.org/3/reference/datamodel.html#special-method-names



