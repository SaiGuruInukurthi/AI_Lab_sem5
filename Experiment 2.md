# Experiment 2

## Aim:
To write a Python program that simulates the behavior of a simple reflex agent for the vacuum cleaner problem in a world with two rooms.

## Description:
The vacuum cleaner problem is a classic search problem in Artificial Intelligence.[1] In this scenario, we have a simple agent, the vacuum cleaner, operating in a small environment consisting of two rooms. The goal of the agent is to clean both rooms, regardless of its starting position and the initial distribution of dirt.

The agent's behavior is determined by its current location and whether that location is dirty. The possible states in this problem are defined by the location of the vacuum cleaner and the status of both rooms (clean or dirty). With two rooms, there are a total of eight possible states. For example, the vacuum could be in the left room with both rooms being dirty, or it could be in the right room with the left room clean and the right room dirty. The agent can perform three basic actions: move left, move right, and suck up dirt.[2] The ultimate goal is to reach a state where both rooms are clean.

The state transition diagram below illustrates the possible transitions between states based on the agent's actions. The agent's performance can be evaluated based on factors like the time taken and the path followed to clean the rooms.

## State Transition Diagram:
```
R
  <--[L,D,R,D]-->
  |      L      |
  S             S
  |             |
  -->[L,C,R,D]<--
      |      R      |
      L             S
      |             |
  <--[L,D,R,C]-->
      |      R      |
      L             S
      |             |
      [L,C,R,C]
      [R,C,L,C]
```

**Legend:**
- L: Left Room State
- R: Right Room State
- C: Clean
- D: Dirty
- VC: Vacuum Cleaner Position
- S: Suck
- <-->: Move Left/Right

## Programme:

```python
# Class Declaration
class VacuumCleaner1x2:
    # Initialization (__init__)
    # self.x = 0, self.y = 0 → Start position at (0, 0)
    # self.room_width = 2 → 2 tiles (1x2 room)
    # Initializes the room grid with all clean tiles:
    def __init__(self, dirty_positions=[(0, 0), (1, 0)]):
        self.x = 0  # Start at left tile (0,0)
        self.y = 0
        self.room_width = 2
        self.room = [['C' for _ in range(self.room_width)]]

        # Mark dirty tiles
        # Then it sets specific tiles to dirty ('D'), based on dirty_positions:
        for (dx, dy) in dirty_positions:  # Iterates through a list of tile coordinates that should be dirty. Example: If dirty_positions = [
            if 0 <= dx < self.room_width and dy == 0:  # Ensures the given coordinates are valid and within the bounds of the 1x2 room.
                self.room[dy][dx] = 'D'
        # Then, Displays initial status using self.display_room()
        # Cleans the starting tile using self.clean_tile()
        print("Vacuum Cleaner initialized at position (0, 0)")
        self.display_room()
        self.clean_tile()

    # Movement Method
    # Allows movement in two directions:
    # 'RIGHT': increases x by 1 (if not already at right edge)
    # 'LEFT': decreases x by 1 (if not already at left edge)
    # Prints the new position
    # Cleans the tile after moving to it:
    def move(self, direction):
        if direction == 'RIGHT' and self.x < self.room_width - 1:
            self.x += 1
        elif direction == 'LEFT' and self.x > 0:
            self.x -= 1
        else:
            print("Invalid move or boundary reached.")
            return
        print(f"\nMoved {direction} to ({self.x}, {self.y})")
        self.clean_tile()

    # Cleaning Method
    # Cleans the current tile if it's dirty.
    # If the current tile has 'D' (dirty), it changes it to 'C' (clean) and prints:
    # If already clean, prints: is already clean.
    # Then displays the current room status.
    def clean_tile(self):
        if self.room[self.y][self.x] == 'D':
            self.room[self.y][self.x] = 'C'
            print(f"Cleaned tile at ({self.x}, {self.y})")
        else:
            print(f"Tile at ({self.x}, {self.y}) is already clean.")
        self.display_room()

    # Prints the current room layout, showing the vacuum position and tile cleanliness.
    # If vacuum is on tile (0,0) and both tiles are clean: V(C) C
    # If vacuum is on tile (1,0): C V(C)
    def display_room(self):
        print("\nRoom status (C = Clean, D = Dirty):")
        row = ""  # Initializes an empty string to build the visual representation of the room row.
        for col in range(self.room_width):  # Iterates over each column in the room.
            if self.x == col:  # Checks if the vacuum cleaner is at this column.
                row += f"V({self.room[0][col]}) "  # V(C) → Vacuum on a clean tile, V(D) → Vacuum on a dirty tile
            else:
                row += f" {self.room[0][col]} "  # Just displays the state of the tile (C or D) without the vacuum.
        print(row + "\n")


# Run the program- This is the main block that runs when the script is executed.
if __name__ == "__main__":
    # Choose dirty tile positions: both dirty by default
    # Creates an instance of the vacuum cleaner.
    # Both tiles are initially dirty.
    cleaner = VacuumCleaner1x2(dirty_positions=[(0, 0), (1, 0)])

    # Movement commands: move right then left
    # The vacuum moves right to tile (1,0), cleans it.
    # Then moves left back to (0,0) and checks/cleans again.
    commands = ['RIGHT', 'LEFT']
    for cmd in commands:  # On each loop iteration, the variable cmd holds the current direction (either 'RIGHT' or 'LEFT').
        cleaner.move(cmd)
```

