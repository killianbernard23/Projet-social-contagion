# Projet-social-contagion: Complete Code Manual & Parameters Documentation

This document serves as the exhaustive "cookbook" manual explaining every single function, global variable, parameter, simulation loop, and visualization block used in the Python script for the **Projet-social-contagion** repository.

---

## 1. Environment Setup, Imports & Path Configurations

The script begins by importing core numerical, data manipulation, and plotting libraries, followed by setting up professional typography and cross-platform directory paths.

*   **`from __future__ import division`**: Ensures standard floating-point division behavior is consistent across Python 2 and Python 3 environments.
*   **`import numpy as np`**: Imports NumPy for high-performance matrix calculations, array operations, and mathematical functions.
*   **`import sys`**: Used for system-level executions, such as gracefully halting execution (`sys.exit(1)`) if probabilities or population shares violate boundary constraints.
*   **`import matplotlib.pyplot as plt`**: Imports Pyplot for generating, configuring, and saving publication-quality charts and graphs.
*   **`import matplotlib as mpl`**: Used for global configuration settings of matplotlib parameters.
*   **`import matplotlib.lines as mlines`**: Manages custom line styles for plots.
*   **`import pandas as pd`**: Imports Pandas for handling DataFrames, organizing Monte Carlo output data, and exporting results to Excel files.
*   **`mpl.rcParams['mathtext.fontset'] = 'stix'`**: Configures the mathematical text rendering engine to use the STIX font set.
*   **`mpl.rcParams['font.family'] = 'STIXGeneral'`**: Sets STIXGeneral as the default font family for consistent academic typography.
*   **`from pathlib import Path`**: Uses modern object-oriented filesystem paths.
    *   **`HERE = Path(__file__).parent`**: Identifies the directory containing the active script.
    *   **`loadpath`**: Points to the target directory (`Game of life/Final3/Results`) for loading pre-computed data files.
    *   **`save_res`**: Designates the destination folder for output Excel results.
    *   **`save_fig`**: Designates the destination folder (`Game of life/Final3/Figures`) for saving generated high-resolution PNG figures.
*   **`col` (Dictionary)**: Maps specific social interaction structures (`Cooperation`, `Competition`, `Attraction`, `Exploitation`, `Homophily`, `Diversity`, etc.) to dedicated color codes for visual tracking in plots.

---

## 2. Foundational Socioeconomic Constants & Reward Levels

These parameters define the status rewards assigned to each social class in the tripartite stratification model:

*   **`RH = 1`**: The reward or status valuation level assigned to the **High-status** class ($H$).
*   **`RM = 0.5`**: The reward or status valuation level assigned to the **Middle-status** class ($M$).
*   **`RL = 0.1`**: The reward or status valuation level assigned to the **Low-status** class ($L$).

---

## 3. Social & Economic Indicator Functions

These core functions quantify aggregate population statistics, inequality, and welfare:

*   **`Average(L, H)`**: Computes the weighted average of the population's socioeconomic status based on the proportions of High ($H$), Middle ($1-L-H$), and Low ($L$) classes.
*   **`inequalAtk(L, H, eta)`**: Computes **Atkinson's inequality index** given a specific inequality aversion parameter $\eta$ (`eta`).
*   **`inequalGini(L, H)`**: Computes the **Gini inequality index** directly from class proportions and status rewards.
*   **`SWAtk(L, H, eta)`**: Evaluates **Social Welfare** under the Atkinson framework for a given inequality aversion level $\eta$.
*   **`SWGini(L, H)`**: Evaluates **Social Welfare** based on the Gini distribution structure.

---

## 4. Social Mobility Functions

These functions process transition matrices (`TRANS`) to evaluate structural and operational mobility across social strata:

*   **`MobDet(TRANS)`**: Computes a mobility index derived from the matrix determinant: $1 - |\det(\text{TRANS})|^{1/2}$.
*   **`MobTr(TRANS)`**: Calculates mobility using the trace of the transition matrix: $(3 - \text{trace}(\text{TRANS})) / 2$.
*   **`MobDif(TRANS, L, H, RH, RM, RL)`**: Measures net differential mobility effects across strata boundaries weighted by reward differences.
*   **`MobOp(TRANS, L, H, RH, RM, RL, eta)`**: Calculates short-term operational mobility based on expected status distributions.
*   **`MobLTOp(TRANS, L, H, RH, RM, RL, eta, beta)`**: Evaluates long-term operational mobility by computing discounted matrix inversions using a temporal discount factor $\beta$ (`beta`, e.g., 0.97).

