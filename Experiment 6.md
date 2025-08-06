# Experiment-6



## Aim:
To implement Breadth-First Search (BFS) and Uniform-Cost Search algorithms for graph traversal and pathfinding, demonstrating uninformed search strategies in Artificial Intelligence.

## Description:

### Breadth-First Search (BFS):
Breadth-First Search explores nodes level by level using a FIFO queue, expanding the shallowest unexpanded node first.

**Solving exp 6.1.png:** BFS traverses the tree systematically - Start with A [A] → Expand A, add B,C [B,C] → Expand B, add D,E [C,D,E] → Expand C, add F,G [D,E,F,G] → Continue until goal found.

**Key Characteristics:**
- **Time Complexity:** O(b^(d+1)), **Space Complexity:** O(b^d)
- **Optimal** when all step costs are equal

### Uniform-Cost Search (UCS):
Uniform-Cost Search expands nodes with the lowest path cost using a priority queue, suitable for weighted graphs.

**Solving exp 6.2.png:** UCS finds optimal paths in weighted graphs - Start at S with cost 0 → Expand lowest-cost nodes first → Calculate cumulative costs (current_cost + edge_cost) → Continue until goal reached with minimum total cost.

**Key Characteristics:**
- **Time/Space Complexity:** O(b^(⌈C*/ε⌉)) where C* = optimal cost
- **Always finds least-cost solution**

**Key Differences from BFS:**
1. Uses priority queue (cost-based) vs FIFO queue
2. Considers edge weights, not just steps
3. Optimal for any step cost function

## Program 1 (BFS):

```python
from collections import deque

class BreadthFirstSearch:
    def __init__(self, graph):
        # Initialize with graph representation (adjacency list)
        self.graph = graph
        
    def bfs(self, start, goal):
        # BFS implementation using FIFO queue
        print(f"Starting BFS from {start} to {goal}")
        
        # Initialize frontier with start node
        frontier = deque([start])
        explored = set()
        parent = {start: None}
        
        while frontier:
            # Choose shallowest unexpanded node (FIFO)
            node = frontier.popleft()
            print(f"Expanding node: {node}")
            
            # Goal test when node is selected for expansion
            if node == goal:
                return self.reconstruct_path(parent, start, goal)
            
            # Add node to explored set
            explored.add(node)
            
            # Generate child nodes
            for neighbor in self.graph.get(node, []):
                if neighbor not in explored and neighbor not in frontier:
                    parent[neighbor] = node
                    frontier.append(neighbor)
                    print(f"Added {neighbor} to frontier")
        
        return None  # No path found
    
    def reconstruct_path(self, parent, start, goal):
        # Reconstruct path from parent pointers
        path = []
        current = goal
        while current is not None:
            path.append(current)
            current = parent[current]
        return path[::-1]

# BFS Example
if __name__ == "__main__":
    # Graph representation (adjacency list)
    graph = {
        'A': ['B', 'C'],
        'B': ['D', 'E'],
        'C': ['F', 'G'],
        'D': ['H'],
        'E': [],
        'F': [],
        'G': [],
        'H': []
    }
    
    bfs_solver = BreadthFirstSearch(graph)
    path = bfs_solver.bfs('A', 'H')
    print(f"BFS Path found: {path}")
```

## Output 1 (BFS):
```
Starting BFS from A to H
Expanding node: A
Added B to frontier
Added C to frontier
Expanding node: B
Added D to frontier
Added E to frontier
Expanding node: C
Added F to frontier
Added G to frontier
Expanding node: D
Added H to frontier
Expanding node: E
Expanding node: F
Expanding node: G
Expanding node: H
BFS Path found: ['A', 'B', 'D', 'H']
```

