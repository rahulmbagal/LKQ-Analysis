# ABC Retail — Marketing Spend and Sales Analysis

An exploratory and hypothesis-testing case study of how **Digital, Radio and TV advertising expenditure is associated with ABC Retail sales**, while accounting for time trends, annual seasonality and selected economic and COVID-period indicators.

**Study period:** January 2020–December 2022 · **Data:** 36 monthly sales observations and 12 quarters of U.S. GDP information · **Tools:** Python, pandas, seaborn, matplotlib, SciPy and statsmodels.

> **Important:** This is an observational analysis of associations. It does not identify causal advertising effects, incremental sales or marketing return on investment (ROI).

## Business questions

1. Are the three advertising channels jointly associated with sales, and what does the evidence show for each channel?
2. How do the channels compare on adjusted association and unique explanatory contribution?
3. Is quarterly U.S. GDP growth associated with sales after accounting for marketing and time controls?
4. Do the case-defined Q2 2020 lockdown and Q3 2020 relaxation periods show differences in sales, descriptively and after adjustment?

## Repository contents

| File | Purpose |
| --- | --- |
| `Final_Analysis.ipynb` | Data cleaning, exploratory analysis, OLS estimation, hypothesis testing and diagnostics. |
| `ABC_Retail_Final_Presentation.pptx` | Presentation with notebook-derived EDA charts, model explanation and findings. |
| `requirements.txt` | Python dependencies. |
| `Retail Sales Case Study.xlsx` | Input workbook expected by the notebook. **Add it only if you have permission to publish the data.** |

The notebook expects the Excel workbook in the **same directory** as the notebook, with sheets named `Retail Sales Case Study` and `GDP Change %`. The workbook is not included in this README package; obtain it from the authorized case-study source. If the data cannot be shared, retain the workbook privately and document how an authorized reviewer may obtain access.

## Run locally

Use **Python 3.11 or newer**. From the repository directory:

```bash
python -m venv .venv
# Windows: .venv\Scripts\activate
# macOS/Linux: source .venv/bin/activate
pip install -r requirements.txt
jupyter notebook Final_Analysis_with_VIF.ipynb
```

Place `Retail Sales Case Study.xlsx` alongside the notebook before running. In Jupyter, select **Run All** to execute the cells in order. If your input workbook is stored elsewhere, update the `file_path` assignment in the data-loading cell.

## Analysis approach

**1. Data preparation and validation.** Read monthly sales and advertising data plus quarterly GDP. Convert TV spending from thousands of dollars to millions for regression; convert GDP decimal growth to percentage points and map it to the corresponding months. Validate the number of rows, dates, missing values and duplicate months.

**2. Exploratory data analysis (EDA).** Examine monthly sales and each advertising channel over time, with Q2/Q3 2020 highlighted; compare average monthly sales and average advertising spend across the two COVID phases and other months; plot pairwise correlations and channel-versus-sales scatterplots. These comparisons are unadjusted.

**3. Models and hypotheses.** Use ordinary least squares (OLS) with a linear time trend and annual sine/cosine terms:

- **Model 0:** Trend + annual seasonality.
- **Model 1:** Model 0 + Digital + Radio + TV.
- **Model 2A:** Model 1 + GDP growth.
- **Model 2B:** Model 1 + Q2 lockdown and Q3 relaxation indicators.

The GDP and COVID indicators are evaluated in **separate extensions**. The notebook computes Newey–West/HAC standard errors using one monthly lag, a joint Wald/F-type test for the media coefficients, individual coefficient tests, and Holm adjustment for the three individual advertising-channel p-values. It also compares unadjusted and adjusted channel estimates and checks variance inflation factors (VIF) using `statsmodels`.

## Findings

### Exploratory results

| Period | Average monthly sales ($M) | Digital spend ($M) | Radio spend ($M) | TV spend ($K) |
| --- | ---: | ---: | ---: | ---: |
| Q2 2020 lockdown (3 months) | 231.60 | 56.83 | 16.83 | 31.98 |
| Q3 2020 relaxation (3 months) | 178.80 | 31.05 | 9.08 | 6.02 |
| Other months (30 months) | 250.62 | 78.98 | 10.71 | 17.17 |

All three channels have their lowest average spending in Q3 2020, when average sales are also lowest. This concurrent movement is **descriptive** and cannot establish what caused the sales difference. “Other months” includes months across 2020–2022; it is not a pure post-COVID control group.

The unadjusted correlations between monthly sales and spending are **Digital: 0.774**, **Radio: 0.445**, and **TV: 0.139**. Correlation does not adjust for other channels, seasonality or the time trend.

### Hypothesis-testing results

| Test | Result | Interpretation |
| --- | --- | --- |
| All media jointly | F ≈ 73.28; p < 0.001 | Reject the joint null: at least one media coefficient is non-zero. |
| Digital | Positive coefficient; Holm-adjusted p < 0.001 | Evidence of a positive adjusted association with sales. |
| Radio | Positive coefficient; Holm-adjusted p < 0.001 | Evidence of a positive adjusted association with sales. |
| TV | p ≈ 0.217 | Insufficient evidence of an adjusted association. |
| GDP growth | p ≈ 0.305 | Insufficient evidence of an adjusted association in Model 2A. |
| Q2 lockdown | p ≈ 0.375 | No statistically detectable adjusted period difference in Model 2B. |
| Q3 relaxation | p ≈ 0.663 | No statistically detectable adjusted period difference in Model 2B. |

**Interpretation:** The model reports positive adjusted associations for Digital and Radio, while the estimates for TV, GDP and the two case-defined COVID periods are inconclusive. Non-significance does not establish a zero effect. None of these estimates should be presented as causal lift or proven ROI.

## Limitations and suggested next steps

- Only **36 monthly observations** are available. GDP has just **12 distinct quarterly values**, even though these are mapped to months.
- There is **no pre-pandemic baseline or unaffected control group**, so COVID's causal effect cannot be identified here.
- Potentially important retail drivers—including prices, discounts, promotions, inventory, store operations and competitor activity—are not available in the supplied analysis.
- A linear model without explicit advertising carryover or diminishing returns cannot estimate a full marketing response curve.
- National U.S. GDP may not represent the retailer's local trading conditions.

For a business decision, validate the findings with richer retail and campaign data and a controlled incremental-budget experiment that measures **incremental sales and profit**, rather than treating the OLS coefficients as ROI.

## Data and reproducibility note

The numerical findings above come from the executed case-study notebook. To reproduce them, use the same workbook and run the notebook from top to bottom. Review the input-data sharing permissions before publishing any workbook, outputs or screenshots to a public GitHub repository.
