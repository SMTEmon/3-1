---
title: "Epochs vs. Batches vs. Iterations (In-Class Quick Note)"
course: CSE 4553 (Machine Learning)
lecture: 2
date: 2026-11-16
tags:
  - machine-learning
  - cse-4553
  - gradient-descent
  - quiz-1
aliases:
  - Epochs vs Iteration
  - No Fking Clue
---

# Epochs vs. Batches vs. Iterations

> [!info] Connected Full Lecture Note
> This quick note expands directly on **[[2. Linear Regression & Gradient Descent#5. Batch GD vs. Stochastic GD (SGD) vs. Mini-Batch GD & Worked Numerical Examples (Slides 20, 22–27)|Lecture 2: Batch vs. Stochastic vs. Mini-Batch Gradient Descent]]** and **[[ML Quiz 1]]**.

> [!note] My Original In-Class Note
> ```text
> Epochs vs Iteration
> - iterations = Epoch * Batchs
> ```

---

## Why That Single Formula Solves Everything!

That exact formula you wrote in class (`iterations = Epoch * Batches`, written by Dr. Hasan Mahmud in red ink on **Slide 22** of `ML_Lecture 2.pdf`) is the master relationship linking **Epochs**, **Batch Size**, and **Iterations**:

![[lec2_slide22_batch_vs_sgd_annotations.png]]

### The 4 Terms Decoded

Suppose your training dataset has **$m$ training examples** (say $m = 1000$):

1. **Epoch:** **One full pass** through all $m$ training examples in the dataset.
2. **Batch Size ($B$):** How many examples the algorithm looks at **before doing 1 weight update**.
3. **Number of Batches (per Epoch):** How many chunks of size $B$ it takes to cover all $m$ examples:
   $$\text{Batches per Epoch} = \left\lceil \frac{m}{B} \right\rceil$$
4. **Iteration:** **One single weight update step** ($\theta_j := \theta_j - \alpha \frac{\partial J}{\partial \theta_j}$). Each batch produces **1 iteration**!

Therefore:
$$\boxed{\text{Total Iterations} = \text{Epochs} \times \text{Batches per Epoch} = \text{Epochs} \times \left\lceil \frac{m}{B} \right\rceil}$$

---

### How It Applies to the 3 Gradient Descent Variants ($m = 1000$, $5$ Epochs)

| Variant | Slide Nickname | Batch Size ($B$) | Batches per Epoch | Iterations per Epoch | Total Iterations in 5 Epochs |
| :--- | :--- | :---: | :---: | :---: | :---: |
| **Batch Gradient Descent (BGD)** | *"Many-at-a-time"* | $B = m = 1000$ | $1$ | $1$ | $5 \times 1 = \mathbf{5}$ |
| **Stochastic Gradient Descent (SGD)** | *"One-at-a-time"* | $B = 1$ | $1000$ | $1000$ | $5 \times 1000 = \mathbf{5,000}$ |
| **Mini-Batch Gradient Descent** | *"Mini-batch of $n$ points"* | e.g., $B = 100$ | $1000 / 100 = 10$ | $10$ | $5 \times 10 = \mathbf{50}$ |

> [!tip] Quick Intuition Check
> - In **Slide 23 (SGD numerical example)**, there are $m = 3$ training samples. So **$1\text{ Epoch} = 3\text{ Iterations}$** (weights update after Sample 1, Sample 2, and Sample 3). Over 4 epochs (Slide 24), that is $4 \times 3 = \mathbf{12\text{ iterations}}$!
> - In **Slide 25–26 (BGD numerical example)**, there are $m = 3$ training samples, and the batch size is all $3$. So **$1\text{ Epoch} = 1\text{ Iteration}$** (all 3 samples use the same frozen weights, and weights update once at the end of the epoch). Over 4 epochs (Slide 26), that is $4 \times 1 = \mathbf{4\text{ iterations}}$!
