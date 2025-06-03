# Python Syntax Dictionary

This section provides a comprehensive reference for Python syntax, covering fundamental elements, control structures, functions, classes, modules, exception handling, and file operations. Each topic includes explanations and illustrative code examples.

## Basic Syntax Elements

### Variables and Assignment
In Python, variables are created when you assign a value to them using the assignment operator (`=`). Python is dynamically typed, meaning you don't need to declare the type of a variable explicitly. The type is inferred from the value assigned.

**Example:**
```python
# Variable assignment
message = "Hello, Python!"
count = 10
price = 99.95
is_active = True

# Printing variables
print(message)  # Output: Hello, Python!
print(count)    # Output: 10
print(price)    # Output: 99.95
print(is_active) # Output: True

# Variables can change type
count = "ten" # Now count is a string
print(count)    # Output: ten
```

**Effect:** Variables store data that can be referenced and manipulated throughout a program. Dynamic typing offers flexibility but requires careful management to avoid type errors.

### Data Types
Python has several built-in data types. Key types include:
- **Numeric:** `int` (integers), `float` (floating-point numbers), `complex` (complex numbers).
- **Sequence:** `str` (strings), `list` (ordered, mutable sequences), `tuple` (ordered, immutable sequences).
- **Mapping:** `dict` (unordered key-value pairs).
- **Set:** `set` (unordered, unique elements, mutable), `frozenset` (immutable).
- **Boolean:** `bool` (True or False).
- **NoneType:** `None` (represents the absence of a value).

**Example:**
```python
# Numeric types
num_int = 100
num_float = 3.14
num_complex = 2 + 3j

# Sequence types
my_string = "Python is fun"
my_list = [1, "apple", 3.5]
my_tuple = (10, 20, 30)

# Mapping type
my_dict = {"name": "Alice", "age": 30}

# Set types
my_set = {1, 2, 3, 3, 2}
my_frozenset = frozenset(["a", "b", "c"])

# Boolean type
is_valid = False

# None type
result = None

print(type(num_int))       # Output: <class 'int'>
print(type(my_string))     # Output: <class 'str'>
print(type(my_list))       # Output: <class 'list'>
print(type(my_dict))       # Output: <class 'dict'>
print(type(my_set))        # Output: <class 'set'>
print(my_set)              # Output: {1, 2, 3}
print(type(is_valid))      # Output: <class 'bool'>
print(type(result))        # Output: <class 'NoneType'>
```

**Effect:** Data types determine the kind of values a variable can hold and the operations that can be performed on it. Understanding data types is crucial for writing correct and efficient code.

### Operators
Python supports various operators for performing operations on values:
- **Arithmetic:** `+`, `-`, `*`, `/`, `%` (modulo), `**` (exponentiation), `//` (floor division).
- **Comparison:** `==`, `!=`, `>`, `<`, `>=`, `<=`.
- **Logical:** `and`, `or`, `not`.
- **Assignment:** `=`, `+=`, `-=`, `*=`, `/=`, etc.
- **Identity:** `is`, `is not` (check if two variables refer to the same object).
- **Membership:** `in`, `not in` (check if a value is present in a sequence).
- **Bitwise:** `&`, `|`, `^`, `~`, `<<`, `>>` (operate on integers as binary numbers).

**Example:**
```python
# Arithmetic operators
a = 10
b = 3
print(f"a + b = {a + b}")      # Output: 13
print(f"a / b = {a / b}")      # Output: 3.333...
print(f"a // b = {a // b}")     # Output: 3
print(f"a ** b = {a ** b}")    # Output: 1000

# Comparison operators
print(f"a == b: {a == b}")     # Output: False
print(f"a > b: {a > b}")       # Output: True

# Logical operators
x = True
y = False
print(f"x and y: {x and y}")   # Output: False
print(f"x or y: {x or y}")    # Output: True
print(f"not x: {not x}")       # Output: False

# Membership operators
my_list = [1, 2, 3]
print(f"2 in my_list: {2 in my_list}") # Output: True
print(f"4 not in my_list: {4 not in my_list}") # Output: True
```

