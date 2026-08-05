# Projet-social-contagion
# Projet-social-contagion: Code Manual & Parameters Documentation

This document serves as the "cookbook" manual explaining every function, parameter, and variable used in the Python script.

## 1. Global Parameters & Economic Constants

Before diving into the functions, the code defines foundational socioeconomic values and transition probability bounds:

*   **`RH = 1`**: Represents the high-status class value or reward level.
*   **`RM = 0.5`**: Represents the middle-status class value or reward level.
*   **`RL = 0.1`**: Represents the low-status class value or reward level.
*   **`Soc`**: The number of social contacts per period (e.g., set to 30 or 40). It dictates the intensity of social interactions.
*   **`eta` ($\eta$)**: The inequality aversion parameter used in Atkinson-based metrics.
*   **`gamma` ($\gamma$)**: The infection or transmission rate parameter used in the pandemic model.
*   **`l_plusplus` ($l^{++}$)**: The recovery or transition probability parameter back to a higher/immune state in the epidemic model.

## 2. Social & Economic Indicator Functions

These functions measure inequality, social welfare, and distribution characteristics across social classes:

*   **`Average(L, H)`**: Calculates the weighted average of the population's socioeconomic status. Parameters: `L` (low-status), `H` (high-status).
*   **`inequalAtk(L, H, eta)`**: Computes Atkinson's inequality index based on the chosen aversion parameter `eta`.
*   **`inequalGini(L, H)`**: Computes the Gini inequality index for the population distribution.
*   **`SWAtk(L, H, eta)`**: Evaluates Social Welfare (Atkinson framework).
*   **`SWGini(L, H)`**: Evaluates Social Welfare using the Gini-based distribution structure.

## 3. Social Mobility Functions

These functions analyze transition matrices (`TRANS`) to evaluate how agents move between social strata:

*   **`MobDet(TRANS)`**: Measures mobility based on the determinant of the transition matrix `TRANS`.
*   **`MobTr(TRANS)`**: Computes a mobility metric using the trace of the transition matrix.
*   **`MobDif(TRANS, L, H, RH, RM, RL)`**: Measures net differential mobility effects across strata transitions.
*   **`MobOp(TRANS, L, H, RH, RM, RL, eta)`**: Calculates short-term operational mobility.
*   **`MobLTOp(TRANS, L, H, RH, RM, RL, eta, beta)`**: Evaluates long-term operational mobility using discounted matrix inversion with a discount factor `beta` (e.g., 0.97).

## 4. Core Model Dynamics & Simulation Functions

*   **`to_equilateral(x, y)`**: Converts 2D cartesian coordinates into coordinates fitted for an equilateral triangle.
*   **`vector_field(X, Y)`**: Constructs a grid-based vector field mapping population share trajectories.
*   **`core_stat(...)`**: Computes steady-state statistics (class proportions, inequality indices, social welfare, mobility).
*   **`run_model(...)`**: Simulates the temporal evolution of social strata over a specified horizon (`Hor`).
*   **`run_TRANS(...)`**: Runs the simulation to construct and return the final transition matrix `TRANS`.
*   **`run_var(...)`**: Computes class distribution variations over a single time step.
*   **`pandemic(...)`**: Simulates an epidemiological SIR-like spreading process across immune (`H`), susceptible (`M`), and sick (`L`) populations.

## 5. Interaction Parameter Breakdown (`psl`, `psd`, `pel`, `ped`, `pil`, `pid`)

The behavioral parameters governing state transitions:

*   **`psl` ($\alpha^+$)**: Probability that a low-status connection induces an upward shift.
*   **`psd` ($\alpha^-$)**: Probability that a low-status connection induces a downward shift.
*   **`pel` ($\beta^+$)**: Probability associated with middle peer interactions shifting upward.
*   **`ped` ($\beta^-$)**: Probability associated with middle peer interactions shifting downward.
*   **`pil` ($\gamma^+$)**: Probability that a high-status connection induces an upward movement.
*   **`pid` ($\gamma^-$)**: Probability that a high-status connection induces a downward shock.

## 6. Plotting and Monte Carlo Simulation

*   **`scatterplot(...)` & `scatterplot2(...)`**: Generate comparative statistical scatter plots of simulation outputs.
*   **`sample_param(typen)`**: Randomly samples parameter tuples for specific game types: `Cooperation`, `Competition`, `Attraction`, `Exploitation`, `Homophily`, `Diversity`.
