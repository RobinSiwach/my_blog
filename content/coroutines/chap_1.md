---
title: "Coroutines 101"
date: 2021-03-25T11:40:29+05:30
---





#### Prerequisite : You should know about iterators and how a for loop in python work behind the scenes. You don't need to know anything about generators or coroutines or the yield keyword.



Just a quick overview of how a for loops works in python



```python
x = [1,2,3,4,5]
for i in x:
    print(i)
```

```foo
OUTPUT : 

1
2
3
4
5
```



The above code mentioned is equivalent to the code given below

```python
x = [1, 2, 3, 4, 5]
y = iter(x)
try:
    while True:
        print(next(y))
except StopIteration as e:
    pass

```

```foo
OUTPUT : 

1
2
3
4
5
```



#### What are Coroutines?

***Coroutines are basically functions whose execution can be paused/suspended at a particular point, and then we can resume the execution from that same point later where we left last time and we can resume it whenever we want.***

* So basically we need a mechanism or to be more precise a keyword by which we can insert a checkpoint and say to the program that here we want you to pause the execution of the function and return control to the point it called from. We'll resume the execution whenever we want.
* In python, we can pause the execution of function using the `yield` keyword.



***So here's where things get interesting*** 

* ***We can think of a coroutine as a function which has one or more than one checkpoints where the execution will be paused and control will be returned to the main function.*** 
* ***So basically a coroutine is function divided into many parts and we can execute the each part of a coroutine as we execute each iteration of a for loop using `next` function.***



So here's a basic example



```python
def func():
    print('Function Starts')

    yield

    print('Function Ends')


try:
    y = func()
    print(type(y))
    next(y)  # First part of the function executed
    next(y)  # Second part of the function executed

except StopIteration as e:
    pass
```



```foo
OUTPUT : 

<class 'generator'>
Function Starts
Function Ends
```



So here from the output we notice some things.

* First we need to call the coroutine/function which will give us a generator object.
* That generator object will have the similar behaviour as the iterator but in case of iterator we are traversing over an iterable, in case of generator we're executing parts of the coroutine.
* As in for loop behind the scenes `StopIteration` exception is thrown and caught, same happens in this case when last part of the coroutine is executed.



Now this pausing of function in between is very interesting and has opened up a few possibilities

* When function is paused, WE DO NOTHING which is the case that we just saw.

* Suppose a variable is being modified several times in a function and we want the value of that particular variable at a certain checkpoint,

  then when we pause that function on that particular checkpoint, it returns the value of that variable.

  Let's see it with an example

  ```python
  def func():
      x = 5
      print('Function Part 1')
  
      yield x
      x += 7
      print('Function part 2')
  
      yield x
  
      print('Function part 3')
  
  
  try:
      y = func()
      z = next(y)		# Function part 1 executed
      print(z)
  
      z = next(y)		# Function part 2 executed
      print(z)
  
      z = next(y)		# Function part 3 executed and StopIteration exception raised
      print(z) 	 	# This print will not be executed
  
  except StopIteration as e:
      pass
  
  ```

  ```foo
  OUTPUT : 
  
  Function Part 1
  5
  Function part 2
  12
  Function part 3 
  ```



​		Here value of x is returned by `yield` at different checkpoints as function execution has been paused

> Whenever we are executing the last part of the function and there is no yield left in the function then after executing that last part		`StopIteration` exception will be raised.

> As it happens with for loop and iterators that when an iterator try to execute the next function but no more elements are left in the iterable, it also raises the `StopIteration` exception.



* Suppose we want to send a value(which can be a constant or a variable) at a certain checkpoint i.e at a certain state of a function. 

  We can also do that using `yield` keyword. When we want to send a value, we'll use `send` function instead of `next`.

  Let's see with an example

  ```python
  def func():
      print('Function part 1')
  
      x = yield
      print(x)
      print('Function part 2')
  
      a = yield
      print(a)
      print('Function part 3')
  
  
  try:
  
      y = func()
  
      next(y)			# Function part 1 executed, to reach the first yield we used next
  
      y.send(6)		# Function part 2 executed and value sent 6
  
      y.send(12)		# Function part 2 executed and value sent 12 and StopIteration raised
  
  except StopIteration as e:
      pass
  
  ```

  ```foo
  OUTPUT : 
  
  Function part 1
  6
  Function part 2
  12
  Function part 3
  ```



> The reason we used `next` first before using send is, we can only used `send ` when we are at checkpoint `yield` and `yield ` is on the right side of the expression. So to reach that first `yield` we have to use the `next` function.



Now here comes an interesting application of coroutines. Suppose we want to switch back and forth between two functions like we do in multithreading.

But in multithreading until an interrupt is encountered by the OS it will keep executing but in this case we can switch whenever we want.

Let's see with an example

```python
def func1():
    print('Function 1 part 1')

    yield
    print('Function 1 part 2')

    yield
    print('Function 1 part 3')

    yield
    print('Function 1 part 4')

    yield
    print('Function 1 part 5')


def func2():
    print('Function 2 part 1')

    yield
    print('Function 2 part 2')

    yield
    print('Function 2 part 3')

    yield
    print('Function 2 part 4')

    yield
    print('Function 2 part 5')


try:

    a = func1()
    b = func2()

    next(a)  	# Will execute Function 1 part 1
    next(b)  	# Will execute Function 2 part 1
    next(a)  	# Will execute Function 1 part 2
    next(a)  	# Will execute Function 1 part 3
    next(b)  	# Will execute Function 2 part 2
    next(b)  	# Will execute Function 2 part 3
    next(b)  	# Will execute Function 2 part 4
    next(a)  	# Will execute Function 1 part 4
    next(a)  	# Will execute Function 1 part 5 and raise StopIteration exception

except StopIteration as e:
    pass

```

```foo
OUTPUT : 

Function 1 part 1
Function 2 part 1
Function 1 part 2
Function 1 part 3
Function 2 part 2
Function 2 part 3
Function 2 part 4
Function 1 part 4
Function 1 part 5
```



Here in the above example we can see that we can switch back and forth between coroutines whenever we want.

So if we write our own custom scheduler which handles the switching between multiple coroutines then we can achieve with single threading which we do with multithreading.

Coroutines has many applications such as concurrency and other programming patterns can also be implemented like Producer Consumer or Sender Receiver in network programming which i'll be sharing with you in the upcoming articles. 

Coroutines are also the building blocks of many frameworks such as asyncio, twisted, aiohttp. They can also be chained together to make pipelines and solve problems.













