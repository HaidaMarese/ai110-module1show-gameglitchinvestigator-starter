# 💭 Reflection: Game Glitch Investigator

Answer each question in 3 to 5 sentences. Be specific and honest about what actually happened while you worked. This is about your process, not trying to sound perfect.

## 1. What was broken when you started?

The game opened successfully in Streamlit with a difficulty selector, a guess input, and Developer Debug Info. In Normal difficulty, it showed one attempt used and seven remaining before I submitted any guesses, even though the history was empty. In my first game, guessing 40 against a secret of 31 displayed “Go HIGHER!”, and in my second game, guessing 20 against a secret of 71 displayed “Go LOWER!”, so both hints pointed in the wrong direction. After guessing 31 correctly in the first game, I won with a final score of 65, but clicking New Game changed the secret to 11 while still displaying “You already won. Start a new game to play again.” These observations pointed to problems in the initial attempt count, the messages returned by check_guess, and the New Game session-state reset.

**Bug Reproduction Log**

| Input / Trigger | Expected Behavior | Actual Behavior | Console Output / Error | Suspected Code Location |
|---|---|---|---|---|
| Open Normal difficulty without submitting a guess | Attempts = 0; attempts left = 8 | Attempts = 1; attempts left = 7; history is empty | None | app.py: initial session-state setup sets st.session_state.attempts = 1 |
| Submit 40 when the secret is 31 | Hint tells me to go lower | Hint tells me to go higher | “📈 Go HIGHER!” | app.py: check_guess, branch where guess > secret |
| Submit 20 when the secret is 71 | Hint tells me to go higher | Hint tells me to go lower | “📉 Go LOWER!” | app.py: check_guess, branch where guess < secret |
| Guess 31 correctly, then click New Game | A new game starts and accepts guesses | The secret changes to 11, but the game still says I already won and prevents further play | “You already won. Start a new game to play again.” | app.py: new_game block does not reset status to "playing"; the status guard calls st.stop() |

---

## 2. How did you use AI as a teammate?

I used ChatGPT to clarify the project instructions and GitHub Copilot in VS Code to explain the code and suggest changes and tests. One correct suggestion was to move check_guess into logic_utils.py, correct the hint directions, and remove the conversion of the secret to a string so comparisons stayed numeric. I reviewed the changes and verified them with five passing tests and manual guesses that produced the correct hints. I did not accept Copilot’s proposed test command using the Playlist Chaos project’s Python environment because it belonged to a different project, so I skipped it and used this project’s .venv interpreter instead. The pytest output confirmed the correct interpreter path and showed all five tests passing.

---

## 3. Debugging and testing your fixes

I checked repairs by reviewing the code differences, running automated tests, and repeating the original actions in Streamlit. Running `.\.venv\Scripts\python.exe -m pytest -v` produced five passing tests, including two new tests checking that 60 against 50 says “Go LOWER!” and 40 against 50 says “Go HIGHER!”. Copilot helped update the starter tests to unpack the documented (outcome, message) tuple and added the message tests. I manually verified that New Game reset attempts, score, history, and playing status after both a win and a loss, and that the restarted game accepted another guess. In a fresh session, I also confirmed that Normal difficulty began with zero attempts used and eight remaining; the automated tests cover guess logic, while the manual checks cover the interface and session state.

---

## 4. What did you learn about Streamlit and state?

I learned that interacting with a Streamlit widget can cause the Python script to run again from top to bottom. Ordinary variables can be recreated during a rerun, while session state preserves values such as the secret number, attempts, score, history, and game status. I would explain session state to a friend as the game’s memory between interactions. The New Game bug showed that changing the secret alone was not enough because the old winning status stayed in memory and blocked further play. Resetting the game’s session values before rerunning allowed a new game to begin.

---

## 5. Looking ahead: your developer habits

I want to keep the habit of reproducing a bug, recording its trigger, and checking the repair with both automated tests and manual play. I also want to continue reviewing AI-generated changes and making separate Git commits for meaningful stages of my work. Next time, I would specify the project folder and Python interpreter earlier so the AI uses the correct environment. This project showed me that AI-generated code needs verification even when it looks convincing or claims to be production-ready. Keeping a human-in-the-loop helps me decide which suggestions fit the project and whether the results support accepting them.