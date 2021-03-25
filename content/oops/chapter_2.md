---
title: "Class Variables"
date: 2020-09-24T03:20:30+05:30
draft: true
weight: 2
---



Class variables are those variables which doesn't belong objects, i.e they are class specific variables and they are shared among objects.



```python
class Employee:

    raise_amount = 1.04

    def __init__(self, first_name, last_name, salary):
        self.first = first_name
        self.last = last_name
        self.pay = salary
        self.email = "{}.{}@company.com".format(first_name, last_name)

    def full_name(self):
        return "{} {}".format(self.first,self.last)

    def apply_raise(self):
        self.pay = int(self.raise_amount * self.pay)

emp1 = Employee("Harvey", "Specter", 1000000)
emp2 = Employee("Donna", "Paulson", 1000000)

print("===== OBJECT ATTRIBUTES =====")

print(emp1.pay)
emp1.apply_raise()
print(emp1.pay)

```



```foo
OUTPUT :

===== OBJECT ATTRIBUTES =====
1000000
1040000
```



1. Class variables are initialised in class without self.

2. We can access the value of class variable using class name or self.



   __If we try to modify the class variable outside the class by accessing object name then it will not modify the class variable in fact it will actually create a new object specific attribute__

Let's check this using namespace



```python
class Employee:

    raise_amount = 1.04

    def __init__(self, first_name, last_name, salary):
        self.first = first_name
        self.last = last_name
        self.pay = salary
        self.email = "{}.{}@company.com".format(first_name, last_name)

    def full_name(self):
        return "{} {}".format(self.first,self.last)

    def apply_raise(self):
        self.pay = int(Employee.raise_amount * self.pay)

emp1 = Employee("Harvey", "Specter", 1000000)
emp2 = Employee("Donna", "Paulson", 1000000)



print('===== NAMESPACE ========')
print(Employee.__dict__)
print('')
print(emp1.__dict__)

emp1.raise_amount = 2

print('\n===== NAMESPACE AFTER MODIFICATION ========\n')
print(Employee.__dict__)
print('--------------------------')
print(emp1.__dict__)

```



```foo
OUTPUT :


===== NAMESPACE ========
{'__module__': '__main__', 'raise_amount': 1.04, '__init__': <function Employee.__init__ at 0x7f859cef6620>, 'full_name': <function Employee.full_name at 0x7f859cef68c8>, 'apply_raise': <function Employee.apply_raise at 0x7f859cef6950>, '__dict__': <attribute '__dict__' of 'Employee' objects>, '__weakref__': <attribute '__weakref__' of 'Employee' objects>, '__doc__': None}
--------------------------
{'first': 'Harvey', 'last': 'Specter', 'pay': 1000000, 'email': 'Harvey.Specter@company.com'}

===== NAMESPACE AFTER MODIFICATION ========

{'__module__': '__main__', 'raise_amount': 1.04, '__init__': <function Employee.__init__ at 0x7f859cef6620>, 'full_name': <function Employee.full_name at 0x7f859cef68c8>, 'apply_raise': <function Employee.apply_raise at 0x7f859cef6950>, '__dict__': <attribute '__dict__' of 'Employee' objects>, '__weakref__': <attribute '__weakref__' of 'Employee' objects>, '__doc__': None}
--------------------------
{'first': 'Harvey', 'last': 'Specter', 'pay': 1000000, 'email': 'Harvey.Specter@company.com', 'raise_amount': 2}
```



Before Modification, the raise_amount only exists in the class Employee namespace.

After modification, the raise_amount also exists in object namespace.



__So We can use this behaviour in custom situations according to the need, for example if we want in a company to give standard raise to all the employees but more than or less than standard raise to a specific employee than we can assign that raise_amount variable for that object and change the raise value for that specific object__

_If we want to count the number of employees or number of objects for any class than we can take a class variable and increment that variable in init method(constructor) so we can keep count of number of objects_

**_PRO TIP : ONLY ACCESS CLASS VARIABLES USING CLASS NAME UNLESS YOU HAVE A SPECIAL USE CASE_**





