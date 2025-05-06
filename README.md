## 📊 Model Estimation and Selection Summary

This project analyzes a balanced panel dataset consisting of 6 airline firms observed over 15 years (90 total observations). The objective is to estimate the relationship between \[insert your dependent variable] and a set of explanatory variables, accounting for both temporal and firm-specific variation.

### 1. Model Specifications Considered

We estimated the following panel data regression models:

- Pooled Ordinary Least Squares (Pooled OLS): Ignores individual (firm-level) heterogeneity.
- Fixed Effects (FE): Controls for unobserved, time-invariant firm-specific characteristics.
- Random Effects (RE): Assumes unobserved firm effects are uncorrelated with explanatory variables.

### 2. Pooled OLS vs. Panel Models

Pooled OLS assumes homogeneity across firms and ignores within-entity variation. To determine whether this model is appropriate, we relied on the logic of the F-test for poolability. Results indicated significant variation across firms, suggesting that a panel model (FE or RE) is more suitable than pooled OLS.

### 3. Fixed Effects vs. Random Effects

To decide between the FE and RE models, we conducted a Hausman test. This test evaluates whether the unique errors (firm-specific effects) are correlated with the explanatory variables. If they are, RE estimates become biased and inconsistent, making FE the preferred model.

The Hausman test yielded the following results:

- Test statistic: 60.8695
- Degrees of freedom: 3
- P-value: 0.0000

Since the p-value is well below the 0.05 threshold, we reject the null hypothesis that the RE model is appropriate. The strong statistical significance indicates that the unobserved firm-level effects are correlated with the regressors, violating a core assumption of the RE model.

### ✅ Final Model Selection

Based on the results of the Hausman test, we conclude that the Fixed Effects (FE) model is the most appropriate for this dataset. It accounts for firm-specific heterogeneity that would otherwise bias the estimates, providing more reliable inference.

