# Claude Code Prompt: DM42 Calculator Drill Web Application

## Project Overview

Build an interactive HTML + JavaScript single-page web application that generates practice drills for a Swiss Micro DM42 scientific calculator (an HP-42S clone running Free42). The target user is a high school math teacher who teaches **AP Statistics** and **Algebra 2**, currently preparing for Unit 3 in both courses.

The app should:
1. Generate randomized drill questions tied to specific curriculum skills
2. Track progress/completion
3. Show expandable tooltips or collapsible sections with exact DM42 keystroke sequences
4. Work offline (no server required)
5. Be clean, functional, and teacher-friendly

---

## Calculator Context: DM42 / HP-42S / Free42

The DM42 is an RPN (Reverse Polish Notation) calculator. Key characteristics:

### RPN Basics
- No "=" key. Operations act on a 4-level stack (X, Y, Z, T)
- To compute 3 + 4: `3 ENTER 4 +` → result 7 in X register
- `ENTER` duplicates X into Y and lifts the stack
- `LASTX` recalls the last X value before an operation

### Relevant Functions for These Drills

| Function | Access | What it does |
|----------|--------|--------------|
| `COMB` | [SHIFT][PROB] menu | C(n,r) combinations. Usage: `n ENTER r COMB` |
| `PERM` | [SHIFT][PROB] menu | P(n,r) permutations |
| `RAN` | [SHIFT][PROB] menu | Random number 0 ≤ x < 1 |
| `IP` | [SHIFT][PARTS] menu | Integer part (truncate toward zero) |
| `y^x` | Direct key | Exponentiation. Usage: `base ENTER exp y^x` |
| `CHS` | Direct key | Change sign (negate) |
| `STO nn` | Direct key | Store X in register nn (00-99) |
| `RCL nn` | Direct key | Recall register nn to X |
| `COMPLEX` | [SHIFT][COMPLEX] | Combine X and Y into complex number X+iY |

### Complex Number Entry
To enter 3 + 4i:
```
3 ENTER 4 [SHIFT][COMPLEX]
```
Display shows: `3.0000 i4.0000`

### Random Integer Generation (1 to N)
```
N ENTER RAN × IP 1 +
```

---

## Curriculum Context

### Algebra 2 Unit 3 Skills Requiring Calculation

| Skill | Input | Operation | Output |
|-------|-------|-----------|--------|
| Evaluate functions | f(x) and value c | Substitution | f(c) |
| Average rate of change | f(x) and interval [a,b] | (f(b)-f(a))/(b-a) | Slope of secant |
| Polynomial addition/subtraction | Two polynomials | Combine like terms | New polynomial |
| Polynomial multiplication | Two polynomials | Distribute + combine | Expanded polynomial |
| Binomial expansion | (x+y)^n | Binomial theorem | Expanded polynomial |
| Polynomial long/synthetic division | Dividend, divisor | Division algorithm | Quotient + remainder |
| Remainder Theorem | P(x) and value a | Evaluate P(a) | Remainder of P(x)÷(x-a) |
| Factor Theorem | P(x) and value a | Check if P(a)=0 | Yes/No for (x-a) factor |
| Find complex roots | Quadratic with negative discriminant | Quadratic formula | a ± bi |
| Factoring (difference of cubes, etc.) | Polynomial | Pattern recognition | Factored form |

### AP Statistics Unit 3 Skills Requiring Calculation

| Skill | Input | Operation | Output |
|-------|-------|-----------|--------|
| Proportional allocation | Population sizes, sample size n | (stratum/total) × n | Sample count per stratum |
| Relative frequency | Frequency table | count/total | Proportions |
| Random sampling | Population size N, sample size n | Random integer generation | n unique integers 1-N |

---

## Drill Categories to Implement

### Category 1: Polynomial Evaluation (Horner's Method)

**Purpose:** Practice Remainder Theorem and Factor Theorem verification

