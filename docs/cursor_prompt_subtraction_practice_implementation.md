# Prompt for Cursor

Create a new file at:

`tools/math/subtraction.html`

The implementation must follow the same architectural, structural, UX, and coding patterns used in `addition.html`.

---

# High-Level Requirements

- Single self-contained HTML file.
- No frameworks.
- No external libraries.
- Vanilla DOM APIs only.
- HTML, CSS, and JavaScript all in one file.
- Worksheet-style right-aligned column layout.
- Three-digit subtraction only.
- No negative results.
- Maintain the same design language and coding style as `addition.html`.

---

# Page Metadata

- `<!doctype html>`
- `<html lang="en">`
- UTF-8 charset
- Responsive viewport meta
- Title: `Subtraction Practice | Math`

Heading:

```html
<h1>Subtraction Practice</h1>
```

---

# Board Structure

The layout must mirror the addition board structure but adapted for subtraction.

## Board Section

```html
<section class="board" aria-label="Subtraction board">
```

## Rows (Top to Bottom)

1. **Borrow row**
   - Three small editable inputs (`.borrow-input`) aligned with the three top digits.
   - These visually represent borrowing from the column to the left.
   - Borrow inputs are NOT validated for correctness.

2. **Top number row**
   - Three static digits rendered into `#top-digits`.

3. **Bottom number row**
   - Three static digits rendered into `#bottom-digits`.
   - Include the `"-"` operator.

4. **Separator line**
   - Same `.line` styling used in addition.

5. **Result row**
   - Three editable result inputs (`.result-input`).
   - Include the `"="` operator.

---

# Arithmetic Rules

- Generate two random three-digit numbers.
- Ensure: `top >= bottom`.
- No negative answers.
- Compute:

```js
expectedDifference = top - bottom;
```

- Only validate the result inputs against `expectedDifference`.
- Borrow inputs are purely instructional and not part of correctness validation.

---

# JavaScript Structure

Follow the same structural pattern as `addition.html`:

## State

```js
let expectedDifference = 0;
```

## Helper Functions

- `randomThreeDigitNumber()`
- `renderDigits(containerId, value)`
- `clearInputs(inputs)`
- `enableSequenceNavigation(sequence)`
- `generateProblem()`

Keep naming consistent with the addition implementation.

---

# Input Behavior

Borrow inputs and result inputs must:

- Accept numeric input only.
- Allow only one digit.
- Sanitize input with:

```js
value.replace(/\D/g, "").slice(0, 1)
```

---

# Guided Input Flow (Right-to-Left Arithmetic Order)

The custom navigation sequence must follow subtraction workflow:

1. Ones result
2. Tens borrow
3. Tens result
4. Hundreds borrow
5. Hundreds result

Use the same focus management logic from addition.

---

# Validation Logic

On `Check` button click:

1. Ensure all three result boxes are filled.
2. Join result digits into a string.
3. Convert to number.
4. Compare with `expectedDifference`.

If correct:

- Feedback: `"Great job! That answer is correct."`
- Apply `feedback success` class.

If incorrect:

- Feedback: `"Not quite yet. Try again."`
- Apply `feedback error` class.

If incomplete:

- Feedback: `"Please fill in all three result boxes."`
- Apply `feedback error` class.

Borrow inputs are ignored during validation.

---

# Styling Rules

- Use the same typography and spacing conventions as addition.
- `.board` uses monospace font.
- Digits use bordered square boxes.
- Borrow inputs are smaller with dashed borders.
- Maintain right-aligned column structure.
- Maintain same feedback styling and behavior.

---

# UX Rules

- On `New Problem` click:
  - Generate a new subtraction problem.
  - Clear all borrow and result inputs.
  - Reset feedback.
  - Focus the ones result cell.

- On page load:
  - Automatically generate the first problem.

---

# Important Constraint

Do NOT dynamically mutate top digits when borrowing in this version.

Borrow inputs are instructional only.
The arithmetic correctness is determined solely by the final result digits.

This keeps the implementation structurally parallel to `addition.html` and reduces complexity.

---

# Goal

Produce a clean, readable, maintainable subtraction worksheet implementation that mirrors the architecture and UX of `addition.html` while adapting the arithmetic logic to subtraction with borrowing support.

