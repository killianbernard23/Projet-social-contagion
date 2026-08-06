# Projet-social-contagion: Code Manual & Parameters Documentation

This document serves as the technical cookbook explaining the model parameters, abbreviations, economic functions, and simulation scripts used in the Python codebase for **Projet-social-contagion** (Ellouze, Fleurbaey, Prigent, 2026).

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

---

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

---

## 16. Dynamic Vector Field Title Formatting

*   **Decimal Handling (`digit`)**: Automatically adjusts the LaTeX display format for transition rates ($\alpha^+, \alpha^-, \beta^+, \beta^-, \gamma^+, \gamma^-$) based on the required level of precision (`digit = 1`, `2`, or `3`).
*   **File Naming for Backups (`save_basename`)**: Generates unique file identifiers incorporating the number of social contacts (`Soc`) and all six transition parameters for the automated archiving of vector graphics.

---

## 17. Empirical Cumulative Distribution Functions (ECDF) by Degree or Intensity (`CDFplot_by_zz`)

*   **`load_xz(label, XX, zzz, Soc)`**: Loads simulation samples for a given game type and simultaneously extracts the target economic indicator (`XX`) and the associated structural metric (`zzz`, representing either degree centrality or interaction intensity).
*   **`CDFplot_by_zz(zz, Soc)`**: Splits the samples into two sub-populations based on the median of the control variable (`z < median_z` vs. `z ≥ median_z`) and plots comparative step-wise cumulative distribution functions (`plt.step`) to evaluate the impact of centrality on economic performance.

---

## 18. Trajectories and Transition Paths in the Simplex (`pathway`)

*   **`pathway(X, Y)`**: Simulates temporal dynamic trajectories over a horizon of $10,000$ periods starting from extreme (`extreme`), median (`middle`), or combined (`both`) initial conditions.
*   **Attractor Identification**: Computes asymptotic limit points by initializing the model from specific distributions to identify basins of attraction and multiple equilibria within the system.

---

## 19. Temporal Evolution of Social Stratification (`plt.fill_between`)

*   **Horizon Simulation (`Hor`)**: Runs `run_model(...)` over short or long horizons (e.g., $20$, $500$, or $10,000$ periods) starting from an initial distribution (`Init`).
*   **Stacked Area Charts**: Graphically represents the evolution of population shares for each social class over time using colored zones:
    *   **Red**: Proportion of the low class ($L$).
    *   **Yellow**: Proportion of the middle class ($M$).
    *   **Blue**: Proportion of the high class ($H$).

---

## 20. Comparison of Limit Points According to Contact Intensity (`Soc1` vs. `Soc2`)

*   **Comparative Sensitivity Analysis**: Simulates and compares the location of stationary equilibrium points under a low contact intensity regime (`Soc1 = 2`, represented by `o` circles) versus a high intensity regime (`Soc2 = 30`, represented by `^` triangles).
*   Allows for a visual analysis of how increasing the number of social interactions per individual alters the structure of attractors within the Gibbs-Simplex triangle.

---

## Appendix: Complete Index and Explanation of Figures (in Ascending Order)

*   **Figure 1 (Decomposition of Basic BGDP Level)**: Stacked bar chart decomposing the basic BGDP change required to move from a reference society to the level of BGDP in each country (from Ethiopia to the USA), broken down by GDP, inequality (Gini), and life expectancy (LE).
*   **Figure 2 (Cross-Country Rank-Reversals)**: Analysis of cross-country rank-reversals with respect to GDP in 2023 across income categories (LIC, LMIC, UMIC, HIC) for aggregate welfare indicators.
*   **Figures 4 & 5 (Empirical Robustness & Dynamics)**: Extracts from the empirical section detailing result robustness and cross-country dynamics by income groups.
*   **Figure 6 (CRRA Functions)**: Graphical representation of CRRA-type functions with a regime change at $z = 3$ (with inequality aversion $\eta = 1$ and $\epsilon = 2$), illustrating the transition between the orange curve (below $z$) and the blue curve (above $z$).
*   **Figure 7 (Gini Contours)**: 
    *   *Left*: Contour lines of Gini inequality mapped within the Gibbs-Simplex triangle.
    *   *Right*: Contour lines of Gini-based social welfare distribution.
*   **Figure 8 (Atkinson Inequality Contours)**: Contour maps of Atkinson inequality across the simplex under varying inequality aversion parameters ($\eta = 0.5$ vs. $\eta = 2$).
*   **Figure 9 (Atkinson Social Welfare Contours)**: Contour maps of Atkinson social welfare across the simplex for specified aversion levels.
*   **Figure 10, 21, 23 (Simplex Scatter Plots)**: Monte Carlo dataset scatter plots projected into the simplex for archetypal interaction structures (*Cooperation, Competition, Homophily, Diversity, Attraction, Exploitation*).
*   **Figures 11 to 13 (Comparative Panels NW, NE, CW, CE, SW, SE)**: Multi-dimensional comparative scatter plots evaluating paired game types across average welfare vs. inequality, welfare vs. mobility, and short-term vs. discounted long-term opportunities.
*   **Figure 14 (Global CDF Indicators)**: Empirical Cumulative Distribution Functions (CDF) comparing baseline economic indicators across interaction types.
*   **Figure 15 (Median-Split Degree Scatterplot)**: Simplex scatter plots split by the median of degree centrality to isolate structural topology effects.
*   **Figure 16 (Dynamic Vector Fields)**: Dynamic vector fields (blue arrows) and asymptotic limit points (attractors) mapped onto equilateral triangle coordinates for specific transition parameters.
*   **Figure 17 & 19 (ECDF by Degree / Intensity)**: ECDFs segmented by whether agents lie below or above median degree or interaction intensity thresholds.
*   **Figure 18 (Median-Split Intensity Scatterplot)**: Simplex scatter plots split by the median of interaction intensity.
*   **Figures 20 to 22 (Simplex Pathways)**: Temporal convergence trajectories starting from extreme, middle, or combined initial conditions toward stable equilibria.
*   **Figure 23 (Contact Sensitivity $s$ Analysis)**: Comparative mapping of stationary equilibrium points under low contact intensity ($s=2$, circles) versus high contact intensity ($s=30$, triangles).
*   **Figure 24 (Temporal Stratification Dynamics)**: Stacked area charts illustrating the dynamic evolution of population class proportions over time ($H$ in blue, $M$ in yellow, $L$ in red).
