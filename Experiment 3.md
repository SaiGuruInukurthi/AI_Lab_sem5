# Experiment 3

## Aim:
To implement an AI agent in Python that solves the classic Monkey and Banana problem by finding an optimal sequence of actions using the Breadth-First Search (BFS) algorithm.

## Description:
The Monkey and Banana problem is a well-known toy problem in artificial intelligence used to demonstrate planning and problem-solving. In this scenario, a monkey is in a room where a banana is hanging from the ceiling, just out of its reach. There is also a box in the room that the monkey can use.

The goal for the monkey (the agent) is to figure out a sequence of actions to get the banana. The problem can be modeled using states and actions:

**State Representation**: The state of the world can be described by a tuple: (monkey_position, box_position, is_monkey_on_box, has_banana).

**Actions**: The monkey can perform several actions, including walk, push the box, climb_up on the box, climb_down from the box, and grab the banana.

This program uses a Breadth-First Search (BFS) algorithm to explore the state space. BFS is an ideal choice here because it systematically explores all possible action sequences layer by layer, guaranteeing that it will find the shortest possible sequence of actions to reach the goal.

## Problem Setup:
The room has three key locations:

- **A**: The monkey's initial position.
- **B**: The box's initial position.
- **C**: The location directly under the banana.

The agent starts at an initial state and searches for a path to the goal state.

**Initial State**: ('A', 'B', False, False)

**Goal State**: ('C', 'C', True, True)

## Programme:

```python
from collections import deque

# Define all locations in the room.
# 'A': Monkey's starting corner
# 'B': Box's starting corner
# 'C': Location under the banana
locations = ['A', 'B', 'C']

# Define the goal state: Monkey and box are at 'C', monkey is on the box, and has the banana.
GOAL = ('C', 'C', True, True)

def get_possible_actions(state):
    """
    Determines all valid actions from the current state.
    """
    monkey, box, on_box, has_banana = state
    actions = []

    # If the monkey has the banana, no more actions are needed.
    if has_banana:
        return []

    # Action: Walk to other locations (monkey must get off the box to walk).
    for loc in locations:
        if loc != monkey:
            actions.append(('walk', loc))

    # Action: Push the box (monkey must be at the same location as the box and not on it).
    if monkey == box and not on_box:
        for loc in locations:
            if loc != monkey:
                actions.append(('push', loc))

    # Action: Climb up or down the box (monkey must be at the same location as the box).
    if monkey == box:
        if not on_box:
            actions.append(('climb_up',))
        else:
            actions.append(('climb_down',))

    # Action: Grab the banana (monkey must be on the box at location 'C').
    if monkey == 'C' and box == 'C' and on_box:
        actions.append(('grab',))

    return actions

def apply_action(state, action):
    """
    Applies an action to a state to get the new state.
    """
    monkey, box, on_box, has_banana = state
    action_type = action[0]

    if action_type == 'walk':
        # Monkey moves to a new location and is no longer on the box.
        return (action[1], box, False, has_banana)
    elif action_type == 'push':
        # Monkey and box move together to a new location.
        return (action[1], action[1], False, has_banana)
    elif action_type == 'climb_up':
        return (monkey, box, True, has_banana)
    elif action_type == 'climb_down':
        return (monkey, box, False, has_banana)
    elif action_type == 'grab':
        return (monkey, box, on_box, True)
    
    return state

def solve():
    """
    Solves the problem using Breadth-First Search (BFS).
    """
    # Define the initial state of the problem.
    start_state = ('A', 'B', False, False)  # (monkey_pos, box_pos, on_box, has_banana)
    
    # Initialize a queue for BFS with the starting state and an empty path.
    queue = deque([(start_state, [])])
    
    # Keep track of visited states to avoid cycles and redundant work.
    visited = {start_state}

    while queue:
        current_state, path = queue.popleft()

        # If the current state is the goal, return the path taken.
        if current_state == GOAL:
            return path + [('Goal Reached!',)]

        # Explore all possible actions from the current state.
        for action in get_possible_actions(current_state):
            new_state = apply_action(current_state, action)
            
            # If the new state has not been visited, add it to the queue and visited set.
            if new_state not in visited:
                visited.add(new_state)
                queue.append((new_state, path + [action]))
    
    # Return None if no solution is found.
    return None

# Run the solver and print the steps of the solution.
solution = solve()
if solution:
    for step in solution:
        print(step)
else:
    print("No solution found.")
```

## Output:
```
('walk', 'B')
('push', 'C')
('climb_up',)
('grab',)
('Goal Reached!',)
```

## Explanation of the output:
The output shows the optimal sequence of actions, found by the BFS algorithm, for the monkey to get the banana. Each line represents one step in the plan.

1. **('walk', 'B')**: The monkey starts at location 'A' and the box is at 'B'. The first logical step is for the monkey to walk to where the box is. The state changes from ('A', 'B', False, False) to ('B', 'B', False, False).

2. **('push', 'C')**: Now that the monkey is at the same location as the box ('B'), it pushes the box to location 'C', which is directly under the banana. Both the monkey and the box move to 'C'. The state becomes ('C', 'C', False, False).

3. **('climb_up',)**: With the box correctly positioned under the banana, the monkey climbs onto the box. The state becomes ('C', 'C', True, False).

4. **('grab',)**: Now that the monkey is on top of the box and under the banana, it can perform the grab action. This achieves the primary goal. The state becomes ('C', 'C', True, True).

5. **('Goal Reached!',)**: This final output confirms that the sequence of actions has led to the goal state, and the problem is successfully solved.

## Explanation of the Programme and Output:

The program implements a classical AI planning problem using state-space search. The solution demonstrates how BFS systematically explores all possible states to find the optimal path.

### State Representation:
The problem state is represented as a 4-tuple: (monkey_position, box_position, is_monkey_on_box, has_banana). This compact representation captures all the essential information needed to determine valid actions and check for the goal condition.

### BFS Algorithm Implementation:
The `solve()` function implements BFS using a queue data structure. It starts with the initial state and explores all reachable states level by level. The algorithm maintains a visited set to avoid revisiting states and prevents infinite loops.

### Action Generation and Application:
The `get_possible_actions()` function analyzes the current state and returns all valid actions. The `apply_action()` function takes a state and an action, returning the resulting new state. This separation of concerns makes the code modular and easy to understand.

### Optimality Guarantee:
Since BFS explores states in order of their distance from the start state, it guarantees finding the shortest sequence of actions. In this case, the solution requires only 4 actions, which is indeed optimal for reaching the goal from the given initial state.
