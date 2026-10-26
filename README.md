# quiz-cli

An interactive command-line quiz game for learning JavaScript and related programming concepts. The application is implemented as a Node.js ES module and provides category selection, configurable question counts, answer feedback, explanations, scoring, progress tracking, and an incorrect-answer review.

> **Current status:** The repository contains a path mismatch that currently prevents the CLI from starting as configured. `index.js` imports several files from `src/` and `data/`, but the corresponding files currently exist at the repository root. The run instructions below are therefore **intended instructions and are subject to fixing that mismatch**.

## Features

- Interactive command-line quiz flow
- Quiz category selection
- Selection of the number of questions
- Multiple-choice questions
- Fisher–Yates question shuffling
- Score and percentage calculation
- Progress display with a 30-character progress bar
- Immediate answer feedback
- Explanations for questions
- Performance messages based on results
- Review of incorrect answers
- Option to replay the quiz
- ANSI-colored terminal output
- Uses only Node.js built-in functionality; no npm dependencies are declared

## Technologies and Dependencies

| Technology | Details |
| --- | --- |
| Node.js | Required version `18` or later |
| JavaScript | ES module-based Node.js application |
| Node.js `readline` | Used for interactive terminal input |
| ANSI escape sequences | Used for terminal colors and styling |
| npm | Used to run the scripts declared in `package.json` |

The project does not declare any runtime dependencies or development dependencies, and no lockfile is present.

## Prerequisites

- Node.js 18 or later
- npm, normally included with Node.js
- A terminal capable of displaying standard ANSI color escape sequences

The required Node.js version is specified in `package.json`:

```json
{
  "engines": {
    "node": ">=18"
  }
}
```

## Installation and Setup

Clone the repository and enter its directory:

```bash
git clone https://github.com/anfa2018/test-app.git
cd test-app
```

There are no declared npm dependencies to install. Running `npm install` is not required by the current `package.json`, although npm may still be used to initialize local project metadata if desired.

## Configuration and Quiz Data

The quiz content is stored in:

```text
questions.json
```

The current data contains three categories:

- `javascript` — JavaScript Basics
- `nodejs` — Node.js Fundamentals
- `general` — General Programming

Each category currently contains five multiple-choice questions. A question has the following structure:

```json
{
  "question": "Question text",
  "options": [
    "First option",
    "Second option",
    "Third option",
    "Fourth option"
  ],
  "answer": 0,
  "explanation": "Explanation shown after answering"
}
```

The `answer` value is a zero-based index into the `options` array. For example, `"answer": 0` identifies the first option as the correct answer.

There are no documented environment variables or external configuration files.

### Data location mismatch

The entry point currently attempts to load quiz data from:

```text
data/questions.json
```

However, the verified repository layout contains the file at:

```text
questions.json
```

This mismatch must be resolved before the application can be expected to start successfully.

## Usage

The package declares the following start command:

```bash
npm start
```

This runs:

```bash
node index.js
```

The intended interactive flow is:

1. Display a welcome banner.
2. Select a quiz category.
3. Select the number of questions.
4. Answer each multiple-choice question.
5. Receive feedback and an explanation.
6. View progress and final results.
7. Review incorrect answers.
8. Choose whether to replay the quiz.

### Important path mismatch

At present, `index.js` imports:

```text
./src/input.js
./src/quiz.js
./src/colors.js
```

and attempts to load:

```text
data/questions.json
```

The actual files are located at the repository root:

```text
input.js
quiz.js
colors.js
questions.json
```

Therefore, the intended command is:

```bash
npm start
```

but it is **subject to fixing the import and data-file paths first**. The repository should not be considered verified as runnable in its current state.

Possible path corrections would need to make the imports and JSON lookup agree with the actual root-level layout. The exact correction strategy is not specified by the repository.

## Build and Run Instructions

No build process is defined. The project is intended to run directly with Node.js.

### Intended run command

```bash
npm start
```

Equivalent command:

```bash
node index.js
```

These commands are currently expected to fail until the path mismatch described above is resolved.

## Testing

The project declares this npm script:

```bash
npm test
```

which runs:

```bash
node --test
```

No test files were found in the repository. As a result, the repository does not currently provide verified automated test coverage, and the declared test command should not be interpreted as evidence that tests exist or pass.

## Project Structure

The verified repository layout is:

```text
test-app/
├── colors.js
├── download
├── index.js
├── input.js
├── package.json
├── questions.json
└── quiz.js
```

### File descriptions

| File | Purpose |
| --- | --- |
| `index.js` | Node.js ES module CLI entry point. Creates the readline interface, loads quiz data, displays the welcome flow, handles category and question-count selection, runs the quiz, displays results, supports replay, handles errors, and closes the readline interface. |
| `input.js` | Provides interactive input helpers built on Node.js `readline`, including interface creation, prompts, selection, confirmation, and “press Enter” behavior. |
| `quiz.js` | Exports the `Quiz` class. Manages question shuffling, current question state, score, answers, category, progress, feedback, explanations, performance messages, and incorrect-answer review. |
| `colors.js` | Defines ANSI terminal styles and colors, plus helpers for applying color and formatting to text. |
| `questions.json` | Contains the quiz categories and multiple-choice question data. |
| `package.json` | Defines package metadata, Node.js module configuration, npm scripts, engine requirements, keywords, and the MIT license declaration. |
| `download` | A binary or non-UTF-8 file whose purpose could not be verified from the repository contents. |

## Package Metadata

The project declares the following metadata in `package.json`:

| Field | Value |
| --- | --- |
| Name | `quiz-cli` |
| Version | `1.0.0` |
| Description | Interactive command-line quiz game for learning JavaScript |
| Entry point | `index.js` |
| Module type | `module` |
| Keywords | `cli`, `quiz`, `game`, `educational` |
| License | `MIT` |
| Node.js requirement | `>=18` |

## Architecture and Main Components

### CLI entry point

`index.js` coordinates the application lifecycle. It creates the terminal input interface, loads the questions, presents the initial selections, creates and runs a `Quiz`, displays the results, handles replay, catches errors, and closes the interface.

### Input layer

`input.js` encapsulates terminal interaction using the built-in `readline` module. Its exported helpers are used for prompts, selections, confirmations, and pause-before-continuing behavior.

### Quiz engine

`quiz.js` contains the `Quiz` implementation. It:

- Randomizes questions using the Fisher–Yates algorithm
- Tracks the current question
- Tracks the score and submitted answers
- Stores the selected category
- Exposes state through getters such as:
  - `currentQuestion`
  - `totalQuestions`
  - `isComplete`
  - `progress`
- Produces answer choices, feedback, and explanations
- Formats progress with a 30-character progress bar
- Calculates percentages
- Provides performance messages
- Supports review of incorrect answers

### Terminal styling

`colors.js` provides ANSI-based color and style helpers used to format CLI output. Terminal appearance may vary depending on terminal support for ANSI escape sequences.

## Limitations and Current Repository Notes

- The application is not currently verified to run successfully.
- `index.js` references `./src/input.js`, `./src/quiz.js`, and `./src/colors.js`, but those files are at the repository root.
- `index.js` references `data/questions.json`, but the verified data file is `questions.json` at the repository root.
- No tests are present, although `npm test` is declared.
- No build script is defined.
- No deployment configuration is present.
- No CI workflows were found.
- No release information was found.
- The purpose of the `download` binary/encoded file could not be verified.
- No environment variables are documented.
- The repository does not include a `LICENSE` file. The `package.json` declares the project license as MIT.

## Deployment

No deployment instructions or deployment configuration are provided. The project is designed as a local command-line application and has no documented server, hosting, or packaging workflow.

## License

`package.json` identifies the project as licensed under the **MIT License**. No separate `LICENSE` file is present in the repository.
