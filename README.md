# Experiment-1

## Aim:
To understand the basics of Python programming for implementing AI concepts.

To write a Python program that simulates an agent for a number guessing game using conditional logic and loops.

---

## Description:
Python is a beginner-friendly and widely used programming language in fields such as Artificial Intelligence, Data Science, and Web Development. Its simplicity in syntax, dynamic typing, and powerful libraries make it ideal for implementing AI-based logic.

This experiment consists of:

- A basic "Hello World" program
- Understanding indentation, data types, and input/output in Python
- Arithmetic operations with user input
- A simple AI agent that plays a number guessing game by interacting with the user

---

## Program 1: Basic Python Operations

```python
# This program prints Hello, World!
print('Hello, world!')

# Indentation Example
if 5 > 2:
    print("Five is greater than two!")

# Data Types
x = 1       # int
y = 2.8     # float
z = 1j      # complex

print(type(x))
print(type(y))
print(type(z))

# Add Two Numbers with User Input
num1 = input('Enter first number: ')
num2 = input('Enter second number: ')
sum = float(num1) + float(num2)
print('The sum of {0} and {1} is {2}'.format(num1, num2, sum))
```

## Output:

```python
Hello, world!
Five is greater than two!
<class 'int'>
<class 'float'>
<class 'complex'>
Enter first number: 1.5
Enter second number: 6.3
The sum of 1.5 and 6.3 is 7.8
```

## Explanation of the Output:

1. The `print()` statement outputs text to the screen.
2. The `if` condition shows how indentation defines blocks.
3. `type()` displays the data type of variables.
4. Inputs are taken from the user, converted to float, added, and displayed using formatted output.

---

## Program 2: Number Guessing Game (AI Agent)

```python
import random

number = random.randint(1, 100)
guess = 0

while guess != number:
    guess = int(input("Enter Guess: "))
    
    if guess < number:
        print("Guess higher!")
    elif guess > number:
        print("Guess lower!")
    else:
        print("You won!")
```

## Output (Sample):

```yaml
Enter Guess: 50
Guess higher!
Enter Guess: 75
Guess lower!
Enter Guess: 67
You won!
```

## Explanation of the Output:

1. A random number between 1 and 100 is generated using `random.randint()`.
2. The user keeps guessing until they match the number.
3. The program gives hints like "Guess higher!" or "Guess lower!" based on comparison.
4. Once guessed correctly, it prints "You won!" and exits.

---
