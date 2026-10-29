# Statistical Analysis of Lithium-Ion Battery Deterioration: Modeling Capacity Fade and End-of-Life Probabilities

A statistical and inferential modeling study quantifying uncertainty, thermal stress impacts, and End-of-Life (EOL) reliability in commercial lithium-ion cells using the **NASA Ames Prognostics Center of Excellence (PCoE)** run-to-failure dataset ($n=34$).

---

## Overview

Deterministic degradation models frequently overlook cell-to-cell manufacturing variance and non-linear chemical degradation kinetics. This project applies classical hypothesis testing, regression analysis, and parametric probability distributions to evaluate:

1. Non-linear capacity fade trajectories ($SOH$).
2. The operational life penalty imposed by elevated ambient operating temperatures.
3. Early-cycle degradation dynamics ("break-in" phase).
4. Statistical independence between initial manufacturing capacity tiers and early cell failure.

---

## Research Questions & Key Findings

| Research Question | Statistical Method | Key Result / Metric | Statistical Decision |
| --- | --- | --- | --- |
| **RQ1: Trend Modeling** | Multiple Linear vs. Exponential Regression | • Linear: $R^2 = 0.2201$<br>

<br>• Exponential: $R^2 = 0.8015$ | **Reject Linear Assumption**: Degradation is inherently non-linear; the exponential model accurately captures accelerating late-stage fade. |
| **RQ2: Thermal Impact** | Independent Two-Sample $t$-Test (Welch-adjusted) | • $24^\circ\text{C}$ Mean Life: $98.21$ cycles<br>

<br>• $43^\circ\text{C}$ Mean Life: $40.00$ cycles<br>

<br>• $t = 2.6619$, $p = 0.0195$ | **Reject $H_0$** ($p < 0.05$): Elevated operating temperature produces a statistically significant $>59\%$ reduction in operational cycle life. |
| **RQ3: Early Fade** | Paired $t$-Test (Cycle 1 vs. Cycle 50) | • Mean drop: $0.1357\text{ Ah}$<br>

<br>• $t = 0.6808$, $p = 0.5046$ | **Fail to Reject $H_0$** ($p > 0.05$): Capacity drop in initial 50 cycles is not statistically distinguishable from noise, confirming an initial break-in period. |
| **RQ3: Reliability** | $Z$-Score Normalization / Parametric EOL Estimation | • Base Survival Rate: $77.14\%$ ($>80\%\text{ capacity}$ at Cycle 100)<br>

<br>• Sensitivity: drops to $70.39\%$ if $\sigma$ rises from $0.18$ to $0.25\text{ Ah}$ | **High Sensitivity to Variance**: Manufacturing dispersion ($\sigma$) influences fleet reliability as much as mean capacity ($\mu$). |
| **RQ4: Batch Independence** | $\chi^2$ Test of Independence ($2 \times 2$ Contingency) | • $\chi^2 = 0.1288$, $\text{dof} = 1$<br>

<br>• $p = 0.7197$ | **Fail to Reject $H_0$** ($p > 0.05$): Initial capacity classification (High vs. Low) is statistically independent of early cell failure. |

---

## Dataset Description

* **Source:** NASA PCoE Battery Data Set (B. Saha & K. Goebel, NASA Ames Research Center).
* **Sample Size:** $n = 34$ commercially available 18650 lithium-ion cells cycled to failure.
* **Key Variables:**
* **Capacity ($\text{Ah}$):** Quantitative continuous response metric.
* **State of Health ($SOH$):** Normalized metric, $SOH = \frac{\text{Capacity}_{\text{cycle}}}{\text{Capacity}_{\text{initial}}}$, used to remove baseline inter-cell manufacturing offset.
* **Cycle Number:** Quantitative discrete predictor.
* **Temperature ($^\circ\text{C}$):** Environmental control factor ($4^\circ\text{C}$, $22^\circ\text{C}$, $24^\circ\text{C}$, $43^\circ\text{C}$, $44^\circ\text{C}$).



---

## Repository Structure

```text
├── data/
│   ├── raw/                 # NASA PCoE raw battery aging profiles
│   └── processed/           # SOH-normalized and feature-extracted tables
├── notebooks/
│   ├── 01_eda_and_cleaning.ipynb
│   ├── 02_regression_models.ipynb       # Linear vs. Exponential fit (RQ1)
│   ├── 03_hypothesis_tests.ipynb        # Two-sample t-test & paired t-test (RQ2, RQ3)
│   └── 04_reliability_and_chi2.ipynb    # Z-score EOL & Chi-squared independence (RQ3, RQ4)
├── scripts/
│   ├── statistical_tests.py # Core test routines & Levene/Shapiro-Wilk validations
│   └── plot_utils.py        # Distribution plots, contingency charts, and fit curves
├── paper/
│   └── Statistical_Analysis_of_Lithium_Ion_Battery_Deterioration.pdf
├── requirements.txt
└── README.md

```

---

## Setup & Reproduction

1. **Clone the repository:**
```bash
git clone https://github.com/<your-username>/<your-repo-name>.git
cd <your-repo-name>

```


2. **Set up a virtual environment:**
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install -r requirements.txt

```


3. **Run the analysis pipeline:**
```bash
python scripts/statistical_tests.py

```



---

## References

* B. Saha and K. Goebel (2007). *Battery Data Set*, NASA Ames Prognostics Data Repository, NASA Ames Research Center, Moffett Field, CA.
* D. C. Montgomery & G. C. Runger (2018). *Applied Statistics and Probability for Engineers* (7th ed.). Wiley.
* J. L. Devore (2015). *Probability and Statistics for Engineering and the Sciences* (9th ed.). Cengage Learning.
