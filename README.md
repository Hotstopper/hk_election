# Hong Kong District Council Voting Analysis (2015–2019, Updated)

## Overview
This project examines how **district-level demographics** shaped the **swing in pan-democratic vote share** between the 2015 and 2019 Hong Kong District Council elections.

The analysis uses **Weighted Least Squares (WLS)** with **heteroskedasticity-robust (HC1)** standard errors, and performs **joint F-tests** on key demographic groups (age, income) and regional dummies.

---

## Data

| Source | Description |
|---------|-------------|
| **Elections (2015, 2019)** | Vote share by party and constituency. Aggregated into district-level pan-democratic vote share. |
| **2021 Census (HKC&SD)** | District-level demographics: population by age, gender, education, and income. |
| **Macro-Regions** | Each district is assigned to one of four macro regions: <br> HK Island (baseline), Kowloon, NT Urban, NT Rural. |

---

## Model Specification

$$
\Delta y_j = \alpha 
+ \sum_a \beta_a \, p_{\text{age},a,j}
+ \sum_i \theta_i \, p_{\text{income},i,j}
+ \gamma_{\text{KL}} D_{\text{KL},j}
+ \gamma_{\text{NTU}} D_{\text{NTU},j}
+ \gamma_{\text{NTR}} D_{\text{NTR},j}
+ \varepsilon_j,
$$

where  
- $ \Delta y_j = y_{j,2019} - y_{j,2015} $ is the change in **pan-democratic vote share**;  
- $ p_{\text{age},a,j} $ are the **shares of population** in age group $ a \in \{15–24, 25–44, 45–64, ≥65\} $;  
- $ p_{\text{income},i,j} $ are **shares of households** in income brackets;  
- $ D_{\text{region}} $ are macro-region dummies;  
- weights are based on **constituency population sizes**.

The model is estimated using **WLS (weights = `w`)** and robust standard errors (`cov_type='HC1'`).

---

## Joint Significance Tests

| Hypothesis | Variables tested | F-statistic | p-value | Interpretation |
|-------------|------------------|-------------|----------|----------------|
| **Age structure** | `p_15-24, p_25-44, p_45-64, p_>65` | 0.46 | 0.76 | No systematic relationship between age composition and vote swing. |
| **Income distribution** | `p_I6000-9999, p_I10000-19999, p_I20000-29999, p_I30000-39999, p_I40000-59999, p_I>60000` | 0.68 | 0.67 | Income structure not significantly related to swing. |
| **Region dummies** | `KL, NT Urban, NT Rural` | 1.94 | 0.12 | Only weak cross-regional variation relative to HK Island baseline. |

## Extension

I also examined how **district-level demographics** shaped **pan-democratic vote shares** in the **2019 Hong Kong District Council elections**.  
Unlike the earlier difference specification (2015→2019 swing), this analysis focuses on **cross-sectional variation in 2019 outcomes only**, using Weighted Least Squares (WLS) regressions to identify demographic and geographic correlates of pan-democratic support.

The aim is to test whether **age**, **income**, or **regional context** systematically predict 2019 pan-democratic vote shares.

## Joint F-tests (2019 Cross-Section)

| Hypothesis | Variables tested | F-statistic | p-value | Interpretation |
|-------------|------------------|-------------|----------|----------------|
| **Age structure** | `p_15-24, p_25-44, p_45-64, p_>65` | 4.822 | 0.0008 | Strong joint significance — 2019 support systematically varies by age composition. |
| **Income distribution** | `p_I6000-9999, p_I10000-19999, p_I20000-29999, p_I30000-39999, p_I40000-59999, p_I>60000` | 8.325 | 1.48×10⁻⁸ | Highly significant — income structure strongly correlates with 2019 voting outcomes. |
| **Region dummies** | `KL, NT Urban, NT Rural` | 5.883 | 0.0006 | Regionally distinct voting patterns relative to HK Island baseline. |

Age and education are positively correlated with higher pan-democratic/localist vote shares. For income, it exhibits an inverted-U with the middle class being most associated with strong pan-dem/localist constituencies. The geography of support aligns with new-town middle-class suburbs, highlighting the diffusion of protest sentiment beyond the urban core.

## Endogeneity Discussion

Because this specification uses **cross-sectional 2019 data only**, estimates are **associational**, not causal.  
Endogeneity arises from several sources:

1. **Omitted variable bias**  
   - Unobserved district characteristics (e.g. protest exposure, media networks, social capital) may correlate with both demographics and voting outcomes.
2. **Simultaneity / feedback**  
   - Migration and self-selection post-2019 protests likely altered demographic composition (especially among young professionals and families).
3. **Measurement error**  
   - Census variables (2021) measured after the election — introducing **post-treatment bias**.
4. **Spatial dependence**  
   - Political mobilisation diffused geographically; residuals may be spatially correlated.

Thus, while the joint significance tests confirm strong *correlations*, they do not establish causal effects of demographics on voting outcomes.