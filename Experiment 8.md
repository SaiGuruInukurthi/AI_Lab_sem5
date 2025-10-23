# Experiment 8

## Aim:
Write a program to implement Hangman game (Or Wordle).

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

def display_word(word, guessed_letters):
    """
    Display the current state of the word with guessed letters revealed.
    """
    display = ""
    for letter in word:
        if letter in guessed_letters:
            display += letter + " "
        else:
            display += "_ "
    return display.strip()

def hangman_game():
    """
    Main function to play the Hangman game.
    """
    # List of fruit names for the game
    word_list = ["apple", "banana", "orange", "mango", "grape", "cherry", 
                 "lemon", "peach", "melon", "berry"]
    
    # Select a random word from the list
    word = random.choice(word_list).lower()
    
    # Set to store guessed letters
    guessed_letters = set()
    
    # Display game start
    print("Guess the word! HINT: word is a name of a fruit")
    
    # Display initial state (all underscores)
    print(display_word(word, guessed_letters))
    
    # Game loop
    while True:
        # Get user input
        guess = input("Enter a letter to guess: ").lower()
        
        # Validate input
        if len(guess) != 1 or not guess.isalpha():
            print("Please enter a single letter.")
            continue
        
        # Check if letter was already guessed
        if guess in guessed_letters:
            print("You already guessed that letter. Try another one.")
            continue
        
        # Add the guess to the set
        guessed_letters.add(guess)
        
        # Display current state of the word
        current_display = display_word(word, guessed_letters)
        print(current_display)
        
        # Check if the player has won
        if all(letter in guessed_letters for letter in word):
            print(f"The word is: {word}")
            print("Congratulations, You won!")
            break

# Run the game
if __name__ == "__main__":
    hangman_game()
```

## Output:
```
Guess the word! HINT: word is a name of a fruit
_ _ _ _ _ _
Enter a letter to guess: b
b _ _ _ _ _
Enter a letter to guess: a
b a _ a _ a
Enter a letter to guess: n
b a n a n a
The word is: banana
Congratulations, You won!
```

## Explanation of the output:
The output demonstrates a successful game of Hangman where the player correctly guesses the word "banana" by entering three strategic letters.

1. **Game Start**: The game begins by displaying a hint that the word is a fruit name, followed by the word represented as six underscores "_ _ _ _ _ _", indicating a six-letter word.

2. **First Guess - Letter 'b'**: The player guesses the letter 'b'. Since 'b' is the first letter of "banana", the display updates to show "b _ _ _ _ _", revealing the position of 'b' at the start of the word.

3. **Second Guess - Letter 'a'**: The player guesses 'a', which appears multiple times in the word "banana". The display updates to "b a _ a _ a", revealing all three occurrences of the letter 'a' in positions 2, 4, and 6.

4. **Third Guess - Letter 'n'**: The player guesses 'n', which is also present in the word. The display now shows "b a n a n a", completing the entire word. At this point, all letters have been guessed correctly.

5. **Game Completion**: The program detects that all letters in the word have been guessed, displays the complete word "banana", and shows a congratulations message, indicating the player has won the game.

This example demonstrates an optimal game where the player wins after just three guesses by choosing letters that appear multiple times in the target word.

## Explanation of the Programme and Output:

The program implements a simplified word-guessing game that demonstrates fundamental AI concepts including state tracking, user interaction, and win-condition evaluation.

### State Representation:
The game maintains its state through two key variables: the secret `word` (selected randomly from a fruit list) and `guessed_letters` (a set tracking all letters the player has guessed). This minimal state representation is sufficient to determine game progress and completion.

### Word Display Function:
The `display_word()` function is the core visualization component. It iterates through each letter in the secret word, displaying the actual letter if it has been guessed, or an underscore if it hasn't. This provides immediate visual feedback to the player about their progress.

### Game Loop and Logic:
The main `hangman_game()` function implements a simple but effective game loop:
- It continuously prompts the user for letter guesses
- Validates that the input is a single alphabetic character
- Checks for duplicate guesses to avoid redundancy
- Updates the display after each valid guess
- Checks the win condition after every guess

### Win Condition:
The program uses Python's `all()` function with a generator expression to check if every letter in the word has been guessed: `all(letter in guessed_letters for letter in word)`. This elegant approach eliminates the need for manual iteration and makes the code more readable.

### User Experience:
The game provides clear prompts and feedback at each step. The hint "word is a name of a fruit" gives context, while the progressive letter revelation keeps the player engaged. The simple output format makes it easy to follow the game's progress without unnecessary complexity.

### Simplicity and Focus:
Unlike more complex implementations with attempt limits and hangman drawings, this version focuses on the core word-guessing mechanic. This makes it ideal for demonstrating basic AI agent concepts without overwhelming detail, while still providing an engaging interactive experience.
