# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This repository contains specifications for building a DM42 Calculator Drill Web Application - a single-page HTML/JS app that generates practice drills for the Swiss Micro DM42 scientific calculator (HP-42S clone running Free42). The target user is a high school math teacher teaching AP Statistics and Algebra 2 Unit 3.

## Key Technical Constraints

- **Single HTML file** with embedded CSS and JavaScript (no build step)
- **No external dependencies** except optional CDN fonts
- **Offline-capable** - no server required
- **localStorage** for progress persistence

## DM42/HP-42S Calculator Specifics

The DM42 uses RPN (Reverse Polish Notation):
- No "=" key; operations act on a 4-level stack (X, Y, Z, T)
- `ENTER` duplicates X into Y and lifts the stack
- Example: `3 ENTER 4 +` → result 7

### Key Functions for Drills
| Function | Access | Usage |
|----------|--------|-------|
| `COMB` | SHIFT+PROB menu | `n ENTER r COMB` for C(n,r) |
| `PERM` | SHIFT+PROB menu | Permutations |
| `RAN` | SHIFT+PROB menu | Random 0 ≤ x < 1 |
| `IP` | SHIFT+PARTS menu | Integer part (truncate) |
| `y^x` | Direct key | `base ENTER exp y^x` |
| `STO nn` / `RCL nn` | Direct | Store/recall registers 00-99 |
| `COMPLEX` | SHIFT+COMPLEX | Combine X,Y into X+iY |

### Random Integer Generation (1 to N)
```
N ENTER RAN × IP 1 +
```

### Complex Number Entry (e.g., 3+4i)
```
3 ENTER 4 [SHIFT][COMPLEX]
```

## Drill Categories to Implement

1. **Polynomial Evaluation (Horner's Method)** - Remainder/Factor Theorem practice
2. **Binomial Coefficients** - Terms in binomial expansion
3. **Complex Number Verification** - Verify complex roots satisfy equations
4. **Factorization Verification** - Difference of squares/cubes patterns
5. **Stratified Sampling (AP Stats)** - Proportional allocation calculations
6. **Random Integer Generation (AP Stats)** - Simple random sampling practice

## Curriculum Context

### Algebra 2 Unit 3 Skills
- Polynomial evaluation, addition, subtraction, multiplication
- Binomial expansion using Binomial Theorem
- Polynomial long/synthetic division
- Remainder Theorem and Factor Theorem
- Complex roots via Quadratic Formula
- Factoring patterns (difference of cubes, etc.)

### AP Statistics Unit 3 Skills
- Stratified sampling proportional allocation: `(stratum/total) × n`
- Relative frequency calculations
- Random sampling without replacement

## UI Requirements

- Keystroke displays should use monospace font with "key" styling
- Show keys as: `[SHIFT]`, `[ENTER]`, `[COMPLEX]`, `[PROB]`
- Expandable "Show Solution" and "Show Keystrokes" sections
- Progress tracking per category via localStorage
- Responsive for desktop and tablet use