---

## 5. Simplex Geometry & Vector Field Functions

*   **`to_equilateral(x, y)`**: Transforms standard 2D Cartesian coordinates into coordinates fitted for an equilateral triangle simplex representing three-class population distributions.
*   **`vector_field(X, Y)`**: Constructs grid-based vector components ($U, V$) mapping population share trajectories across the simplex by evaluating single-period variations.

---

## 6. Core Model Dynamics & Simulation Functions

*   **`core_stat(...)`**: Computes steady-state statistics, including class proportions, inequality indices, social welfare measures, mobility indices, and parameter distance/intensity metrics.
*   **`run_model(...)`**: Simulates the temporal evolution of social strata over a specified time horizon (`Hor`) using non-linear probability adjustments and social contact scaling (`Soc`). Includes strict safety checks to catch boundary violations ($[0,1]$).
*   **`run_TRANS(...)`**: Executes the temporal simulation steps to construct and return the final $3 \times 3$ transition matrix (`TRANS`).
*   **`run_var(...)`**: Computes class distribution variations (`HV`, `MV`, `LV`) over a single time period.

---

## 7. Interaction Parameter Breakdown (`psl`, `psd`, `pel`, `ped`, `pil`, `pid`)

The behavioral parameters governing state transitions via upward ($\cdot^+$) and downward ($\cdot^-$) interaction matrices (`PU` and `PD`):

*   **`psl` ($\alpha^+$)**: Probability that a low-status connection induces an upward shift.
*   **`psd` ($\alpha^-$)**: Probability that a low-status connection induces a downward shift.
*   **`pel` ($\beta^+$)**: Probability associated with middle peer interactions shifting upward.
*   **`ped` ($\beta^-$)**: Probability associated with middle peer interactions shifting downward.
*   **`pil` ($\gamma^+$)**: Probability that a high-status connection induces an upward movement.
*   **`pid` ($\gamma^-$)**: Probability that a high-status connection induces a downward shock.

---

## 8. Epidemiological & Pandemic Simulation Blocks

*   **Prevalence Probability Check**: Calculates statistical bounds and failure probabilities for observing rare events across sample sizes ($n = 10000$).
*   **Infection Curve Models**: Compares non-linear infection formulas against linear SIR formulations as a function of disease prevalence.
*   **`pandemic(...)`**: Simulates an SIR-like epidemic spreading across immune/recovered ($H$), susceptible ($M$), and sick ($L$) populations.
*   **Visualization Scripts**: Generates multi-layered filled temporal stack plots for pandemic dynamics, learning processes, and flattened curves resulting from contact reduction ($s=5$).

---

## 9. Simplex Visualization & Geometry Construction

*   Constructs a 2D meshgrid restricted by simplex constraints ($X + Y \le 1$).
*   Renders professional equilateral triangle boundaries, directional vertex arrows, scale ticks, vertex labels ($H, M, L$), and interior grid lines.
*   Plots initial steady-state coordinate markers with formatted text boxes.

---

## 10. Monte Carlo Simulation, Parameter Sampling & Data Export

*   **`sample_param(typen)`**: Randomly draws and tests uniform parameters against strict inequality constraints to generate valid parameter sets for six distinct social games:
    *   `Cooperation`
    *   `Competition`
    *   `Attraction`
    *   `Exploitation`
    *   `Homophily`
    *   `Diversity`
*   **Monte Carlo Iteration Loops**: Runs up to 1000 convergent simulations per game type using defined tolerances (`tol = 1e-5`) and social contact parameters (`Soc = 40`).
*   **Divergence Tracking**: Captures non-convergent runs into dedicated divergence datasets (`SAMPLEdiv`).
*   **Excel Export**: Compiles results into Pandas DataFrames (`dfinit`, `dfindiv`) and exports them directly into `.xlsx` files inside the results directory.

---

## 11. Comparative Boxplot Visualizations

*   Loads pre-computed Monte Carlo Excel datasets.
*   Extracts parameter distributions (`psl`, `psd`, `pel`, `ped`, `pil`, `pid`) for comparative game types (e.g., `Cooperation` vs. `Competition`).
*   Generates grouped boxplots featuring custom color fills, mathematical LaTeX labels ($\alpha^+, \alpha^-, \beta^+, \beta^-, \gamma^+, \gamma^-$), custom whiskers (`[5, 95]`), legends, and dashed grids.
*   Saves final high-resolution comparison figures at 300 DPI to the designated figures directory.
