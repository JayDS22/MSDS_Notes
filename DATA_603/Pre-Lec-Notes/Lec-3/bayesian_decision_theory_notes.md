# Introduction to Optimization: Bayesian Decision Theory
**Professor Richard La | University of Maryland | September 16, 2025**

---

## **Course Context & Today's Focus**

Building on our foundation in linear algebra and probability theory, today we transition into **optimization through the lens of uncertainty**. Bayesian Decision Theory provides the mathematical framework for making optimal decisions when we have incomplete information about the world.

**Why This Matters**: Most real-world optimization problems involve uncertainty - we don't know exact demand, precise system parameters, or future conditions. Bayesian methods give us principled ways to incorporate this uncertainty into our decision-making.

---

## **I. Fundamental Framework of Decision Theory**

### **1.1 The Decision Problem Setup**

Every decision problem consists of four key components:

**🎯 Decision Space (Actions)**: A = {a₁, a₂, ..., aₘ}
- The set of all possible actions/decisions available to the decision maker
- **Example**: Choose investment portfolio, select production level, pick algorithm parameters

**🌍 State Space (Nature)**: Θ = {θ₁, θ₂, ..., θₙ}  
- The set of all possible "states of the world" or unknown parameters
- **Example**: Market conditions, actual demand, true model parameters

**💰 Loss Function**: L(θ, a)
- The "cost" or "loss" incurred when taking action a in state θ
- **Alternative**: Utility function U(θ, a) = -L(θ, a)

**📊 Information Structure**: What we know about θ
- **Prior information**: P(θ) - our initial beliefs
- **Sample information**: Data/observations that update our beliefs

### **1.2 The Bayesian Philosophy**

**Core Principle**: Uncertainty about unknown quantities should be represented by probability distributions.

**Subjective Probability**: Probabilities represent degrees of belief, not just frequencies
- Prior P(θ): Our initial uncertainty about θ  
- Likelihood P(data|θ): How likely observed data is under each θ
- Posterior P(θ|data): Updated beliefs after observing data

---

## **II. Bayes' Theorem in Decision Making**

### **2.1 The Fundamental Update Rule**

```
P(θ|x) = P(x|θ) × P(θ) / P(x)
```

**Components Explained**:
- **P(θ|x)**: Posterior - what we believe about θ after seeing data x
- **P(x|θ)**: Likelihood - how well each θ explains the observed data  
- **P(θ)**: Prior - our initial beliefs about θ
- **P(x)**: Evidence/Marginal - normalizing constant = ∑ P(x|θ)P(θ)

### **2.2 Sequential Learning**

**Key Insight**: Today's posterior becomes tomorrow's prior

**Mathematical Formulation**:
```
P(θ|x₁, x₂) ∝ P(x₂|θ) × P(θ|x₁)
                ↑           ↑
           new data    previous posterior
```

**This enables**: Dynamic updating as new information arrives

### **2.3 Conjugate Priors**

**Definition**: Prior and posterior belong to the same distributional family

**Most Important Examples**:
1. **Beta-Binomial**: Beta prior + Binomial likelihood → Beta posterior
2. **Normal-Normal**: Normal prior + Normal likelihood → Normal posterior  
3. **Gamma-Poisson**: Gamma prior + Poisson likelihood → Gamma posterior

**Why Conjugacy Matters**: Analytical solutions instead of numerical approximation

---

## **III. Decision Rules and Optimality**

### **3.1 Types of Decision Rules**

**Non-randomized Decision Rule**: δ: X → A
- For each possible observation x, specify exactly one action δ(x)

**Randomized Decision Rule**: δ: X → Δ(A)  
- For each x, specify a probability distribution over actions

### **3.2 Risk Functions**

**Frequentist Risk**: R(θ, δ) = E[L(θ, δ(X))|θ]
- Expected loss when true parameter is θ and using decision rule δ

**Bayes Risk**: r(π, δ) = E[R(θ, δ)] = ∫ R(θ, δ) π(θ) dθ
- Expected risk with respect to prior distribution π

### **3.3 Optimality Criteria**

**Bayes Decision Rule**: δ*_π minimizes Bayes risk
```
δ*_π = arg min_δ r(π, δ)
```

**Minimax Decision Rule**: δ*_M minimizes maximum risk
```
δ*_M = arg min_δ max_θ R(θ, δ)
```

**Relationship**: As prior becomes "non-informative," Bayes rules approach minimax rules

---

## **IV. The Posterior Decision Problem**

### **4.1 Acting on Posterior Beliefs**

Once we observe data x, the optimal action solves:
```
a* = arg min_a ∫ L(θ, a) P(θ|x) dθ
```

**Interpretation**: Choose action that minimizes expected loss with respect to posterior beliefs

### **4.2 Common Loss Functions**

**Quadratic Loss**: L(θ, a) = (θ - a)²
- **Optimal Action**: Posterior mean E[θ|x]
- **Used for**: Parameter estimation, regression

**Absolute Loss**: L(θ, a) = |θ - a|  
- **Optimal Action**: Posterior median
- **Used for**: Robust estimation

**0-1 Loss**: L(θ, a) = 1{θ ≠ a}
- **Optimal Action**: Posterior mode (MAP estimate)  
- **Used for**: Classification, hypothesis testing

### **4.3 Credible Intervals**

**95% Credible Interval**: [θₗ, θᵤ] such that P(θₗ ≤ θ ≤ θᵤ|x) = 0.95

**Interpretation**: "There's a 95% probability the true θ lies in this interval"
- **Different from**: Confidence intervals (frequentist interpretation)

---