**Effect:** Operators allow you to perform calculations, comparisons, logical evaluations, and other manipulations on data.

### Comments
Comments are used to explain code and are ignored by the Python interpreter. Single-line comments start with `#`. Multi-line comments or docstrings are enclosed in triple quotes (`'''` or `"""`).

**Example:**
```python
# This is a single-line comment

'''
This is a multi-line comment (docstring).
It can span multiple lines.
'''

def my_function():
    """This is a docstring explaining the function."""
    pass # pass is a null operation - nothing happens when it executes

variable = 10 # This comment explains the variable
```

**Effect:** Comments improve code readability and maintainability by providing explanations and context.

### Indentation
Python uses indentation (whitespace at the beginning of a line) to define code blocks, such as loops, conditional statements, functions, and classes. The standard is four spaces per indentation level. Inconsistent indentation will cause errors.

**Example:**
```python
def greet(name):
    if name: # Start of if block (indentation level 1)
        print(f"Hello, {name}!") # Inside if block
        print("Welcome!") # Inside if block
    else: # Start of else block (indentation level 1)
        print("Hello there!") # Inside else block
    print("End of greeting.") # Back to indentation level 0

greet("Alice")
greet(None)
```

**Effect:** Indentation enforces a clean and consistent code structure, making Python code highly readable. It is a fundamental part of Python's syntax.

## Control Structures

### Conditional Statements (`if`, `elif`, `else`)
Conditional statements allow code execution to depend on whether certain conditions are true or false.

**Syntax:**
```python
if condition1:
    # Code block to execute if condition1 is True
elif condition2:
    # Code block to execute if condition1 is False and condition2 is True
else:
    # Code block to execute if all preceding conditions are False
```

**Example:**
```python
score = 75

if score >= 90:
    grade = "A"
elif score >= 80:
    grade = "B"
elif score >= 70:
    grade = "C"
else:
    grade = "D"

print(f"Your grade is: {grade}") # Output: Your grade is: C
```

**Effect:** `if`, `elif`, and `else` control the flow of execution based on evaluating conditions, allowing programs to make decisions.

### `for` Loops
`for` loops iterate over a sequence (like a list, tuple, string, or range) or other iterable object, executing a block of code for each item in the sequence.

**Syntax:**
```python
for item in iterable:
    # Code block to execute for each item
```

**Example:**
```python
# Iterating over a list
fruits = ["apple", "banana", "cherry"]
for fruit in fruits:
    print(fruit)

# Iterating over a range of numbers
for i in range(5): # range(5) generates numbers 0, 1, 2, 3, 4
    print(f"Number: {i}")

# Iterating over a string
for char in "Python":
    print(char)

# Iterating over dictionary keys
my_dict = {"a": 1, "b": 2}
for key in my_dict:
    print(f"Key: {key}, Value: {my_dict[key]}")
```

**Effect:** `for` loops provide a concise way to repeat a block of code for each element in a collection.

### `while` Loops
`while` loops execute a block of code as long as a specified condition remains true.

**Syntax:**
```python
while condition:
    # Code block to execute as long as condition is True
```

**Example:**
```python
count = 0
while count < 5:
    print(f"Count is: {count}")
    count += 1 # Increment count

print("Loop finished.")
```

**Effect:** `while` loops are used when the number of iterations is not known beforehand and depends on a condition being met.

### `break` and `continue`
- `break`: Exits the innermost enclosing `for` or `while` loop immediately.
- `continue`: Skips the rest of the current iteration and proceeds to the next iteration of the loop.

**Example:**
```python
# Using break
print("\nUsing break:")
for i in range(10):
    if i == 5:
        print("Breaking loop at i=5")
        break
    print(i)

# Using continue
print("\nUsing continue:")
for i in range(10):
    if i % 2 == 0: # If i is even
        continue # Skip the rest of the iteration
    print(f"Odd number: {i}")
```

**Effect:** `break` and `continue` provide finer control over loop execution, allowing early termination or skipping specific iterations based on conditions.

## Functions and Methods

### Defining Functions
Functions are blocks of reusable code defined using the `def` keyword. They can accept input parameters and return output values.

