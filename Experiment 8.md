# Experiment 8

## Aim:
To implement a Hangman word-guessing game in Python using artificial intelligence concepts to demonstrate interactive game playing, state management, and user interaction.

## Description:
Hangman is a classic word-guessing game where a player tries to guess a hidden word by suggesting letters within a certain number of attempts. The game is an excellent demonstration of AI concepts such as state representation, decision-making, and game logic.

In this implementation, the computer selects a random word from a predefined list, and the player attempts to guess it letter by letter. The game provides feedback after each guess and tracks the number of remaining attempts.

**State Representation**: The game state includes the hidden word, correctly guessed letters, incorrect guesses, and remaining attempts.

**Game Mechanics**: The player guesses one letter at a time. If the letter is in the word, it's revealed in its correct position(s). If not, the number of remaining attempts decreases. The game continues until the player either guesses the word completely or runs out of attempts.

## Problem Setup:

**Game Components**:
- **Word List**: A collection of words from which the game randomly selects one
- **Hidden Word**: The target word displayed as underscores initially
- **Guessed Letters**: Track of all letters the player has guessed
- **Max Attempts**: Maximum number of incorrect guesses allowed (typically 6)
- **Remaining Attempts**: Current number of attempts left

**Win Condition**: All letters in the word are correctly guessed

**Lose Condition**: Player runs out of attempts before guessing the word

## Programme:

```python
import random

class HangmanGame:
    """
    A class to represent the Hangman game.
    """
    
    def __init__(self, word_list, max_attempts=6):
        """
        Initialize the game with a word list and maximum attempts.
        """
        self.word_list = word_list
        self.max_attempts = max_attempts
        self.secret_word = ""
        self.guessed_letters = set()
        self.incorrect_guesses = set()
        self.remaining_attempts = max_attempts
        self.game_over = False
        self.won = False
    
    def start_new_game(self):
        """
        Start a new game by selecting a random word and resetting the game state.
        """
        self.secret_word = random.choice(self.word_list).upper()
        self.guessed_letters = set()
        self.incorrect_guesses = set()
        self.remaining_attempts = self.max_attempts
        self.game_over = False
        self.won = False
        print("\n" + "="*50)
        print("NEW HANGMAN GAME STARTED!")
        print("="*50)
    
    def get_display_word(self):
        """
        Return the current state of the word with guessed letters revealed.
        """
        display = ""
        for letter in self.secret_word:
            if letter in self.guessed_letters:
                display += letter + " "
            else:
                display += "_ "
        return display.strip()
    
    def display_hangman(self):
        """
        Display the hangman figure based on remaining attempts.
        """
        stages = [
            # Stage 6 - Final stage
            """
               --------
               |      |
               |      O
               |     \\|/
               |      |
               |     / \\
               -
            """,
            # Stage 5
            """
               --------
               |      |
               |      O
               |     \\|/
               |      |
               |     / 
               -
            """,
            # Stage 4
            """
               --------
               |      |
               |      O
               |     \\|/
               |      |
               |      
               -
            """,
            # Stage 3
            """
               --------
               |      |
               |      O
               |     \\|
               |      |
               |     
               -
            """,
            # Stage 2
            """
               --------
               |      |
               |      O
               |      |
               |      |
               |     
               -
            """,
            # Stage 1
            """
               --------
               |      |
               |      O
               |    
               |      
               |     
               -
            """,
            # Stage 0 - Initial stage
            """
               --------
               |      |
               |      
               |    
               |      
               |     
               -
            """
        ]
        return stages[self.remaining_attempts]
    
    def make_guess(self, letter):
        """
        Process a player's guess and update the game state.
        """
        letter = letter.upper()
        
        # Check if letter was already guessed
        if letter in self.guessed_letters or letter in self.incorrect_guesses:
            print(f"\nYou already guessed '{letter}'. Try a different letter.")
            return False
        
        # Check if the guess is valid (single letter)
        if len(letter) != 1 or not letter.isalpha():
            print("\nPlease enter a single letter.")
            return False
        
        # Process the guess
        if letter in self.secret_word:
            self.guessed_letters.add(letter)
            print(f"\nGood guess! '{letter}' is in the word.")
            
            # Check if the player has won
            if set(self.secret_word) == self.guessed_letters:
                self.won = True
                self.game_over = True
        else:
            self.incorrect_guesses.add(letter)
            self.remaining_attempts -= 1
            print(f"\nSorry! '{letter}' is not in the word.")
            
            # Check if the player has lost
            if self.remaining_attempts == 0:
                self.game_over = True
        
        return True
    
    def display_game_state(self):
        """
        Display the current state of the game.
        """
        print("\n" + self.display_hangman())
        print(f"\nWord: {self.get_display_word()}")
        print(f"Remaining attempts: {self.remaining_attempts}")
        print(f"Incorrect guesses: {', '.join(sorted(self.incorrect_guesses)) if self.incorrect_guesses else 'None'}")
        print(f"Correct guesses: {', '.join(sorted(self.guessed_letters)) if self.guessed_letters else 'None'}")
    
    def play(self):
        """
        Main game loop.
        """
        self.start_new_game()
        
        while not self.game_over:
            self.display_game_state()
            guess = input("\nEnter your guess (a single letter): ").strip()
            self.make_guess(guess)
        
        # Display final game state
        self.display_game_state()
        
        # Display result
        if self.won:
            print("\n" + "="*50)
            print("🎉 CONGRATULATIONS! YOU WON! 🎉")
            print(f"The word was: {self.secret_word}")
            print("="*50)
        else:
            print("\n" + "="*50)
            print("😢 GAME OVER! YOU LOST! 😢")
            print(f"The word was: {self.secret_word}")
            print("="*50)


# Main program
if __name__ == "__main__":
    # Define a list of words for the game
    word_list = [
        "python", "javascript", "programming", "algorithm", "computer",
        "artificial", "intelligence", "machine", "learning", "network",
        "database", "software", "hardware", "keyboard", "monitor"
    ]
    
    # Create and play the game
    game = HangmanGame(word_list, max_attempts=6)
    
    while True:
        game.play()
        
        # Ask if the player wants to play again
        play_again = input("\nDo you want to play again? (yes/no): ").strip().lower()
        if play_again not in ['yes', 'y']:
            print("\nThanks for playing Hangman! Goodbye!")
            break
```

