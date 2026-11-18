---
title: "CSE 4553: Machine Learning — Quiz 1 Master Study Hub"
course: CSE 4553 (Machine Learning)
date: 2026-11-16
tags:
  - machine-learning
  - cse-4553
  - quiz-1
  - exam-prep
  - formula-sheet
  - mock-exam
aliases:
  - ML Quiz 1
  - Machine Learning Quiz 1
  - Quiz 1 Hub
---

# CSE 4553: Machine Learning — Quiz 1 Master Study Hub

> [!important] Quiz 1 Logistics & Syllabus
> - **Course:** CSE 4553 — Machine Learning (*Dr. Hasan Mahmud*, Room #404 AB2, `hasan@iut-dhaka.edu`)
> - **Date:** **Tuesday, 17th November 2026** (Class Time)
> - **Syllabus:** **Everything covered in class** (`ML_Lecture 0.pdf`, `ML_Lecture 1.pdf`, `ML_Lecture 2.pdf`)
> - **Separated Self-Contained Topic Notes (with all Slide Screenshots Embedded):**
>   1. **[[1. Intro|Lectures 0 & 1: Intro to ML, 4 Learning Paradigms, 3 Pillars, Gait Recognition & Data Leakage]]**
>   2. **[[2. Linear Regression & Gradient Descent|Lecture 2 (Part 1): Linear Regression, OLS, Normal Equation & BGD/SGD/Mini-Batch]]**
>   3. **[[3. Logistic Regression, Bias-Variance & Regularization|Lecture 2 (Part 2): Logistic Regression, Sigmoid, MLE Log-Loss, Bias-Variance, Regularization & AR(p)]]**
>   4. **[[No Fking Clue|Quick Note: Epochs vs. Batches vs. Iterations]]**
> - **Original Course PDFs:** [[BSc_Engg_SWE_Winter_CSE4553_2025.pdf]] | [[ML_Lecture 0.pdf]] | [[ML_Lecture 1.pdf]] | [[ML_Lecture 2.pdf]] | [[SWE_ML_Lab_1_Winter_2026.pdf]]
> - **Past Exam Reference:** [[ML [Mid Exam].pdf]] (Q1a, Q1b, Q2a = 40/75 marks directly from Lectures 0–2!)

---

## 0. Zero-to-Ready Crash Plan & Past-Paper Pattern

> [!tip] Starting From Zero? Follow These 4 Blocks (~3.5–4 Hours Total)
> 1. **Block 1 (45 min) — [[1. Intro]]:** Conventional vs. ML, $\langle T, P, E \rangle$ (Traffic, Fraud, Gait), **3 Pillars (Representation, Evaluation, Optimization)** with Linear Regression example, 4 learning types, and **Data Leakage**.
> 2. **Block 2 (75 min) — [[2. Linear Regression & Gradient Descent]]:** Closed-Form OLS ($a,b$), **Normal Equation $\to$ predict $x_{\text{new}}$ $\to$ compute MSE**, GD partial derivatives, **1-epoch BGD vs. SGD hand-trace**, MSE vs. MAE, GD vs. Normal Eq. ($O(n^3)$ vs. $O(n)$).
> 3. **Block 3 (75 min) — [[3. Logistic Regression, Bias-Variance & Regularization]]:** Why Linear Regression fails classification, hyperplane distance, Sigmoid + $g'(z)$, Log-Loss + MLE, **BCE gradients ($\frac{\partial L}{\partial w_j}$) + $x_1\to 100x_1$ scaling + Ridge**, Bias–Variance curve, L1/L2, $\text{AR}(p)$.
> 4. **Block 4 (45 min) — Section 4 Mock Quiz + [[No Fking Clue|Iterations = Epoch × Batches]]:** Solve all 5 mock questions on blank paper with only a non-programmable calculator.

> [!note] Past-Paper Pattern (`ML [Mid Exam].pdf`)
> - **No programmable calculators:** Small integer tables/matrices ($3\times 2$ $X$, 4-row tables, $w=\mathbf{0} \implies \sigma(0)=0.5$) meant to be solved by hand.
> - **Explain + Illustrate:** CO1 questions ask you to explain concepts *and* map them onto a concrete algorithm (e.g., 3 Pillars $\to$ Linear Regression).
> - **Chained Sub-Parts:** Normal Eq. $\to$ Predict $x_{\text{new}}$ $\to$ MSE; or BCE Loss $\to$ Gradients $\to$ Feature Rescaling + Ridge.
> - **Formula Check:** Exams may write total BCE loss as $L = -\sum [\dots]$ (no $\frac{1}{n}$) and denote learning rate by $\eta$ instead of $\alpha$. Always match the exact formula printed on the paper!

---

## 1. Master Formula & Notation Cram Sheet

### A. Foundations & Learning Paradigms *(Lectures 0 & 1 — [[1. Intro]])*

| Concept | Exact Formula / Framework | Key Exam Detail |
| :--- | :--- | :--- |
| **Well-Posed Learning (Mitchell, 1998)** | $\langle T,\; P,\; E \rangle$ | Performance on **Task $T$**, measured by **Performance $P$**, improves with **Experience $E$**. |
| **3 Pillars of Every ML Algorithm** | **1. Representation** $\to$ **2. Evaluation** $\to$ **3. Optimization** | Hypothesis shape $\to$ Loss/Cost function $\to$ Search algorithm (e.g., Least SSE / GD). |
| **Supervised Dataset** | $D = \{(x^{(i)}, y^{(i)})\}_{i=1}^n$ | Labeled data: **Classification** ($y$ discrete) vs. **Regression** ($y \in \mathbb{R}$ continuous). |
| **Expected Risk Minimization** | $\min_f \; \mathbb{E}_{(x,y)\sim D}\bigl[\ell(f(x), y)\bigr]$ | True distribution $(x,y)\sim D$; model $f(x)$; loss $\ell(\cdot)$. |
| **Zero/One Loss** | $\ell(y,\hat{y}) = 0 \text{ if } y=\hat{y},\; \text{else } 1$ | Used for Classification accuracy/error. |
| **Squared Loss** | $\ell(y,\hat{y}) = (y - \hat{y})^2$ | Used for Regression (penalizes large errors quadratically). |
| **Unsupervised Dataset** | $D = \{x^{(i)}\}_{i=1}^n$ | No labels: **Clustering** (discrete ID $x'$) vs. **Dimensionality Reduction** (continuous $x'$). |
| **Reinforcement Learning (RL)** | $s_{t+1} = \delta(s_t, a_t),\; r_t = r(s_t,a_t),\; \pi:\mathcal{S}\to\mathcal{A}$ | Trajectory $s_0 \xrightarrow[r_0]{a_0} s_1 \xrightarrow[r_1]{a_1} s_2 \cdots$; delayed scalar rewards. |

---

### B. Linear Regression & Gradient Descent *(Lecture 2, Slides 1–33 — [[2. Linear Regression & Gradient Descent]])*

| Concept | Formula | Key Detail / Trap |
| :--- | :--- | :--- |
| **Hypothesis (Univariate / Multi)** | $h_\theta(x) = \theta_0 + \theta_1 x = \boldsymbol{\theta}^T\mathbf{x}$ | Prepend $x_0^{(i)} = 1$ (bias dummy feature). Equivalent to $a+bx$, $w_0+w_1x$, $\beta_0+\beta_1x$. |
| **Closed-Form OLS (Numerical Analysis)** | $b = \dfrac{\langle xy \rangle - \langle x \rangle\langle y \rangle}{\langle x^2 \rangle - \langle x \rangle^2}, \qquad a = \langle y \rangle - b\langle x \rangle$ | $\langle f \rangle = \frac{1}{N}\sum_{n=1}^N f(x^n, y^n)$; line passes through $(\langle x\rangle, \langle y\rangle)$. |
| **Normal Equation (Linear Algebra)** | $\vec{\beta} = (X^T X)^{-1} X^T \vec{y}$ | $2\times 2$ form: $\begin{bmatrix} N & \sum x_i \\ \sum x_i & \sum x_i^2 \end{bmatrix}^{-1}\begin{bmatrix} \sum y_i \\ \sum x_i y_i \end{bmatrix}$. Fails if $X^TX$ is singular ($O(n^3)$!). |
| **Half-MSE Cost Function** | $J(\boldsymbol{\theta}) = \dfrac{1}{2m}\sum_{i=1}^m \bigl(h_\theta(\mathbf{x}^{(i)}) - y^{(i)}\bigr)^2$ | The $\frac{1}{2}$ cancels the exponent $2$ during differentiation. |
| **Univariate Partial Derivatives** | $\dfrac{\partial J}{\partial \theta_0} = \dfrac{1}{m}\sum (h_\theta - y)$, $\quad \dfrac{\partial J}{\partial \theta_1} = \dfrac{1}{m}\sum x_i(h_\theta - y)$ | $\theta_0$ adjusts average bias; $\theta_1$ weights error by $x_i$. |
| **Batch GD Update (Simultaneous!)** | $\theta_j := \theta_j - \alpha \dfrac{1}{m}\sum_{i=1}^m \bigl(h_\theta(\mathbf{x}^{(i)}) - y^{(i)}\bigr)x_j^{(i)}$ | Compute `temp0, temp1` before overwriting! $1\text{ Epoch} = 1\text{ Iteration}$. Learning rate may also be written $\eta$. |
| **Stochastic GD (Widrow-Hoff / LMS)** | $\theta_j := \theta_j - \alpha\bigl(h_\theta(\mathbf{x}^{(i)}) - y^{(i)}\bigr)x_j^{(i)}$ | Shuffle before looping; updates **after every single sample**! $1\text{ Epoch} = m\text{ Iterations}$. |
| **Iterations Formula** | $\text{Iterations} = \text{Epochs} \times \text{Batches} = \text{Epochs} \times \left\lceil \dfrac{m}{B} \right\rceil$ | BGD: $B=m$; SGD: $B=1$; Mini-Batch: $1 < B < m$. |

---

### C. Logistic Regression, Metrics, Bias–Variance & Regularization *(Lecture 2, Slides 34–49 — [[3. Logistic Regression, Bias-Variance & Regularization]])*

| Concept | Formula | Key Detail |
| :--- | :--- | :--- |
| **Signed Distance to Hyperplane** | $\pm \dfrac{\mathbf{w}^T\mathbf{x} + b}{\|\mathbf{w}\|}$ | Hyperplane $\mathbf{w}^T\mathbf{x} + b = 0$; positive on $\mathbf{w}$ side, negative on opposite side. |
| **Sigmoid Function & Hypothesis** | $g(z) = \dfrac{1}{1 + e^{-z}}, \quad h_\theta(\mathbf{x}) = g(\boldsymbol{\theta}^T\mathbf{x})$ | Maps $(-\infty,\infty) \to (0,1)$. Predict $y=1$ iff $h_\theta(\mathbf{x}) \ge 0.5 \iff \boldsymbol{\theta}^T\mathbf{x} \ge 0$. |
| **Sigmoid Derivative** | $g'(z) = g(z)\bigl(1 - g(z)\bigr)$ | Maximum slope is $g'(0) = 0.25$ at $z = 0$. |
| **Binary Cross-Entropy (Log-Loss)** | $J(\theta) = -\dfrac{1}{m}\sum_{i=1}^m \Bigl[y^{(i)}\log h_\theta(x^{(i)}) + (1-y^{(i)})\log(1-h_\theta(x^{(i)}))\Bigr]$ | Convex! Note: some exams omit $\frac{1}{m}$ ($L = -\sum[\dots]$); $\log = \ln$. |
| **Logistic Regression Gradient** | $\dfrac{\partial L}{\partial w_j} = \sum_{i=1}^n (\hat{y}_i - y_i)x_{ij}$ *(or $\frac{1}{m}\sum$ for $J$)* | Derived via $\sigma'(z) = \sigma(z)(1-\sigma(z))$ cancellation; rescaling $x_k \to c x_k$ scales gradient by $c$. |
| **MLE Log-Likelihood** | $\ell(\theta) = \sum_{i=1}^m \Bigl[y^{(i)}\log h_\theta(x^{(i)}) + (1-y^{(i)})\log(1-h_\theta(x^{(i)}))\Bigr]$ | Derived from $p(y\mid x;\theta) = h^y(1-h)^{1-y}$. Note $J(\theta) = -\frac{1}{m}\ell(\theta)$. |
| **MAE / MSE / RMSE** | $\frac{1}{n}\sum\lvert y_i-\hat{y}_i\rvert \;\big|\; \frac{1}{n}\sum(y_i-\hat{y}_i)^2 \;\big|\; \sqrt{\text{MSE}}$ | $\text{RMSE} \ge \text{MAE}$ always; MAE is outlier-robust; RMSE restores units of $y$. |
| **L1 (Lasso) / L2 (Ridge) / Elastic Net** | $\text{Cost} + \lambda\sum\lvert w_i\rvert \;\big|\; +\lambda\sum w_i^2 \;\big|\; +\lambda_1\sum\lvert w_i\rvert + \lambda_2\sum w_i^2$ | Works on both Linear (MSE) & Logistic (BCE) loss; L1 gives **sparse 0s**; L2 shrinks smoothly; $\lambda\uparrow \implies$ bias$\uparrow$, variance$\downarrow$. |
| **Autoregression $\text{AR}(1)$ & $\text{AR}(p)$** | $X_t = c + \phi_1 X_{t-1} + \dots + \phi_p X_{t-p} + \varepsilon_t$ | Predicts time series from its own $p$ past lags + white noise $\varepsilon_t$. |

---

## 2. The 5 "Must-Memorize" Derivations for Quiz 1

Make sure you can reproduce every one of these **5 exact slide derivations** cleanly on blank paper:

1. **Closed-Form OLS Parameters $a$ and $b$** *(Lec 2, Slide 8)* $\longrightarrow$ [[2. Linear Regression & Gradient Descent#2.2 Complete Step-by-Step Closed-Form Derivation (Slide 8)|Full Steps]]
2. **Matrix Normal Equation $\vec{\beta} = (X^T X)^{-1} X^T \vec{y}$** *(Lec 2, Slide 11)* $\longrightarrow$ [[2. Linear Regression & Gradient Descent#3.2 Deriving the Normal Equation (Slide 11)|Full Steps]]
3. **Gradient Descent Partial Derivatives $\frac{\partial J}{\partial \theta_0}, \frac{\partial J}{\partial \theta_1}, \frac{\partial J}{\partial \theta_j}$** *(Lec 2, Slides 14, 15, 19)* $\longrightarrow$ [[2. Linear Regression & Gradient Descent#4.2 Complete Derivation of partial J / partial theta_0 and partial J / partial theta_1 (Slides 14–15)|Full Steps]]
4. **Sigmoid Derivative $g'(z) = g(z)\bigl(1 - g(z)\bigr)$** *(Lec 2, Slide 39)* $\longrightarrow$ [[3. Logistic Regression, Bias-Variance & Regularization#3.2 Complete Derivation of the Sigmoid Derivative (Slide 39)|Full Steps]]
5. **Logistic Regression MLE Log-Likelihood $\ell(\theta)$** *(Lec 2, Slide 43)* $\longrightarrow$ [[3. Logistic Regression, Bias-Variance & Regularization#4.3 Maximum Likelihood Estimation (MLE) & Log-Likelihood Derivation (Slides 42–43)|Full Steps]]

---

## 3. High-Yield Exam Traps & Quick Showdowns

> [!error] 7 Classic Quiz Traps to Watch Out For!
> 1. **SGD vs. BGD Hand-Trace Trap:** In **SGD**, weights update **after every single sample** (Sample 1's New Weights become Sample 2's Old Weights). In **BGD**, weights stay **frozen across every sample in the epoch** and update **only once** using the average gradient at the end!
> 2. **Simultaneous Update Trap:** Never compute $\theta_1$'s derivative using the already-updated $\theta_0$; compute both `temp0` and `temp1` from old $(\theta_0, \theta_1)$ first!
> 3. **Logistic Regression Is CLASSIFICATION:** Despite its name, it solves **classification** ($y \in \{0,1\}$), not regression!
> 4. **Why Linear Regression Fails Classification (2 Reasons):** (a) Far-right outliers tilt the line and shift the $0.5$ boundary, and (b) linear predictions exceed $[0, 1]$.
> 5. **Why Squared Error Fails Logistic Regression:** Sigmoid makes squared error **non-convex** with local minima; Binary Cross-Entropy / Log-Loss is convex.
> 6. **Data Leakage in Supervised Learning:** Fitting scalers (`StandardScaler`) or imputers (`df.mean()`) **before** splitting train/test leaks test statistics into training! Always split first!
> 7. **Training Error Alone Can't Spot Overfitting:** Training error keeps dropping to $0$ in the overfitting zone (Slide 47). Always check **Test / Validation Error** (High Train + High Test = **High Bias / Underfitting**; Low Train + High Test = **High Variance / Overfitting**).

### Gradient Descent vs. Normal Equation vs. Loss Functions

| Showdown | Option A | Option B | Winning Choice |
| :--- | :--- | :--- | :--- |
| **Small $n$ ($n \le 10^3$) vs. Huge $n$ ($n > 10^4$)** | Normal Eq. ($O(n^3)$, no $\alpha$) | Gradient Descent ($O(n)$/iter, needs $\alpha$) | Normal Eq. for small $n$; **GD for large $n$** or non-invertible $X^TX$. |
| **Clean data vs. Extreme outliers (Regression)** | MSE ($\frac{1}{n}\sum e_i^2$, quadratic) | MAE ($\frac{1}{n}\sum \lvert e_i\rvert$, linear) | MSE for clean data & smooth gradients; **MAE when outliers exist**. |
| **Sparse feature selection vs. Smooth shrinkage** | L1 Lasso ($\lambda\sum\lvert w_i\rvert$) | L2 Ridge ($\lambda\sum w_i^2$) | **L1** zeros out irrelevant weights; **L2** shrinks all weights smoothly. |

---

## 4. Full Timed 25-Mark Mock Quiz (Exam Simulation)

> [!tip] How to Use This Mock Quiz
> Grab blank paper and a calculator, set a **35-minute timer**, and solve all 5 questions completely before expanding the answer callouts!

### Mock Q1 [5 Marks — Foundations, 3 Pillars, $\langle T,P,E \rangle$ & Data Leakage]
1. **[2 Marks]** Explain the **3 Fundamental Pillars** of every ML algorithm (**Representation**, **Evaluation**, **Optimization**) and illustrate each using **Linear Regression**.
2. **[2 Marks]** Formulate **Credit Card Fraud Detection** (including the slide's asymmetric penalty detail) AND **Gait Recognition** as well-posed learning problems $\langle T, P, E \rangle$.
3. **[1 Mark]** Explain what **Data Leakage** is in supervised learning and why calling `StandardScaler().fit_transform(X)` before `train_test_split` causes it.

> [!success]- Answer & Rubric (Mock Q1 — 5 Marks)
> 1. **[2 Marks]** **Representation** = hypothesis family ($h_\theta(x) = \theta_0 + \theta_1 x = \boldsymbol{\theta}^T\mathbf{x}$); **Evaluation** = loss/cost scoring fit ($\text{MSE}/\text{SSE}$: $J(\theta) = \frac{1}{2m}\sum (h_\theta(x^{(i)})-y^{(i)})^2$); **Optimization** = algorithm finding optimal $\theta^*$ (OLS calculus, Normal Equation $(X^TX)^{-1}X^Ty$, or Gradient Descent). *(Also know Conventional Programming = Data+Program $\to$ Output vs. ML = Data+Output $\to$ Program, and Alpaydin's predictive vs. descriptive.)*
> 2. **[2 Marks]**
>    - **Credit Card Fraud (`1 mark`):** $T =$ classify transactions as `fraud`/`not fraud`; $P =$ classifier accuracy **with higher penalty when fraud is labeled as not fraud**; $E =$ historical labeled transactions.
>    - **Gait Recognition (`1 mark`):** $T =$ identify a person from walking pattern sequences; $P =$ identification accuracy on unseen gait sequences; $E =$ labeled database of walking sequences.
> 3. **[1 Mark]** Data leakage occurs when test-set or future target information contaminates training. Calling `.fit_transform(X)` before splitting computes $\mu$ and $\sigma$ using test rows, leaking test distribution info into training.

---

### Mock Q2 [5 Marks — Linear Regression Calculus & Simultaneous Update]
1. **[3 Marks]** Starting from $J(\theta_0, \theta_1) = \frac{1}{2m}\sum_{i=1}^m (y_i - (\theta_0 + \theta_1 x_i))^2$, derive both $\frac{\partial J}{\partial \theta_0}$ and $\frac{\partial J}{\partial \theta_1}$ step by step, and briefly interpret what each term measures physically.
2. **[2 Marks]** Write the **correct simultaneous** Gradient Descent update pseudocode for $(\theta_0, \theta_1)$ and explain the bug caused by updating $\theta_0$ in place before computing $\text{temp1}$.

> [!success]- Answer & Rubric (Mock Q2 — 5 Marks)
> 1. **[3 Marks]**
>    - $\frac{\partial J}{\partial \theta_0} = \frac{1}{2m}\sum_{i=1}^m 2(y_i - \theta_0 - \theta_1 x_i)(-1) = \mathbf{\frac{1}{m}\sum_{i=1}^m (h_\theta(x_i) - y_i)}$ (`1 mark`). Measures the average prediction error (whether predictions are overall too high/low) (`0.5 mark`).
>    - $\frac{\partial J}{\partial \theta_1} = \frac{1}{2m}\sum_{i=1}^m 2(y_i - \theta_0 - \theta_1 x_i)(-x_i) = \mathbf{\frac{1}{m}\sum_{i=1}^m x_i(h_\theta(x_i) - y_i)}$ (`1 mark`). Measures the error weighted by input $x_i$ (high-$x_i$ points have stronger leverage on slope) (`0.5 mark`).
> 2. **[2 Marks]**
>    - Correct pseudocode (`1 mark`): compute `temp0 := θ0 - α*(∂J/∂θ0)` and `temp1 := θ1 - α*(∂J/∂θ1)` first, then assign `θ0 := temp0; θ1 := temp1`.
>    - Bug explanation (`1 mark`): updating $\theta_0$ immediately causes $\frac{\partial J}{\partial \theta_1}$ to evaluate $h_\theta(x_i)$ using the **new** $\theta_0$ instead of the **old** $(\theta_0, \theta_1)$, violating true gradient descent.

---

### Mock Q3 [6 Marks — Numerical Hand-Trace: SGD vs. BGD]
Consider the dataset with $2$ features ($x_1, x_2$) and hypothesis $\hat{y} = w_0 + w_1 x_1 + w_2 x_2$:
- Sample 1: $(x_1, x_2) = (1, 2),\; y = 5$
- Sample 2: $(x_1, x_2) = (2, 1),\; y = 4$
- Sample 3: $(x_1, x_2) = (1, 1),\; y = 3$

Given initial weights $(w_0, w_1, w_2) = (0, 0, 0)$ and learning rate $\alpha = 0.1$:
1. **[3 Marks]** Perform **1 full epoch of Stochastic Gradient Descent (SGD)**, showing the weights after every sample and the **Epoch 1 Average SGD Loss** ($L^{(i)} = \frac{1}{2}(\hat{y}^{(i)} - y^{(i)})^2$).
2. **[3 Marks]** Perform **1 full epoch of Batch Gradient Descent (BGD)**, showing the gradient terms per sample, the single end-of-epoch weight update, and the **Epoch 1 BGD MSE**.

> [!success]- Answer & Rubric (Mock Q3 — 6 Marks)
> 1. **SGD Epoch 1 [3 Marks]:** ($\Delta w_j = -0.1(\hat{y}-y)x_j$)
>    - **Sample 1** $(1, 2, y=5)$, old $(0, 0, 0)$: $\hat{y}=0$, Err $=-5$, $\Delta w = (0.5, 0.5, 1.0) \implies$ **New $(0.5, 0.5, 1.0)$**, $L^{(1)} = 0.5(25) = 12.5$.
>    - **Sample 2** $(2, 1, y=4)$, old $(0.5, 0.5, 1.0)$: $\hat{y} = 0.5 + 0.5(2) + 1.0(1) = 2.5$, Err $= -1.5$, $\Delta w = (0.15, 0.30, 0.15) \implies$ **New $(0.65, 0.80, 1.15)$**, $L^{(2)} = 0.5(2.25) = 1.125$.
>    - **Sample 3** $(1, 1, y=3)$, old $(0.65, 0.80, 1.15)$: $\hat{y} = 0.65 + 0.80 + 1.15 = 2.60$, Err $= -0.40$, $\Delta w = (0.04, 0.04, 0.04) \implies$ **New $(0.69, 0.84, 1.19)$**, $L^{(3)} = 0.5(0.16) = 0.08$.
>    - **Avg SGD Loss:** $(12.5 + 1.125 + 0.08)/3 = 13.705 / 3 \approx \mathbf{4.5683}$.
> 2. **BGD Epoch 1 [3 Marks]:** (All 3 samples use frozen $(0, 0, 0) \implies \hat{y} = 0$)
>    - **Sample 1:** Err $= -5 \implies (\text{Err}\cdot 1, \text{Err}\cdot x_1, \text{Err}\cdot x_2) = (-5, -5, -10)$
>    - **Sample 2:** Err $= -4 \implies (-4, -8, -4)$
>    - **Sample 3:** Err $= -3 \implies (-3, -3, -3)$
>    - **Sum:** $(-12, -16, -17)$ $\xrightarrow{\div 3}$ **Avg Grad:** $(-4.000, -5.333, -5.667)$
>    - **New Weights ($\times (-0.1)$):** **$(0.400,\; 0.533,\; 0.567)$**
>    - **Epoch 1 BGD MSE:** $\dfrac{(-5)^2 + (-4)^2 + (-3)^2}{3} = \dfrac{25 + 16 + 9}{3} = \dfrac{50}{3} \approx \mathbf{16.667}$.

---

### Mock Q4 [5 Marks — Logistic Regression, BCE Gradients, Rescaling & Ridge]
1. **[1.5 Marks]** Prove that $g'(z) = g(z)\bigl(1 - g(z)\bigr)$ for $g(z) = \dfrac{1}{1 + e^{-z}}$. *(Also review the MLE Log-Likelihood $\ell(\theta)$ derivation and Problem 11 in `3.`!)*
2. **[2.5 Marks]** Given 2 samples $(x_1=1, y=0)$ and $(x_1=3, y=1)$ with $z = w_0 + w_1 x_1$, $\hat{y} = \sigma(z)$, and initial weights $w_0 = w_1 = 0$: compute the total BCE loss $L = -\sum [y_i\ln\hat{y}_i + (1-y_i)\ln(1-\hat{y}_i)]$ and gradients $\frac{\partial L}{\partial w_0}, \frac{\partial L}{\partial w_1}$.
3. **[1 Mark]** If $x_1$ is rescaled to $100x_1$, state the new numerical value of $\frac{\partial L}{\partial w_1}$ and write the Ridge-regularized BCE loss.

> [!success]- Answer & Rubric (Mock Q4 — 5 Marks)
> 1. **[1.5 Marks]** $g'(z) = \frac{e^{-z}}{(1+e^{-z})^2} = \frac{1}{1+e^{-z}} \cdot \frac{(1+e^{-z})-1}{1+e^{-z}} = g(z)(1-g(z))$.
> 2. **[2.5 Marks]** At $w_0=w_1=0$, $z_i = 0 \implies \hat{y}_i = 0.5$ for both samples.
>    - Total loss: $L = -2\ln(0.5) = 2\ln(2) \approx \mathbf{1.3863}$ (`1 mark`).
>    - Residuals $(\hat{y}_i - y_i)$: Sample 1 is $0.5 - 0 = +0.5$; Sample 2 is $0.5 - 1 = -0.5$.
>    - $\frac{\partial L}{\partial w_0} = 0.5(1) - 0.5(1) = \mathbf{0}$; $\;\frac{\partial L}{\partial w_1} = 0.5(1) - 0.5(3) = \mathbf{-1.0}$ (`1.5 marks`).
> 3. **[1 Mark]** Rescaling $x_1 \to 100x_1$ gives $\frac{\partial L}{\partial w_1'} = 0.5(100) - 0.5(300) = \mathbf{-100}$ ($100\times$ larger magnitude). Ridge loss: $L_{\text{Ridge}} = -\sum [y_i\ln\hat{y}_i + (1-y_i)\ln(1-\hat{y}_i)] + \lambda w_1^2$.

---

### Mock Q5 [4 Marks — Bias–Variance, Regularization & Metrics]
1. **[2 Marks]** Sketch/describe the **Bias–Variance Trade-off** (Slide 47): how do $\text{Bias}^2$, $\text{Variance}$, Training Error, and Generalization (Test) Error behave as model complexity increases?
2. **[2 Marks]** Write the regularized Linear Regression cost functions for **L1 (Lasso)** and **L2 (Ridge)**, and state one key difference in how they affect model weights $w_i$.

> [!success]- Answer & Rubric (Mock Q5 — 4 Marks)
> 1. **[2 Marks]** As complexity increases: $\text{Bias}^2$ strictly decreases, $\text{Variance}$ strictly increases, Training Error monotonically decreases toward zero, and Test Error ($\text{Bias}^2 + \text{Variance} + \text{Irreducible Error}$) forms a **U-shaped curve**—high on the left (**underfitting zone**), minimized at the middle sweet spot, and high again on the right (**overfitting zone**).
> 2. **[2 Marks]**
>    - **L1 (Lasso):** $\frac{1}{2m}\sum (y_i - h(x_i))^2 + \lambda \sum_{i=1}^n |w_i|$ (`0.5 mark`)
>    - **L2 (Ridge):** $\frac{1}{2m}\sum (y_i - h(x_i))^2 + \lambda \sum_{i=1}^n w_i^2$ (`0.5 mark`)
>    - **Key Difference (`1 mark`):** L1 forces unimportant weights to **exact $0$** (producing sparse models / automatic feature selection), whereas L2 shrinks weights smoothly toward zero without forcing them strictly to zero.

---

## 5. All-in-One Continuous Reader (Full Syllabus Embedded)

If you want to read **every single lecture, slide screenshot, derivation, and practice problem continuously in one single note** without switching files, the three complete lecture notes are embedded below:

### Part 1: Lectures 0 & 1 (`1. Intro`)
![[1. Intro]]

---

### Part 2: Lecture 2 — Linear Regression & Gradient Descent
![[2. Linear Regression & Gradient Descent]]

---

### Part 3: Lecture 2 — Logistic Regression, Bias–Variance & Regularization
![[3. Logistic Regression, Bias-Variance & Regularization]]