**Syntax:**
```python
def function_name(parameter1, parameter2, ...):
    """Optional docstring explaining the function."""
    # Code block (function body)
    # ...
    return value # Optional return statement
```

**Example:**
```python
def add_numbers(x, y):
    """Returns the sum of two numbers."""
    result = x + y
    return result

def greet(name="Guest"):
    """Greets the user. Uses a default parameter value."""
    print(f"Hello, {name}!")

# Calling functions
sum_result = add_numbers(5, 3)
print(f"Sum: {sum_result}") # Output: Sum: 8

greet("Bob") # Output: Hello, Bob!
greet()      # Output: Hello, Guest!
```

**Effect:** Functions promote code organization, reusability, and modularity.

### Parameters and Arguments
- **Parameters:** Variables listed inside the parentheses in the function definition.
- **Arguments:** Values passed to the function when it is called.
- **Positional Arguments:** Passed in the order parameters are defined.
- **Keyword Arguments:** Passed using `parameter_name=value` syntax, order doesn't matter.
- **Default Arguments:** Parameters with default values specified in the definition.
- **Variable-Length Arguments:** `*args` (for non-keyword arguments) and `**kwargs` (for keyword arguments) allow a function to accept an arbitrary number of arguments.

**Example:**
```python
def describe_pet(pet_name, animal_type="dog"):
    """Displays information about a pet."""
    print(f"I have a {animal_type}.")
    print(f"My {animal_type}	's name is {pet_name.title()}.")

describe_pet("willie") # Positional argument, default for animal_type
describe_pet(pet_name="harry", animal_type="hamster") # Keyword arguments
describe_pet(animal_type="cat", pet_name="whiskers") # Order doesn't matter with keywords

def make_pizza(size, *toppings):
    """Summarize the pizza we are about to make."""
    print(f"\nMaking a {size}-inch pizza with the following toppings:")
    for topping in toppings:
        print(f"- {topping}")

make_pizza(16, "pepperoni")
make_pizza(12, "mushrooms", "green peppers", "extra cheese")

def build_profile(first, last, **user_info):
    """Build a dictionary containing everything we know about a user."""
    profile = {"first_name": first, "last_name": last}
    for key, value in user_info.items():
        profile[key] = value
    return profile

user_profile = build_profile("albert", "einstein",
                             location="princeton",
                             field="physics")
print(user_profile)
```

**Effect:** Different ways of passing arguments provide flexibility in how functions are called and used.

### Return Values
Functions use the `return` statement to send a value back to the caller. If `return` is omitted or used without a value, the function returns `None`.

**Example:**
```python
def square(number):
    return number * number

result = square(4)
print(result) # Output: 16

def check_even(number):
    if number % 2 == 0:
        return True
    # Implicitly returns None if number is odd

print(check_even(10)) # Output: True
print(check_even(7))  # Output: None
```

**Effect:** `return` allows functions to produce results that can be used elsewhere in the program.

### Scope
Scope refers to the region of a program where a variable is accessible.
- **Local Scope:** Variables defined inside a function are local to that function.
- **Enclosing Function Locals:** Variables in the local scope of enclosing functions (for nested functions).
- **Global Scope:** Variables defined outside any function.
- **Built-in Scope:** Names pre-assigned in Python (e.g., `print`, `len`).
Python follows the LEGB rule (Local, Enclosing, Global, Built-in) to resolve variable names.

**Example:**
```python
global_var = "I am global"

def outer_function():
    enclosing_var = "I am enclosing"
    
    def inner_function():
        local_var = "I am local"
        print(local_var)      # Access local
        print(enclosing_var)  # Access enclosing
        print(global_var)     # Access global
        
    inner_function()

outer_function()
# print(local_var) # This would cause an error (NameError)
```

**Effect:** Scope rules determine variable visibility and lifetime, preventing naming conflicts and managing program state.

## Classes and Objects (Object-Oriented Programming)

### Defining Classes
Classes are blueprints for creating objects. They are defined using the `class` keyword.

