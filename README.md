# Decorators in Python

Decorators are a powerful feature in Python that allows you to modify the behavior of a function or class. They are typically used to add functionality to functions or methods in a clean, readable, and reusable way.

## How Decorators Work

In Python, a decorator is a function that takes another function as an argument, adds some kind of functionality, and returns another function. This is useful for abstracting away common functionality that you want to apply to many functions.

### Basic Syntax

Here's the basic syntax of a decorator:

```python
def my_decorator(func):
    def wrapper():
        print("Something is happening before the function is called.")
        func()
        print("Something is happening after the function is called.")
    return wrapper

def say_hello():
    print("Hello!")

say_hello = my_decorator(say_hello)
```

In this example, `my_decorator` is a decorator function that takes `say_hello` as an argument, wraps it in another function (`wrapper`), and returns the wrapper function. When `say_hello` is called, it actually calls `wrapper`.

### Using the `@` Syntax

Python provides a more elegant way to apply decorators using the `@` symbol:

```python
@my_decorator
def say_hello():
    print("Hello!")

say_hello()
```

This is functionally equivalent to the previous example but is cleaner and more readable.

## Examples of Decorators

### Logging Decorator

A common use case for decorators is logging. Here's an example of a logging decorator:

```python
def log_decorator(func):
    def wrapper(*args, **kwargs):
        print(f"Function {func.__name__} is about to be called with arguments: {args} and keyword arguments: {kwargs}")
        result = func(*args, **kwargs)
        print(f"Function {func.__name__} returned {result}")
        return result
    return wrapper

@log_decorator
def add(a, b):
    return a + b

add(2, 3)
```

Output:
```
Function add is about to be called with arguments: (2, 3) and keyword arguments: {}
Function add returned 5
```

### Authorization Decorator

Decorators can also be used for authorization checks:

```python
def authorize(role):
    def decorator(func):
        def wrapper(*args, **kwargs):
            if role != 'admin':
                raise PermissionError("You do not have permission to access this function.")
            return func(*args, **kwargs)
        return wrapper
    return decorator

@authorize('admin')
def delete_database():
    print("Database deleted!")

delete_database()
```

If a non-admin role is passed to the `authorize` decorator, it will raise a `PermissionError`.

### Caching Decorator

A caching decorator can be useful to store expensive function call results:

```python
def cache_decorator(func):
    cache = {}
    def wrapper(*args):
        if args in cache:
            return cache[args]
        result = func(*args)
        cache[args] = result
        return result
    return wrapper

@cache_decorator
def fibonacci(n):
    if n < 2:
        return n
    return fibonacci(n-1) + fibonacci(n-2)

print(fibonacci(10))
```

This decorator caches the results of the `fibonacci` function to avoid redundant calculations.

## Conclusion

Decorators are a very useful feature in Python that can help you write cleaner, more maintainable code. They allow you to abstract out common functionality and apply it to many functions in a readable and reusable way.
