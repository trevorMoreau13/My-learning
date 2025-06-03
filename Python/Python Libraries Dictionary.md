# Python Libraries Dictionary

This section provides an overview of commonly used Python libraries, categorized by their primary use case. It includes descriptions, installation instructions (where applicable), and basic usage examples.

## Standard Library Modules

Python comes with a rich standard library, offering a wide range of modules for various tasks without requiring separate installation. These modules provide functionalities for string operations, data types, file I/O, operating system interaction, networking, and much more.

### `os` Module

**Description:**
The `os` module provides a way of using operating system-dependent functionality like reading or writing to the file system, interacting with environment variables, and managing processes. It allows Python scripts to interact with the underlying operating system in a portable way.

**Installation:**
Part of the Python standard library. No installation is needed.

**Usage Example:**
```python
import os

# Get the current working directory
current_directory = os.getcwd()
print(f"Current Directory: {current_directory}")

# List files and directories in the current directory
print("Files in current directory:")
for item in os.listdir(current_directory):
    print(f"- {item}")

# Create a new directory
directory_name = "my_new_directory"
try:
    os.mkdir(directory_name)
    print(f"Directory 	'{directory_name}	' created.")
except FileExistsError:
    print(f"Directory 	'{directory_name}	' already exists.")

# Get an environment variable
path_variable = os.environ.get(	'PATH	')
print(f"PATH Environment Variable (first 100 chars): {path_variable[:100]}...")

# Check if a path exists
print(f"Does 	'{directory_name}	' exist? {os.path.exists(directory_name)}")

# Join path components (OS-independent)
file_path = os.path.join(current_directory, directory_name, 	'my_file.txt	')
print(f"Constructed file path: {file_path}")

# Clean up the created directory
if os.path.exists(directory_name):
    os.rmdir(directory_name)
    print(f"Directory 	'{directory_name}	' removed.")
```

**Effect:**
The `os` module enables scripts to perform system-level operations such as navigating the file system, creating directories, accessing environment variables, and constructing platform-independent paths. It is essential for scripts that need to interact with the operating system environment.

### `sys` Module

**Description:**
The `sys` module provides access to system-specific parameters and functions. It allows interaction with the Python interpreter itself, such as accessing command-line arguments passed to a script, manipulating the Python path, and exiting the script.

**Installation:**
Part of the Python standard library. No installation is needed.

**Usage Example:**
```python
import sys

# Get command-line arguments
print(f"Script name: {sys.argv[0]}")
if len(sys.argv) > 1:
    print(f"Command-line arguments: {sys.argv[1:]}")
else:
    print("No command-line arguments provided.")

# Get Python version
print(f"Python version: {sys.version}")

# Get platform identifier
print(f"Platform: {sys.platform}")

# Get Python path (where modules are searched)
print("Python Path:")
for path in sys.path:
    print(f"- {path}")

# Exit the script
# print("Exiting script...")
# sys.exit(0) # Exit with status code 0 (success)
```

**Effect:**
The `sys` module provides introspection capabilities into the Python interpreter and its environment, allowing scripts to access command-line arguments, version information, module search paths, and control script termination.

### `datetime` Module

**Description:**
The `datetime` module supplies classes for working with dates and times. It allows for manipulation of dates, times, and time intervals with varying levels of precision. It supports time zone awareness and formatting dates and times into strings.

**Installation:**
Part of the Python standard library. No installation is needed.

**Usage Example:**
```python
import datetime

# Get the current date and time
now = datetime.datetime.now()
print(f"Current date and time: {now}")

# Get the current date
today = datetime.date.today()
print(f"Today	's date: {today}")

# Create a specific date
specific_date = datetime.date(2023, 10, 26)
print(f"Specific date: {specific_date}")

# Create a specific time
specific_time = datetime.time(14, 30, 0)
print(f"Specific time: {specific_time}")

# Create a specific datetime
specific_datetime = datetime.datetime(2023, 10, 26, 14, 30, 0)
print(f"Specific datetime: {specific_datetime}")

# Calculate time difference (timedelta)
yesterday = today - datetime.timedelta(days=1)
print(f"Yesterday	's date: {yesterday}")

# Format datetime object into a string
formatted_now = now.strftime("%Y-%m-%d %H:%M:%S")
print(f"Formatted current time: {formatted_now}")

# Parse a string into a datetime object
date_string = "2023-11-15 09:00:00"
parsed_datetime = datetime.datetime.strptime(date_string, "%Y-%m-%d %H:%M:%S")
print(f"Parsed datetime: {parsed_datetime}")
```

**Effect:**
The `datetime` module provides robust tools for handling date and time information, including creation, manipulation, formatting, and parsing of dates, times, and durations.

### `json` Module

**Description:**
The `json` module provides methods for working with JSON (JavaScript Object Notation) data. It allows you to encode Python objects (like dictionaries and lists) into JSON strings and decode JSON strings back into Python objects. JSON is a common data format for web APIs and configuration files.

**Installation:**
Part of the Python standard library. No installation is needed.

