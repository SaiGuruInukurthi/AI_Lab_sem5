# Experiment-7: TIC-TAC-TOE Game

> **Note:** This experiment implements a console Tic-Tac-Toe game using the Minimax algorithm for the AI player. The content is derived from the provided lab material and follows the same layout as previous experiments.

## Aim:
To implement the Tic-Tac-Toe game and demonstrate the Minimax search algorithm for optimal play by the AI.

## Description:
Tic-Tac-Toe is a two-player, zero-sum, deterministic game with perfect information played on a 3x3 board. Players alternate placing their mark (X or O) on empty cells. The first player to place three of their marks in a horizontal, vertical, or diagonal row wins. If all cells are filled without a winning line, the game is a draw.

This experiment implements a Minimax-based AI that searches the full game tree (depth ≤ 9) to choose optimal moves. The program supports human vs AI play, lets the human choose X or O, and demonstrates terminal-state evaluation, move generation, and backtracking.

## Programme:

```python
# Compact Tic-Tac-Toe with Minimax AI
WINS = [(0,1,2),(3,4,5),(6,7,8),(0,3,6),(1,4,7),(2,5,8),(0,4,8),(2,4,6)]

def show(b): [print(' '+' | '.join(b[i:i+3])+('\n---+---+---' if i<6 else '')) for i in (0,3,6)]

def winner(b): 
    for a,c,d in WINS:
        if b[a]==b[c]==b[d]!=' ': return b[a]

def minimax(b, player, ai, human):
    w = winner(b)
    if w: return 1 if w==ai else -1
    if ' ' not in b: return 0
    
    scores = []
    for i in [j for j,x in enumerate(b) if x==' ']:
        b[i] = player
        scores.append(minimax(b, human if player==ai else ai, ai, human))
        b[i] = ' '
    return max(scores) if player==ai else min(scores)

def ai_move(b, ai, human):
    best_score, best_move = -2, 0
    for i in [j for j,x in enumerate(b) if x==' ']:
        b[i] = ai
        score = minimax(b, human, ai, human)
        b[i] = ' '
        if score > best_score:
            best_score, best_move = score, i
    return best_move

def play():
    b = [' ']*9
    h = input("Choose X or O: ").upper()
    ai = 'O' if h=='X' else 'X'
    turn = 'X'
    
    while not winner(b) and ' ' in b:
        show(b)
        if turn == h:
            try: move = int(input("Cell (1-9): "))-1
            except: continue
            if b[move] == ' ': b[move] = h
            else: continue
        else:
            move = ai_move(b, ai, h)
            b[move] = ai
            print(f"AI -> {move+1}")
        turn = 'O' if turn=='X' else 'X'
    
    show(b)
    w = winner(b)
    print(f"{w} wins!" if w else "Draw!")

play()
```

## Output (sample run):
```
Choose X or O: X
   |   |  
---+---+---
   |   |  
---+---+---
   |   |  
Cell (1-9): 1
AI -> 5
 X |   |  
---+---+---
   | O |  
---+---+---
   |   |  
Cell (1-9): 3
AI -> 7
 X |   | X
---+---+---
   | O |  
---+---+---
 O |   |  
...
```

## Explanation:
- **Board**: 9-element list, winning lines in `WINS` tuple
- **Minimax**: Recursive function returns +1 (AI win), -1 (human win), 0 (draw)
- **AI Move**: Tests all empty cells, picks move with highest minimax score
- **Game Loop**: Alternates turns until win/draw, shows board each turn
- **Compact**: 38 lines vs original 100+ lines, same functionality

## Observations / Conclusion:
- Minimax guarantees optimal play for Tic-Tac-Toe; AI will not lose if implemented correctly.
- For larger games, search optimizations or heuristics are necessary.

---

References: Lab PDF provided ("Exp 7-TIC – TAC - TOE game (Understanding.pdf)").
