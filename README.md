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

- Arbitrary argument examples
  In function , `*` builds a collection of arguments. In caller, `*` it unpacks a collection of arguments.
  ```python
  def f(*args):
    print(args)
    
  f() # output is '()'
  f(1) # output is '(1,)'
  f(1, 2, 3, 4) # output is '(1, 2, 3, 4)'
  
  args = (1, 2)
  f(*args) # output is '1, 2, 3, 4'
  ```
  `**` is similar to `*`, except it works for dictionary
  
- Python 3.0 Keyword-Only Arguments

  they make it easier to allow a function to accept both any number of positional arguments to be processed, and con- figuration options passed as keywords.
  ```pythonh
  def f(a, *b, c):
    print(a, b, c)
  
  f(1, 2, c=3) # output is '1 (2,) 3'
  f(a=1, c=3) # output is '1 () 3'
  f(1, 2, 3) # Error! TypeError: kwonly() needs keyword-only argument c
  ```
  `c` must be passsed by keyword only
  
  **Note:** keyword-only argument must be specified after `*args` and before `**kwargs`
  
  ```python
  def f(a, *b, **d, c=6):
    print(a, b, c, d)  # error! SyntaxError: invalid syntax, keyword-only before **
    
  def f(a, *b, c=6, **d):
    print(a, b, c, d) # works!
  ````
## Mixin
- In Python the class hierarchy is defined right to left.
  ```python
  # Incorrect example
  class Mixin1(object):
    def test(self):
        print "Mixin1"

  class Mixin2(object):
    def test(self):
        print "Mixin2"

  class MyClass(BaseClass, Mixin1, Mixin2):
    pass
  ```
  In this case `Mixin2` class is the base class, extended by `Mixin1` and finally by BaseClass. This is usually fine because many times the mixin class don't override each other's, or the base class' methods. But if you do override methods or properties in your mixins this can lead to unexpected results because the priority of how methods are resolved is from left to right.
  ```python
  object = MyClass()
  obj.test() # output will be Mixin1
  ```
  
  The correct way to use mixins is like in the reverse order:
  ```python
  class MyClass(Mixin2, Mixin1, BaseClass):
    pass
  ```
  This kind of looks counter-intuitive at first because most people would read a top-down class hierarchy from left to right but if you include the class you are defining, you can read correctly up the class hierarchy (MyClass => Mixin2 => Mixin1 => BaseClass. If you define your classes this way you won't have to many conflicts and run into too many bugs.
  ```python
  obj = MyClass()
  obj.test() # Output will be Mixin2
  ```
## Iterations and Comprehensions
**Note**: In python 3.0, it is named `.__next__()`, in python 2.6, it is named `.next()`. There is a bult-in function `next()` which is available in both 3.0 and 2.6. It calls `.__next__()` in python 3.0 and `.next()` in python 2.6.

![alt text](https://github.com/dongliang3571/Python-Notes/blob/master/screenshot/iteration.png?raw=true "itertor")

- **Iterator**, has a method named `__next__`, it advances iterator to next item
    ```python
    f = open('script1.py')
    f.__next__() # output is 'import sys\n'
    ```
- Iterable, such as List, Dictonary need to call `iter()` to obtain iterator
  ```python
  myList = [1, 2, 3]
  a = iter(myList)
  ```
- File Iterators, `open()` method opens file and make itertor itself, `.readline()` will read next line from a file each    time it is called. Will return empty string `''` when it's end of the line. `.readline()` actually calls `.__next__()`,   only difference is that `.readline()` won't raise exception, it returns empty string instead.
  
    ```python
    f = open('script.py') # read a 4-line script file in this directory
    
    #.readline() loads one line on each call
    f.readline() # output 'import sys\n'
    f.readline() # output 'print(sys.path)\n'
    f.readline() # output 'x = 2\n'
    f.readline() # output 'print(2 ** 33)\n'
    f.readline() # output ''
    
    f = open('script.py') # read a 4-line script file in this directory
    f.__next__() # output 'import sys\n'
    f.__next__() # output 'print(sys.path)\n'
    f.__next__() # output 'x = 2\n'
    f.__next__() # output 'print(2 ** 33)\n'
    f.__next__() # Error! output 'Traceback (most recent call last): StopIteration'
    ```
- List Comprehensions(It's faster)
  ```python
  # regular way create list
  L = [1, 2, 3, 4, 5]
  for i in range(len(L)):
    L[i] += 10
  print(L) # output is '[11, 12, 13, 14, 15]'
  
  # List comprehensions
  L = [1, 2, 3, 4, 5]
  L = [x + 10 for x in L]
  print(L) # output is '[11, 12, 13, 14, 15]'
  ```
  
  
## Read File
- read file using `.readlines()`, it will loads the file into a list of line strings all at once

  ```python
  f = open('script1.py')
  lines = f.readlines()
  print(lines) # output is ['import sys\n', 'print(sys.path)\n', 'x = 2\n', 'print(2 ** 33)\n']
  ```
- read file using `.readline()`, it will loads the file into one single string with new lines.

  ```python
  f = open('script1.py')
  lines = f.readline()
  print(lines) # output is 'import sys\nprint(sys.path)\nx = 2\nprint(2 ** 33)\n'
  ```
