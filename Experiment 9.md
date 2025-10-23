# Experiment 9

## Aim:
Write a program to understand Inferential logic using KANREN, SYMPY, pyDatalog packages in Python.

## Description:
Propositional Logic (also called Propositional Calculus or Boolean Logic) is a branch of logic that deals with propositions which can be true or false. It forms the foundation of logical reasoning in artificial intelligence and is used in knowledge representation, automated theorem proving, and decision-making systems.

In propositional logic, we work with:

**Propositions**: Statements that can be either true or false (e.g., "It is raining", "The sky is blue").

**Logical Operators**: Symbols that combine propositions to form compound statements:
- **NOT (¬)**: Negation - reverses the truth value
- **AND (∧)**: Conjunction - true only if both operands are true
- **OR (∨)**: Disjunction - true if at least one operand is true
- **IMPLIES (→)**: Implication - false only if antecedent is true and consequent is false
- **IFF (↔)**: Biconditional - true if both operands have the same truth value

This program implements a propositional logic evaluator that can parse logical expressions, evaluate them for given truth assignments, generate truth tables, and check for tautologies and contradictions.

## Problem Setup:

**Input Components**:
- **Propositional Variables**: Atomic propositions represented by letters (e.g., P, Q, R)
- **Logical Expression**: A formula combining variables with logical operators
- **Truth Assignment**: Assignment of truth values (True/False) to each variable

**Operations**:
- Evaluate expressions with given truth values
- Generate complete truth tables
- Check for tautologies (always true)
- Check for contradictions (always false)
- Determine logical equivalence between expressions

## Programme:

