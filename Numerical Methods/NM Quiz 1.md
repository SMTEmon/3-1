---
title: "Numerical Methods — Quiz 1 Master Study Hub"
course: SWE Math 4543 (Numerical Methods)
tags:
  - numerical-methods
  - quiz-1
  - exam-prep
  - formula-sheet
  - mock-exam
aliases:
  - NM Quiz 1
  - Numerical Methods Quiz 1
  - Quiz 1 Hub
---

> [!important] Quiz-01 Official Logistics & Syllabus
> - **Course:** SWE Math 4543 — Numerical Methods
> - **Date:** Monday, 23rd November 2026 | **Time:** 01:50 PM (Afternoon)
> - **Marks:** **22.5** | **Duration:** **35 minutes** *(~1.55 min per mark!)*
> - **Location:** Room-301, 302
> - **Syllabus:** **Lectures 1, 2, 3, and 4**
>
> **Topic Note Hubs:**
> - [[1. Introduction & Bisection Method]]
> - [[2. Errors]]
> - [[3. Newton- Raphson Method]]
> - [[4. Secant & False Position Method]]
>
> **Original PDFs & Practice Links:**
> - Course Outline & Past Papers: [[SWE Math 4543 - OBE Course Outline.pdf]] | [[Numericals [Quiz 01].pdf]] | [[Numericals [Mid Exam].pdf]] | [[Numericals [Final Exam].pdf]]
> - Lecture Slides: [[SWE-Math4543-Lecture1.pdf]] | [[SWE-Math4543-Lecture2.pdf]] | [[SWE-Math4543-Lecture3.pdf]] | [[SWE-Math4543-Lecture4.pdf]]
> - External Practice (Holistic Numerical Methods): [Numerical Methods Unabridged](http://nm.mathforcollege.com/NumericalMethodsTextbookUnabridged/) | [Physical Problems](https://nm.mathforcollege.com/physical-problems/) | [ChemE MWS](http://nm.mathforcollege.com/mws/che/) | [numericalmethodsguy](https://www.youtube.com/@numericalmethodsguy)

---

## 1. Master Formula Cram Sheet

### A. Error Formulas & Significant Digits *(Lecture 2 — [[2. Errors]])*

| Concept | Formula | Key Detail |
| :--- | :--- | :--- |
| **True Error** | $E_t = \text{True} - \text{Approx}$ | Keeps sign & physical units |
| **Relative True Error** | $\epsilon_t = \dfrac{\text{True} - \text{Approx}}{\text{True}} \times 100\%$ | Dimensionless (scale-invariant) |
| **Approximate Error** | $E_a = \text{Present} - \text{Previous}$ | Used when true value is **unknown** (e.g., Gradient Descent / iteration) |
| **Relative Approx. Error** | $\epsilon_a = \dfrac{\text{Present} - \text{Previous}}{\text{Present}} \times 100\%$ | Denominator is **Present** (newest)! |
| **Sig. Digits Criterion** | $\lvert \epsilon_a \rvert \le 0.5 \times 10^{2-m}\%$ | Guarantees at least **$m$** sig figs ($m=1: 5\%$, $m=2: 0.5\%$, $m=3: 0.05\%$; fractional: $\le 0.5\times 10^{-m}$) |
| **Solve for $m$** | $m = \max\!\left(0,\ \left\lfloor 2 - \log_{10}(2\lvert \epsilon_a \rvert\%) \right\rfloor\right)$ | Use $\lvert \epsilon_a \rvert > 0$ **in percent (`%`)** (clamped at $0$ for $\lvert\epsilon_a\rvert > 50\%$) |

---

### B. Root-Finding Update Formulas *(Lectures 1, 3, 4)*

| Method | Formula | Starting Requirement | Update / Bracket Rule |
| :--- | :--- | :--- | :--- |
| **Bisection** ([[1. Introduction & Bisection Method\|Lec 1]]) | $x_m = \dfrac{x_l + x_u}{2}$ | Continuous $f$, $f(x_l)f(x_u) < 0$ | $f(x_l)f(x_m)<0 \implies x_u=x_m$; $>0 \implies x_l=x_m$ |
| **Newton–Raphson** ([[3. Newton- Raphson Method\|Lec 3]]) | $x_{i+1} = x_i - \dfrac{f(x_i)}{f'(x_i)}$ | $x_0$, $f(x)$, $f'(x) \neq 0$ | No bracket needed; replace $x_i \leftarrow x_{i+1}$ |
| **Secant** ([[4. Secant & False Position Method\|Lec 4]]) | $x_{i+1} = x_i - \dfrac{f(x_i)(x_i - x_{i-1})}{f(x_i) - f(x_{i-1})}$ | $x_{-1}, x_0$ with $f(x_0) \neq f(x_{-1})$ | Always drops $x_{i-1}$; keeps $(x_i, x_{i+1})$ |
| **False Position** ([[4. Secant & False Position Method\|Lec 4]]) | $x_r = \dfrac{x_U f(x_L) - x_L f(x_U)}{f(x_L) - f(x_U)}$ | Continuous $f$, $f(x_L)f(x_U) < 0$ | $f(x_L)f(x_r)<0 \implies x_U=x_r$; $>0 \implies x_L=x_r$ |

### C. Special Hardware / Quiz-Favorite Derivations *(Lecture 3)*

| Target Function | Set $f(x) = 0$ as... | Final Iteration Formula | Why It Matters |
| :--- | :--- | :--- | :--- |
| **Hardware $1/a$ (No Div!)** | $f(c) = a - \dfrac{1}{c} = 0$ | $c_{i+1} = c_i(2 - c_i a)$ | Uses only `fmul` + `fsub` (`9` cycles vs `15–25` `fdiv`) |
| **Hardware $1/\sqrt{R}$ (No Div!)** | $f(x) = \dfrac{1}{x^2} - R = 0$ | $x_{i+1} = \dfrac{1}{2} x_i(3 - R x_i^2)$ | Then $\sqrt{R} = R \cdot (1/\sqrt{R})$ with zero divisions |
| **Heron $\sqrt{R}$** | $f(x) = x^2 - R = 0$ | $x_{i+1} = \dfrac{1}{2}\!\left(x_i + \dfrac{R}{x_i}\right)$ | Uses 1 division per step |
| **General $k$-th root $\sqrt[k]{R}$** | $f(x) = x^k - R = 0$ | $x_{i+1} = \dfrac{1}{k}\!\left[(k-1)x_i + \dfrac{R}{x_i^{k-1}}\right]$ | Asked directly in **IUT Quiz-01 Q1**! |

---

## 2. 4-Method Comparison & Floating Ball Benchmark

Benchmark equation across Lectures 1, 3, and 4:
$$f(x) = x^3 - 0.165x^2 + 3.993 \times 10^{-4} = 0 \qquad (\text{True root } x \approx 0.06238\text{ m})$$

| Method | Category | Order $p$ | Bracket Needed? | Needs $f'(x)$? | Guaranteed? | $\lvert \epsilon_a \rvert$ at Iter 1? | Floating Ball Benchmark |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :--- |
| **Bisection** | Bracketing | $1$ (Linear) | Yes | No | **Yes** | No (N/A) | **9 iters** ($0.3436\%, m=2$); Iter 10: $0.172\%$ |
| **False Position** | Bracketing | **$1$ (Linear)** *(one bound stalls)* | Yes | No | **Yes** | No (N/A) | Iter 3: $0.0624$ ($2.05\%, m=1$); **Iter 4: $m=3$ ($0.0203\%$)** |
| **Secant** | Open | $\approx 1.618$ | No | No | No | Yes | **4 iters** $\to 0.06238$ ($3.64\times 10^{-4}\%, \mathbf{m=5}$) |
| **Newton–Raphson** | Open | **$2$ (Quad)** | No | **Yes** | No | Yes | **3 iters** $\to 0.06238$ ($\approx 0\%, m \ge 4$) |

---

## 3. 35-Minute Non-Programmable Calculator (`Casio fx-991ES / EX`) & Sketch Survival Guide

Since programmable calculators are **prohibited** in IUT Room-301 / 302, mastering `CALC`, `SHIFT STO`, and `ANS` saves **5–10 minutes** on a 4-iteration table:

| Method | Fast Calculator Workflow (`Casio fx-991ES / EX`) | 1-Second Sanity Check |
| :--- | :--- | :--- |
| **Trig Check (ALL)** | Check the top LCD for **`R` (`RAD`)** before touching any $\sin/\cos/\tan/\arctan$ key! | Does $\sin(\pi/2) = 1$ (NOT $0.0274$)? |
| **Bisection** | Type $f(X)$ once; press `CALC` for endpoints, track `+`/`-` signs in margin, then `CALC` only $x_m$. | $x_l < x_m < x_u$; bracket halves cleanly. |
| **False Position / Secant** | Store $x_L \to A,\ x_U \to B,\ f(x_L) \to C,\ f(x_U) \to D$ (`SHIFT STO`); run `(B*C - A*D)/(C - D)` then `CALC` $f(X)$ for $f(x_r)$. | **Always verify $x_L < x_r < x_U$** (FP)! Check $\lvert f \rvert \to 0$. |
| **Newton–Raphson & Series** | Seed `x0 =`, then chain `ANS - f(ANS)/f'(ANS)` (or keep $X$ via `CALC` to write $f(x_i), f'(x_i)$ for step marks). | Digits roughly double near simple roots; $\lvert f(x_{i+1}) \rvert \ll \lvert f(x_i) \rvert$. |

> [!tip] Precision Rule & 1-Page Sketch Checklist (`Quiz-01 Q3` & `Final Q1b`)
> - **Rounding Rule:** Keep full precision inside calculator memory/registers while computing $f(x_r)$ or $x_{i+1}$, unless a problem specifically specifies decimal chopping/rounding at every step (state your rounding choice clearly!).
> - **Quiz-01 Q3 Sketch Checklist ($f_1$ vs $f_2$):** Draw labeled axes $(x, y)$, mark opposite-sign endpoints $(x_l, x_u)$, draw the straight secant chord:
>   1. **$f_1(x)$ (FP beats Bisection):** Smooth, gently curved line with root right next to the **smaller-$\lvert f\rvert$ endpoint** while midpoint misses it.
>   2. **$f_2(x)$ (Bisection beats FP):** Flat near small-$\lvert f\rvert$ endpoint across most of $[x_l, x_u]$, bending sharply near the **steep opposite endpoint** so secant chords pin that steep bound permanently.
> - **Final Q1b Sketch Checklist (Bisection Drawbacks):** Include labeled sketches for: (a) closer initial guess wastes midpoint steps, (b) tangent/even root $x^2$ (same signs on both sides), and (c) singularity $1/x$ on $[-2, 3]$ (opposite signs at a vertical asymptote).

---

## 4. Past IUT Exam Question Map (Winter 2024–25)

Every single question in scope for Quiz 01 from past IUT papers is mapped below to its exact marks and solution:

| Past Paper & Question | Marks (Orig.) | Topic | Where Full Solution Lives |
| :--- | :---: | :--- | :--- |
| **Quiz-01 Q1** | **3** | Derive NR formula for $k$-th root $R^{1/k}$ (`1` formula + `2` derivation) | [[3. Newton- Raphson Method#6. Practice Problems\|Lec 3, Problem 1 (k-th root)]] |
| **Quiz-01 Q2a, Q2b** | **1 + 3 = 4** | Sig fig def + Scarborough (`0.5+0.5`) + 6 numbers (`0.5` each) | [[2. Errors#4. Practice Problems\|Lec 2, Problem 1 (Sig Figs)]] |
| **Quiz-01 Q3** | **3** | Sketch $f_1, f_2$ showing FP vs Bisection advantage (`1.5` each) | [[4. Secant & False Position Method#4. Practice Problems\|Lec 4, Problem 2 (FP vs Bisection)]] |
| **Quiz-01 Q4** | **5** | Plastic ratio $x^3 - x - 1 = 0$ on $[-1, 2]$ via Regula Falsi (`4` iters, `0.25/cell`) | [[4. Secant & False Position Method#4. Practice Problems\|Lec 4, Problem 1 (Plastic Ratio)]] |
| **Mid Exam Q1a** | **4** | Analytical vs Numerical Methods table (`2` pts per valid difference) | [[1. Introduction & Bisection Method#1. Solving Engineering Problems & Role of Numerical Methods\|Lec 1, Section 1 (Comparison)]] |
| **Mid Exam Q1b** | **6** | Geometric derivation of Secant Method (`4` derivation + `2` formula) | [[4. Secant & False Position Method#1. The Secant Method\|Lec 4, Secant Derivation]] |
| **Mid Exam Q1c** | **14** (`2.5 + 11.5`) | Kepler $\theta - 0.017\sin\theta - \frac{2\pi t}{T} = 0$ at $t = T/8$ (`1 pt/iter` subst. + `0.5/cell`) | [[3. Newton- Raphson Method#6. Practice Problems\|Lec 3, Problem 2 (Kepler)]] |
| **Mid Exam Q2a** | **5** | Truncation error definition + 3 avenues | [[2. Errors#3. Sources of Numerical Error\|Lec 2, Section 3B (Truncation)]] |
| **Final Exam Q1a** | **5** (`3 + 2`) | 4 steps of engineering problem solving + example | [[1. Introduction & Bisection Method#1. Solving Engineering Problems & Role of Numerical Methods\|Lec 1, Section 1 (4 Steps)]] |
| **Final Exam Q1b** | **6** | 4 drawbacks of Bisection + examples & sketches | [[1. Introduction & Bisection Method#5. Advantages & Drawbacks of the Bisection Method\|Lec 1, Section 5 (Drawbacks)]] |
| **Final Exam Q1c** | **18** (`12 + 6`) | Crossed ladders alley width $f(x) = 0$ (`12`) + NR formula (`6`) | [[3. Newton- Raphson Method#6. Practice Problems\|Lec 3, Problem 3 (Crossed Ladders)]] |
| **Final Exam Q2a** | **5** | Round-off error definition + sources + examples | [[2. Errors#3. Sources of Numerical Error\|Lec 2, Section 3A (Round-Off)]] |

*(Out-of-Scope Note: **`Mid Exam Q2b`** and **`Final Exam Q2b, Q2c`** cover Taylor/Maclaurin series expansions and Taylor's Remainder Theorem, which belong to **Week 4 / Lecture 5** and are outside Quiz-01's Lectures 1–4 syllabus.)*

---

## 5. Timed 22.5-Mark Mock Quiz (35 Minutes)

> [!tip] How to Take This Mock Quiz
> Set a **35-minute timer**, turn your calculator to **`RAD` mode**, grab blank paper, and solve all 4 questions below before opening the collapsed answer blocks! Pacing guide: **Q1 (7 min)** $\to$ **Q2 (7 min)** $\to$ **Q3 (8 min)** $\to$ **Q4 (13 min)**.

---

### Mock Q1 [5 Marks — Theory & Hardware Derivation]
1. **[3 Marks]** Using Newton–Raphson, derive an **iterative reciprocal formula that requires NO division operations** to compute $c = \frac{1}{a}$ (so that CPU division $\frac{b}{a}$ reduces to $b \cdot c$). Explain in one sentence why choosing $f(c) = ac - 1 = 0$ defeats the purpose.
2. **[2 Marks]** Perform **2 iterations** of your derived formula to approximate $\frac{1}{7}$ ($a = 7$) starting from LUT seed $c_0 = 0.14$. Compute $\lvert \epsilon_a \rvert\%$ and guaranteed significant digits $m$ at Iteration 2.

> [!success]- Answer & Rubric (Mock Q1 — 5 Marks)
> **1. Derivation [3 Marks]:**
> - **[1 Mark]** Set up $f(c) = a - \frac{1}{c} = 0$ (whose root is $c = \frac{1}{a}$) and differentiate: $f'(c) = +\frac{1}{c^2}$.
> - **[1 Mark]** Substitute into Newton–Raphson:
>   $$c_{i+1} = c_i - \frac{a - 1/c_i}{1/c_i^2} = c_i - c_i^2\!\left(a - \frac{1}{c_i}\right) = c_i(2 - c_i a)$$
> - **[1 Mark]** Why not $f(c) = ac - 1$? Because $f'(c) = a$, which yields $c_{i+1} = c_i - \frac{ac_i - 1}{a} = \frac{1}{a}$, requiring the very division by $a$ we are trying to avoid!
>
> **2. Numerical Run ($a = 7, c_0 = 0.14$) [2 Marks]:**
> Update rule: $c_{i+1} = c_i(2 - 7 c_i)$.
> - **Iteration 1:**
>   $$c_1 = 0.14\bigl(2 - 7(0.14)\bigr) = 0.14(2 - 0.98) = 0.14(1.02) = \mathbf{0.1428}$$
>   $$\lvert \epsilon_a \rvert = \left|\frac{0.1428 - 0.14}{0.1428}\right| \times 100\% = 1.9608\%$$
> - **Iteration 2:**
>   $$c_2 = 0.1428\bigl(2 - 7(0.1428)\bigr) = 0.1428(2 - 0.9996) = 0.1428(1.0004) = \mathbf{0.14285712}\text{ (exact 8-dp product)}$$
>   $$\lvert \epsilon_a \rvert = \left|\frac{0.14285712 - 0.1428}{0.14285712}\right| \times 100\% = \mathbf{0.03998\%}$$
> - Since $0.03998\% \le 0.5 \times 10^{2-3}\% = 0.05\%$, **$m = 3$ significant digits** are guaranteed (`0.142`). *(Compare with exact $1/7 \approx 0.14285714$: only $2 \times 10^{-8}$ off after 2 multiplications!)*

---

### Mock Q2 [5 Marks — Errors, Significant Digits & Sources]
1. **[2.5 Marks]** Write the tolerance inequality guaranteeing $m$ significant digits, then list the **minimum number of significant digits** for:
   (i) `0.004050` &nbsp; (ii) `40500` &nbsp; (iii) `40500.` &nbsp; (iv) `4.050 × 10^8` &nbsp; (v) `04050`
2. **[2.5 Marks]** Differentiate **Truncation Error** from **Round-Off Error** in a concise sentence each, give one concrete example of each, and explain why picking $h = 10^{-20}$ in $\frac{f(x+h)-f(x)}{h}$ fails in finite-precision floating-point arithmetic.

> [!success]- Answer & Rubric (Mock Q2 — 5 Marks)
> **1. Sig Figs [2.5 Marks]:**
> - **[0.5 Mark]** Inequality: $\lvert \epsilon_a \rvert \le 0.5 \times 10^{2-m}\%$.
> - **[2.0 Marks — 0.4 each]:**
>   - (i) `0.004050` $\to$ **4** (leading `0.00` don't count; captive `0` and post-decimal trailing `0` count)
>   - (ii) `40500` $\to$ **3** (no decimal point $\implies$ trailing `00` omitted)
>   - (iii) `40500.` $\to$ **5** (explicit decimal point makes all digits significant)
>   - (iv) `4.050 × 10^8` $\to$ **4** (only mantissa `4.050` counts)
>   - (v) `04050` $\to$ **3** (leading `0` and integer trailing `0` omitted; `405` = 3)
>
> **2. Sources of Error [2.5 Marks]:**
> - **[1 Mark]** **Round-off error** comes from finite numeric precision/digits (e.g., chopping $1/3 \to 0.333333$, or 24-bit fixed-point chopping of $0.1_{10}$ in the 1991 Patriot missile). **Truncation error** comes from approximating an exact mathematical procedure (e.g., taking 5 terms of Maclaurin $e^x$, finite $\Delta x$ derivative, or finite-width Riemann rectangles).
> - **[1.5 Marks]** At $h = 10^{-20}$ (well below IEEE 754 double-precision machine epsilon $\sim 2.22 \times 10^{-16}$), floating-point addition rounds $x + 10^{-20}$ back to $x$, causing subtractive cancellation $f(x+h) - f(x) = 0 \implies 0/10^{-20} = 0$ (catastrophic round-off error).

---

### Mock Q3 [5 Marks — Qualitative Understanding & Sketches]
1. **[2.5 Marks]** Sketch a smooth curve on $[x_l, x_u]$ ($f(x_l) < 0, f(x_u) > 0$) illustrating **one-sided stalling** where **Bisection beats False Position**, and explain geometrically why False Position stalls.
2. **[2.5 Marks]** For each scenario below, name the numerical failure mode and explain in one line what happens:
   - (i) Bisection on $f(x) = \frac{1}{x - 2}$ over $[1, 3]$
   - (ii) Newton–Raphson on $f(x) = x^3 - 0.03x^2 + 2.4 \times 10^{-6}$ from $x_0 = 0.02$
   - (iii) Secant Method on $f(x) = \cos(x)$ from $x_{-1} = -\frac{\pi}{3}, x_0 = \frac{\pi}{3}$

> [!success]- Answer & Rubric (Mock Q3 — 5 Marks)
> **1. One-Sided Stalling Sketch & Explanation [2.5 Marks]:**
> - **[1.5 Marks Sketch]** Draw $[x_l, x_u]$ where $f(x)$ stays flat slightly below $0$ for most of the interval, crosses near $x_u$, and shoots sharply upward to a huge positive $f(x_u)$ (like $x^{10} - 1$ on $[0, 1.3]$). Show secant chords repeatedly hitting the $x$-axis on the *left* side of the root while $x_u$ remains anchored (see the $f_2(x)$ sketch below).
> - **[1 Mark Reason]** Because the secant chord stays strictly on one side of the curve across the root, every new $x_r$ lies on the same side, pinning $x_u$ permanently so interval width never shrinks to $0$—while Bisection halves the bracket every step.
>
> ![[lec4_quiz01_q3_sketches.png]]
>
> **2. Failure Mode Diagnosis [2.5 Marks]:**
> - (i) **[1 Mark]** **Singularity / Discontinuity Trap:** $f(1) = -1 < 0$ and $f(3) = +1 > 0$, but $f$ has a vertical asymptote at $x = 2$; Bisection falsely converges to the singularity $x = 2$, which is not a root.
> - (ii) **[0.75 Mark]** **Division by Zero ($f'(x_0) = 0$):** $f'(x) = 3x^2 - 0.06x \implies f'(0.02) = 0$, horizontal tangent fails on Step 1.
> - (iii) **[0.75 Mark]** **Horizontal Secant Division by Zero:** $\cos(-\pi/3) = \cos(\pi/3) = 0.5 \implies f(x_0) - f(x_{-1}) = 0$, horizontal secant line fails on Step 1.

---

### Mock Q4 [7.5 Marks — Multi-Method Numerical Computation]
Consider solving the polynomial equation:
$$f(x) = x^3 - 5x + 1 = 0$$
1. **[3.5 Marks]** Using the **False Position (Regula Falsi)** method on bracket **$[x_L, x_U] = [0, 1]$**, conduct **4 iterations** and fill out the 6-column exam table (`x_L, x_U, x_r, |\epsilon_a|%, m, f(x_r)`), keeping full calculator register precision and reporting to 5 decimal places.
2. **[4.0 Marks]** Using the **Newton–Raphson Method** starting at **$x_0 = 0$**, perform **2 iterations**. Report $x_1, x_2$, $\lvert \epsilon_a \rvert\%$, and the guaranteed number of significant digits $m$ at Iteration 2.

> [!success]- Answer & Rubric (Mock Q4 — 7.5 Marks)
> **Part 1: False Position on $[0, 1]$ for 4 Iterations [3.5 Marks]:**
> Initial check: $f(0) = 1 > 0$, $f(1) = 1 - 5 + 1 = -3 < 0$ ($f(0)f(1) < 0$, valid bracket).
> - **Iteration 1:**
>   $$x_r^{(1)} = \frac{1(1) - 0(-3)}{1 - (-3)} = \frac{1}{4} = \mathbf{0.25000}, \qquad f(0.25) = (0.25)^3 - 5(0.25) + 1 = \mathbf{-0.23438} < 0$$
>   Since $f(0)f(0.25) < 0$, update $x_U = 0.25000 \implies$ bracket $[0, 0.25]$. *(0.75 Mark)*
> - **Iteration 2:**
>   $$x_r^{(2)} = \frac{0.25(1) - 0(-0.234375)}{1 - (-0.234375)} = \frac{0.25}{1.234375} = \mathbf{0.20253}, \qquad \lvert \epsilon_a \rvert = 23.438\% \ (m = 0), \qquad f(x_r^{(2)}) = \mathbf{-0.00435}$$
>   Update $x_U = 0.20253 \implies$ bracket $[0, 0.20253]$. *(0.75 Mark)*
> - **Iteration 3:**
>   $$x_r^{(3)} = \frac{0.202532(1) - 0}{1 - (-0.004351)} = \mathbf{0.20165}, \qquad \lvert \epsilon_a \rvert = 0.43506\% \ (m = 2), \qquad f(x_r^{(3)}) = \mathbf{-0.00007}$$
>   Update $x_U = 0.20165 \implies$ bracket $[0, 0.20165]$. *(0.75 Mark)*
> - **Iteration 4:**
>   $$x_r^{(4)} = \frac{0.201654(1) - 0}{1 - (-0.000072)} = \mathbf{0.20164}, \qquad \lvert \epsilon_a \rvert = \mathbf{0.00715\%} \ (m = 3), \qquad f(x_r^{(4)}) = \mathbf{-0.000001}$$
>   *(0.75 Mark + 0.5 Mark for complete 6-column table below)*
>
> | Iteration | $x_L$ | $x_U$ | $x_r$ | $\lvert \epsilon_a \rvert$ (%) | $m$ | $f(x_r)$ |
> | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
> | 1 | $0.00000$ | $1.00000$ | $0.25000$ | — | — | $-0.23438$ |
> | 2 | $0.00000$ | $0.25000$ | $0.20253$ | $23.438\%$ | $0$ | $-0.00435$ |
> | 3 | $0.00000$ | $0.20253$ | $0.20165$ | $0.43506\%$ | $2$ | $-0.00007$ |
> | 4 | $0.00000$ | $0.20165$ | $0.20164$ | $0.00715\%$ | $3$ | $-0.000001$ |
>
> **Part 2: Newton–Raphson from $x_0 = 0$ [4.0 Marks]:**
> Derivative: $f'(x) = 3x^2 - 5$. *(0.5 Mark)*
> - **Iteration 1:**
>   $$x_1 = 0 - \frac{f(0)}{f'(0)} = 0 - \frac{1}{-5} = \mathbf{0.20000}$$
>   $$\lvert \epsilon_a \rvert = \left|\frac{0.20000 - 0}{0.20000}\right| \times 100\% = \mathbf{100.00\%}$$
>   *(1.5 Marks)*
> - **Iteration 2:**
>   $f(0.2) = (0.2)^3 - 5(0.2) + 1 = 0.008$, $\quad f'(0.2) = 3(0.04) - 5 = -4.88$.
>   $$x_2 = 0.2 - \frac{0.008}{-4.88} = 0.2 + \frac{1}{610} \approx \mathbf{0.20164}$$
>   $$\lvert \epsilon_a \rvert = \left|\frac{0.201639 - 0.200000}{0.201639}\right| \times 100\% = \mathbf{0.81301\%}$$
>   Check Scarborough criterion: $0.81301\% \le 5\%$ ($m=1$) holds, while $0.81301\% > 0.5\%$ ($m=2$) fails $\implies$ **$m = 1$ significant digit** guaranteed at Iter 2! *(2.0 Marks — Exact root $\approx 0.201640$! Notice NR hits $0.20164$ in **2 iterations** vs. False Position's **4 iterations**!)*

---

## 6. Last-Minute Pre-Exam Checklist (Room-301 / 302)

> [!error] 5 Things to Check Before Turning In Your Script
> 1. **Calculator in `RAD` Mode?** Every trigonometric function ($\sin\theta, \cos\theta$) in Numerical Methods assumes radians!
> 2. **Did you divide by `Present` in $\epsilon_a$?** Double-check $\frac{x_{\text{new}} - x_{\text{old}}}{x_{\text{new}}}$, NOT divided by $x_{\text{old}}$!
> 3. **Exact sign vs magnitude:** Write $E_t = \text{True} - \text{Approx}$ and $E_a = \text{Present} - \text{Previous}$ with their sign unless $\lvert \cdot \rvert$ is asked.
> 4. **Iteration 1 Error:** Bisection and False Position have **N/A** for $\lvert \epsilon_a \rvert$ at Iteration 1; Newton–Raphson and Secant **DO** have $\lvert \epsilon_a \rvert$ at Iteration 1!
> 5. **Did you verify IVT continuity AND opposite signs?** If asked whether Bisection works on $[a,b]$, explicitly verify **both** continuity and $f(a)f(b) < 0$.