## Output:
```
==================================================
NEW HANGMAN GAME STARTED!
==================================================

               --------
               |      |
               |      
               |    
               |      
               |     
               -
            
Word: _ _ _ _ _ _
Remaining attempts: 6
Incorrect guesses: None
Correct guesses: None

Enter your guess (a single letter): p

Good guess! 'P' is in the word.

               --------
               |      |
               |      
               |    
               |      
               |     
               -
            
Word: P _ _ _ _ _
Remaining attempts: 6
Incorrect guesses: None
Correct guesses: P

Enter your guess (a single letter): y

Good guess! 'Y' is in the word.

               --------
               |      |
               |      
               |    
               |      
               |     
               -
            
Word: P Y _ _ _ _
Remaining attempts: 6
Incorrect guesses: None
Correct guesses: P, Y

Enter your guess (a single letter): t

Good guess! 'T' is in the word.

               --------
               |      |
               |      
               |    
               |      
               |     
               -
            
Word: P Y T _ _ _
Remaining attempts: 6
Incorrect guesses: None
Correct guesses: P, T, Y

Enter your guess (a single letter): h

Good guess! 'H' is in the word.

               --------
               |      |
               |      
               |    
               |      
               |     
               -
            
Word: P Y T H _ _
Remaining attempts: 6
Incorrect guesses: None
Correct guesses: H, P, T, Y

Enter your guess (a single letter): o

Good guess! 'O' is in the word.

               --------
               |      |
               |      
               |    
               |      
               |     
               -
            
Word: P Y T H O _
Remaining attempts: 6
Incorrect guesses: None
Correct guesses: H, O, P, T, Y

Enter your guess (a single letter): n

Good guess! 'N' is in the word.

               --------
               |      |
               |      
               |    
               |      
               |     
               -
            
Word: P Y T H O N
Remaining attempts: 6
Incorrect guesses: None
Correct guesses: H, N, O, P, T, Y

==================================================
🎉 CONGRATULATIONS! YOU WON! 🎉
The word was: PYTHON
==================================================

Do you want to play again? (yes/no): no

Thanks for playing Hangman! Goodbye!
```

## Explanation of the output:
The output demonstrates a successful game of Hangman where the player correctly guesses the word "PYTHON" without making any incorrect guesses.

1. **Game Initialization**: The game starts by selecting a random word from the word list and displaying the initial state with all letters hidden as underscores.

2. **Letter 'P' Guess**: The player guesses 'P', which is in the word. The display updates to show "P _ _ _ _ _" and the letter is added to the correct guesses list.

3. **Letter 'Y' Guess**: The player guesses 'Y', another correct letter. The word display becomes "P Y _ _ _ _".

4. **Letter 'T' Guess**: The player correctly guesses 'T', revealing more of the word: "P Y T _ _ _".

5. **Letter 'H' Guess**: The player guesses 'H', which is in the word. The display shows "P Y T H _ _".

6. **Letter 'O' Guess**: The player correctly guesses 'O', and the word becomes "P Y T H O _".

7. **Letter 'N' Guess**: The final guess 'N' completes the word "P Y T H O N", triggering the win condition.

8. **Victory Message**: The game displays a congratulations message and reveals the complete word. Since the player made no incorrect guesses, all 6 attempts remain unused.

## Explanation of the Programme and Output:

The program implements an interactive word-guessing game using object-oriented programming principles and demonstrates several AI and game development concepts.

### State Representation:
The game state is maintained through class attributes including `secret_word`, `guessed_letters`, `incorrect_guesses`, and `remaining_attempts`. This comprehensive state tracking allows the game to make decisions about valid moves and end conditions.

### Game Logic and Decision Making:
The `make_guess()` method implements the core game logic, validating input, checking if letters have been guessed before, and updating the game state accordingly. It demonstrates conditional decision-making based on the current state.

### Visual Feedback:
The `display_hangman()` method provides visual feedback through ASCII art representations of the hangman at different stages. This enhances user experience and makes the game more engaging.

### Win/Lose Conditions:
The program checks for two terminal conditions:
- **Win**: When all unique letters in the secret word have been correctly guessed (`set(self.secret_word) == self.guessed_letters`)
- **Lose**: When the player runs out of attempts (`remaining_attempts == 0`)

### Interactive Loop:
The `play()` method implements the main game loop, continuously accepting user input and updating the game state until a terminal condition is reached. This demonstrates interactive agent behavior responding to user actions.

### Replayability:
The program includes functionality to play multiple games in succession, allowing the user to continue playing or exit gracefully after each game concludes.
