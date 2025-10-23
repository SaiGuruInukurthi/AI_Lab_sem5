# Experiment-4

## Aim:
Write a program to solve Water-Jug problem (PROLOG).

## Description:
The Water Jug problem is a well-known problem in AI. The general problem statement involves being given two jugs with different capacities and a water source, with the objective of measuring out a specific amount of water in one of the jugs.

This problem is modeled using a state-space search, where each state represents the amount of water in each jug.

**State:** A state is represented by an ordered pair (x, y), where 'x' is the amount of water in the first jug (e.g., a 4-liter jug) and 'y' is the amount in the second jug (e.g., a 3-liter jug).

**Initial State:** The starting point of the problem, typically with both jugs empty, represented as (0, 0).

**Goal State:** The desired final configuration. For this experiment, the goal is to have exactly 2 liters in the 4-liter jug, represented as (2, 0).

**Operations (Rules):** A set of actions that can be performed to transition from one state to another, such as filling a jug completely, emptying a jug, or pouring water from one jug to another. The program proceeds by applying a sequence of these operations to move from the initial state to the goal state.

## Programme:

```python
a=int(input("Enter Jug A Capacity: "));
b=int(input("Enter Jug B Capacity: "));
ai=int(input("Initially Water in Jug A: "));
bi=int(input("Initially Water in Jug B: "));
af=int(input("Final State of Jug A: "));
bf=int(input("Final State of Jug B: "));
print("List Of Operations You can Do:\n");
print("1.Fill Jug A Completely\n");
print("2.Fill Jug B Completely\n");
print("3.Empty Jug A Completely\n");
print("4.Empty Jug B Completely\n");
print("5.Pour From Jug A till Jug B filled Completely or A becomes empty\n");
print("6.Pour From Jug B till Jug A filled Completely or B becomes empty\n");
print("7.Pour all From Jug B to Jug A\n");
print("8.Pour all From Jug A to Jug B\n");
while ((ai!=af or bi!=bf)):
    op=int(input("Enter the Operation: "));
    if(op==1):
        ai=a;
    elif(op==2):
        bi=b;
    elif(op==3):
        ai=0;
    elif(op==4):
        bi=0;
    elif(op==5):
        if(b-bi>ai):
            bi=ai+bi;
            ai=0;
        else:
            ai=ai-(b-bi);
            bi=b;
    elif(op==6):
        if(a-ai>bi):
            ai=ai+bi;
            bi=0;
        else:
            bi=bi-(a-ai);
            ai=a;
    elif(op==7):
        ai=ai+bi;
        bi=0;
    elif(op==8):
        bi=bi+ai;
        ai=0;
    print(ai,bi);
```

## Output:
```
Enter Jug A Capacity: 4
Enter Jug B Capacity: 3
Initially Water in Jug A: 0
Initially Water in Jug B: 0
Final State of Jug A: 2
Final State of Jug B: 0
List Of Operations You can Do:

1.Fill Jug A Completely
2.Fill Jug B Completely
3.Empty Jug A Completely
4.Empty Jug B Completely
5.Pour From Jug A till Jug B filled Completely or A becomes empty
6.Pour From Jug B till Jug A filled Completely or B becomes empty
7.Pour all From Jug B to Jug A
8.Pour all From Jug A to Jug B
Enter the Operation: 2
0 3
Enter the Operation: 6
3 0
Enter the Operation: 2
3 3
Enter the Operation: 6
4 2
Enter the Operation: 3
0 2
Enter the Operation: 6
2 0
```

## Explanation of the output:
The program solves the water jug problem by selecting operations to reach state (2, 0) from initial state (0, 0).

**Initial State:** (0, 0) → Target: (2, 0)

**Step 1:** Fill Jug B → (0, 3)  
**Step 2:** Pour B to A → (3, 0)  
**Step 3:** Fill Jug B → (3, 3)  
**Step 4:** Pour B to A until A is full → (4, 2)  
**Step 5:** Empty Jug A → (0, 2)  
**Step 6:** Pour B to A → (2, 0) ✓

The current state (2, 0) matches the final state, successfully solving the problem.