**Question format:** "Evaluate P(x) = [polynomial] at x = [value]"

**Generation rules:**
- Generate polynomials of degree 3-5
- Coefficients: integers from -10 to 10 (some can be 0)
- Evaluation points: integers from -5 to 5
- Include some where the answer is 0 (to practice Factor Theorem)

**Keystroke pattern for Horner's method:**
For P(x) = ax³ + bx² + cx + d at x = k:
```
k CHS STO 00           [if k is negative, use k directly without CHS]
a ENTER
RCL 00 × b +
RCL 00 × c +
RCL 00 × d +
```

**Tooltip should show:** The Horner form rewrite AND the keystroke sequence

---

### Category 2: Binomial Coefficients

**Purpose:** Practice computing terms in binomial expansion

**Question format:** "Find the coefficient of x^[power] in the expansion of (ax + b)^n"

**Generation rules:**
- n: 4 to 7
- a, b: small integers (-3 to 3, nonzero)
- Ask for specific term (not full expansion)

**Calculation:** For term k in (ax + b)^n:
- Coefficient = C(n,k) × a^(n-k) × b^k

**Keystroke pattern:**
```
n ENTER k COMB         [gives C(n,k)]
a ENTER (n-k) y^x ×    [multiply by a^(n-k)]
b ENTER k y^x ×        [multiply by b^k]
```

---

### Category 3: Complex Number Verification

**Purpose:** Verify complex roots satisfy quadratic equations

**Question format:** "Verify that [a + bi] is a root of x² + px + q = 0"

**Generation rules:**
- Start with complex root a + bi where a, b are small integers
- Compute p = -2a and q = a² + b²
- Student verifies by computing (a+bi)² + p(a+bi) + q = 0

**Keystroke pattern:**
```
a ENTER b [SHIFT][COMPLEX] STO 00   [store z]
RCL 00 ENTER ×                       [z²]
RCL 00 p × +                         [z² + pz]
q +                                  [should give 0 + 0i]
```

---

### Category 4: Factorization Verification

**Purpose:** Numerically verify factored forms are correct

**Question format:** "Verify that [expression] = [factored form] by evaluating both at x = [value]"

**Types to include:**
- Difference of squares: a² - b² = (a-b)(a+b)
- Difference of cubes: a³ - b³ = (a-b)(a² + ab + b²)
- Sum of cubes: a³ + b³ = (a+b)(a² - ab + b²)
- General polynomial factoring

**Keystroke pattern:** Evaluate each side at given x value, confirm equality

---

### Category 5: Stratified Sampling (AP Stats)

**Purpose:** Calculate sample sizes for stratified random samples

**Question format:** "A population has [n1] in group A, [n2] in group B, [n3] in group C. Calculate the stratified sample sizes for a total sample of [n]."

**Generation rules:**
- 2-4 strata
- Population sizes: 50-500 per stratum
- Total sample: 20-100
- Ensure answers come out to whole numbers (or discuss rounding)

**Keystroke pattern:**
```
stratum_size ENTER total_pop ÷ sample_n ×
```

---

### Category 6: Random Integer Generation (AP Stats)

**Purpose:** Practice generating random samples

**Question format:** "Generate [n] random integers from 1 to [N] for a simple random sample. Record your results."

**This is a PRACTICE drill** - no single right answer, but teaches the keystroke:
```
N ENTER RAN × IP 1 +
```
Repeat and record, skipping duplicates.

---

## UI/UX Requirements

### Layout
1. **Header:** "DM42 Drill System - Algebra 2 & AP Stats Unit 3"
2. **Category selector:** Tabs or dropdown to choose drill category
3. **Progress tracker:** Shows completed/total for current session
4. **Main drill area:**
   - Question display
   - Input field for answer
   - "Check Answer" button
   - "Show Solution" expandable section
   - "Show Keystrokes" expandable section with step-by-step calculator instructions
5. **New Question button**

