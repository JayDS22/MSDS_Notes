# Data601 Homework 2 - Complete Solutions

## Problem 1: Probability that John gets a head before Sarah

**Given:**
- John tosses a coin with P(H) = p
- Sarah tosses a coin with P(H) = q
- Tosses are independent
- Find: P(John gets head before Sarah)

**Solution:**

Let A = "John gets head before Sarah"

We can think of this as a race where both keep tossing until one gets a head. John wins if:
1. John gets head on first toss and Sarah doesn't: p(1-q)
2. Both get tails on first toss, then John wins the "restart": (1-p)(1-q)·P(A)

Setting up the equation:
P(A) = p(1-q) + p·q·(1/2) + (1-p)(1-q)·P(A)

Wait, let me reconsider. If both get heads simultaneously, we need to define what happens. Let's assume we want the probability John gets his first head before Sarah gets her first head.

The cleaner approach: John wins if he gets a head before Sarah. Let's consider all possible outcomes:

P(A) = p(1-q) + (1-p)(1-q)·P(A)

This accounts for:
- John gets H, Sarah gets T: probability p(1-q)
- Both get T, game continues: probability (1-p)(1-q), then we're back to the original situation

Solving for P(A):
P(A) = p(1-q) + (1-p)(1-q)·P(A)
P(A) - (1-p)(1-q)·P(A) = p(1-q)
P(A)[1 - (1-p)(1-q)] = p(1-q)
P(A)[1 - (1-p-q+pq)] = p(1-q)
P(A)[p + q - pq] = p(1-q)

**Answer:** P(A) = p(1-q)/(p + q - pq)

---

## Problem 2: Disease Testing with Sensitivity and Specificity

**Given:**
- Disease prevalence: P(D) = p
- Sensitivity: P(Pos|D) = q₁
- Specificity: P(Neg|Dᶜ) = q₂
- Therefore: P(Pos|Dᶜ) = 1 - q₂

### Part (a): Calculate P(D|Pos) and P(Dᶜ|Neg)

Using Bayes' theorem:

**P(D|Pos):**
P(D|Pos) = P(Pos|D)·P(D) / P(Pos)

Where P(Pos) = P(Pos|D)·P(D) + P(Pos|Dᶜ)·P(Dᶜ)
= q₁·p + (1-q₂)·(1-p)

Therefore:
**P(D|Pos) = q₁p / [q₁p + (1-q₂)(1-p)]**

**P(Dᶜ|Neg):**
P(Dᶜ|Neg) = P(Neg|Dᶜ)·P(Dᶜ) / P(Neg)

Where P(Neg) = P(Neg|D)·P(D) + P(Neg|Dᶜ)·P(Dᶜ)
= (1-q₁)·p + q₂·(1-p)

Therefore:
**P(Dᶜ|Neg) = q₂(1-p) / [(1-q₁)p + q₂(1-p)]**

### Part (b): Find minimum q₂ when P(D) = 0.99, q₁ = 0.95, P(D|Pos) ≥ 0.9

Given: p = 0.99, q₁ = 0.95

We need: 0.95 × 0.99 / [0.95 × 0.99 + (1-q₂) × 0.01] ≥ 0.9

Simplifying:
0.9405 / [0.9405 + 0.01(1-q₂)] ≥ 0.9
0.9405 ≥ 0.9[0.9405 + 0.01(1-q₂)]
0.9405 ≥ 0.84645 + 0.009(1-q₂)
0.09405 ≥ 0.009(1-q₂)
10.45 ≥ 1-q₂
q₂ ≥ -9.45

Since q₂ must be ≤ 1, **any q₂ ∈ (0,1] works**.

**Interpretation:** When disease prevalence is very high (99%), even a test with poor specificity will have good positive predictive value because most positive results are true positives.

### Part (c): Find minimum q₂ when P(D) = 0.01, q₁ = 0.95, P(Dᶜ|Neg) ≥ 0.9

Given: p = 0.01, q₁ = 0.95

We need: q₂ × 0.99 / [0.05 × 0.01 + q₂ × 0.99] ≥ 0.9

Simplifying:
0.99q₂ / [0.0005 + 0.99q₂] ≥ 0.9
0.99q₂ ≥ 0.9[0.0005 + 0.99q₂]
0.99q₂ ≥ 0.00045 + 0.891q₂
0.099q₂ ≥ 0.00045
q₂ ≥ 0.00454545...

**Minimum q₂ = 0.00455** (approximately)

**Interpretation:** When disease prevalence is very low (1%), we need only minimal specificity to ensure that negative results are reliable, since most people truly don't have the disease.

---

## Problem 3: Multiple Testing Scenario

**Given:**
- Disease prevalence: P(D) = p
- Test accuracy: P(correct) = q (so P(Pos|D) = P(Neg|Dᶜ) = q)
- Therefore: P(Pos|Dᶜ) = P(Neg|D) = 1-q

