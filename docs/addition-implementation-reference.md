# Addition Implementation Reference

This document captures how `tools/math/addition.html` is implemented so another agent can build a new version that follows the same structure, UX flow, and coding style.

## High-Level Pattern

- Single self-contained file with HTML, CSS, and JavaScript together.
- No frameworks, no external libraries, and no build tooling.
- Uses vanilla DOM APIs and small helper functions.
- Worksheet-style UI with digit boxes and column-based addition flow.

## Document and Page Structure

### Metadata

- Standard HTML5 document with:
  - `<!doctype html>`
  - `<html lang="en">`
  - UTF-8 charset
  - responsive viewport meta
  - title: `Addition Practice | Math`

### Body Layout

1. Heading:
   - `<h1>Addition Practice</h1>`
2. Addition board section:
   - `<section class="board" aria-label="Addition board">`
3. Controls and feedback:
   - `Check` button (`#check-answer`)
   - `New Problem` button (`#new-problem`)
   - feedback paragraph (`#feedback`, `aria-live="polite"`)

## Board Composition

The board is visually arranged like paper column addition:

1. **Carry row**
   - 3-slot row above addends.
   - Two editable carry inputs (`.carry-input`) for hundreds and tens carry.
   - One empty placeholder (`.carry-empty`) on the ones side.
2. **Top addend row**
   - static digits rendered into `#top-digits`.
3. **Bottom addend row**
   - static digits rendered into `#bottom-digits`.
   - row includes `"+"` operator.
4. **Separator line**
   - horizontal rule-like element with class `.line`.
5. **Result row**
   - includes `"="` operator.
   - four editable result inputs (`.result-input`) for thousands/hundreds/tens/ones.

## CSS Patterns and Styling Conventions

### Typography and spacing

- `body` uses Arial/sans-serif with a simple page margin.
- `.board` switches to monospace (`Courier New`) for consistent digit alignment.
- Digit scale is intentionally large (`font-size: 2rem`) for worksheet readability.

### Row and column alignment

- `.row` is flex, right-aligned (`justify-content: flex-end`) so columns line up from the right.
- `.digits` uses CSS grid with fixed-width columns:
  - addend rows: `repeat(3, 1.4em)`
  - result row: `repeat(4, 1.4em)` via `.result-digits`

### Cell visuals

- Static digits (`.digit`) use solid 2px bordered boxes.
- Carry inputs (`.carry-input`) are smaller and lighter (dashed border) to visually differentiate carry notes.
- Result inputs (`.result-input`) match main box styling and are the primary answer entry fields.

### Feedback states

- Base class: `.feedback`
- Success variant: `.feedback.success` (green)
- Error variant: `.feedback.error` (red)
- `min-height` is set to keep layout stable when message text appears/disappears.

## JavaScript Architecture

### State

- One module-level mutable value:
  - `let expectedSum = 0;`

### Core helper functions

1. `randomThreeDigitNumber()`
   - Returns a random integer from 100 to 999.

2. `renderDigits(containerId, value)`
   - Converts number to a padded 3-character string (`padStart(3, "0")`).
   - Splits into digits.
   - Clears target container and appends one `<span class="digit">` per digit.

3. `enableSequenceNavigation(inputSequence)`
   - Adds keyboard/input flow behavior across a custom sequence:
     - On input: auto-advance focus when a cell receives a digit.
     - On Backspace in an empty cell: move focus to previous cell.
   - Also selects target input text on navigation (`select()`).

4. `clearInputs(inputs)`
   - Sets each input value to empty string.

5. `generateProblem()`
   - Generates two 3-digit numbers.
   - Computes and stores `expectedSum`.
   - Renders both addends.
   - Clears carry/result inputs.
   - Resets feedback text and class.

## DOM References and Event Wiring

### Queried elements

- `carryInputs` from `.carry-input`
- `resultInputs` from `.result-input`
- `checkButton` from `#check-answer`
- `newProblemButton` from `#new-problem`
- `feedback` from `#feedback`

### Input sanitization behavior

Both carry and result inputs attach `input` listeners that enforce:

- numeric-only (`replace(/\D/g, "")`)
- single character (`slice(0, 1)`)

This means paste/input is normalized immediately.

### Guided entry sequence

The custom navigation order is:

1. ones result (`resultInputs[3]`)
2. tens carry (`carryInputs[1]`)
3. tens result (`resultInputs[2]`)
4. hundreds carry (`carryInputs[0]`)
5. hundreds result (`resultInputs[1]`)
6. thousands result (`resultInputs[0]`)

This mirrors manual right-to-left addition steps instead of plain DOM order.

## Validation and Answer Checking

On `Check` button click:

1. Read all four result boxes (trimmed).
2. If any is empty:
   - feedback: `"Please fill in all four result boxes."`
   - class: `"feedback error"`
   - exit early.
3. Join result digits into one string, convert to number.
4. Compare with `expectedSum`.
   - If equal:
     - `"Great job! That answer is correct."`
     - `"feedback success"`
   - Else:
     - `"Not quite yet. Try again."`
     - `"feedback error"`

Important detail:

- Carry inputs are not part of correctness validation; only the four result digits are validated.

## New Problem and Initial Load Behavior

- On `New Problem` click:
  - call `generateProblem()`
  - focus the rightmost result cell (ones place).
- On page load:
  - call `generateProblem()` once to initialize the first exercise.

## Accessibility and UX Notes

- `aria-label` is used for board and key rows/inputs.
- `aria-live="polite"` on feedback supports assistive announcement of correctness messages.
- Inputs use `inputmode="numeric"` and `maxlength="1"` for mobile-friendly digit entry and per-cell constraints.
- `tabindex` values are explicitly set, but JavaScript focus steering is the primary interaction flow.

## Style and Pattern Checklist for a Follow-Up Implementation

To preserve existing project patterns, a new implementation should keep:

- single-file, dependency-free HTML/CSS/JS structure
- worksheet-like right-aligned digit columns
- per-digit input boxes and sanitization
- random 3-digit addends with computed expected answer state
- guided right-to-left arithmetic entry flow
- explicit `Check` and `New Problem` controls
- short direct feedback messages with success/error visual states
- straightforward, readable vanilla JS helper-function organization