**Syntax:**
```python
class ClassName:
    """Optional docstring explaining the class."""
    
    # Class variable (shared among all instances)
    class_attribute = value
    
    # Constructor method (initializes instance)
    def __init__(self, parameter1, ...):
        # Instance variables (unique to each instance)
        self.instance_attribute1 = parameter1
        # ...
        
    # Instance method
    def method_name(self, parameter1, ...):
        # Method body (operates on instance data)
        # ...
        return value
```

**Example:**
```python
class Dog:
    """A simple attempt to model a dog."""
    
    # Class variable
    species = "Canis familiaris"
    
    def __init__(self, name, age):
        """Initialize name and age attributes."""
        self.name = name
        self.age = age
        
    def sit(self):
        """Simulate a dog sitting in response to a command."""
        print(f"{self.name} is now sitting.")
        
    def roll_over(self):
        """Simulate rolling over in response to a command."""
        print(f"{self.name} rolled over!")
```

**Effect:** Classes define the structure (attributes) and behavior (methods) of objects.

### Instantiation (Creating Objects)
Objects are created (instantiated) by calling the class name as if it were a function.

**Example:**
```python
my_dog = Dog("Willie", 6)
your_dog = Dog("Lucy", 3)

# Accessing attributes
print(f"My dog's name is {my_dog.name}.")
print(f"My dog is {my_dog.age} years old.")
print(f"My dog belongs to the species {my_dog.species}.") # Accessing class variable

# Calling methods
my_dog.sit()
your_dog.roll_over()
```

**Effect:** Instantiation creates individual objects based on the class blueprint, each with its own state (instance variables).

### Inheritance
Inheritance allows a class (child class) to inherit attributes and methods from another class (parent class).

**Syntax:**
```python
class ChildClass(ParentClass):
    """Inherits from ParentClass."""
    
    def __init__(self, parent_param1, ..., child_param1, ...):
        super().__init__(parent_param1, ...) # Call parent constructor
        # Initialize child-specific attributes
        self.child_attribute = child_param1
        
    # Override parent method
    def parent_method(self, ...):
        # Optional: Call parent method
        # super().parent_method(...)
        # Add child-specific behavior
        pass
        
    # Add new child-specific methods
    def child_method(self, ...):
        pass
```

**Example:**
```python
class ElectricCar(Dog): # Incorrect inheritance, just for syntax example
    """Represents aspects of a car, specific to electric vehicles."""
    
    def __init__(self, name, age, battery_size=75):
        """Initialize attributes of the parent class and battery."""
        super().__init__(name, age) # Initialize Dog attributes
        self.battery_size = battery_size
        
    def describe_battery(self):
        """Print a statement describing the battery size."""
        print(f"This car has a {self.battery_size}-kWh battery.")
        
    # Override a method (though nonsensical here)
    def sit(self):
        print("Electric cars cannot sit!")

my_tesla = ElectricCar("Tesla Model S", 2, 100)
print(my_tesla.name) # Inherited attribute
my_tesla.sit() # Overridden method
my_tesla.describe_battery() # Child-specific method
```

**Effect:** Inheritance promotes code reuse and allows for creating specialized classes based on existing ones.

## Modules and Packages

### Importing Modules
Modules are Python files (`.py`) containing code. Packages are collections of modules. You use the `import` statement to use code from other modules.

**Syntax:**
```python
import module_name
import module_name as alias
from module_name import specific_function, specific_class
from module_name import specific_function as func_alias
from module_name import *
```

**Example:**
```python
# Import the entire math module
import math
print(math.sqrt(16)) # Access using module name

# Import with an alias
import math as m
print(m.pi)

# Import specific functions
from math import sqrt, pi
print(sqrt(25))
print(pi)

# Import specific function with an alias
from math import factorial as fact
print(fact(5))

# Import all names (generally discouraged)
# from math import *
# print(cos(0))
```

**Effect:** Importing allows you to use code defined in other files, organizing large projects and leveraging existing libraries.

### Creating Modules
Any Python file can be a module. Simply save your code in a `.py` file, and you can import it into other scripts located in the same directory or in directories listed in Python's `sys.path`.

**Example:**
```python
# File: my_module.py
def greet_module(name):
    print(f"Hello from my_module, {name}!")

PI = 3.14159

# File: main_script.py
import my_module

my_module.greet_module("World")
print(f"Value of PI from module: {my_module.PI}")
```