### Part (a): P(D|n positive tests)

Let Posₙ = "all n tests are positive"

P(Posₙ|D) = qⁿ
P(Posₙ|Dᶜ) = (1-q)ⁿ

Using Bayes:
**qₙ = P(D|Posₙ) = pqⁿ / [pqⁿ + (1-p)(1-q)ⁿ]**

### Part (b): lim(n→∞) qₙ

As n → ∞:
- If q > 1-q (i.e., q > 0.5): qⁿ grows faster than (1-q)ⁿ, so qₙ → 1
- If q < 0.5: (1-q)ⁿ grows faster than qⁿ, so qₙ → 0  
- If q = 0.5: qₙ → p/(p + (1-p)) = p

**Answer:**
- If q > 0.5: lim qₙ = 1
- If q < 0.5: lim qₙ = 0
- If q = 0.5: lim qₙ = p

### Part (c): P(Dᶜ|n negative tests)

Let Negₙ = "all n tests are negative"

P(Negₙ|D) = (1-q)ⁿ
P(Negₙ|Dᶜ) = qⁿ

**qₙ = P(Dᶜ|Negₙ) = (1-p)qⁿ / [p(1-q)ⁿ + (1-p)qⁿ]**

### Part (d): lim(n→∞) qₙ for negative tests

Following similar logic:
- If q > 0.5: lim qₙ = 1
- If q < 0.5: lim qₙ = 0  
- If q = 0.5: lim qₙ = 1-p

---

## Problem 4: Coin Selection - Time to First Head

**Given:**
- 10 coins total: 4 fair (p=0.5), 3 with p=0.8, 3 with p=0.2
- Randomly select one coin, toss until head appears
- Eₖ = "at most k tosses to get head"

**Setup:**
- P(Fair) = 4/10 = 0.4
- P(p=0.8) = 3/10 = 0.3  
- P(p=0.2) = 3/10 = 0.3

For geometric distribution, P(at most k tosses) = 1 - (1-p)ᵏ

**P(Eₖ|Fair) = 1 - (0.5)ᵏ**
**P(Eₖ|p=0.8) = 1 - (0.2)ᵏ**
**P(Eₖ|p=0.2) = 1 - (0.8)ᵏ**

**P(Eₖ) = 0.4[1-(0.5)ᵏ] + 0.3[1-(0.2)ᵏ] + 0.3[1-(0.8)ᵏ]**
**= 1 - 0.4(0.5)ᵏ - 0.3(0.2)ᵏ - 0.3(0.8)ᵏ**

### Part (a): P(Fair|Eₖ)
**P(Fair|Eₖ) = 0.4[1-(0.5)ᵏ] / [1 - 0.4(0.5)ᵏ - 0.3(0.2)ᵏ - 0.3(0.8)ᵏ]**

### Part (b): P(p=0.8|Eₖ)  
**P(p=0.8|Eₖ) = 0.3[1-(0.2)ᵏ] / [1 - 0.4(0.5)ᵏ - 0.3(0.2)ᵏ - 0.3(0.8)ᵏ]**

### Part (c): P(p=0.2|Eₖ)
**P(p=0.2|Eₖ) = 0.3[1-(0.8)ᵏ] / [1 - 0.4(0.5)ᵏ - 0.3(0.2)ᵏ - 0.3(0.8)ᵏ]**

---

## Problem 5: Coin Selection - Binomial Outcomes

**Given:**
- Same coin setup as Problem 4
- Toss selected coin 10 times
- Eₖ = "at most k heads in 10 tosses"

**Setup:**
For binomial distribution with n=10:
**P(Eₖ|Fair) = Σⱼ₌₀ᵏ C(10,j)(0.5)¹⁰**
**P(Eₖ|p=0.8) = Σⱼ₌₀ᵏ C(10,j)(0.8)ʲ(0.2)¹⁰⁻ʲ**  
**P(Eₖ|p=0.2) = Σⱼ₌₀ᵏ C(10,j)(0.2)ʲ(0.8)¹⁰⁻ʲ**

**P(Eₖ) = 0.4·P(Eₖ|Fair) + 0.3·P(Eₖ|p=0.8) + 0.3·P(Eₖ|p=0.2)**

### Part (a): P(Fair|Eₖ)
**P(Fair|Eₖ) = 0.4·P(Eₖ|Fair) / P(Eₖ)**

### Part (b): P(p=0.8|Eₖ)
**P(p=0.8|Eₖ) = 0.3·P(Eₖ|p=0.8) / P(Eₖ)**

### Part (c): P(p=0.2|Eₖ)
**P(p=0.2|Eₖ) = 0.3·P(Eₖ|p=0.2) / P(Eₖ)**

The exact values depend on the specific value of k and require computing the binomial sums.