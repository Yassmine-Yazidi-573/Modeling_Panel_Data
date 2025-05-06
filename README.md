## 📊 Model Estimation and Selection Summary

Predictors:
I = Airline,
T = Year,
Q = Output, in revenue passenger miles, index number,
PF = Fuel price,
LF = Load factor, the average capacity utilization of the fleet.

Response:
C = Total cost, in $1000

This project analyzes a balanced panel dataset of 6 airline firms observed over 15 years (T = 15, N = 6), totaling 90 firm-year observations. The goal is to estimate the impact of key explanatory variables — output (Q), Price Fuel (PF), and Load Factor (LF) — on the outcome variable C=Total Cost. Given the panel structure of the data, three econometric models were estimated and evaluated:

* Pooled Ordinary Least Squares (OLS)
* Fixed Effects (FE) using PanelOLS
* Random Effects (RE)

---

### 📌 1. Pooled Ordinary Least Squares (OLS)

This baseline model treats all observations as independent and identically distributed (i.i.d.), ignoring firm-level heterogeneity.

* Model Equation:

  $$
  y_{it} = \beta_0 + \beta_1 Q_{it} + \beta_2 PF_{it} + \beta_3 LF_{it} + \epsilon_{it}
  $$

* R-squared (Overall): 0.9461 → very high explanatory power

* All variables (Q, PF, LF) are statistically significant (p < 0.05)

  * Coefficient signs:

    * Q: Positive
    * PF: Positive
    * LF: Negative

* Interpretation:

  * Suggests increasing Q and PF increases output, while LF decreases it

* Limitation:

  * Ignores firm-specific factors (e.g., fleet size, management quality, regional differences)
  * May suffer from omitted variable bias

➡️ Not appropriate for panel data — used only as a baseline for comparison.

---

### 📌 2. Fixed Effects (FE) Model – PanelOLS

This model controls for all unobserved, time-invariant characteristics of each airline (e.g., fixed cost structure, regional base), removing firm-specific bias.

* Model Equation:

  $$
  y_{it} = \alpha_i + \beta_1 Q_{it} + \beta_2 PF_{it} + \beta_3 LF_{it} + u_{it}
  $$

* R-squared (Within): 0.9294

* F-test for Poolability: 14.595, p = 0.0000 → Reject pooled OLS in favor of FE

* Coefficient behavior:

  * All regressors are statistically significant (p < 0.05)
  * Signs match Pooled OLS:

    * Q ↑ → y ↑
    * PF ↑ → y ↑
    * LF ↑ → y ↓

* Interpretation:

  * This model captures only within-firm variation over time
  * By differencing out firm-specific αi, we remove bias due to unobserved heterogeneity

➡️ Very strong candidate model, especially when firm-specific effects correlate with explanatory variables.

---

### 📌 3. Random Effects (RE) Model

RE assumes firm-specific effects are random draws and uncorrelated with the regressors.

* Model Equation:

  $$
  y_{it} = \beta_0 + \beta_1 Q_{it} + \beta_2 PF_{it} + \beta_3 LF_{it} + (\mu_i + \epsilon_{it})
  $$

* R-squared (Overall): 0.9331

* Coefficient estimates and signs similar to FE

* Interpretation:

  * RE is more efficient if its assumptions hold (i.e., E\[xit ⋅ μi] = 0)
  * However, if firm effects correlate with any explanatory variable, RE becomes biased

➡️ Suitable only if μi is uncorrelated with Q, PF, LF — requires validation via Hausman test.

---

### 🧪 4. Hausman Test: FE vs RE

To determine if RE is consistent (i.e., whether its assumptions are valid), we conducted a Hausman test. This test compares the difference in estimated coefficients between FE and RE, and checks if it is statistically significant.

* Test Statistic: 60.8695
* Degrees of Freedom: 3
* p-value: 0.0000 ✅

🟢 Conclusion: The test clearly rejects the null hypothesis that RE is appropriate. The RE estimator is inconsistent due to correlation between regressors and unobserved firm effects.

➡️ FE is statistically superior to RE.

---

### ✅ Final Model Selection Summary

| Model          | Suitable? | Justification                                                                    |
| -------------- | --------- | -------------------------------------------------------------------------------- |
| Pooled OLS     | ❌         | Ignores heterogeneity; rejected by F-test (p < 0.001)                            |
| Random Effects | ❌         | Inconsistent; rejected by Hausman test (p = 0.0000)                              |
| Fixed Effects  | ✅         | Best model — controls for unobserved firm effects and passes specification tests |

Final model choice:

* ✅ Fixed Effects (PanelOLS)
* Justification:

  * Statistically robust (R² within = 0.9294)
  * Controls for unobserved firm-level characteristics
  * Supported by both F-test and Hausman test

---

📌 Note: In the code, we used Python’s linearmodels library to estimate all models and perform the Hausman test manually via covariance matrices.

Let me know if you’d like a diagram showing the model selection logic flow (e.g., F-test → Hausman test → FE chosen), or help visualizing the coefficients across models