## Output:
```
Vacuum Cleaner initialized at position (0, 0)

Room status (C = Clean, D = Dirty):
V(D)  D 

Cleaned tile at (0, 0)

Room status (C = Clean, D = Dirty):
V(C)  D 


Moved RIGHT to (1, 0)
Cleaned tile at (1, 0)

Room status (C = Clean, D = Dirty):
C V(C) 


Moved LEFT to (0, 0)
Tile at (0, 0) is already clean.

Room status (C = Clean, D = Dirty):
V(C)  C
```

## Explanation of the output:
The program simulates a vacuum cleaner in a two-room environment.

1. **Initialization**: The vacuum cleaner starts in the left room (position 0,0), and both rooms are initially dirty (V(D) D).
2. **First Action (Clean)**: Since the starting room is dirty, the vacuum cleans it. The status of the left room changes from 'D' (Dirty) to 'C' (Clean), and the display shows V(C) D.
3. **Second Action (Move Right)**: The agent then moves to the right room. The output indicates this movement and its new position (1, 0).
4. **Third Action (Clean)**: Upon arriving in the right room and finding it dirty, the vacuum cleans it. The room status is updated to show both rooms are now clean (C V(C)).
5. **Fourth Action (Move Left)**: The agent moves back to the left room.
6. **Final State**: Upon returning to the left room, it checks the status and finds it already clean. The final output V(C) C shows that both rooms are clean and the vacuum is in the left room, successfully completing its goal.

## Explanation of the Programme and Output:

The program defines a `VacuumCleaner1x2` class to represent the agent and its environment. The main execution block (`if __name__ == "__main__":`) creates an instance of this agent and directs its actions.

### Initialization (`__init__` method):
**Code**: When `cleaner = VacuumCleaner1x2(dirty_positions=[(0, 0), (1, 0)])` is executed, the `__init__` method is called. It sets the vacuum's starting position `self.x` to 0 (the left room). It creates a list `self.room` representing the two rooms, initially all 'C' (Clean). It then iterates through the `dirty_positions` list and changes the corresponding tiles to 'D' (Dirty).

**Output**: The code then prints `Vacuum Cleaner initialized at position (0, 0)`. The `display_room()` method is called, which shows the initial state: `V(D) D`. This indicates the vacuum (V) is in the left room, which is dirty (D), and the right room is also dirty (D).

**Code**: Immediately after displaying the state, the `__init__` method calls `self.clean_tile()`.

**Output**: The `clean_tile()` method checks the current room (position 0) and finds it is 'D'. It changes it to 'C' and prints `Cleaned tile at (0, 0)`. It then calls `display_room()` again to show the updated status: `V(C) D`.

### Main Execution Loop (`for cmd in commands`):
**Code**: The program defines a list of actions: `commands = ['RIGHT', 'LEFT']`. It then loops through this list, calling the `cleaner.move()` method for each command.

#### First Iteration: Moving Right
**Code**: The loop first calls `cleaner.move('RIGHT')`. Inside the move method, the condition `direction == 'RIGHT'` is true, so `self.x` is incremented to 1.

**Output**: The program prints `Moved RIGHT to (1, 0)`.

**Code**: The move method then immediately calls `self.clean_tile()`. The agent is now at position x=1. The `clean_tile` method checks this room, finds it is 'D', changes it to 'C', and prints a confirmation.

**Output**: The output shows `Cleaned tile at (1, 0)`. The subsequent call to `display_room()` shows the new state: `C V(C)`, indicating the left room is clean and the vacuum is in the right room, which is now also clean.

#### Second Iteration: Moving Left
**Code**: The loop proceeds to the next command, calling `cleaner.move('LEFT')`. The move method updates `self.x` back to 0.

**Output**: The program prints `Moved LEFT to (0, 0)`.

**Code**: move again calls `self.clean_tile()`. This time, the agent is at x=0. The if condition `self.room[self.y][self.x] == 'D'` is false, as this tile is now 'C'. The else block is executed.

**Output**: The program prints `Tile at (0, 0) is already clean.`. Finally, `display_room()` is called one last time, showing the final state `V(C) C`, which confirms the agent's goal is complete.
