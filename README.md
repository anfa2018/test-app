# Quiz CLI

An interactive command-line quiz game for learning JavaScript, Node.js, and general programming concepts. The application presents category and question-count menus, accepts answers through the terminal, provides immediate feedback and explanations, and displays a final score with incorrect-answer review.

> **Repository status note:** The current repository layout does not match the import and data paths used by `index.js`. The entry point imports modules from `src/` and loads questions from `data/questions.json`, while the repository currently contains `input.js`, `quiz.js`, `colors.js`, and `questions.json` at the root. As a result, `npm start` will not run successfully without first aligning those paths or moving the files into the directories expected by `index.js`.

## Features

- Interactive terminal-based quiz flow.
- Three built-in categories:
  - JavaScript Basics
  - Node.js Fundamentals
  - General Programming
- Choice of all available questions, three questions, or five questions when the category contains enough questions.
- Fisher–Yates shuffling of questions for each quiz.
- Numbered answer selection with validation for invalid input.
- Immediate correct/incorrect feedback and optional explanations.
- Progress bar and question counter.
- Final score, percentage, performance message, and review of incorrect answers.
- Option to play again without restarting the process.
- ANSI terminal styling implemented internally; no runtime color dependency is declared.

## Technologies and Dependencies

| Area | Details |
| --- | --- |
| Runtime | Node.js `>=18.0.0` |
| Language | JavaScript using ES modules |
| Input | Node.js built-in `readline` module |
| File access | Node.js built-in `node:fs/promises`, `node:path`, and `node:url` modules |
| Data format | JSON |
| External runtime packages | None declared |

The project uses ES module syntax because `package.json` sets `"type": "module"`.

## Prerequisites

- Node.js 18 or newer.
- A terminal capable of interactive standard input/output.
- A local checkout of this repository.

No environment variables or external services are specified by the repository.

## Installation

Clone the repository and enter its directory:

```bash
git clone <repository-url>
cd <repository-directory>
```

There are currently no declared npm dependencies, so `npm install` is not required for the listed source files. It may still be used to initialize a local npm workspace if desired:

```bash
npm install
```

Before running the game, resolve the path mismatch described in [Known limitation](#known-limitation): either place the implementation modules under `src/` and the question file under `data/`, or update the paths in `index.js` to match the files at the repository root.

## Configuration

Configuration is stored in `questions.json`. The file contains a top-level `categories` object. Each category has a display `name` and a `questions` array. Each question contains:

- `question`: question text.
- `options`: answer choices in display order.
- `answer`: zero-based index of the correct option.
- `explanation`: feedback displayed after the answer.

To add or edit quiz content, preserve this structure and ensure that `answer` points to an item in `options`.

There are no documented environment variables, command-line flags, configuration files, or network settings.

## Usage

The package scripts declare the following commands:

```bash
npm start
npm test
```

`npm start` runs `node index.js`. The intended interaction is:

1. Choose a category by entering its number.
2. Choose all questions, three questions, or five questions where available.
3. Press Enter to begin.
4. Select an answer for each question by entering its number.
5. Press Enter between questions.
6. Review the result summary and incorrect answers.
7. Answer `y` or `n` when asked whether to play again.

The entry point can also be invoked directly:

```bash
node index.js
```

## Example Interaction

The exact question order is randomized, but the flow is similar to:

```text
Choose a category:
  1. JavaScript Basics
  2. Node.js Fundamentals
  3. General Programming

Your choice (enter number): 1

How many questions?
  1. All questions
  2. 3 questions
  3. 5 questions

Your choice (enter number): 3
```

For each question, the program displays numbered options and accepts a number. Invalid selections are rejected until a number in the displayed range is entered.

## Architecture and File Structure

The repository currently contains the following files:

```text
.
├── colors.js       # ANSI color/style helpers
├── download        # Binary or non-UTF-8 repository item; contents were not readable as text
├── index.js        # Application entry point and main quiz loop
├── input.js        # readline interface, menus, confirmation, and pause helpers
├── package.json    # npm metadata, scripts, and Node.js engine requirement
├── questions.json  # Categories and quiz questions
└── quiz.js         # Quiz class, scoring, shuffling, progress, and results
```

### Main components

- **`index.js`**: loads question data, renders the banner, presents category and question-count menus, creates a `Quiz`, and controls replay and error handling.
- **`quiz.js`**: exports the `Quiz` class. It shuffles questions, tracks progress and score, asks questions, records answers, and renders results.
- **`input.js`**: wraps Node.js `readline` in Promise-based helpers for prompts, numbered selections, yes/no confirmation, and pause points.
- **`colors.js`**: provides ANSI escape-code styling functions such as `success`, `error`, `info`, and `highlight`.
- **`questions.json`**: supplies the quiz categories and questions.

## Testing

The `package.json` defines:

```bash
npm test
```

This runs Node's built-in test runner with `node --test`. No test files are present in the repository listing, so the repository does not currently provide automated test cases.

## Build, Run, and Deployment

There is no build step, bundler, compiled output, deployment configuration, or CI workflow specified. The intended runtime is a local Node.js process launched with `npm start` or `node index.js` after the path mismatch is corrected.

## Known Limitations

- **Import/data paths are inconsistent with the repository layout.** `index.js` imports `./src/input.js`, `./src/quiz.js`, and `./src/colors.js`, and reads `data/questions.json`; those paths are not present in the listed repository structure. The corresponding files currently exist at the root. This must be corrected before the application can start.
- **No automated tests are included**, although an npm test script is declared.
- **Question selection uses the first questions from a category before the `Quiz` class shuffles them.** Selecting three or five questions therefore limits the pool before randomization rather than randomly sampling from the complete category.
- **Terminal colors depend on ANSI escape-code support.** The application does not provide a configuration switch to disable colors.
- The `download` item could not be interpreted as UTF-8 text, so its purpose and contents are not documented here.

## License

The package metadata declares the **MIT License**. No separate license file was present in the repository listing.
