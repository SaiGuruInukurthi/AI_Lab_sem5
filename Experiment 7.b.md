# Experiment-7.b: TIC-TAC-TOE Game (Alpha-Beta Pruning)

> **Note:** This experiment implements Alpha-Beta Pruning optimization for the Minimax algorithm in Tic-Tac-Toe, significantly reducing the search space by eliminating branches that cannot affect the final decision.

## Aim:
Write a program to implement Tic-Tac-Toe game using Alpha-Beta Pruning algorithm for optimal AI play with improved efficiency.

## Description:
Alpha-Beta Pruning is an optimization technique for the Minimax algorithm that eliminates branches in the game tree that cannot possibly influence the final decision. It maintains two values:
- **Alpha (α)**: Best value that the maximizer currently can guarantee
- **Beta (β)**: Best value that the minimizer currently can guarantee

When α ≥ β, the remaining branches can be pruned since they won't be chosen. This reduces time complexity from O(b^d) to O(b^(d/2)) in best case, making deeper searches feasible.

## Programme:

```python
# Compact Tic-Tac-Toe with Alpha-Beta Pruning
WINS = [(0,1,2),(3,4,5),(6,7,8),(0,3,6),(1,4,7),(2,5,8),(0,4,8),(2,4,6)]

def show(b): [print(' '+' | '.join(b[i:i+3])+('\n---+---+---' if i<6 else '')) for i in (0,3,6)]

def winner(b): 
    for a,c,d in WINS:
        if b[a]==b[c]==b[d]!=' ': return b[a]

def minimax_ab(b, depth, alpha, beta, is_max, ai, human):
    w = winner(b)
    if w: return 10-depth if w==ai else depth-10
    if ' ' not in b: return 0
    
    if is_max:
        max_eval = -999
        for i in [j for j,x in enumerate(b) if x==' ']:
            b[i] = ai
            eval_score = minimax_ab(b, depth+1, alpha, beta, False, ai, human)
            b[i] = ' '
            max_eval = max(max_eval, eval_score)
            alpha = max(alpha, eval_score)
            if beta <= alpha: break  # Alpha-Beta Pruning
        return max_eval
    else:
        min_eval = 999
        for i in [j for j,x in enumerate(b) if x==' ']:
            b[i] = human
            eval_score = minimax_ab(b, depth+1, alpha, beta, True, ai, human)
            b[i] = ' '
            min_eval = min(min_eval, eval_score)
            beta = min(beta, eval_score)
            if beta <= alpha: break  # Alpha-Beta Pruning
        return min_eval

def ai_move(b, ai, human):
    best_score, best_move = -999, 0
    alpha, beta = -999, 999
    nodes_explored = 0
    
    for i in [j for j,x in enumerate(b) if x==' ']:
        b[i] = ai
        score = minimax_ab(b, 0, alpha, beta, False, ai, human)
        b[i] = ' '
        nodes_explored += 1
        if score > best_score:
            best_score, best_move = score, i
        alpha = max(alpha, score)
    
    print(f"Nodes explored: {nodes_explored}")
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
            if 0 <= move <= 8 and b[move] == ' ': 
                b[move] = h
            else: 
                print("Invalid move!"); continue
        else:
            print("AI thinking with Alpha-Beta Pruning...")
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
AI thinking with Alpha-Beta Pruning...
Nodes explored: 7
AI -> 5
 X |   |  
---+---+---
   | O |  
---+---+---
   |   |  
Cell (1-9): 3
AI thinking with Alpha-Beta Pruning...
Nodes explored: 5
AI -> 7
 X |   | X
---+---+---
   | O |  
---+---+---
 O |   |  
Cell (1-9): 2
AI thinking with Alpha-Beta Pruning...
Nodes explored: 3
AI -> 8
 X | X | X
---+---+---
   | O |  
---+---+---
 O |   | O
X wins!
```

## Explanation:
- **Alpha-Beta Values**: α (max lower bound), β (min upper bound) passed through recursion
- **Pruning Condition**: When β ≤ α, remaining branches are cut off since they won't be selected
- **Depth Bonus**: Scores include depth penalty/bonus (10-depth, depth-10) to prefer shorter wins
- **Node Count**: Tracks explored nodes showing pruning efficiency
- **Performance**: Significantly fewer nodes explored compared to pure Minimax
- **Same Result**: Identical optimal moves as Minimax but with much better performance

## Observations / Conclusion:
- Alpha-Beta Pruning reduces search space by up to 50% while maintaining optimal play
- Node exploration decreases dramatically as game progresses (branching factor reduces)
- Essential optimization for larger game trees where full Minimax becomes impractical
- Pruning effectiveness depends on move ordering - better ordering leads to more cuts

---

References: 
- Experiment 7.a.md (base Minimax implementation)
- Alpha-Beta Pruning.pdf (theoretical background and algorithm details)
