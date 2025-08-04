# Experiment-6

> **⚠️ Warning!** 
> 
> This is still unrefined and needs some clarification from lab lecturer before noting down. Use this as a reference at your own risk.

## Aim:
To implement Breadth-First Search (BFS) and Uniform-Cost Search algorithms for graph traversal and pathfinding, demonstrating uninformed search strategies in Artificial Intelligence.

## Description:

### Breadth-First Search (BFS):
Breadth-First Search is a simple strategy in which the root node is expanded first, then all the successors of the root node are expanded next, then their successors, and so on. BFS is an instance of the general graph-search algorithm, in which the shallowest unexpanded node is chosen for expansion.

**Solving the Tree Problem in exp 6.1.png:**
As illustrated in **exp 6.1.png**, the diagram shows a tree structure where we need to find a path from root A to a goal node. BFS would solve this by exploring level by level:

1. **Step 1:** Start with root node A in the frontier queue: [A]
2. **Step 2:** Expand A (marked with ▷), add its children B and C to frontier: [B, C]  
3. **Step 3:** Expand B (first in queue), add its children D and E to frontier: [C, D, E]
4. **Step 4:** Expand C next, add its children F and G to frontier: [D, E, F, G]
5. **Step 5:** Continue expanding D, E, F, G in order until goal is found

The progressive expansion shown in the four tree diagrams demonstrates how BFS systematically explores each level before moving deeper. The algorithm uses a FIFO (First-In-First-Out) queue for the frontier, ensuring that nodes are processed in the order they were discovered. The goal test is applied when a node is selected for expansion. BFS is optimal when all step costs are equal, as it finds the solution with the minimum number of steps.

**Key Characteristics:**
- **Completeness:** Always finds a solution if one exists
- **Optimality:** Optimal when path cost = depth (uniform step costs)
- **Time Complexity:** O(b^(d+1)) where b = branching factor, d = depth level  
- **Space Complexity:** O(b^d)

### Uniform-Cost Search (UCS):
Uniform-Cost Search expands the node with the lowest path cost g(n) instead of the shallowest node. Unlike BFS, UCS considers the actual cost of reaching each node, making it suitable for weighted graphs where edge costs vary.

**Solving the Weighted Graph Problem in exp 6.2.png:**
As shown in **exp 6.2.png**, the diagram presents a weighted graph with nodes S, E, F, C, D, A and edges with specific costs. UCS would solve this pathfinding problem as follows:

1. **Initialize:** Start at node S with cost 0, priority queue: [(0, S)]
2. **Step 1:** Expand S (lowest cost), add neighbors with cumulative costs:
   - To E: cost = 0 + edge_cost(S→E) 
   - To F: cost = 0 + edge_cost(S→F)
   - Priority queue updates with new costs
3. **Step 2:** Select node with lowest cumulative cost from frontier
4. **Step 3:** Expand selected node, calculate new path costs to neighbors:
   - For each neighbor: new_cost = current_cost + edge_cost
   - Add to priority queue if not explored or if better path found
5. **Step 4:** Continue until goal node (e.g., A) is selected for expansion

The red highlighted edges in the diagram likely represent the optimal path found by UCS. The algorithm maintains a priority queue ordered by cumulative path cost and always expands the node with the lowest total cost from the start, ensuring the optimal solution is found.

**Key Characteristics:**
- **Completeness:** Complete if step cost ≥ ε (some small positive constant)
- **Optimality:** Always finds the least-cost solution
- **Time/Space Complexity:** O(b^(⌈C*/ε⌉)) where C* is the cost of optimal solution
- **Priority Queue:** Uses min-heap to efficiently select lowest-cost node

**Key Differences from BFS:**
1. Uses priority queue ordered by path cost instead of FIFO queue
2. Goal test applied when node is selected for expansion, not when generated
3. Considers edge weights/costs, not just number of steps
4. May expand the same node multiple times if a better path is found
5. Optimal for any step cost function, not just uniform costs

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