### Keystroke Display
- Use a distinctive monospace font or styled "key" appearance
- Show keys like: `[SHIFT]`, `[ENTER]`, `[COMPLEX]`, `[PROB]`
- Group logical steps with brief explanations
- Example format:
  ```
  Step 1: Store x = -3
  ┌─────────────────────────┐
  │ 3  CHS  STO  00        │
  └─────────────────────────┘
  
  Step 2: Start with leading coefficient
  ┌─────────────────────────┐
  │ 1  ENTER               │
  └─────────────────────────┘
  ```

### Progress Tracking
- Use localStorage to persist:
  - Questions attempted per category
  - Questions correct per category
  - Last session date
- Display streak/progress visually

### Responsive Design
- Should work on desktop and tablet
- Teacher might use this on a laptop while holding the calculator

---

## Technical Requirements

1. **Single HTML file** with embedded CSS and JavaScript (for easy distribution)
2. **No external dependencies** except possibly a CDN font
3. **Randomization** should produce diverse, non-repeating questions within a session
4. **Answer validation:**
   - For numerical answers: accept within small tolerance (0.0001)
   - For complex numbers: validate both real and imaginary parts
   - Show "Correct!" or "Try again" with option to see solution
5. **Clean, accessible UI** - good contrast, readable fonts, logical tab order

---

## Sample Generated Questions

### Polynomial Evaluation
**Question:** Evaluate P(x) = 2x⁴ - 3x³ + x² - 5x + 7 at x = -2
**Answer:** 77
**Horner form:** P(x) = ((((2)x - 3)x + 1)x - 5)x + 7
**Keystrokes:**
```
2 CHS STO 00
2 ENTER
RCL 00 × 3 CHS +     [2(-2) - 3 = -7]
RCL 00 × 1 +         [-7(-2) + 1 = 15]
RCL 00 × 5 CHS +     [15(-2) - 5 = -35]
RCL 00 × 7 +         [-35(-2) + 7 = 77]
```

### Binomial Coefficient
**Question:** Find the coefficient of x³ in (2x - 3)⁵
**Answer:** -720
**Calculation:** C(5,2) × 2³ × (-3)² = 10 × 8 × 9 = 720, but the term is C(5,2)(2x)³(-3)² so coefficient is +720... wait, let me recalculate:
- Term for x³: k=2 (since power of x is 5-k=3)
- C(5,2) × (2)³ × (-3)² = 10 × 8 × 9 = 720
**Keystrokes:**
```
5 ENTER 2 COMB       [10]
2 ENTER 3 y^x ×      [80]
3 CHS ENTER 2 y^x ×  [720]
```

### Complex Verification
**Question:** Verify that 2 + 3i is a root of x² - 4x + 13 = 0
**Expected:** Computation yields 0 + 0i
**Keystrokes:**
```
2 ENTER 3 [SHIFT][COMPLEX] STO 00
RCL 00 ENTER ×           [gives -5 + 12i]
RCL 00 4 × CHS +         [-5+12i - (8+12i) = -13 + 0i]
13 +                     [0 + 0i] ✓
```

### Stratified Sampling
**Question:** A company has 180 engineers, 120 managers, and 60 executives. Calculate stratified sample sizes for n = 30.
**Answers:** Engineers: 15, Managers: 10, Executives: 5
**Keystrokes (for engineers):**
```
180 ENTER 360 ÷ 30 ×    [15]
```

---

## Stretch Goals (if time permits)

1. **Export progress** as JSON for backup
2. **Custom drill mode** where teacher can input specific polynomials
3. **Timed mode** for building speed
4. **Print worksheet** option that generates pen-and-paper versions

---

## Files to Create

Create a single `dm42_drills.html` file that contains everything. Use internal `<style>` and `<script>` tags.

The user (Lynn) is a 42-year-old AP Statistics and Algebra 2 teacher with strong technical skills. She wants efficient, no-busywork practice that ties directly to her curriculum. The interface should be professional and functional, not flashy.
