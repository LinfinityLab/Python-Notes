# Python-Notes
Some notes from book I read


Learning Python - by Mark Lutz
==============================
- Python is dynamic typing, it defines the types during runtime.
  ```python
  number = 3 # it makes varibale number be integer 3
  ```

- Python's variables are just names that can reference objects, difference variables can reference to the same object. 
  ```python
  number = 3 # it makes varibale number be integer 3
  x = number # x is now 3 and it reference to the same object 3 as number reference to
  ```

- Some object types are immutable(integer, float, String, tuple, etc.), some mutable(List, Dictionary, Set, Byte Arrary etc.).
  ```python
  number = 3 # it makes varibale number be integer 3
  
   # it creates a new object integer 4 and assign to number,
   # it won't change object integer 3 and object integer 3 will 
   # be collected by garbage collector
  number = 4
  
  line = 'Hello world'
  line[0] = 'O' # Error raised, because String is immutable
  
  mylist = [1, 2, 3]
  mylist[0] = 5 # now mylist is [5, 2, 3], list is mutable
  
  mytuple = (1, 2, 3)
  mytuple[0] = 5 # Error raised, because tuple is immutable
  ```
## Argument  
- Function's arguments are passed by assignment, so that they are passed by reference or pointer
  ```python
  def change(a, b):
    a = 4
    b[0] = 4
  
  x = 3
  mylist = [1, 2, 3]
  
  # a in the function header will be assigned to x, eg. a = x
  # a in the function body will be the local variable to the scope of the function
  # change a won't change x
  # but b will be change, because b is list which is mutable
  change(x, mylist) 
  ```
  
- Arugment matching syntax

  | Syntax                    | Location        | Interpretation                                                     |
  | ------------------------- |:---------------:|:-------------------------------------------------------------------|
  | `func(value)`             | Caller          |   Normal argument:matched by position                              |
  | `func(name=value)`        | Caller          |   Keyword argument: matched by name                                |
  | `func(*sequence)`         | Caller          |   Pass all objects in sequence as individual positional arguments  |
  | `func(**dict)`            | Caller          |   Pass all key/value pairs in dict as individual keyword arguments |
  | `def func(name)`          | Function        |   Normal argument: matches any passed value by position or name    |
  | `def func(name=value)`    | Function        |   Default argument value, if not passed in the call                |
  | `def func(*name)`         | Function        |   Matches and collects remaining positional arguments in a tuple   |
  | `def func(**name)`        | Function        |   Matches and collects remaining keyword arguments in a dictionary |
  | `def func(*args, name)`   | Function        |   Arguments that must be passed by keyword only in calls (3.0)     |
  | `def func(*, name=value)` |                 |                                                                    |
  example:
  ```python
  def f(a, b, c):
      print(a, b, c)
  
  f(1, 2, 3) # good
  f(a=1, b=2, c=3) # good
  f(b=2, a=1, c=3) # good
  f(1, 2, c=3) # good
  f(b=2, 1, c=3) # error
  ```
