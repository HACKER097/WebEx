# Question 1

double underscored variables in python, which are called dunders for short are used to take control over a python operator

```python
class fakeInt:
    def __init__(self, x):
        self.x = x

    def __int__(self):
        return self.x

    def __add__(self, other):
        return fakeInt(1337)
    
    def __str__(self):
        return "VXYgQ2VuZnVuYWc"

a = fakeInt(1)
b = fakeInt(2)
print(int(a + b))
print(a + b)
```

gives

```
1337
VXYgQ2VuZnVuYWc
```

Here I am using `__add__` to modify the `+` operator the return nonsense, and even more nonsense when someone tries to print it to debug. But the use of dunders is not limited to evil. Its can be useful when you are making a library and a lot of operations might need to be done to the objects your library provides. Its more convinient to have `f1 + f2` than having `add(f1, f2)`

But not all of them are intended to be used like this, for eg `__import__`

# Question 2

Here is my top 10 list of dunders:

## 1. `__init__`

Init is probably the most used python dunder, its used to initialise new instances of our class. I am not giving an example for this because Its already been used in the previous example, and will come up later.

## 2. `__name__`

Probably the second most used dunder. It tells us the name of the script/module being run. Except it returns `__main__` when the script is run directly. Usage example:

```python
if __name__ == "__main__":
    print("Ran directly")
else:
    print("Ran via import")
```

## 3. `__str__`

Defines the output of the `str()` function, which converts other objects to strings. But its #3 because it also changes the way a object is printed, because `print()` uses it internally. Example already given.

## 4. `__call__`

Very useful, allows us to call our objects like functions. 

```python
class Text:
  def __init__(self, username):
    self,username = username

  def __call__(self, text):
    print(self.username, ": ", text, sep="")

a = Text("USER A")
b = Text("USER B")
a("eGaSsEmSdRoWKcAb")
b("what?")
```

Gives

```
USER A: eGaSsEmSdRoWKcAb
USER B: what?
```

## 5. `__doc__`

Gives doccumentsation string for given class. Useful when you don't have access to docs for some reason.

`print(str.__doc__)`

Gives 

```
str(object='') -> str
str(bytes_or_buffer[, encoding[, errors]]) -> str

Create a new string object from the given object. If encoding or
errors is specified, then the object must expose a data buffer
that will be decoded using the given encoding and error handler.
Otherwise, returns the result of object.__str__() (if defined)
or repr(object).
encoding defaults to sys.getdefaultencoding().
errors defaults to 'strict'.
```

## 6. `__slots__`

Limits what attributes an object can have. Basically locks an object so no one adds extra attributes to it.

```python
class TaskManager:
    __slots__ = ['task_name', 'priority', 'due_date', 'status', 'assignee']

    def __init__(self, task_name, priority, due_date, status='pending', assignee=None):
        self.task_name = task_name
        self.priority = priority
        self.due_date = due_date
        self.status = status
        self.assignee = assignee

# Create a task
task = TaskManager("Make top 10 list", "High", "2024-05-22")
task.timeLeft = 1000 # gives error 
```

## 7. `__import__`

Used by the `import` keyword, but we can import dirctly using it.

```python
m = __import__("math")
print(m.pi)
```

Gives

`3.141592653589793`

## 8. `__repr__`

Gives the string representation of the object. External libraries ue it often to help with debugging, the string representation helps us recreate the object and see exactly what it looks like.

```python
class Car:
    def __init__(self, brand, model, year):
        self.brand = brand
        self.model = model
        self.year = year

    def __repr__(self):
        return f"Car(brand='{self.brand}', model='{self.model}', year={self.year})"

my_car = Car("Porche", "911", 2001)

print(repr(my_car))
```

Gives

```
Car(brand='Porche', model='911', year=2001)
```

## 9. `__iter__`
## 10. `__next__`

Used for to change functioning of a for loop. I think `range()` function also used this dunder to iterate without storing a huge list.

```python
class Countdown:
    def __init__(self, start):
        self.start = start

    def __iter__(self):
        self.current = 0
        return self

    def __next__(self):
        if self.current <= self.start:
            current = self.current
            self.current += 1
            return current
        else:
            raise StopIteration

countdown = Countdown(5)

for number in countdown:
    print(number)
```

Gives

```
0
1
2
3
4
5
```



