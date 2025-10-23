# Experiment-5

## Aim:
Write a program to solve 8-tiles puzzle problem (Using heuristics).

## Description:
The 8-Puzzle is a classic problem in Artificial Intelligence that involves a 3x3 grid with 8 numbered tiles and one blank space (_). The objective is to rearrange the tiles from a random initial configuration to a specified goal configuration by repeatedly sliding a tile into the adjacent blank space.

This problem is solved using the A* search algorithm, which finds the shortest path from a start to a goal state. A* uses an evaluation function f(n) = g(n) + h(n), where g(n) is the cost to reach the current node and h(n) is the heuristic value (number of misplaced tiles).

## Programme:

```python
class Node:
    def __init__(self, data, level, fval):
        # Initialize node with puzzle state, level, and f-value
        self.data = data
        self.level = level
        self.fval = fval

    def generate_child(self):
        # Generate child nodes by moving blank tile in 4 directions
        x, y = self.find(self.data, '_')
        val_list = [[x, y - 1], [x, y + 1], [x - 1, y], [x + 1, y]]
        children = []
        for i in val_list:
            child = self.shuffle(self.data, x, y, i[0], i[1])
            if child is not None:
                child_node = Node(child, self.level + 1, 0)
                children.append(child_node)
        return children

    def shuffle(self, puz, x1, y1, x2, y2):
        # Move blank tile to new position if valid
        if x2 >= 0 and x2 < len(self.data) and y2 >= 0 and y2 < len(self.data):
            temp_puz = self.copy(puz)
            temp = temp_puz[x2][y2]
            temp_puz[x2][y2] = temp_puz[x1][y1]
            temp_puz[x1][y1] = temp
            return temp_puz
        else:
            return None

    def copy(self, root):
        # Create deep copy of puzzle matrix
        temp = []
        for i in root:
            t = []
            for j in i:
                t.append(j)
            temp.append(t)
        return temp

    def find(self, puz, x):
        # Find position of element x in puzzle
        for i in range(0, len(self.data)):
            for j in range(0, len(self.data)):
                if puz[i][j] == x:
                    return i, j

class Puzzle:
    def __init__(self, size):
        # Initialize puzzle with size and open/closed lists
        self.n = size
        self.open = []
        self.closed = []

    def accept(self):
        # Accept puzzle input from user
        puz = []
        for i in range(0, self.n):
            temp = input().split(" ")
            puz.append(temp)
        return puz

    def f(self, start, goal):
        # Calculate f(n) = g(n) + h(n)
        return self.h(start.data, goal) + start.level

    def h(self, start, goal):
        # Calculate heuristic: number of misplaced tiles
        temp = 0
        for i in range(0, self.n):
            for j in range(0, self.n):
                if start[i][j] != goal[i][j] and start[i][j] != '_':
                    temp += 1
        return temp

    def process(self):
        # Main A* search algorithm
        print("Enter the start state matrix \n")
        start_data = self.accept()
        print("Enter the goal state matrix \n")
        goal_data = self.accept()

        start = Node(start_data, 0, 0)
        start.fval = self.f(start, goal_data)
        self.open.append(start)
        print("\n\n")
        
        while True:
            cur = self.open[0]  # Get node with lowest f-value
            print("--------------")
            for row in cur.data:
                for val in row:
                    print(val, end=" ")
                print("")
            
            if(self.h(cur.data, goal_data) == 0):  # Goal reached
                break

            # Generate children and add to open list
            for i in cur.generate_child():
                i.fval = self.f(i, goal_data)
                self.open.append(i)
            
            self.closed.append(cur)
            del self.open[0]
            self.open.sort(key=lambda x: x.fval, reverse=False)  # Sort by f-value

# Create puzzle and start solving
puz = Puzzle(3)
puz.process()
```

## Output:
```
Enter the start state matrix 

1 2 3
4 _ 6
7 5 8
Enter the goal state matrix 

1 2 3
4 5 6
7 8 _
--------------
1 2 3 
4 _ 6 
7 5 8 
--------------
1 2 3 
4 5 6 
7 _ 8 
--------------
1 2 3 
4 5 6 
7 8 _ 
```

## Explanation of the output:
The program uses A* algorithm to find the shortest path from start state to goal state.

**Input:** Start state (1 2 3 / 4 _ 6 / 7 5 8) and goal state (1 2 3 / 4 5 6 / 7 8 _)

**Process Steps:**
- Initial state has heuristic value 2 (tiles '5' and '8' misplaced)
- First move: swap blank with '5' → heuristic becomes 1  
- Second move: swap blank with '8' → heuristic becomes 0 (goal reached)

The algorithm terminates when h(n) = 0, successfully solving the puzzle in optimal steps.
