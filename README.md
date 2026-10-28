Superstore Sales & Profitability Analysis

**Analyzing 1,500 superstore transactions to identify sales patterns, investigate recorded returns, and recommend operational priorities.**

Cash transactions had the highest observed return rate among the five payment methods: **30.8%**, versus **20.3%** for Gift Card transactions. This project uses exploratory analysis, statistical comparisons, and logistic regression to turn that finding into practical business recommendations.

**[Explore the prepared dataset](transaction_inference_features.csv)** | **[View the regression results](return_logit_model_results.csv)** | **[Compare model specifications](return_model_specification_summary.csv)**

## Business Problem

Product returns can create additional handling costs and affect sales performance. For a superstore, the key business question is:

> Which transaction characteristics are associated with recorded returns, and where should the business focus further investigation?

The analysis focuses on how Cash transactions compare with Gift Card transactions after accounting for available transaction characteristics. It also examines sales patterns across products, regions, and customer types. Financial impacts cannot be quantified without cost, margin, and refund data.

## Results at a Glance

| Metric | Result |
| --- | ---: |
| Total transactions analyzed | 1,500 |
| Transactions with a recorded return | 372 |
| Overall observed return rate | 24.8% |

### Returns by Payment Method

| Payment method | Transactions | Recorded returns | Observed return rate |
| --- | ---: | ---: | ---: |
| **Cash** | **312** | **96** | **30.8%** |
| Debit Card | 290 | 78 | 26.9% |
| Online | 323 | 77 | 23.8% |
| Credit Card | 299 | 65 | 21.7% |
| Gift Card | 276 | 56 | 20.3% |

These groups cover all 1,500 transactions. Return rates measure the share of transactions marked `Returned = 1`, rather than the share of units returned or transaction value refunded.

**Business takeaway:** Cash transactions had a recorded return rate **10.5 percentage points higher** than Gift Card transactions. After adjustment, Cash transactions had **1.74 times the return odds** (95% confidence interval: **1.18–2.55**). The association persisted across all five model specifications, making Cash return processes a priority for investigation. These associations do not prove that payment method causes returns.

## Approach & Tools

**Analysis tools:** Python, Pandas, and NumPy.

1. **Prepare the data:** Check transaction records, retain missing promotion information, and create derived features and outlier indicators.
2. **Explore sales and returns:** Compare transaction values, sales totals, and recorded return rates across transaction groups.
3. **Assess group differences:** Use Welch ANOVA and Kruskal–Wallis tests to compare log transaction value.
4. **Adjust the return comparison:** Use logistic regression with payment method, product, region, customer type, store location, discount, and log transaction value.
5. **Check robustness:** Compare five model specifications, including promotion, store, transaction-value, and outlier sensitivity analyses.

Promotion information was missing for **370 transactions (24.7%)**. These records were retained, with missing values represented as `MissingPromotion` in the prepared dataset. The primary model used all 1,500 transactions.

## Model Evaluation

| Reported metric | Result | What it measures |
| --- | ---: | --- |
| Cash-versus-Gift-Card adjusted odds ratio | 1.74 | Relative odds of a recorded return after adjustment |
| 95% confidence interval | 1.18–2.55 | Uncertainty around the adjusted odds ratio |
| Cash-versus-Gift-Card p-value | 0.0047 | Evidence against a zero Cash-versus-Gift-Card coefficient in the fitted model |
| Adjusted probability difference | 10.36 pp | Model-based Cash-minus-Gift-Card difference in return probability |
| McFadden pseudo-R² | 0.0122 | Improvement in model fit relative to an intercept-only model |
| AIC | 1,701.92 | Fit-complexity criterion for comparison with models fitted to the same outcome and sample |
| Converged | Yes | Whether the fitting algorithm reported convergence |

*pp = percentage points.*

The positive Cash-versus-Gift-Card association persisted across all five specifications, with adjusted probability differences of approximately **10.2–10.8 percentage points**. The P99-excluded model used 1,485 transactions after excluding 15 flagged records.

The primary model has limited explanatory power. Pseudo-R² is not the percentage of return variation explained, and an odds ratio is not a probability ratio. Calibration and holdout-validation results are not included in the supplied exports, so **operational return prediction has not been established**.

## Recommended Operational Investigations

| Business priority | Proposed action | Outcome to measure |
| --- | --- | --- |
| Investigate Cash transaction returns | Audit return recording, refund handling, and transaction workflows | Return reasons, recording discrepancies, and refund amounts |
| Use Gift Card transactions as a benchmark | Compare similar Cash and Gift Card transactions and their return processes | Return-rate differences after accounting for transaction characteristics |
| Strengthen product and regional evidence | Collect larger subgroup samples and evaluate payment-method interactions | Subgroup estimates with confidence intervals |
| Assess financial impact | Collect product costs, margins, refund amounts, and return-handling costs | Net financial impact of recorded returns |
| Validate payment method as a screening signal | Evaluate the association in additional data before changing policies | Stability of the association and validated predictive performance |

These are proposed investigations. The project does not report implemented policy changes, reduced returns, or realized profit improvements.

## Limitations & Next Steps

- **Association versus causation:** Adjusting for observed transaction characteristics does not establish why Cash transactions had more recorded returns.
- **Limited model performance:** Low pseudo-R² and the absence of supplied calibration and holdout-validation results restrict predictive use.
- **Subgroup evidence:** The supplied outputs do not establish reliable payment-method differences for specific product-region combinations. Small subgroup samples limit targeted recommendations.
- **Missing promotion information:** Missing values may contain useful information, but do not establish whether a promotion was absent or unrecorded.
- **Profitability data:** Cost, margin, and refund information are insufficient to quantify financial losses or profitability impacts.
- **Reproducibility:** The supplied materials contain the original data, prepared features, and statistical outputs. A reproducible notebook, runnable model pipeline, and saved visualizations remain planned additions.

## Project Files

| File | Contents |
| --- | --- |
| [Original transaction dataset](Product-Sales-Region.csv) | Original dataset containing 1,500 transactions and 19 columns |
| [Prepared transaction dataset](transaction_inference_features.csv) | Transaction records with derived features, categorical fields, and outlier indicators |
| [Logistic regression results](return_logit_model_results.csv) | Primary model coefficients, adjusted odds ratios, confidence intervals, p-values, and diagnostics |
| [Model specification comparison](return_model_specification_summary.csv) | Cash-versus-Gift-Card results across five model specifications |
| [Transaction-value statistical tests](stat_value_log_omnibus_tests.csv) | Welch ANOVA and Kruskal–Wallis comparisons of log transaction value |

NOTE! Data Limitation — Currency: In the Jupyter Notebooks file number 4 exploratory data analysis, it lists "currency unspecified." The source dataset does not specify the currency used for transaction values. Therefore, all monetary figures are treated as recorded numeric values, and no currency denomination is assumed. Comparisons reflect relative transaction values rather than verified amounts in a specific currency.

