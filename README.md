# Python Decorators – Enhancing Functions with Powerful Wrappers


Decorators in Python allow us to modify the behavior of functions and classes without changing their actual code. They are a powerful tool for code reusability, logging, authentication, performance monitoring, and more.

In this post, we’ll cover:

✅ What are decorators and how they work

✅ Creating and using function decorators

✅ Using built-in decorators like @staticmethod and @property

✅ Real-world use cases for decorators

Let’s dive in! 🚀

## 1️⃣ Understanding Decorators in Python

✅ What is a Decorator?

A decorator is a function that takes another function as input, enhances its behavior, and returns it.

Think of it like wrapping a function inside another function to modify its behavior dynamically.

🔹 Basic Example Without a Decorator

```python

def greet():
    return "Hello, World!"

print(greet())  # Output: Hello, World!

```

Now, let’s enhance this function by logging its execution.

✅ Using a Simple Decorator

```python

def log_decorator(func):
    def wrapper():
        print(f"Calling function: {func.__name__}")
        result = func()
        print(f"Function {func.__name__} executed successfully.")
        return result
    return wrapper

@log_decorator  # Applying decorator
def greet():
    return "Hello, World!"

print(greet())

```

🔹 Output:

```python

Calling function: greet
Function greet executed successfully.
Hello, World!

```

✅ What’s Happening?

    The log_decorator wraps around greet(), adding logging before and after execution.
    Using @log_decorator is the same as writing:

```python

    greet = log_decorator(greet)

```

## 2️⃣ Understanding the Inner Workings of Decorators

🔹 Decorators with Arguments

If a function takes arguments, the decorator must handle them properly.

```python

def log_decorator(func):
    def wrapper(*args, **kwargs):  # Accept arguments
        print(f"Executing {func.__name__} with arguments {args}, {kwargs}")
        return func(*args, **kwargs)
    return wrapper

@log_decorator
def add(a, b):
    return a + b

print(add(3, 5))  # Logs the function execution

```

🔹 Output:

```python

Executing add with arguments (3, 5), {}
8

```

## 3️⃣ Applying Multiple Decorators

You can stack multiple decorators to modify a function in different ways.

```python

def uppercase_decorator(func):
    def wrapper():
        return func().upper()
    return wrapper

def exclamation_decorator(func):
    def wrapper():
        return func() + "!!!"
    return wrapper

@uppercase_decorator
@exclamation_decorator
def greet():
    return "hello"

print(greet())  # Output: HELLO!!!

```

✅ Order Matters!

    @exclamation_decorator adds "!!!".
    @uppercase_decorator converts text to uppercase AFTER the exclamation marks are added.

## 4️⃣ Using Built-in Python Decorators

Python provides built-in decorators like @staticmethod, @classmethod, and @property.

✅ Using @staticmethod and @classmethod in Classes

```python

class MathOperations:
    @staticmethod
    def add(a, b):
        return a + b

    @classmethod
    def class_method_example(cls):
        return f"This is a class method of {cls.__name__}"

print(MathOperations.add(3, 4))  # Output: 7
print(MathOperations.class_method_example())  # Output: This is a class method of MathOperations

```

✅ Key Differences:

    @staticmethod: No self, doesn’t access instance attributes.
    @classmethod: Uses cls, can modify class state.

✅ Using @property for Getter and Setter Methods


```python

class Person:
    def __init__(self, name):
        self._name = name

    @property
    def name(self):  # Getter
        return self._name

    @name.setter
    def name(self, new_name):  # Setter
        if len(new_name) > 2:
            self._name = new_name
        else:
            raise ValueError("Name too short!")

p = Person("Alice")
print(p.name)  # Calls @property method
p.name = "Bob"  # Calls @name.setter method
print(p.name)  

```

✅ Why Use @property?

    Avoids writing get_name() and set_name() methods.
    Provides cleaner and more Pythonic code.

## 5️⃣ Real-World Use Cases for Decorators

🔹 1. Logging Function Execution

```python

import time

def timing_decorator(func):
    def wrapper(*args, **kwargs):
        start = time.time()
        result = func(*args, **kwargs)
        end = time.time()
        print(f"{func.__name__} took {end - start:.5f} seconds to execute.")
        return result
    return wrapper

@timing_decorator
def slow_function():
    time.sleep(2)  # Simulate delay
    return "Finished!"

print(slow_function())

```

✅ Use Case: Performance monitoring for slow functions.

🔹 2. Authentication Check in APIs

```python

def authentication_required(func):
    def wrapper(user):
        if not user.get("is_authenticated"):
            raise PermissionError("User not authenticated!")
        return func(user)
    return wrapper

@authentication_required
def get_user_data(user):
    return f"Welcome, {user['name']}!"

user = {"name": "Alice", "is_authenticated": True}
print(get_user_data(user))  # Works

unauthenticated_user = {"name": "Bob", "is_authenticated": False}
# print(get_user_data(unauthenticated_user))  # Raises PermissionError

```

✅ Use Case: Restricting access to certain functions based on authentication.

🔹 3. Retry Mechanism for Failing Functions

```python

import time
import random

def retry_decorator(func):
    def wrapper(*args, **kwargs):
        for attempt in range(3):  # Retry 3 times
            try:
                return func(*args, **kwargs)
            except Exception as e:
                print(f"Attempt {attempt+1} failed: {e}")
                time.sleep(1)
        raise Exception("Function failed after 3 attempts!")
    return wrapper

@retry_decorator
def unstable_function():
    if random.random() < 0.7:  # 70% failure rate
        raise ValueError("Random failure occurred!")
    return "Success!"

print(unstable_function())

```

✅ Use Case: Handling unreliable operations like API calls, database queries, and network requests.

## 6️⃣ Best Practices for Using Decorators

✅ Use functools.wraps(func) inside decorators to preserve function metadata.

```python

from functools import wraps

def log_decorator(func):
    @wraps(func)  # Preserves original function name and docstring
    def wrapper(*args, **kwargs):
        print(f"Executing {func.__name__}")
        return func(*args, **kwargs)
    return wrapper

```

✅ Use decorators only when necessary to keep code readable.

✅ Stack decorators carefully as order matters.

## 🔹 Conclusion

✅ Decorators modify functions without changing their core logic.

✅ Built-in decorators like @staticmethod and @property improve class functionality.

✅ Use decorators for logging, authentication, and performance monitoring.

✅ Understanding decorators makes your code more reusable and efficient! 🚀

## What’s Next?

In the next post, we’ll explore Metaclasses in Python – The Power Behind Class Creation. Stay tuned! 🔥

## 💬 What Do You Think?

Have you used decorators in your projects? What’s your favorite use case? Let’s discuss in the comments! 💡
