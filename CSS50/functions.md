# Functions

### What is a function in programming?

Functions are reusable blocks of code that perform a specific task. They allow you to encapsulate logic, making your code more organized and easier to maintain.

Functions can take inputs, called parameters, and can return outputs, allowing for dynamic behavior based on the provided arguments.

In programming, functions help to reduce redundancy and improve code readability by allowing developers to define a piece of functionality once and use it multiple times throughout their codebase.

### "A black box with a set of 0+ inputs and 1 output"

The output is determined by the inputs and the logic defined within the function. Functions can be called or invoked from other parts of the code.

For example, let's take the following function: `func(a, b, c)`. This function takes three inputs (a, b, and c) and performs some operation.

Let's give our function a name `add(a, b, c)`. Now we can define what this function does. Let's make some examples:

    ```c
    int add(int a, int b, int c) {
        return a + b + c;
    }
    ```

    ```c
    int multiply(int a, int b, int c) {
        return a * b * c;
    }
    ```

    ```c
    int subtract(int a, int b, int c) {
        return a - b - c;
    }
    ```

These functions take three inputs and return a single output based on the operation defined within them. The internal workings of the function are hidden from the user, which is why we refer to it as a "black box." You provide the inputs, and you get the output without needing to know how the function processes those inputs internally.


### Why we call it a black box?

If we are not writing the function ourselves, we don't know how it works internally. We only know what inputs it takes and what output it produces. This abstraction allows us to use functions without needing to understand their internal implementation, making our code cleaner and easier to work with.

For example the `"printf()"` function in `C` is a black box. We know that it takes an input (the value we want to print) and produces an output (the printed value on the console), but we don't need to know how it works internally to use it effectively.

### Why use functions?

- **Organization**: Functions help organize code into logical blocks, making it easier to read and understand.

- **Simplification**: Functions allow you to break down complex problems into smaller, manageable pieces, making it easier to solve and maintain.

- **Reusability**: Functions can be reused across different parts of a program or even in different programs, reducing code duplication and promoting consistency.

# Creating Functions

To create a function, you need to define it with a name and specify any parameters it may take.

Declarations are the way to define a function, and they consist of three main components:

- **Return Type**: This specifies the type of value that the function will return. In Python, you don't need to explicitly declare the return type, as it is dynamically determined based on the value returned by the function.

- **Function Name**: This is the identifier used to call the function. It should be descriptive and follow naming conventions.

- **Argument List**: This is a list of parameters that the function can accept. Parameters are variables that allow you to pass data into the function for processing.

```c
function_name(parameter1, parameter2, ...) {
    // Function body
    // Code to be executed when the function is called
}
```

There are two main types of functions:

- **Built-in Functions**: These are functions that are provided by the programming language or its standard library. They are ready to use and perform common tasks, such as mathematical operations, string manipulation, and input/output handling. Examples include `print()`, `len()`, and `sum()` in Python and `printf()`, `scanf()`, and `strlen()` in C.

- **User-defined Functions**: These are functions that you create yourself to perform specific tasks. You define the logic and behavior of these functions based on your program's requirements. User-defined functions allow you to encapsulate code, making it reusable and easier to maintain.

Functions Definitions:

- The second step in creating a function is to define its behavior. This involves writing the code that specifies what the function does when it is called. The function definition includes the function's name, parameters, and the block of code that executes when the function is invoked.

```c
float mult_two/-reals(float x, float y) {
    float product = x * y;
    return product;
}
```

The function `mult_two_reals` takes two floating-point numbers as input parameters, multiplies them together, and returns the product. The `return` statement specifies the value that will be sent back to the caller when the function is executed.

## Function Calls

To use a function, you need to call it by its name and provide the required arguments. When a function is called, the program execution jumps to the function's definition, executes the code within it, and then returns to the point where the function was called.

You can call a function in different ways, like using its name in the file where it is defined or importing it from another file or module. The function call can be made with the appropriate arguments, and the returned value can be stored in a variable or used directly in expressions.

In the command line, you can call a function by typing its name followed by parentheses, which may contain arguments if the function requires them. For example, if you have a function named `add`, you can call it like this:

```bash
> add(5, 10, 15)
```

Both built-in and user-defined functions can be called in this manner, allowing you to leverage their functionality in your programs.

---

# Default Parameters

Default parameters allow you to specify default values for function parameters. If the caller does not provide a value for a parameter with a default, the function will use the default value instead. This feature enhances flexibility and simplifies function calls.

The syntax for defining default parameters varies between programming languages. In C and C++, you can specify default values in the function declaration. In Python, you can assign default values directly in the function definition.

```c
// C/C++ example
int add(int a, int b = 10) {
    return a + b;
}
```

```python
# Python example
def add(a, b=10):
    return a + b
```

Both examples define a function `add` that takes two parameters, `a` and `b`. If the caller does not provide a value for `b`, it will default to 10. This allows for more concise function calls when the default behavior is desired.

# Function Overloading

Function overloading is a feature in some programming languages that allows you to define multiple functions with the same name but different parameter lists. This enables you to create functions that can handle different types or numbers of arguments, providing flexibility and improving code readability.

Some programming languages, like C++ and Java, support function overloading, while others, like Python, do not have built-in support for it. In languages that support overloading, the compiler determines which version of the function to call based on the arguments provided.

# Function Scope

In programming, scope refers to the visibility and accessibility of variables and functions within different parts of a program. Function scope specifically relates to the variables defined within a function and their accessibility.

For example, variables declared inside a function are typically only accessible within that function. This means that they cannot be accessed or modified from outside the function, which helps prevent unintended side effects and promotes encapsulation.

# Function Recursion

Recursion is a programming technique where a function calls itself in order to solve a problem. Recursive functions are often used to solve problems that can be broken down into smaller, similar subproblems.

Let's take an example of a recursive function that calculates the factorial of a number:

```python
# python
def factorial(n):
    if n == 0 or n == 1:
        return 1
    else:
        return n * factorial(n - 1)
```

In this example, the `factorial` function calls itself with a decremented value of `n` until it reaches the base case (when `n` is 0 or 1). The function then returns the product of `n` and the factorial of `n - 1`, effectively calculating the factorial of the original number.

Now another example of a recursive function that calculates the nth Fibonacci number:

```c
# c
int fibonacci(int n) {
    if (n <= 1) {
        return n;
    } else {
        return fibonacci(n - 1) + fibonacci(n - 2);
    }
}
```

In this example, the `fibonacci` function calls itself twice with decremented values of `n` until it reaches the base cases (when `n` is 0 or 1). The function then returns the sum of the two previous Fibonacci numbers, effectively calculating the nth Fibonacci number.

---

# Function as First-Class Citizens

In programming languages that treat functions as first-class citizens, functions can be assigned to variables, passed as arguments to other functions, and returned from other functions. This allows for greater flexibility and enables functional programming paradigms.

This means that functions can be treated like any other data type, such as integers or strings. You can store functions in data structures, pass them around in your code, and use them to create higher-order functions that operate on other functions.

Let's take an example of a higher-order function that takes another function as an argument:

```c
int apply_function(int (*func)(int), int value) {
    return func(value);
}
```

In python

```python
def apply_function(func, value):
    return func(value)
```