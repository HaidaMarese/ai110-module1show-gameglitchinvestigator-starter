# 🎮 Game Glitch Investigator: The Impossible Guesser

## 🚨 The Situation

This project investigates and repairs an AI-generated number guessing game built with Python and Streamlit. The starter game contained misleading hints, an incorrect initial attempt count, and a New Game button that did not fully reset the game.

I reproduced the bugs, documented their triggers, used AI assistance to propose focused repairs, reviewed the changes, and verified the results through pytest and manual gameplay.

## 🛠️ Setup

Run these commands from the project folder on Windows.

1. Create a virtual environment if one does not already exist:

   ```bat
   python -m venv .venv
   ```

2. Install dependencies using the project’s Python interpreter:

   ```bat
   .\.venv\Scripts\python.exe -m pip install -r requirements.txt
   ```

3. Start the game:

   ```bat
   .\.venv\Scripts\python.exe -m streamlit run app.py
   ```

4. Open the Local URL displayed in the terminal.

5. Run the automated tests:

   ```bat
   .\.venv\Scripts\python.exe -m pytest -v
   ```

## 🕵️‍♂️ Investigation and Repairs

- Played the game with different guesses and recorded reproducible bugs in reflection.md.
- Used Developer Debug Info to inspect the secret, attempts, score, and history.
- Moved check_guess from app.py into logic_utils.py and imported it into the app.
- Corrected the high and low hint messages.
- Removed the attempt-based conversion of the secret to a string so comparisons remain numeric.
- Updated the starter tests to check the outcome from the documented (outcome, message) tuple.
- Added two tests that check the exact hint messages.
- Fixed New Game to reset attempts, history, score, and status, and generate a secret within the selected difficulty’s range.
- Changed the initial attempt count from one to zero.

## 📝 Document Your Experience

- [x] Describe the game's purpose.
- [x] Detail which bugs you found.
- [x] Explain what fixes you applied.

### Game Purpose

The player guesses a secret number within the selected difficulty’s range. The game provides hints, tracks attempts and score, and ends when the player guesses correctly or runs out of attempts.

### Bugs Found

1. Normal difficulty initially showed one attempt used and seven remaining before any guesses.
2. A guess above the secret displayed “Go HIGHER!”, while a guess below the secret displayed “Go LOWER!”.
3. Clicking New Game after winning changed the secret but preserved the winning status, preventing further play.

The investigation also revealed that the app converted the secret to a string on alternating attempts. This could cause string comparisons instead of numeric comparisons.

### Fixes Applied and AI Collaboration

I used ChatGPT to clarify the project instructions and GitHub Copilot in VS Code to explain the code and suggest repairs and tests. I reviewed the changes before accepting them, ran pytest, and checked the game manually.

The guess comparison now lives in logic_utils.py and returns the correct outcome and hint. New Game clears the previous game’s state, and a fresh session starts with zero attempts used.

I changed Copilot’s proposed test command when it used another project’s Python environment. I ran the tests with this project’s .venv interpreter instead.

## 📸 Demo Walkthrough

The secret number is randomly generated. The following walkthrough combines the verified hint checks with the verified win-and-restart check.

1. Open the game in a fresh session and select Normal difficulty. The game starts with zero attempts used, eight remaining, score zero, and empty history.
2. In a game with secret 74, enter 75 and click Submit Guess. The game displays “📉 Go LOWER!”.
3. Enter 73 and click Submit Guess. The game displays “📈 Go HIGHER!”.
4. In a separate game started with New Game, Developer Debug Info shows secret 6, zero attempts, score zero, and empty history.
5. Enter 6 and click Submit Guess. The game displays “🎉 Correct!” and a winning message with a final score of 80.
6. Click New Game. In the verified run, the new secret was 5, attempts and score returned to zero, history was empty, and the old winning message disappeared.
7. In another Normal game with secret 40, submit 1 eight times. The game reaches zero attempts remaining and ends the session.
8. Click New Game after the loss. Attempts, score, and history reset, and the new secret is 53.
9. Submit 1 against secret 53. The game accepts the guess and correctly displays “📈 Go HIGHER!”.

Developer Debug Info is used here to make the verification steps explicit.

## 🧪 Test Results

Command:

```bat
.\.venv\Scripts\python.exe -m pytest -v
```

The latest run passed all five tests. The following is an excerpt from the terminal output:

```text
collected 5 items

tests/test_game_logic.py::test_winning_guess PASSED
tests/test_game_logic.py::test_guess_too_high PASSED
tests/test_game_logic.py::test_guess_too_low PASSED
tests/test_game_logic.py::test_guess_too_high_hint_says_go_lower PASSED
tests/test_game_logic.py::test_guess_too_low_hint_says_go_higher PASSED

5 passed in 0.06s
```

The automated tests cover winning, high and low outcomes, and the exact hint messages. Manual checks verified first-load attempts and restarting after both a win and a loss.

These checks verify the documented repairs; they do not cover every possible input, scoring rule, or difficulty-change scenario.

## 📁 Project Files

- app.py — Streamlit interface and game-session handling.
- logic_utils.py — refactored check_guess function; other starter helper placeholders remain unused by the app.
- tests/test_game_logic.py — three updated starter tests and two new hint-message tests.
- reflection.md — bug reproduction logs, AI collaboration, verification, and learning reflections.
- requirements.txt — project dependencies.

## 🚀 Stretch Features

No optional stretch challenge was completed.