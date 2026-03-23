# 🧠 Quiz App

A lightweight, browser-based quiz application built with vanilla HTML, CSS, and JavaScript — no frameworks or libraries required.

---

## 📁 Project Structure

```
quiz-game/
├── index.html   # App markup and screen layout
├── style.css    # Styling, responsive design, and animations
└── script.js    # Quiz logic, state management, and DOM manipulation
```

---

## 🚀 How to Run

1. Clone or download the project files.
2. Open `index.html` in any modern web browser.
3. No build step, server, or dependencies needed.

---

## 🏗️ How It Was Built

### 1. HTML Structure (`index.html`)

The UI is organized into **three screens** inside a single `.container` div. Only one screen is visible at a time using the `.active` CSS class:

| Screen ID        | Purpose                                      |
|------------------|----------------------------------------------|
| `#start-screen`  | Welcome message and Start button             |
| `#quiz-screen`   | Displays questions, answers, score, progress |
| `#result-screen` | Shows final score and performance message    |

Answer buttons are **not hardcoded** in the HTML. They are dynamically created by JavaScript and injected into `#answers-container` at runtime.

---

### 2. Styling (`style.css`)

- **CSS Reset** — `margin`, `padding`, and `box-sizing` are normalized across all elements.
- **Layout** — `flexbox` centers the `.container` both vertically and horizontally on the page using `min-height: 100vh`.
- **Screen Toggling** — All `.screen` elements are hidden by default (`display: none`). Adding the `.active` class switches them to `display: block`.
- **Answer Feedback** — Two utility classes handle visual feedback after an answer is selected:
  - `.correct` — green highlight for the right answer
  - `.incorrect` — red highlight for a wrong selection
- **Progress Bar** — A CSS-animated bar (`transition: width 0.3s ease`) whose width is updated by JavaScript as the quiz progresses.
- **Responsive Design** — A `@media (max-width: 500px)` breakpoint adjusts font sizes and padding for mobile screens.

---

### 3. JavaScript Logic (`script.js`)

#### Data

All questions live in a `quizQuestions` array of objects. Each object has:
```js
{
  question: "Question text here",
  answers: [
    { text: "Option A", correct: false },
    { text: "Option B", correct: true },
    ...
  ]
}
```

#### State Variables

| Variable               | Purpose                                      |
|------------------------|----------------------------------------------|
| `CurrentQuestionIndex` | Tracks which question is currently displayed |
| `score`                | Accumulates the player's correct answers     |
| `answersDisabled`      | Prevents multiple clicks on one question     |

#### Core Functions

**`startQuiz()`**
Resets the index and score, hides the start screen, shows the quiz screen, and calls `showQuestion()`.

**`showQuestion()`**
Reads the current question from the array, updates the question text, dynamically builds answer `<button>` elements and appends them to `#answers-container`, and updates the progress bar width.

**`selectAnswer(event)`**
Fires when the player clicks an answer. It:
1. Guards against double-clicks via `answersDisabled`.
2. Reads the `data-correct` attribute set on each button to check correctness.
3. Applies `.correct` / `.incorrect` classes to all buttons for visual feedback.
4. Increments the score if the answer is right.
5. Uses `setTimeout` (1000ms) to pause before advancing to the next question or showing results.

**`showResults()`**
Hides the quiz screen, shows the result screen, and displays a performance message based on the score percentage:

| Score       | Message                             |
|-------------|-------------------------------------|
| 100%        | "Perfect! You're a genius!"         |
| 80% – 99%   | "Great job! You know your stuff!"   |
| 60% – 79%   | "Good effort! Keep learning!"       |
| 40% – 59%   | "Not bad! Try again to improve!"    |
| Below 40%   | "Keep studying! You'll get better!" |

**`restartQuiz()`**
Hides the result screen and calls `startQuiz()` to reset everything from the beginning.

---

## 💡 Key Concepts Used

- **DOM Manipulation** — `createElement`, `appendChild`, `innerHTML`, `textContent`
- **Event Listeners** — `click` events on start, restart, and dynamically generated answer buttons
- **`dataset` API** — `button.dataset.correct` stores whether an answer is correct without extra variables
- **CSS Class Toggling** — `classList.add` / `classList.remove` drives all screen transitions and feedback states
- **`setTimeout`** — Creates a brief delay after answering so users can see feedback before moving on

---

## ➕ Extending the Project

Some ideas for taking this further:

- **Add a timer** — Give players a countdown per question using `setInterval`.
- **Shuffle questions** — Randomize the `quizQuestions` array order on each start.
- **Add more questions** — Simply push new objects into the `quizQuestions` array.
- **Persist high scores** — Use `localStorage` to save and display the player's best score.
- **Add categories** — Group questions by topic and let players choose a category on the start screen.