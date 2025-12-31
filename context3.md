reply from hp-42s manual : "The statistical functions available under the $\Sigma$ and Statistics menus include general statistical operations and functions for curve fitting and forecasting.
### Statistical Functions (STAT Menu)
The main statistical functions available in the STAT menu include:
*   **$\Sigma+$ (Summation plus):** Accumulates a pair of x- and y-values into the summation registers.
*   **SUM (Sum):** Returns the sums ($\Sigma x$ and $\Sigma y$) into the X- and Y-registers, respectively.
*   **MEAN (Mean):** Returns the arithmetic mean (average) of the x-values and y-values into the X- and Y-registers, respectively.
*   **WMEAN (Weighted mean):** Calculates the mean of x-values weighted by the y-values ($\Sigma xy \div \Sigma y$).
*   **SDEV (Standard deviation):** Computes the sample standard deviations ($s_x$ and $s_y$) and places them in the X- and Y-registers, respectively.
*   **CFIT (Curve-fitting submenu):** Displays the submenu for curve fitting.
    *   The CFIT submenu provides functions for curve fitting models (LINF, LOGF, EXPF, PWRF, BEST) and calculations (FCSTX, FCSTY, SLOPE, YINT, CORR).
*   **ALL$\Sigma$ (All-statistics mode):** Selects the mode that uses all 13 summation coefficients for statistical calculations (the default mode).
*   **LIN$\Sigma$ (Linear statistics mode):** Selects the mode that uses only six summation coefficients, the minimum set required for linear curve fitting.
*   **$\Sigma$REG (Summation registers):** Defines which storage register begins the block of summation registers.
*   **$\Sigma$REG? (Summation registers location):** Returns the register number of the first summation register.
The HP-42S can be set to use all 13 summation coefficients (ALL$\Sigma$ mode) or only six (LIN$\Sigma$ mode), which affects which curve fitting models are available.
---
### (1) How to access the COMB function for combinations
The **COMB** (combinations) function is found in the **PROB** (probability) menu.
To access it, display the PROB menu by pressing the shifted key for PROB ($\clubsuit$ [PROB]). The **COMB** function calculates the number of possible sets of $y$ different items taken in quantities of $x$ items at a time.
### (2) How to enter and operate on complex numbers
**Entering Complex Numbers**
To key in a complex number (such as $z = x + iy$ or $z = r \angle \theta$):
1.  Key in the left-hand part ($x$ or $r$), then press **[ENTER]**.
2.  Key in the right-hand part ($y$ or $\theta$).
3.  Press the shifted **[COMPLEX]** key ($\clubsuit$ [COMPLEX]) to convert the two real numbers in the X- and Y-registers into a complex number in the X-register.
The coordinate mode (Rectangular or Polar) determines how the calculator interprets the parts entered and displays the complex numbers.
**Operating on Complex Numbers**
Most standard arithmetic functions (+, -, $\times$, $\div$) work with complex numbers just as they do with real numbers. The HP-42S also includes **Complex numbers and vector functions**, allowing for vector operations using complex numbers.
### (3) How to generate random integers within a range
The HP-42S has a **RAN** (random number) function, which is available in the PROB menu.
The **RAN** function returns a random number in the range **0 $\leq$ x $<$ 1**.
The sources **do not** specify a dedicated function for generating random *integers* within a *specific range*. You would typically use the RAN function and then apply mathematical operations (like scaling and truncation) to achieve a desired integer range, although this process is not detailed in the provided materials.
### (4) Whether there's a polynomial evaluation or Horner's method function
The provided sources **do not** mention a dedicated function for **polynomial evaluation** or **Horner's method** by name.
The closest related application is the **Solver** (a built-in application and root finder). The Solver enables you to solve an equation for any of its variables. To use the Solver, you enter a program that evaluates the function $f(x)$ you wish to solve, where the goal is to find the roots (where $f(x) = 0$). This technique is demonstrated using examples of polynomial equations."

reply from free42 manual : "Based on the "Alternative HP-42S/Free42 Manual" and the current capabilities of the DM42 (which runs the Free42 simulator), here are the answers regarding extensions and syntax.
### **Do Free42/DM42 have extensions for...?**
In short: **No.**
While Free42 adds many system-level features (like infinite stack, file I/O, and larger word sizes in BASE mode), it **does not** extend the core mathematical command set of the original HP-42S in the three specific areas you requested. You must rely on the original HP-42S methods or user programs for these.
1.  **Random Integer Generation:**
    *   **No Extension:** There is no built-in `RANDINT` command.
    *   **Method:** You must use the standard `RAN` command (which generates a number $0 \le x < 1$) and perform the math yourself.
    *   *Formula:* To get a random integer between $0$ and $N-1$, use:
        `N`, `×`, `RAN`, `IP` (Integer Part).
2.  **Combinatorics:**
    *   **No Extension:** The device retains the standard HP-42S probability set: `COMB` (Combinations), `PERM` (Permutations), `N!` (Factorial), and `GAM` (Gamma function).
    *   *Note:* Free42 allows for much larger factorials (up to approx $170!$) compared to the original hardware because it uses decimal floating-point math libraries with higher dynamic range, but the *functions* themselves are the same.
3.  **Polynomial Operations:**
    *   **No Extension:** There are no built-in commands for finding polynomial roots (like `ROOTS` or `PROOT` found on the HP-48/50g series).
    *   **Method:** You must use the **Solver** to find roots individually or use **Matrices** to solve systems of linear equations. For finding all roots of a polynomial, you would need to load a specific user program (a library).
---
### **Complex Number Entry Syntax**
The DM42/Free42 preserves the HP-42S RPN entry style for complex numbers. You cannot type `3+4i` directly in one line; you must build the number on the stack.
**The Syntax:**
1.  Enter the **Real** part.
2.  Press `ENTER`.
3.  Enter the **Imaginary** part.
4.  Press `SHIFT` + `COMPLEX` (located above the `x²` key on the DM42/HP-42S).
**Example:** To enter $3 + 4i$
`3` `ENTER` `4` `SHIFT` `COMPLEX`
*Result on display:* `3.0000 i4.0000`
**Alternative Method (Polar):**
If you are in Polar mode `MODES` -> `POLAR`), the same keystrokes will interpret the first number as the **Magnitude (r)** and the second as the **Angle ($\theta$)**."