## Explanation of Output 1 (BFS):
1. **Initialization:** BFS starts at node 'A' with goal 'H', using FIFO queue
2. **Level 1 Expansion:** Node 'A' is expanded first, adding children 'B' and 'C' to frontier
3. **Level 2 Expansion:** Node 'B' is expanded (first in queue), adding 'D' and 'E' to frontier
4. **Level 2 Continued:** Node 'C' is expanded next, adding 'F' and 'G' to frontier
5. **Level 3 Expansion:** Node 'D' is expanded first (FIFO order), adding 'H' to frontier
6. **Remaining Nodes:** Nodes 'E', 'F', 'G' are expanded but have no children
7. **Goal Found:** When 'H' is expanded, goal test succeeds and path ['A', 'B', 'D', 'H'] is returned

The BFS explores nodes level by level, ensuring the shortest path in terms of number of edges.

## Program 2 (UCS):

```python
import heapq

class UniformCostSearch:
    def __init__(self, graph):
        # Initialize with weighted graph representation
        self.graph = graph
        
    def ucs(self, start, goal):
        # UCS implementation using priority queue ordered by path cost
        print(f"Starting UCS from {start} to {goal}")
        
        # Priority queue: (cost, node, path)
        frontier = [(0, start, [start])]
        explored = set()
        
        while frontier:
            # Choose lowest-cost node in frontier
            cost, node, path = heapq.heappop(frontier)
            print(f"Expanding node: {node} with cost: {cost}")
            
            # Goal test when node is selected for expansion
            if node == goal:
                return path, cost
            
            # Add node to explored
            if node not in explored:
                explored.add(node)
                
                # Generate child nodes
                for neighbor, edge_cost in self.graph.get(node, []):
                    if neighbor not in explored:
                        new_cost = cost + edge_cost
                        new_path = path + [neighbor]
                        # Check if better path exists in frontier
                        heapq.heappush(frontier, (new_cost, neighbor, new_path))
                        print(f"Added {neighbor} to frontier with cost: {new_cost}")
        
        return None, float('inf')  # No path found

# UCS Example
if __name__ == "__main__":
    # Weighted graph representation (adjacency list with costs)
    graph = {
        'Sibiu': [('Rimnicu Vilcea', 80), ('Fagaras', 99)],
        'Rimnicu Vilcea': [('Pitesti', 97)],
        'Fagaras': [('Bucharest', 211)],
        'Pitesti': [('Bucharest', 101)]
    }
    
    ucs_solver = UniformCostSearch(graph)
    path, cost = ucs_solver.ucs('Sibiu', 'Bucharest')
    print(f"UCS Path found: {path} with total cost: {cost}")
```

## Output 2 (UCS):
```
Starting UCS from Sibiu to Bucharest
Expanding node: Sibiu with cost: 0
Added Rimnicu Vilcea to frontier with cost: 80
Added Fagaras to frontier with cost: 99
Expanding node: Rimnicu Vilcea with cost: 80
Added Pitesti to frontier with cost: 177
Expanding node: Fagaras with cost: 99
Added Bucharest to frontier with cost: 310
Expanding node: Pitesti with cost: 177
Added Bucharest to frontier with cost: 278
Expanding node: Bucharest with cost: 278
UCS Path found: ['Sibiu', 'Rimnicu Vilcea', 'Pitesti', 'Bucharest'] with total cost: 278
```

## Explanation of Output 2 (UCS):
1. **Initialization:** UCS starts at 'Sibiu' with goal 'Bucharest', using priority queue ordered by path cost
2. **First Expansion:** 'Sibiu' (cost 0) is expanded, adding 'Rimnicu Vilcea' (cost 80) and 'Fagaras' (cost 99) to frontier
3. **Lowest Cost Selection:** 'Rimnicu Vilcea' (cost 80) has lowest cost, so it's expanded next, adding 'Pitesti' (cost 177)
4. **Next Lowest Cost:** 'Fagaras' (cost 99) is expanded next, adding 'Bucharest' (cost 310) to frontier
5. **Better Path Found:** 'Pitesti' (cost 177) is expanded, adding better path to 'Bucharest' (cost 278)
6. **Optimal Solution:** The lowest-cost 'Bucharest' (cost 278) is selected, returning optimal path with total cost 278

UCS guarantees the optimal solution by always expanding the lowest-cost node first, ensuring the least-cost path is found.