**Effect:** Creating modules allows you to structure your code logically and reuse components across different parts of your application.

## Exception Handling

### `try`, `except`, `else`, `finally`
Exception handling allows you to gracefully manage errors that occur during program execution.

**Syntax:**
```python
try:
    # Code block where exceptions might occur
except SpecificExceptionType as e:
    # Code block to handle SpecificExceptionType
    # 'e' holds the exception object
except AnotherExceptionType:
    # Code block to handle AnotherExceptionType
except Exception as e: # Catch any other exception
    # Handle other exceptions
else:
    # Code block to execute if no exceptions occurred in the try block
finally:
    # Code block that always executes, regardless of exceptions
    # Often used for cleanup (e.g., closing files)
```

**Example:**
```python
try:
    numerator = 10
    denominator = int(input("Enter a denominator: "))
    result = numerator / denominator
except ZeroDivisionError:
    print("Error: Cannot divide by zero.")
except ValueError:
    print("Error: Please enter a valid integer.")
except Exception as e:
    print(f"An unexpected error occurred: {e}")
else:
    print(f"The result is: {result}")
finally:
    print("Execution finished.")
```

**Effect:** Exception handling prevents program crashes due to errors and allows for specific error recovery or reporting.

### Raising Exceptions
You can raise exceptions deliberately using the `raise` statement.

**Syntax:**
```python
raise ExceptionType("Optional error message")
```

**Example:**
```python
def calculate_sqrt(number):
    if number < 0:
        raise ValueError("Cannot calculate square root of a negative number.")
    return number ** 0.5

try:
    print(calculate_sqrt(9))
    print(calculate_sqrt(-4))
except ValueError as e:
    print(f"Error: {e}")
```

**Effect:** `raise` allows you to signal errors or exceptional conditions within your own code.

## File Operations

### Opening and Closing Files
Use the `open()` function to open files and the `close()` method to close them. The `with` statement is the preferred way as it automatically handles closing the file.

**Syntax:**
```python
# Using with (recommended)
with open("filename.txt", mode="r") as file_object:
    # Perform operations on file_object
# File is automatically closed here

# Manual open/close (less safe)
file_object = open("filename.txt", mode="r")
try:
    # Perform operations
finally:
    file_object.close()
```
**Modes:**
- `'r'`: Read (default).
- `'w'`: Write (truncates file or creates new).
- `'a'`: Append (adds to end or creates new).
- `'b'`: Binary mode (e.g., `'rb'`, `'wb'`).
- `'+'`: Update (reading and writing, e.g., `'r+'`, `'w+'`).

### Reading Files
Methods like `read()`, `readline()`, and `readlines()` are used to read content.

**Example:**
```python
# Create a dummy file
with open("sample.txt", "w") as f:
    f.write("First line.\n")
    f.write("Second line.\n")
    f.write("Third line.\n")

# Read the entire file
with open("sample.txt", "r") as f:
    content = f.read()
    print("\nReading entire file:")
    print(content)

# Read line by line
with open("sample.txt", "r") as f:
    print("\nReading line by line:")
    for line in f:
        print(line.strip()) # strip() removes leading/trailing whitespace

# Read all lines into a list
with open("sample.txt", "r") as f:
    lines = f.readlines()
    print("\nReading all lines into list:")
    print(lines)

# Clean up dummy file
import os
os.remove("sample.txt")
```

### Writing Files
Use the `write()` method to write strings to a file opened in write (`'w'`) or append (`'a'`) mode.

**Example:**
```python
# Writing to a new file (overwrites if exists)
with open("output.txt", "w") as f:
    f.write("This is the first line written.\n")
    f.write("This is the second line.\n")

# Appending to a file
with open("output.txt", "a") as f:
    f.write("This line is appended.\n")

# Verify content
with open("output.txt", "r") as f:
    print("\nContent of output.txt:")
    print(f.read())

# Clean up output file
import os
os.remove("output.txt")
```

**Effect:** File operations allow programs to read data from and write data to persistent storage.
