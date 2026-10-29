# Statistical Analysis of Lithium-Ion Battery Deterioration: Modeling Capacity Fade and End-of-Life Probabilities

A statistical and inferential modeling study quantifying degradation kinetics, thermal stress impacts, and End-of-Life (EOL) reliability in commercial lithium-ion cells using the **NASA Ames Prognostics Center of Excellence (PCoE)** run-to-failure dataset ($n=34$).

---

## Executive Summary

Deterministic battery degradation models frequently fail to capture cell-to-cell manufacturing variance and non-linear chemical degradation kinetics. This study applies classical inferential statistics—multiple regression, two-sample and paired hypothesis tests, parametric probability distributions, and contingency tests—to model capacity fade trajectories, quantify the operational cycle-life penalty under thermal stress, and assess manufacturing batch dependency.

---

## Research Questions & Key Findings

| Research Question | Statistical Method | Key Result / Metric | Statistical Decision |
| :--- | :--- | :--- | :--- |
| **RQ1: Degradation Trend Modeling** | Multiple Linear vs. Exponential Regression | • Linear: $R^2 = 0.2201$<br>• Exponential: $R^2 = 0.8015$ | **Reject Linear Degradation Assumption**: Capacity fade is highly non-linear. The exponential model accurately resolves late-stage accelerating capacity fade. |
| **RQ2: Thermal Impact on Longevity** | Independent Two-Sample $t$-Test (Welch-adjusted) | • $24^\circ\text{C}$ Mean Life: $98.21\text{ cycles}$<br>• $43^\circ\text{C}$ Mean Life: $40.00\text{ cycles}$<br>• $t = 2.6619$, $p = 0.0195$ | **Reject $H_0$** ($p < 0.05$): Elevated operating temperature causes a statistically significant $>59\%$ reduction in operational cycle life. |
| **RQ3: Early-Stage Fade Confirmation** | Paired $t$-Test (Cycle 1 vs. Cycle 50) | • Mean drop: $0.1357\text{ Ah}$<br>• $t = 0.6808$, $p = 0.5046$ | **Fail to Reject $H_0$** ($p > 0.05$): Early capacity change is statistically indistinguishable from zero, confirming a stable initial "break-in" phase. |
| **RQ3: End-of-Life (EOL) Probability** | $Z$-Score Normalization / Central Limit Theorem | • Base Reliability: $77.14\%$ ($>80\%\text{ capacity}$ at Cycle 100)<br>• Sensitivity: Drops to $70.39\%$ when $\sigma$ increases from $0.18$ to $0.25\text{ Ah}$ | **Variance-Governed Reliability**: Manufacturing dispersion ($\sigma$) influences fleet reliability as critically as nominal mean capacity ($\mu$). |
| **RQ4: Batch Independence & Early Failure** | $\chi^2$ Test of Independence ($2 \times 2$ Contingency) | • $\chi^2 = 0.1288$, $\text{dof} = 1$<br>• $p = 0.7197$ | **Fail to Reject $H_0$** ($p > 0.05$): Initial capacity tier (High vs. Low) is statistically independent of early cell failure. |

---

## Dataset & Pre-processing

* **Data Source:** [NASA PCoE Battery Dataset](https://www.nasa.gov/intelligent-systems-division/discovery-and-systems-health/pcoe/pcoe-data-set-repository/) ($n = 34$ commercially available 18650 cells cycled to failure).
* **State of Health (SOH) Normalization:**
  $$\text{SOH} = \frac{\text{Capacity}_{\text{cycle}}}{\text{Capacity}_{\text{initial}}}$$
  Normalizing raw capacity to initial capacity eliminates inter-cell vertical scatter caused by raw manufacturing capacity offsets, lifting the model explanatory power from near-zero to $R^2 = 0.8015$.
* **Variables Evaluated:**
  * **Capacity ($\text{Ah}$):** Quantitative continuous response variable.
  * **Cycle Number:** Quantitative discrete predictor.
  * **Temperature ($^\circ\text{C}$):** Quantitative environmental factor ($4^\circ\text{C}$, $22^\circ\text{C}$, $24^\circ\text{C}$, $43^\circ\text{C}$, $44^\circ\text{C}$).

---

## Sensitivity Analysis (Cycle 100 Reliability)

Reliability was modeled as the probability $P(X \ge 1.016\text{ Ah})$ at Cycle 100 ($80\%$ of nominal capacity):

| Scenario | Mean Capacity ($\mu$, Ah) | Std Deviation ($\sigma$, Ah) | $Z$-Score | Survival Probability ($P$) |
| :--- | :---: | :---: | :---: | :---: |
| **Base Case** | $1.15$ | $0.18$ | $-0.744$ | **$77.14\%$** |
| **Better Performance** | $1.20$ | $0.18$ | $-1.022$ | **$84.66\%$** |
| **Worse Performance** | $1.10$ | $0.18$ | $-0.467$ | **$67.99\%$** |
| **Higher Variance** | $1.15$ | $0.25$ | $-0.536$ | **$70.39\%$** |

> **Key Takeaway:** An increase in standard deviation of just $0.07\text{ Ah}$ reduces reliability by nearly $7$ percentage points, demonstrating that tightening manufacturing tolerances is essential to prevent early pack failures.


## References

1. **B. Saha and K. Goebel (2007).** *"Battery Data Set"*, NASA Ames Prognostics Data Repository, NASA Ames Research Center, Moffett Field, CA.
2. **D. C. Montgomery and G. C. Runger (2018).** *Applied Statistics and Probability for Engineers* (7th ed.). Wiley.
3. **J. L. Devore (2015).** *Probability and Statistics for Engineering and the Sciences* (9th ed.). Cengage Learning.
