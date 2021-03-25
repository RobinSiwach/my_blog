---
title: "Property Decorators - Getters, Setters and Deleters"
menutitle: "Getters & Setters"
date: 2020-09-24T03:20:40+05:30
weight: 6
---





Property in classes is a function which we can access as an attribute.

Getters are basically properties which we use to access attributes.

Setters are used to set that value of the attribute, and deleters are used to delete the value of the attribute.

```python
class Employee:

    raise_amount = 1.04

    def __init__(self, first_name, last_name, salary):
        self.first = first_name
        self.last = last_name
        self.pay = salary
        self.email = "{}.{}@suits.com".format(first_name, last_name)

    @property
    def full_name(self):
        return "{} {}".format(self.first, self.last)

    @full_name.setter
    def full_name(self,name):
        self.first, self.last = name.split(' ')

    @full_name.deleter
    def full_name(self):
        print('Deleting Name!')
        self.first = None
        self.last = None

emp1 = Employee('Corey','Schafer',50000)

emp1.full_name = 'Robin Siwach'

print(emp1.first)
print(emp1.last)
print(emp1.full_name)

del emp1.full_name
```



```foo
OUTPUT :

Robin
Siwach
Robin Siwach
Deleting Name!
```



Before defining a setter or deleter we need to define the getter.