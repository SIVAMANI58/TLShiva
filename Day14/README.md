# Error Handling, Logging & Writing
---
#### Error:
 - An error is a problem in the code or during execution that prevents the program from working properly.
---
##### Types:

###### Syntax Error:
 - Occurs when Python code breaks the grammar rules of the language. It is caught before the program runs.

print("Hello"

 - Missing bracket → SyntaxError.

###### Indentation Error:
 - Python requires proper indentation (spaces/tabs).If indentation is missing or incorrect error.

def greet():
print("Hello")

 - Code not indented → IndentationError.

###### Name Error:
 - Occurs when you use a variable/function that is not defined.

print(age)

 - age is not defined → NameError.

###### Attribute Error:
 - Happens when you try to use an attribute or function that the object does not have.

x = 10
x.append(5)

 - Integers don’t have .append() → AttributeError.

###### FileNotFoundError:
 - Occurs when a program tries to open a file that does not exist.

open("data.txt")

 - File missing → FileNotFoundError.

###### Index Error:
 - Occurs when you try to access an index that is out of range.

items = [10, 20, 30]
print(items[5])

 - Index 5 doesn't exist → IndexError.

###### Import Error:
 - Python cannot find the module you are trying to import.

import unknown_module

 - Module does not exist → ImportError.
---
##### Error:
 - Problem in the program that usually cannot be handled.
 - Incorrect code (syntax/structure).
 - Before program runs (compile-time).
##### Exception:
 - Problem that occurs during runtime and can be handled.
 - Unexpected events during execution.
 - While the program is running (runtime).
---
#### Exception Handling
 - Exception handling allows your program to run safely even when an error occurs.
 - (try, except, else, finally) Main keywords.

##### try:
 - The block where you write the code that may cause an error.

try:
    x = 10 / 0


##### except:
 - This block catches the error and prevents the program from crashing.

try:
    x = 10 / 0
except ZeroDivisionError:
    print("Cannot divide by zero!")


##### else:
 - Runs ONLY if no exception occurs.

try:
    num = int(input("Enter a number: "))
except ValueError:
    print("Invalid input")
else:
    print("You entered:", num)


##### finally:
 - Runs always, whether an exception occurred or not. Used for cleanup.

try:
    f = open("data.txt")
    print(f.read())
except FileNotFoundError:
    print("File not found!")
finally:
    print("Execution finished.")

---

#### Logging

 - Logging helps you record what your program is doing useful for debugging, monitoring, and catching errors.
- Create a Logger: This is done automatically when you call logging.info(), logging.error().
- Set Logging Level: You decide which messages should show ( DEBUG, INFO, WARNING, ERROR, CRITICAL ).
- Formatter: This decides how the output looks(time, level, message, etc.).
- Handler: Where the message Console (terminal) File (log file) Network, etc.


##### Logging to a File:
 - You can save all logs into a file,

import logging

logging.basicConfig(
    filename="app.log",              # file name
    level=logging.DEBUG,             # log everything
    format="%(asctime)s - %(levelname)s - %(message)s"
)

logging.info("Application started")
logging.error("An error occurred")

 - This will create a file app.log with logs

2025-02-01 18:00:22,101 - INFO - Application started
2025-02-01 18:00:22,102 - ERROR - An error occurred

---
#### Task

##### Create a Resilient Network/API Requests Script


###### Objective:
 - Learn how to build scripts that gracefully handle common failures: Host unreachable, Request timeout, Invalid or non-JSON responses, Status-code-based failures.


import requests

def fetch_data(url):
    try:
        response = requests.get(url, timeout=5)   # API call
        
        # Check status code
        if response.status_code != 200:
            print("Status Code Error:", response.status_code)
            return None
  
        # Try to parse JSON
        try:
            return response.json()
        except:
            print("Invalid JSON Response")
            return None

    except requests.exceptions.Timeout:
        print("Request Timeout")

    except requests.exceptions.ConnectionError:
        print("Host Unreachable")

    except Exception as e:
        print("Other Error:", e)

    return None


# Testing
good = "https://jso nplaceholder.typicode.com/todos/1"
bad = "https://wrongdomain1234567.com/data"
not_json = "https://www.google.com"

print("Good URL:", fetch_data(good))
print("Bad URL:", fetch_data(bad))
print("Invalid JSON:", fetch_data(not_json))

###### Summary:
 - This script safely fetches API data using requests and handles all common errors like timeout, bad domain, invalid JSON, and non-200 status codes. It ensures the program never crashes and always returns a clean, safe output.
---
##### Create a exception class for APIRESPONSE ERROR

###### Objective:
 - Define a custom exception (APIResponseError) to handle invalid API results in a structured way.

import os
import logging
import requests

# Create 'logs' folder if it doesn't exist
os.makedirs('logs', exist_ok=True)

# Setup logging to file
logging.basicConfig(filename='logs/log.txt', level=logging.WARNING)

# Custom Exception for API Response Error
class APIResponseError(Exception):
    """Custom exception for invalid API response."""
    def _init_(self, message, status_code=None):
        super()._init_(message)
        self.status_code = status_code

# Network function to fetch API data
def fetch_data(url):
    try:
        response = requests.get(url)
        if response.status_code != 200:
            raise APIResponseError(f'Invalid response: {response.status_code}', response.status_code)
        return response.json()
    except requests.exceptions.RequestException as e:
        raise APIResponseError(f'Network error: {e}') from e

# Test with a good URL
try:
    data = fetch_data('https://jsonplaceholder.typicode.com/posts/1')
    print('Success:', data)
except APIResponseError as e:
    logging.error(f'APIResponseError: {e} (Status code: {getattr(e, "status_code", None)})')
    print(f'APIResponseError: {e} (Status code: {getattr(e, "status_code", None)})')

# Test with a bad URL
try:
    data = fetch_data('https://jsonplaceholder.typicode.com/badendpoint')
    print('Success:', data)
except APIResponseError as e:
    logging.error(f'APIResponseError: {e} (Status code: {getattr(e, "status_code", None)})')
    print(f'APIResponseError: {e} (Status code: {getattr(e, "status_code", None)})')

###### Summary:
 - This code creates a custom exception (APIResponseError) to handle API failures and logs any invalid responses or network errors. It ensures API issues are caught cleanly instead of crashing the program.
---
##### Configure logging for Scripts

###### Objective:
 -  Configure application-level logging with: File logging, Timestamp, Log levels, Consistent format, Info/Warning/Error entries.

import logging
import os


os.makedirs("logs", exist_ok=True)

logging.basicConfig(
    filename="logs/log.txt",
    level=logging.INFO,
    format="%(asctime)s - %(levelname)s - %(message)s"
)

def calculator(a, b, op):
    if op == "add":
        logging.info("Performed addition")
        return a + b
    elif op == "sub":
        logging.info("Performed subtraction")
        return a - b
    elif op == "mul":
        logging.info("Performed multiplication")
        return a * b
    elif op == "div":
        if b == 0:
            logging.error("Attempted division by zero")
            return "Cannot divide by zero"
        logging.info("Performed division")
        return a / b
    else:
        logging.warning("Invalid operation entered")
        return "Invalid operation"


a = int(input("Enter first number: "))
b = int(input("Enter second number: "))
op = input("Enter operation (add/sub/mul/div): ").lower()

result = calculator(a, b, op)
print("Result:", result)


print("\n--- Log File Contents ---")
with open("logs/log.txt", "r") as f:
    print(f.read())

###### Summary:
 - This script configures logging to record all calculator operations info, warnings, and errorswith timestamps into a log file. It logs every action (add/sub/mul/div) and captures invalid inputs or division by zero.
---
#####  Create a Decorator to Auto- Log Function

###### Objective:
 - Automatically log every function call without adding print/log statements inside them.

import requests
import logging

# Logging Setup
logging.basicConfig(
    filename="decorator_log.txt",
    level=logging.INFO,
    format="%(asctime)s - %(message)s"
)

def auto_log(func):
    def wrapper(*args, **kwargs):
        logging.info(f"Function '{func.__name__}' called with args={args} kwargs={kwargs}")
        return func(*args, **kwargs)
    return wrapper

@auto_log
def fetch_api(url):
    response = requests.get(url, timeout=3)
    return response.status_code

@auto_log
def add(a, b):
    return a + b

# Testing
print(fetch_api("https://jsonplaceholder.typicode.com/todos/1"))
print(add(5, 10))

###### Summary:
 - This code uses a decorator (auto_log) to automatically log every function call with its arguments. It logs API requests and math operations without adding logging code inside the functions.
---