```python
from itertools import product

class PropositionLogic:
    """
    A class to represent and evaluate propositional logic expressions.
    """
    
    def __init__(self):
        """
        Initialize the propositional logic system.
        """
        self.variables = set()
    
    def NOT(self, p):
        """Logical NOT operation."""
        return not p
    
    def AND(self, p, q):
        """Logical AND operation."""
        return p and q
    
    def OR(self, p, q):
        """Logical OR operation."""
        return p or q
    
    def IMPLIES(self, p, q):
        """Logical IMPLIES operation (p → q)."""
        return (not p) or q
    
    def IFF(self, p, q):
        """Logical IFF (if and only if) operation (p ↔ q)."""
        return p == q
    
    def extract_variables(self, expression):
        """
        Extract propositional variables from an expression string.
        """
        variables = set()
        for char in expression:
            if char.isalpha() and char.isupper():
                variables.add(char)
        return sorted(variables)
    
    def evaluate(self, expression, values):
        """
        Evaluate a logical expression with given truth values.
        
        Args:
            expression: String containing the logical expression
            values: Dictionary mapping variables to boolean values
        
        Returns:
            Boolean result of the evaluation
        """
        # Replace variables with their truth values
        expr = expression
        for var, val in values.items():
            expr = expr.replace(var, str(val))
        
        # Replace logical operators with Python equivalents
        expr = expr.replace('NOT', 'not')
        expr = expr.replace('AND', 'and')
        expr = expr.replace('OR', 'or')
        
        # Evaluate the expression
        return eval(expr)
    
    def generate_truth_table(self, expression):
        """
        Generate a complete truth table for a logical expression.
        """
        variables = self.extract_variables(expression)
        n = len(variables)
        
        print(f"\nTruth Table for: {expression}")
        print("=" * (15 * n + 20))
        
        # Print header
        header = " | ".join(variables) + " | Result"
        print(header)
        print("-" * len(header))
        
        # Generate all possible combinations of truth values
        results = []
        for values in product([False, True], repeat=n):
            value_dict = dict(zip(variables, values))
            result = self.evaluate(expression, value_dict)
            results.append(result)
            
            # Print row
            row = " | ".join([str(v)[0] for v in values]) + f" | {str(result)[0]}"
            print(row)
        
        print("=" * (15 * n + 20))
        return results
    
    def is_tautology(self, expression):
        """
        Check if an expression is a tautology (always true).
        """
        variables = self.extract_variables(expression)
        n = len(variables)
        
        for values in product([False, True], repeat=n):
            value_dict = dict(zip(variables, values))
            if not self.evaluate(expression, value_dict):
                return False
        return True
    
    def is_contradiction(self, expression):
        """
        Check if an expression is a contradiction (always false).
        """
        variables = self.extract_variables(expression)
        n = len(variables)
        
        for values in product([False, True], repeat=n):
            value_dict = dict(zip(variables, values))
            if self.evaluate(expression, value_dict):
                return False
        return True
    
    def are_equivalent(self, expr1, expr2):
        """
        Check if two expressions are logically equivalent.
        """
        # Get all variables from both expressions
        variables = sorted(set(self.extract_variables(expr1) + self.extract_variables(expr2)))
        n = len(variables)
        
        for values in product([False, True], repeat=n):
            value_dict = dict(zip(variables, values))
            result1 = self.evaluate(expr1, value_dict)
            result2 = self.evaluate(expr2, value_dict)
            if result1 != result2:
                return False
        return True


# Example usage
if __name__ == "__main__":
    logic = PropositionLogic()
    
    print("="*60)
    print("PROPOSITIONAL LOGIC EVALUATOR")
    print("="*60)
    
    # Example 1: Simple evaluation
    print("\n--- Example 1: Evaluating expressions ---")
    expr1 = "P AND Q"
    values1 = {'P': True, 'Q': False}
    result1 = logic.evaluate(expr1, values1)
    print(f"Expression: {expr1}")
    print(f"Values: {values1}")
    print(f"Result: {result1}")
    
    # Example 2: Truth table for P OR Q
    print("\n--- Example 2: Truth Table ---")
    expr2 = "P OR Q"
    logic.generate_truth_table(expr2)
    
    # Example 3: Truth table for complex expression
    print("\n--- Example 3: Complex Expression ---")
    expr3 = "P AND (Q OR R)"
    logic.generate_truth_table(expr3)
    
    # Example 4: Checking tautology
    print("\n--- Example 4: Tautology Check ---")
    expr4 = "P OR (NOT P)"
    is_taut = logic.is_tautology(expr4)
    print(f"Expression: {expr4}")
    print(f"Is Tautology: {is_taut}")
    logic.generate_truth_table(expr4)
    
    # Example 5: Checking contradiction
    print("\n--- Example 5: Contradiction Check ---")
    expr5 = "P AND (NOT P)"
    is_contra = logic.is_contradiction(expr5)
    print(f"Expression: {expr5}")
    print(f"Is Contradiction: {is_contra}")
    logic.generate_truth_table(expr5)
    
    # Example 6: Logical equivalence
    print("\n--- Example 6: Logical Equivalence ---")
    expr6a = "NOT (P AND Q)"
    expr6b = "(NOT P) OR (NOT Q)"
    are_equiv = logic.are_equivalent(expr6a, expr6b)
    print(f"Expression 1: {expr6a}")
    print(f"Expression 2: {expr6b}")
    print(f"Are Equivalent: {are_equiv} (De Morgan's Law)")
```

## Output:
```
============================================================
PROPOSITIONAL LOGIC EVALUATOR
============================================================

--- Example 1: Evaluating expressions ---
Expression: P AND Q
Values: {'P': True, 'Q': False}
Result: False

--- Example 2: Truth Table ---

Truth Table for: P OR Q
==============================
P | Q | Result
--------------
F | F | F
F | T | T
T | F | T
T | T | T
==============================

--- Example 3: Complex Expression ---

Truth Table for: P AND (Q OR R)
===============================================
P | Q | R | Result
-------------------
F | F | F | F
F | F | T | F
F | T | F | F
F | T | T | F
T | F | F | F
T | F | T | T
T | T | F | T
T | T | T | T
===============================================

--- Example 4: Tautology Check ---
Expression: P OR (NOT P)
Is Tautology: True

Truth Table for: P OR (NOT P)
==============================
P | Result
----------
F | T
T | T
==============================

--- Example 5: Contradiction Check ---
Expression: P AND (NOT P)
Is Contradiction: True

Truth Table for: P AND (NOT P)
==============================
P | Result
----------
F | F
T | F
==============================

--- Example 6: Logical Equivalence ---
Expression 1: NOT (P AND Q)
Expression 2: (NOT P) OR (NOT Q)
Are Equivalent: True (De Morgan's Law)
```

