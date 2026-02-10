# Study Notes: UTokyo CS Entrance Exam (Math)
**Textbook:** Probability and Statistics for Engineering and the Sciences (Devore)
**Goal:** Master Calculus-based Statistics & Probability

---

## 🚨 Critical Gaps to Fill (Not in Book?)
*The Devore book is an Engineering text and may skip these theoretical requirements. Verify immediately.*

- [ ] **Moment Generating Functions (MGFs):**
    - Does Ch 4 cover finding MGFs ($E[e^{tX}]$)?
    - *Action:* If no, learn from Bertsekas or Wikipedia.
- [ ] **Transformation of Variables (Jacobian Method):**
    - Does Ch 5.5 cover $Y = g(X_1, X_2)$ using determinants?
    - *Action:* If it only covers linear sums ($aX+bY$), find a supplement on the "Jacobian Method" for statistics.

---

## 📚 Tier 1: Critical Core (The "Fail" Zone)
*These chapters map to 80% of UTokyo exam questions. Focus on **derivation**, not just formulas.*

### Chapter 3: Discrete Random Variables
**Focus:** Sections 3.4 (Binomial) & 3.6 (Poisson)
- [ ] **Concept:** Mass Functions (PMF) & CDFs.
- [ ] **Drill:** Derive Mean ($E[X]$) and Variance ($V[X]$) for Binomial distribution from summation def.
- [ ] **Drill:** Derive Mean/Variance for Poisson distribution from summation def.

### Chapter 4: Continuous Random Variables
**Focus:** Sections 4.3 (Normal) & 4.4 (Exponential/Gamma)
- [ ] **Concept:** Integrating PDFs to find Probabilities.
- [ ] **Concept:** Expected Values ($E[X]$) via integration.
- [ ] **Drill:** Prove $\int_{-\infty}^{\infty} f(x) dx = 1$ for Exponential distribution.
- [ ] **Drill:** Derive Mean/Variance for Gamma distribution using integrals.

### Chapter 5: Joint Probability Distributions (⭐⭐⭐ Most Important)
**Focus:** 5.1 (Joint), 5.2 (Covariance), 5.4 (Sample Mean)
- [ ] **Skill:** Compute Marginal PDFs from Joint PDFs (integration).
- [ ] **Skill:** Compute Conditional Expectation $E[X|Y]$.
- [ ] **Skill:** Covariance matrices & Correlation.
- [ ] **Skill:** Double integrals for probability over a region.

### Chapter 6: Point Estimation
**Focus:** 6.1 (General) & 6.2 (Methods)
- [ ] **Technique:** Maximum Likelihood Estimation (MLE).
    - *Task:* Practice taking log-likelihood, differentiating, and solving for $\hat{\theta}$.
- [ ] **Concept:** Unbiasedness & Consistency.

---

## 📝 Tier 2: Standard Syllabus (Likely to Appear)
*Do these after Tier 1 is solid.*

### Chapter 8: Tests of Hypotheses
**Focus:** 8.1 - 8.4
- [ ] **Concept:** Null vs. Alternative Hypothesis.
- [ ] **Technique:** Likelihood Ratio Test (LRT).
- [ ] **Technique:** t-tests and z-tests logic.

### Chapter 12: Simple Linear Regression
**Focus:** 12.1 - 12.3
- [ ] **Derivation:** Least Squares Estimators (minimize SSE via calculus).
- [ ] **Concept:** Residuals & Coefficients.

---

## 🗑️ Tier 3: Low Priority / Skip

- Chapter 1 (Descriptive Stats)
- Chapter 2 (Basic Probability - unless weak on Bayes' Theorem)
- Chapter 9 (Inference based on Two Samples)
- Chapter 10 (ANOVA)
- Chapter 11 (Multifactor ANOVA)
- Chapter 13 (Nonlinear Regression)
- Chapter 14 (Goodness of Fit)
- Chapter 15 (Non-parametric)
- Chapter 16 (Quality Control)