## **V. Computational Approaches**

### **5.1 When Analytical Solutions Exist**

**Conjugate Priors + Simple Loss Functions**:
- Beta-Binomial with quadratic loss
- Normal-Normal with quadratic loss
- Closed-form posterior distributions and optimal actions

### **5.2 When Numerical Methods Are Needed**

**Markov Chain Monte Carlo (MCMC)**:
- Generate samples from complex posterior distributions
- Use samples to approximate posterior expectations

**Variational Inference**:
- Approximate complex posteriors with simpler distributions
- Optimize KL divergence to find best approximation

**Grid Methods**:
- Discretize parameter space
- Compute posterior probabilities on grid points

---

## **VI. Applications in Optimization**

### **6.1 Robust Optimization**

**Traditional Approach**: Optimize for worst-case scenario
**Bayesian Approach**: Weight scenarios by their posterior probabilities

**Example**: Portfolio optimization under parameter uncertainty
```
max_w E[return|data] - λ × Var[return|data]
```

### **6.2 Multi-Armed Bandit Problems**

**Setup**: Choose among K arms, each with unknown reward distribution
**Bayesian Solution**: 
1. Maintain posterior beliefs about each arm's parameters
2. Use Thompson sampling or Upper Confidence Bounds
3. Balance exploration vs. exploitation optimally

### **6.3 Bayesian Optimization**

**Problem**: Optimize expensive black-box function f(x)
**Approach**:
1. Place GP prior on f(x)
2. Update posterior after each function evaluation  
3. Choose next x to maximize acquisition function (exploration/exploitation tradeoff)

---

## **VII. Advanced Topics**

### **7.1 Hierarchical Models**

**Structure**: θ ~ G(α), where α ~ H(β)
- **Level 1**: Data distribution depends on θ
- **Level 2**: θ distribution depends on hyperparameter α  
- **Level 3**: α has prior distribution with hyperparameter β

**Benefits**: Borrows strength across related problems

### **7.2 Non-informative Priors**

**Jeffreys Prior**: π(θ) ∝ √|I(θ)|
- Where I(θ) is Fisher Information Matrix
- Invariant under parameter transformations

**Reference Priors**: Maximize expected information gain from data

### **7.3 Decision Theory for Model Selection**

**Problem**: Choose between competing models M₁, M₂, ..., Mₖ
**Bayesian Approach**: Compute posterior model probabilities
```
P(Mᵢ|data) ∝ P(data|Mᵢ) × P(Mᵢ)
```
Where P(data|Mᵢ) is the marginal likelihood (evidence) for model i

---

## **VIII. Key Theorems and Results**

### **8.1 Complete Class Theorem**

**Statement**: Under mild regularity conditions, every admissible decision rule is either Bayes or a limit of Bayes rules

**Implication**: Bayesian approach is "complete" - no good decision rules are excluded

### **8.2 Blackwell-Rao Theorem**

**Setup**: If δ is any decision rule and T is sufficient statistic
**Result**: δ*(x) = E[δ(X)|T(X)] has uniformly smaller risk

**Application**: Always condition on sufficient statistics when possible

### **8.3 Stein's Paradox**

**Surprising Result**: When estimating θ ∈ ℝᵖ with p ≥ 3 under quadratic loss, the sample mean is inadmissible

**Bayesian Interpretation**: Shrinkage estimators (pulling toward prior) can dominate MLE

---

## **IX. Connections to Machine Learning**

### **9.1 Bayesian Neural Networks**

**Standard NN**: Point estimates of weights
**Bayesian NN**: Posterior distributions over weights
- Uncertainty quantification in predictions
- Natural regularization through weight priors

### **9.2 Gaussian Processes**

**Non-parametric Bayesian method**:
- Prior over functions: f ~ GP(μ(x), k(x,x'))
- Posterior after observing data: analytical for Gaussian noise
- Optimal predictions under quadratic loss

### **9.3 Bayesian Reinforcement Learning**

**Problem**: Unknown environment dynamics
**Approach**: Maintain posterior over MDP parameters
- Thompson sampling for exploration
- Bayes-optimal policies (when tractable)

---

## **X. Potential Exam Questions & Key Concepts**

### **Conceptual Understanding**
1. "Explain the difference between Bayesian and frequentist approaches to uncertainty"
2. "When would you prefer minimax to Bayes decision rules?"
3. "What role does the loss function play in determining optimal actions?"

### **Mathematical Derivations**
1. "Derive the posterior mean as optimal under quadratic loss"
2. "Show that posterior mode minimizes 0-1 loss"  
3. "Prove that Bayes risk decreases with additional information"

### **Applied Problems**
1. "Design a Bayesian approach to A/B testing"
2. "Formulate inventory management as a Bayesian decision problem"
3. "Compare Bayesian and classical approaches to linear regression"

### **Computational Methods**
1. "When would you use MCMC vs. variational inference?"
2. "Implement Thompson sampling for multi-armed bandits"
3. "Design acquisition function for Bayesian optimization"

---

## **Key Takeaways**

1. **Uncertainty is Fundamental**: Real optimization problems involve unknown parameters
2. **Bayes Provides Framework**: Principled way to incorporate uncertainty and prior knowledge
3. **Loss Functions Matter**: Different losses lead to different optimal actions
4. **Learning is Dynamic**: Update beliefs as new information arrives
5. **Computation is Key**: Modern Bayesian methods rely on sophisticated algorithms
6. **Applications are Broad**: From simple estimation to complex ML algorithms

**Professor La's Emphasis**: Understand both the theory (why Bayesian methods work) and the practice (how to implement them for real problems).