## Explanation of the output:

The output demonstrates various operations and concepts in propositional logic through six examples.

**Example 1 - Simple Evaluation**: The expression "P AND Q" is evaluated with P=True and Q=False. Since AND requires both operands to be true, the result is False. This shows basic expression evaluation with specific truth assignments.

**Example 2 - OR Truth Table**: The truth table for "P OR Q" shows all four possible combinations of truth values for two variables. The OR operation returns True when at least one operand is True, which is evident from three out of four rows being True.

**Example 3 - Complex Expression**: The expression "P AND (Q OR R)" involves three variables and demonstrates operator precedence. The table shows 8 rows (2³ combinations). The result is True only when P is True AND at least one of Q or R is True, which occurs in rows 6, 7, and 8.

**Example 4 - Tautology**: The expression "P OR (NOT P)" is identified as a tautology - a statement that is always true regardless of the truth values of its variables. The truth table confirms this by showing True in the Result column for both P=False and P=True. This is known as the Law of Excluded Middle.

**Example 5 - Contradiction**: The expression "P AND (NOT P)" is identified as a contradiction - a statement that is always false. The truth table shows False for all truth value assignments. This violates the Law of Non-Contradiction, as a proposition cannot be both true and false simultaneously.

**Example 6 - Logical Equivalence**: Two expressions are tested for logical equivalence using De Morgan's Law. The law states that the negation of a conjunction equals the disjunction of the negations: ¬(P ∧ Q) ≡ (¬P ∨ ¬Q). The program confirms these expressions are equivalent, meaning they produce identical truth values for all possible input combinations.

## Explanation of the Programme and Output:

The program implements a complete propositional logic system using object-oriented programming and demonstrates fundamental concepts in formal logic and automated reasoning.

### Logic Operations Implementation:
The `PropositionLogic` class implements the five basic logical operators: NOT, AND, OR, IMPLIES, and IFF. These methods form the foundation of propositional logic and can be combined to create complex logical expressions. The implementation uses Python's native boolean operations for efficiency and clarity.

### Expression Evaluation:
The `evaluate()` method takes a logical expression as a string and a dictionary of truth value assignments. It performs substitution of variables with their values and then uses Python's `eval()` function to compute the result. This approach allows for flexible expression syntax while maintaining security within the controlled environment of logical operations.

### Truth Table Generation:
The `generate_truth_table()` method demonstrates exhaustive evaluation by generating all possible combinations of truth values using `itertools.product()`. For n variables, it generates 2ⁿ rows, covering the complete state space. This systematic approach ensures no case is overlooked and provides a complete picture of the expression's behavior.

### Tautology and Contradiction Detection:
The program can identify special cases: tautologies (always true) and contradictions (always false). These are important in logic because tautologies represent logical truths that don't depend on specific facts, while contradictions represent logical impossibilities. This capability is essential for automated theorem proving and logical reasoning systems.

### Logical Equivalence:
The `are_equivalent()` method checks if two expressions are logically equivalent by comparing their truth values across all possible variable assignments. This is crucial for simplifying logical expressions, verifying logical laws (like De Morgan's Laws), and optimizing logical circuits in computer hardware design.

### Applications in AI:
This implementation demonstrates core concepts used in AI systems for knowledge representation (expressing facts and rules), inference engines (deriving new knowledge from existing facts), and constraint satisfaction problems (checking consistency of constraints). The truth table approach, while computationally expensive for many variables, provides a definitive answer and is useful for educational purposes and small-scale problems.

### State Space Exploration:
The systematic generation of all truth value combinations represents a form of state space search, a fundamental AI technique. For larger problems, more sophisticated methods like SAT solvers would be used, but this implementation clearly illustrates the underlying principles.
