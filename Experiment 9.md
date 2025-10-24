# Experiment 9

## Aim:
Write a program to understand Inferential logic using KANREN, SYMPY, pyDatalog packages in Python.

## Description:
Propositional logic (or Boolean logic) deals with statements that are either True or False and logical connectives like AND, OR, NOT, IMPLIES.

This experiment demonstrates the use of three Python libraries for logical reasoning:

### SymPy — Symbolic Propositional Logic
SymPy is a symbolic mathematics library that can handle logical expressions and evaluate them.

**Key Features:**
- Define propositional variables
- Build expressions using And, Or, Not, Implies, Equivalent
- Evaluate truth values for different assignments

### KANREN — Logical Relations
KANREN is a logic programming library in Python. It is not purely propositional logic, but helps in defining relations and querying them (like small logic programs).

**Key Features:**
- Define relations and facts
- Use logic variables to query unknowns
- Supports relational reasoning

### pyDatalog — Logic Programming with Rules
pyDatalog is a Python library that brings logic programming features into Python using Datalog.

**Key Features:**
- Define facts and rules
- Perform logical inference
- Supports queries that derive new facts from existing ones

## Problem Setup:

This experiment requires installing three Python packages:
- **kanren**: For logical relations and relational reasoning
- **sympy**: For symbolic propositional logic
- **pyDatalog**: For logic programming with rules and facts

Install using:
```bash
pip install kanren sympy pyDatalog
```

## Programme:

### Installing Required Packages:
```python
!pip install kanren sympy pyDatalog
```

### Example 1: SymPy - Symbolic Propositional Logic
```python
from sympy import symbols
from sympy.logic.boolalg import And, Or, Not

# Define logical variables
P, Q = symbols('P Q')

# Simple logical expression: P AND Q
expr = And(P, Q)

# Evaluate with different values
print(expr.subs({P: True, Q: True}))   # True
print(expr.subs({P: True, Q: False}))  # False
```

### Example 2: pyDatalog - Logic Programming with Rules
```python
from pyDatalog import pyDatalog

pyDatalog.create_terms('parent, X')

# Add a simple fact
+ parent('Alice', 'Bob')

# Query
print(parent('Alice', X))
```

## Output:

### Package Installation Output:
```
Requirement already satisfied: kanren in /usr/local/lib/python3.12/dist-packages (0.2.3)
Requirement already satisfied: sympy in /usr/local/lib/python3.12/dist-packages (1.13.3)
Requirement already satisfied: pyDatalog in /usr/local/lib/python3.12/dist-packages (0.17.4)
Requirement already satisfied: toolz in /usr/local/lib/python3.12/dist-packages (from kanren) (0.12.1)
Requirement already satisfied: multipledispatch in /usr/local/lib/python3.12/dist-packages (from kanren) (1.0.0)
Requirement already satisfied: unification in /usr/local/lib/python3.12/dist-packages (from kanren) (0.2.2)
Requirement already satisfied: mpmath<1.4,>=1.1.0 in /usr/local/lib/python3.12/dist-packages (from sympy) (1.3.0)
```

### Example 1 Output - SymPy:
```
True
False
```

### Example 2 Output - pyDatalog:
```
X
---
Bob
```

## Explanation of the output:

**Package Installation**: The first output shows that all three required packages (kanren, sympy, and pyDatalog) are successfully installed along with their dependencies. These packages enable different approaches to logical reasoning in Python.

**SymPy Example**: 
- The expression `And(P, Q)` represents the logical conjunction "P AND Q"
- First evaluation: `P=True, Q=True` → Result is `True` (both conditions are met)
- Second evaluation: `P=True, Q=False` → Result is `False` (not all conditions are met)

This demonstrates how SymPy can symbolically represent logical expressions and evaluate them with different truth value assignments.

**pyDatalog Example**:
- We define a fact: `parent('Alice', 'Bob')` meaning "Alice is the parent of Bob"
- We query: `parent('Alice', X)` asking "Who is Alice a parent of?"
- The system responds with `X = Bob`, finding all values that satisfy the relation

This demonstrates logic programming where facts are declared and queries can find unknown values that make statements true.

## Explanation of the Programme and Output:

### SymPy - Symbolic Logic:
SymPy treats logical variables as symbolic objects rather than simple boolean values. This allows for algebraic manipulation of logical expressions, simplification, and evaluation. The `And`, `Or`, and `Not` functions create symbolic expressions that can be evaluated using the `.subs()` method by substituting actual truth values.

### pyDatalog - Declarative Logic Programming:
pyDatalog follows the Datalog paradigm where you declare facts and rules, then query the system. The `+` operator adds facts to the knowledge base. When querying with a variable (like `X`), the system performs logical inference to find all values that satisfy the relationship. This is particularly useful for relational data and deriving new information from existing facts.

### KANREN - Relational Programming:
While not demonstrated in the basic example, KANREN enables miniKanren-style logic programming in Python. It allows defining relations between variables and solving constraint satisfaction problems through unification and search.

### Applications in AI:
These libraries are valuable for:
- **Knowledge Representation**: Storing facts and rules about a domain
- **Automated Reasoning**: Deriving new conclusions from existing knowledge
- **Constraint Solving**: Finding values that satisfy logical constraints
- **Expert Systems**: Building systems that reason with symbolic logic
- **Semantic Web**: Processing and reasoning with ontologies and relationships

The combination of these three approaches provides a comprehensive toolkit for logical reasoning in AI applications.
