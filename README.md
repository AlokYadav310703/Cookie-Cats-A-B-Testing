# Cookie Cats: A/B Test Analysis — Data-Driven Decision Making in Mobile Gaming

> **How Moving a Single Game Gate Impacts Player Retention: A Rigorous Statistical Analysis**

**Analysis Date:** August 2026
**Data Source:** [Kaggle: Mobile Games A/B Testing — Cookie Cats](https://www.kaggle.com/datasets/mursideyarkin/mobile-games-ab-testing-cookie-cats)

---

## Business Problem

**The Challenge:**  
Mobile game monetization depends critically on player engagement and retention. Cookie Cats, a popular mobile puzzle game, faced a strategic question:

> *Does moving the progression gate from **Level 30 to Level 40** improve or harm 1-day and 7-day player retention?*

**Business Impact:**
- **Revenue at stake:** Retention directly affects lifetime value (LTV) and ad impressions per user
- **Product trade-off:** A higher gate delays monetization but may reduce early churn; conversely, a lower gate may enable faster progression but with lower long-term engagement
- **Scale:** Small retention shifts (0.5–1%) can translate to millions in annual revenue for a large player base

**Key Questions:**
1. Does increasing gate difficulty (L30 → L40) statistically significantly impact day-1 retention?
2. Does the effect compound over time (day-7 retention)?
3. Is any observed difference practically meaningful for business decisions?
4. Are there hidden segments where the impact differs?

---

## Solution Approach

This analysis applies **statistical rigor and best practices** to provide defensible answers:

### **Data Quality & Validation**
- Verified 90,000+ players across two randomized groups
- Checked for duplicates, missing values, and schema violations
- Confirmed no sample ratio mismatch (no evidence of broken randomization)
- Flagged and removed data-quality outliers (bots/instrumentation errors)

### **Exploratory Data Analysis (EDA)**
- Identified heavily right-skewed engagement distribution
- Applied log transformation for interpretable visualization
- Summarized retention patterns: ~44% day-1, ~19% day-7 retention baseline

### **Statistical Hypothesis Testing**
Conducted rigorous hypothesis tests with proper assumptions checking:

| Test | Metric | Finding |
|------|--------|---------|
| **Two-proportion Z-test** | Day-1 retention | 0.6pp gap (gate_30 better), p = 0.074 |
| **Two-proportion Z-test** | Day-7 retention | 1.9pp gap (gate_30 better), p = 0.002 ⭐ |
| **Mann-Whitney U** | Engagement (sum_gamerounds) | No significant difference, p = 0.166 |

### **Multiple Comparisons Correction**
- Applied **Holm-Bonferroni** and **Benjamini-Hochberg** corrections
- Result: Only **7-day retention survives correction** → Most defensible finding
- Protects against false positives from running multiple tests on the same experiment

### **Confidence Intervals & Effect Sizes**
- Computed 95% confidence intervals using **Newcomb method** (superior to Wald for proportions)
- Cohen's h effect sizes: Small but meaningful for mobile gaming at scale
- **7-day retention CI:** gate_30 retains 1.0–2.8pp better than gate_40

### **Statistical Power Analysis**
- Achieved power: 45% (test was underpowered for day-1 retention)
- Required sample size: 47K per group to detect 1pp effect at 80% power
- **Implication:** Day-1 non-significance is *absence of evidence*, not evidence of absence

### **Bootstrap Simulation**
- Non-parametric validation of Z-test assumptions
- Empirical 95% CI for retention difference: Mirrors parametric result ✓

### **Bayesian Analysis**
- Complementary probabilistic framework (no p-value fallacies)
- Direct posterior statement: **P(gate_30 > gate_40) = 96.2%**
- Aligns with frequentist conclusion but adds interpretability

### **Segmentation (Exploratory)**
- Post-hoc analysis: Does effect differ by engagement level?
- Finding: Both low- and high-engagement players show the same directional trend

---

## Key Results

### **Primary Finding: 7-Day Retention is Materially Better with Gate at Level 30**

```
Control (Gate 30):   18.9% of players return after 7 days
Treatment (Gate 40): 17.0% of players return after 7 days

Difference:          1.9 percentage points  
Relative Impact:     -10.1% (gate at 40 reduces retention)
P-value:             0.002 (survives multiple-comparisons correction)
95% CI:              [1.0pp, 2.8pp]
```

### **Secondary Finding: Day-1 Retention Shows Same Trend, Weaker Signal**
- Gate 30: 44.8%, Gate 40: 44.2%, Difference: 0.6pp, p = 0.074
- Likely underpowered; directional consistency with day-7 bolsters confidence

### **Engagement is Unaffected**
- No statistically significant difference in game rounds played (p = 0.166)
- Gate level does not drive play intensity; it affects *retention*

---

## Business Recommendation

**Keep the gate at Level 30**  
The data provide statistically rigorous and practically meaningful evidence that moving the gate to Level 40 would reduce 7-day player retention by ~2 percentage points. For a large player base, this translates to significant lifetime-value loss.

---

## Technical Highlights

### **Statistical Best Practices Applied**
1. Assumptions checked (normality, SRM)  
2. Non-parametric tests where data violates assumptions  
3. Multiple-comparisons correction before drawing conclusions  
4. Confidence intervals & effect sizes (not just p-values)  
5. Power analysis to interpret non-significant results  
6. Bootstrap validation of parametric tests  
7. Bayesian complement to frequentist framework  

### **Data Handling Mastery**
- Detected and handled outliers with domain reasoning (bots likely)
- Applied log transformation for heavy-tailed distributions
- Centralized reusable test functions (prevents copy-paste bugs)
- Comprehensive data quality suite before analysis begins

### **Code Organization**
- Modular helper functions for repeated patterns
- Clear section markers and navigation (Table of Contents)
- Extensive comments explaining statistical reasoning
- Publication-ready visualizations with proper axis labels

---

## Project Structure

```
.
├── README.md                          # This file
├── 01_eda_&_Testing.ipynb            # Full notebook with all analysis
├── data/
│   └── cookie_cats.csv               # Raw dataset (90,000+ players)
└── requirements.txt                   # Python dependencies (scipy, pandas, etc.)
```

---

## Technical Dependencies

- **pandas**: Data manipulation & aggregation
- **scipy.stats**: Hypothesis tests (z-test, Mann-Whitney U, Shapiro-Wilk)
- **statsmodels**: Proportion testing, confidence intervals, power analysis
- **numpy**: Numerical computations & bootstrap simulation
- **matplotlib**: Visualizations

---


