---
title: "Class Methods and Static Methods"
date: 2020-09-24T03:20:33+05:30
weight: 3
---





Class Methods are those methods which belong to the whole class and doesn't accept instance as a first argument.



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

    @classmethod
    def set_raise_amt(cls,amount):
        cls.raise_amount = amount


emp1 = Employee("Harvey", "Specter", 1000000)
emp2 = Employee("Donna", "Paulson", 1000000)


Employee.set_raise_amt(1.05) # It's same as Employee.raise_amount = 1.05

print(Employee.raise_amount)
print(emp1.raise_amount)
print(emp2.raise_amount)

```



```foo
OUTPUT :

1.05
1.05
1.05

```



1.  First parameter of class method is cls which denotes class.
2.  classmethod decorator is used to make the methods class method.
3.  We can call class method using both class name and instance name but class name is preferred.



#### Applications of class methods

One Interesting use case of class methods is they can be used as alternative constructors.

Suppose in the context of above example we need to create employees from a data which is given in form a string separated by a delimiter or in some other form. So to create that data from that object we first need to parse that data to extract the value of fields required to create the object and then create the object. So We can do all that parsing in a class method and can create and return that object directly from that class method.

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

    @classmethod
    def set_raise_amt(cls,amount):
        cls.raise_amount = amount

    @classmethod
    def from_string(cls,emp_str):
        first, last, pay = emp_str.split('-')
        return cls(first,last,int(pay))

emp1 = Employee("Harvey", "Specter", 1000000)
emp2 = Employee("Donna", "Paulson", 1000000)


emp_str_1 = 'Jessica-Pearson-10000000'
emp_str_2 = 'Mike-Ross-1000000'
emp_str_3 = 'Rachel-Zane-1000000'


new_emp_1 = Employee.from_string(emp_str_1)

print(new_emp_1.full_name())
print(new_emp_1.email)
```

```foo
OUTPUT :

Jessica Pearson
Jessica.Pearson@suits.com
```





1.  Generally alternative constructor's name start by keyword 'from'.
2.  A perfect example of this is datetime module of python in which we have methods like from timestamp and other formats from which we can create datetime objects.



### Static Methods

Static Methods are those methods which doesn't take class or object as a first parameter. So they behave like normal functions. The reason we keep them in class is because they have a logical connection to the class.

We can call static method by both using class name or instance name.

_**If you are not using object attributes or class variables in the method than that method should be static method**_

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

    @classmethod
    def set_raise_amt(cls,amount):
        cls.raise_amount = amount

    @classmethod
    def from_string(cls,emp_str):
        first, last, pay = emp_str.split('-')
        return cls(first,last,int(pay))

    @staticmethod
    def is_workday(day):
        if day.weekday == 5 or day.weekday == 6:
            return False
        return True


emp1 = Employee("Harvey", "Specter", 1000000)
emp2 = Employee("Donna", "Paulson", 1000000)


import datetime
my_date = datetime.date(2016,7,10)

print(Employee.is_workday(my_date))

```

```foo
OUTPUT :

True
```