**Usage Example:**
```python
import json

# Python dictionary (similar structure to JSON object)
python_data = {
    "name": "Alice",
    "age": 30,
    "city": "New York",
    "isStudent": False,
    "courses": ["Math", "Physics"]
}

# Encode Python object to JSON string
json_string = json.dumps(python_data, indent=4) # indent for pretty printing
print("JSON String:")
print(json_string)

# Decode JSON string to Python object
json_data_string = 	'{	"name	": 	"Bob	", 	"age	": 25, 	"city	": 	"Los Angeles	"}	'
python_object = json.loads(json_data_string)
print("\nDecoded Python Object:")
print(python_object)
print(f"Name: {python_object[	'name	']}")

# Working with JSON files
file_name = 	'data.json	'

# Write Python data to a JSON file
with open(file_name, 	'w	') as f:
    json.dump(python_data, f, indent=4)
print(f"\nData written to {file_name}")

# Read Python data from a JSON file
with open(file_name, 	'r	') as f:
    loaded_data = json.load(f)
print("\nData loaded from file:")
print(loaded_data)

# Clean up the created file
import os
if os.path.exists(file_name):
    os.remove(file_name)
```

**Effect:**
The `json` module facilitates the serialization and deserialization of data between Python objects and the JSON format, enabling easy data exchange with web services and other systems that use JSON.

### `math` Module

**Description:**
The `math` module provides access to mathematical functions defined by the C standard. It includes functions for basic arithmetic operations, trigonometry, logarithms, exponentiation, and constants like pi and e.

**Installation:**
Part of the Python standard library. No installation is needed.

**Usage Example:**
```python
import math

# Constants
print(f"Pi: {math.pi}")
print(f"Euler	's number (e): {math.e}")

# Basic functions
print(f"Square root of 16: {math.sqrt(16)}")
print(f"5 to the power of 3: {math.pow(5, 3)}")
print(f"Absolute value of -10: {math.fabs(-10)}")

# Trigonometry (angles in radians)
angle_radians = math.pi / 4 # 45 degrees
print(f"Sine of pi/4: {math.sin(angle_radians)}")
print(f"Cosine of pi/4: {math.cos(angle_radians)}")

# Logarithms
print(f"Natural logarithm of e: {math.log(math.e)}")
print(f"Base-10 logarithm of 100: {math.log10(100)}")

# Ceiling and Floor
print(f"Ceiling of 4.3: {math.ceil(4.3)}")
print(f"Floor of 4.8: {math.floor(4.8)}")

# Factorial
print(f"Factorial of 5: {math.factorial(5)}")
```

**Effect:**
The `math` module offers a comprehensive set of mathematical functions and constants, useful for scientific computing, engineering, and any application requiring mathematical calculations beyond basic arithmetic.

### `random` Module

**Description:**
The `random` module implements pseudo-random number generators for various distributions. It allows you to generate random numbers, select random elements from sequences, shuffle sequences, and more.

**Installation:**
Part of the Python standard library. No installation is needed.

**Usage Example:**
```python
import random

# Generate a random float between 0.0 and 1.0
print(f"Random float (0.0-1.0): {random.random()}")

# Generate a random integer within a range (inclusive)
print(f"Random integer (1-10): {random.randint(1, 10)}")

# Generate a random float within a range
print(f"Random float (1.0-10.0): {random.uniform(1.0, 10.0)}")

# Choose a random element from a sequence
my_list = [	'apple	', 	'banana	', 	'cherry	', 	'date	']
print(f"Random choice from list: {random.choice(my_list)}")

# Choose multiple random elements without replacement
print(f"Random sample (2 elements): {random.sample(my_list, 2)}")

# Shuffle a sequence in place
print(f"Original list: {my_list}")
random.shuffle(my_list)
print(f"Shuffled list: {my_list}")

# Generate random numbers from a normal distribution
mu = 0
sigma = 1
print(f"Random number from N(0, 1): {random.normalvariate(mu, sigma)}")
```

**Effect:**
The `random` module provides functions for generating random data, which is useful in simulations, games, statistical sampling, cryptography (though not cryptographically secure by default), and randomized algorithms.

### `re` Module

**Description:**
The `re` module provides support for regular expressions (regex). Regular expressions are powerful sequences of characters that define a search pattern, primarily used for string searching and manipulation. The `re` module allows you to match patterns, search for occurrences, split strings, and perform substitutions based on these patterns.

**Installation:**
Part of the Python standard library. No installation is needed.

**Usage Example:**
```python
import re

text = "The quick brown fox jumps over the lazy dog. The fox is quick."
pattern = r"fox"

# Search for the first occurrence of a pattern
match = re.search(pattern, text)
if match:
    print(f"Found 	'{pattern}	' at index: {match.start()}")
else:
    print(f"	'{pattern}	' not found.")

# Find all occurrences of a pattern
all_matches = re.findall(pattern, text)
print(f"All occurrences of 	'{pattern}	': {all_matches}")

# Split string by a pattern
pattern_split = r"\s+" # Split by whitespace
split_result = re.split(pattern_split, text)
print(f"Split text: {split_result}")

# Substitute occurrences of a pattern
pattern_sub = r"fox"
replacement = "cat"
substituted_text = re.sub(pattern_sub, replacement, text)
print(f"Substituted text: {substituted_text}")

# Compile a pattern for efficiency
compiled_pattern = re.compile(r"\b\w{5}\b") # Find words with exactly 5 letters
five_letter_words = compiled_pattern.findall(text)
print(f"Five-letter words: {five_letter_words}")

# Match at the beginning of the string
pattern_match = r"The"
match_start = re.match(pattern_match, text)
if match_start:
    print(f"String starts with 	'{pattern_match}	'")
```

**Effect:**
The `re` module provides comprehensive tools for pattern matching in strings using regular expressions, enabling complex text searching, validation, and manipulation tasks.


