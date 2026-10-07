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

- Which AI tools did you use on this project (for example: ChatGPT, Gemini, Copilot)?
- Give one example of an AI suggestion that was correct, including what the AI suggested and how you verified the result.
- Give one example of an AI suggestion you did not accept as written. Include what the AI suggested, why you rejected or changed it, and how you verified your version. It does not have to have been wrong: over-engineered, out of scope, harder to read, or a poor fit for this codebase all count.

---

## 3. Debugging and testing your fixes

- How did you decide whether a bug was really fixed?
- Describe at least one test you ran, manually or using pytest, and what it showed you about your code.
- Did AI help you design or understand any tests? How?

---

## 4. What did you learn about Streamlit and state?

- How would you explain Streamlit reruns and session state to a friend who has never used Streamlit?

---

## 5. Looking ahead: your developer habits

- What is one habit or strategy from this project that you want to reuse in future labs or projects? This could be a testing habit, a prompting strategy, or a way you used Git.
- What is one thing you would do differently next time you work with AI on a coding task?
- In one or two sentences, describe how this project changed the way you think about AI-generated code.