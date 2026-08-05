# Projet-social-contagion: Code Manual & Parameters Documentation

This document serves as the technical cookbook explaining the model parameters, abbreviations, and economic functions used in the Python codebase for **Projet-social-contagion** (Ellouze, Fleurbaey, Prigent, 2026).
# Projet-social-contagion

[![Python Version](https://img.shields.io/badge/python-3.x-blue.svg)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

A Python simulation and quantitative analysis framework modeling social contagion, stratification, inequality, and mobility based on the theoretical paper *Social contagion, inequality and mobility* by Ali Ellouze, Marc Fleurbaey, and Jean-Luc Prigent (2026).

---

## What the Project Does

**Projet-social-contagion** adapts discrete-time Susceptible-Infected-Recovered (SIR) epidemiological mechanisms to study social stratification and mobility across three distinct classes: High ($H$), Middle ($M$), and Low ($L$). The repository contains robust simulation scripts, Monte Carlo sampling pipelines for archetypal social interaction structures, and publication-quality visualization tools (simplex projections, vector fields, and comparative distribution boxplots).

---

## Why the Project is Useful

*   **Taxonomy of Social Interactions**: Implements algorithmic parameter sampling for core archetypal interaction types (`Cooperation`, `Competition`, `Attraction`, `Exploitation`, `Homophily`, and `Diversity`) based on transition probability constraints.
*   **Advanced Welfare & Inequality Metrics**: Computes Gini coefficients, Atkinson inequality indexes (with configurable aversion parameter $\eta$), and Atkinson/Gini-based social welfare functions.
*   **Comprehensive Mobility Analysis**: Evaluates structural and operational mobility through transition matrix determinants, traces, differential mobility effects, and long-term operational discounted matrix inversions.
*   **Epidemiological & Learning Analogies**: Explores pandemic-like contagion waves, learning processes, and flattened curves resulting from contact reduction ($s$).

---

## Getting Started

### Prerequisites

Ensure you have Python 3 installed along with the required numerical and plotting libraries:


pip install numpy matplotlib pandas
---

## 1. Socioeconomic Constants & Reward Levels

*   **`RH = 1`**: Status reward valuation for the High-status class ($H$).
*   **`RM = 0.5`**: Status reward valuation for the Middle-status class ($M$).
*   **`RL = 0.1`**: Status reward valuation for the Low-status class ($L$).
*   **`Soc` ($s$)**: Number of one-on-one social contacts per period per individual.
*   **`eta` ($\eta$)**: Inequality aversion parameter used in Atkinson welfare and inequality metrics.
*   **`gamma` ($\gamma$)**: Infection or transition probability parameter used in the pandemic model.
*   **`l_plusplus` ($l^{++}$)**: Exogenous recovery probability back to an immune/higher state.

---

## 2. Behavioral Interaction Parameters (`psl`, `psd`, `pel`, `ped`, `pil`, `pid`)

The six core transition probabilities governing upward ($\cdot^+$) and downward ($\cdot^-$) mobility based on meeting partners from lower ($\alpha$), equal ($\beta$), or higher ($\gamma$) classes:

*   **`psl` ($\alpha^+$)**: Probability that a low-status connection induces an upward shift.
*   **`psd` ($\alpha^-$)**: Probability that a low-status connection induces a downward shift.
*   **`pel` ($\beta^+$)**: Probability associated with middle peer interactions shifting upward.
*   **`ped` ($\beta^-$)**: Probability associated with middle peer interactions shifting downward.
*   **`pil` ($\gamma^+$)**: Probability that a high-status connection induces an upward movement.
*   **`pid` ($\gamma^-$)**: Probability that a high-status connection induces a downward shock.

---

## 3. Inequality & Social Welfare Functions

*   **`Average(L, H)`**: Computes the weighted average status of the population based on class proportions.
*   **`inequalAtk(L, H, eta)`**: Computes Atkinson's inequality index for a given aversion parameter $\eta$.
*   **`inequalGini(L, H)`**: Computes the Gini inequality index from class distributions and rewards.
*   **`SWAtk(L, H, eta)`**: Evaluates Social Welfare under the Atkinson framework.
*   **`SWGini(L, H)`**: Evaluates Social Welfare using the Gini distribution structure.

---

## 4. Social Mobility Functions

*   **`MobDet(TRANS)`**: Measures mobility derived from the transition matrix determinant ($1 - |\det(\text{TRANS})|^{1/2}$).
*   **`MobTr(TRANS)`**: Measures mobility using the trace of the transition matrix.
*   **`MobDif(TRANS, L, H, RH, RM, RL)`**: Calculates net differential mobility effects across strata boundaries.
*   **`MobOp(TRANS, L, H, RH, RM, RL, eta)`**: Computes short-term operational mobility.
*   **`MobLTOp(TRANS, L, H, RH, RM, RL, eta, beta)`**: Evaluates long-term operational mobility via discounted matrix inversion ($\beta = 0.97$).

---

## 5. Core Simulation & Dynamics Functions

*   **`core_stat(...)`**: Computes steady-state statistics (proportions, inequality, welfare, mobility indices, parameter distance/intensity metrics).
*   **`run_model(...)`**: Simulates the temporal evolution of social strata shares over a time horizon (`Hor`) using non-linear probability adjustments and contact scaling.
*   **`run_TRANS(...)`**: Runs temporal simulation steps to construct and return the final $3 \times 3$ transition matrix (`TRANS`).
*   **`run_var(...)`**: Computes single-period class distribution variations (`HV`, `MV`, `LV`).
*   **`to_equilateral(x, y)`**: Transforms standard Cartesian coordinates into simplex triangle coordinates.
*   **`pandemic(...)`**: Simulates an SIR-like epidemiological contagion process across immune ($H$), susceptible ($M$), and sick ($L$) states.
*   **`sample_param(typen)`**: Randomly samples uniform parameters satisfying strict inequality constraints for the core archetypal game types: `Cooperation`, `Competition`, `Attraction`, `Exploitation`, `Homophily`, and `Diversity`.
*   # Projet-social-contagion: Code Manual & Parameters Documentation (Part 2)

## 6. Simplex Geometry & Coordinate Transformations

*   **`to_equilateral(x, y)`**: Transforms standard 2D Cartesian coordinates into equilateral triangle simplex coordinates for three-class population distributions ($H, M, L$).
*   **`vector_field(X, Y)`**: Constructs grid-based vector field components ($U, V$) mapping population share trajectories across the simplex via single-period variations.

---

## 7. Archetypal Interaction Taxonomies (`sample_param`)

The `sample_param(typen)` function randomly samples parameters satisfying specific strict inequality constraints corresponding to the core interaction types defined in the theoretical framework:

*   **`Cooperation`**: Mutual help where upward probabilities exceed downward ones ($\alpha^+ > \alpha^-$, $\beta^+ > \beta^-$, $\gamma^+ > \gamma^-$), and the capacity to help increases with social rank ($\alpha^+ > \beta^+ > \gamma^+$ and $\alpha^- < \beta^- < \gamma^-$).
*   **`Competition`**: Adversarial/conflictual interactions where downward probabilities dominate ($\alpha^+ < \alpha^-$, $\beta^+ < \beta^-$, $\gamma^+ < \gamma^-$), and meeting higher ranks increases downward risk ($\alpha^+ < \beta^+ < \gamma^+$ and $\alpha^- > \beta^- > \gamma^-$).
*   **`Attraction`**: Interactions where meeting superiors induces pushes ($\alpha^+ > \alpha^-$) but meeting inferiors pulls down ($\gamma^- > \gamma^+$), governing norm and information transmission.
*   **`Exploitation`**: The inverse of attraction, where higher classes take advantage of lower classes ($\alpha^+ < \alpha^-$ and $\gamma^- < \gamma^+$).
*   **`Diversity`**: Interactions where out-class meetings are more productive than in-class meetings ($\beta^+ < \alpha^+, \gamma^+$ and $\beta^- > \alpha^-, \gamma^-$).
*   **`Homophily`**: In-group favoritism and chauvinistic interaction where in-class meetings outperform out-class meetings ($\beta^+ > \alpha^+, \gamma^+$ and $\beta^- < \alpha^-, \gamma^-$).

---

## 8. Monte Carlo Simulation & Data Export

*   **Monte Carlo Loops**: Runs up to 1000 convergent steady-state simulations per game type using convergence tolerance ($\text{tol} = 10^{-5}$) and social contact parameters ($s = 40$).
*   **`SAMPLEdiv`**: Tracking array capturing non-convergent or divergent runs for robustness checks.
*   **Excel Export**: Compiles steady-state indicators (Gini, Atkinson indices, social welfare, mobility indices) and raw parameters into Pandas DataFrames and exports them as `.xlsx` files into the results folder.

---

## 9. Visualization & Plotting Functions

*   **`scatterplot(...)` & `scatterplot2(...)`**: Generates comparative statistical scatter plots of simulation outputs, including custom social indifference curves for Atkinson/Gini welfare frameworks.
*   **Pandemic Simulation (`pandemic`)**: Implements an SIR-like epidemiological spreading process across immune ($H$), susceptible ($M$), and sick ($L$) populations with recovery rate $l^{++}$.
*   **Boxplot Comparison Modules**: Extracts parameter distributions (`psl`, `psd`, `pel`, `ped`, `pil`, `pid`) for comparative game types (e.g., Cooperation vs. Competition) and outputs grouped, color-coded boxplots with LaTeX notation ($\alpha^+, \alpha^-, \beta^+, \beta^-, \gamma^+, \gamma^-$) at 300 DPI.
*   # Projet-social-contagion: Code Manual & Parameters Documentation (Part 3 - Visualizations & Vector Fields)

This concluding section of the manual details the visualization scripts, contour plot generators, Monte Carlo scatter plotters, cumulative distribution functions (CDF), and dynamic vector field simulation blocks.

---

## 10. Inequality & Social Welfare Contour Plots on the Simplex

These blocks construct grid spaces inside the constrained simplex region ($x + y \le 1, x \ge 0, y \ge 0$), compute welfare or inequality metrics, transform Cartesian coordinates into equilateral triangle coordinates (`to_equilateral`), and render contour maps.

*   **Gini Inequality Contours**: Evaluates `inequalGini(X, Y)` across grid meshes and traces contour levels with vertex labels. Saved as `Gini contours-inequality`.
*   **Gini Social Welfare Contours**: Evaluates `SWGini(X, Y)` to map aggregate welfare distributions over the social classes. Saved as `Gini contours-SW`.
*   **Atkinson Inequality Contours**: Evaluates `inequalAtk(X, Y, eta)` under adjustable inequality aversion parameters ($\eta = 0.5$ or $\eta = 2$). Saved as `Atkinson inequality contours_etaX.png`.
*   **Atkinson Social Welfare Contours**: Evaluates `SWAtk(X, Y, eta)` for specified aversion levels. Saved as `Atkinson social welfare contours_etaX.png`.

---

## 11. Monte Carlo Simplex Scatter Plotting (`scattersimplot`)

*   **`scattersimplot(typen, XX, YY)`**: Loads pre-computed Monte Carlo Excel datasets (`SAMPLE_s{Soc}_{type1}_{type2}.xlsx`) based on specific interaction structures (`Cooperation`, `Competition`, `Homophily`, `Diversity`, `Attraction`, `Exploitation`).
*   Extracts specified column indices (`XX`, `YY`), transforms coordinates into the equilateral triangle space, applies specific taxonomy colors (`col[typen]`), and exports high-resolution scatter graphics (`MC_s{Soc}_simplex_{typen}.png`).

---

## 12. Comparative Scatter & Indifference Curves (`scatterplot` / `scatterplot2`)

*   **`scatterplot2(...)` & `scatterplot(...)`**: Generates comparative statistical scatter plots between paired game types (e.g., Cooperation vs. Competition) across multiple dimensions:
    *   **North-West (NW)**: Average welfare vs. Gini inequality.
    *   **North-East (NE)**: Average welfare vs. Atkinson inequality ($\eta = 0.5$).
    *   **Centre-West (CW)**: Average welfare vs. Atkinson inequality ($\eta = 2$).
    *   **Centre-East (CE)**: Gini social welfare vs. Mobility (Determinant).
    *   **South-West (SW)**: Atkinson social welfare ($\eta = 2$) vs. Mobility (Difference).
    *   **South-East (SE)**: Short-term opportunities vs. Long-term discounted opportunities ($\eta = 2$).

---

## 13. Cumulative Distribution Functions (`CDF`)

*   Iterates over all 10 core quantitative indicators (Gini/Atkinson inequalities, social welfare variations, mobility determinants/differences, and opportunities).
*   Extracts simulation outcomes for competing interaction types, sorts series values, computes cumulative fractions via expanding counts, and generates step-plots (`plt.step(..., where='post')`) exported as CDF comparison charts.

---

## 14. Median-Split Degree Scatterplots (`scattersimplot_median_split`)

*   **`scatter_subset_plot(...)`**: Loads simulation arrays including degree centrality metrics from designated column indexes (`zz = 25`).
*   Splits datasets relative to the median degree value (`z < median_z` vs. `z >= median_z`) to isolate and visualize structural effects of low-degree versus high-degree social connectivity configurations within the simplex.

---

## 15. Dynamic Vector Fields & Limit Points

*   **`run_model(...)`**: Simulates temporal convergence trajectories over large time horizons ($T = 20,000$) starting from various initial conditions ([0.25, 0.5, 0.25], [1, 0, 0], [0, 0, 1]) to isolate steady-state limit points ($X_0, Y_0$, $XH_0, YH_0$, $XL_0, YL_0$).
*   **`vector_field(X, Y)`**: Computes single-period variation vector components ($U, V$) across meshgrids.
*   **`plt.quiver(...)`**: Renders blue vector field arrows mapped onto equilateral triangle coordinates (`U_tri`, `V_tri`), highlighting directional trajectories, stability basins, and asymptotic limits for specific game configurations.
