# Doubly Robust Population-Adjusted Indirect Comparison (DR-PAIC)

This repository contains a simulation study comparing doubly robust estimators for population-adjusted indirect comparisons with limited accessibility to individual-level patient data. The scenario involves having individual-level patient data (IPD) for one trial while only aggregate-level data (ALD) - such as first and second moments - is available for the comparator trial.

## File Structure

### Data Generation
- **`ess_cal.R`** - Determines ALD trial locations corresponding to specific effective sample size (ESS) reductions
- **`gen_pop.R`** - Generates IPD and ALD trial data for different sample sizes and ESS reduction levels

### Estimator Implementation
- **`aipw_marginalization.R`** - Implements AIPW, MAIC, Bayesian G-computation, and weighted doubly robust estimators for limited individual-level data scenarios
- **`tmle_marginalization.R`** - Implements TMLE doubly robust estimator for the same limited data context

### Simulation and Analysis
- **`main.R`** - Main simulation loop executing all estimators across scenarios and saving results

### Analysis and Visualization
- **`analysis_dr.R`** - Analyses simulation results and generates plots and tables for manuscript

## Methods Compared

- **MAIC**: Matching-Adjusted Indirect Comparison
- **AIPW**: Augmented Inverse Probability Weighting
- **TMLE**: Targeted Maximum Likelihood Estimation
- **Weighted DR**: Doubly robust estimation with weighted regressions
- **Bayesian G-computation**: Outcome modeling approach

## Simulation Setup

### Data Structure
- **Individual-level data**: Available for AC trial (treatment A vs C)
- **Aggregate-level data**: Available for BC trial (treatment B vs C) - only summary statistics
- **Objective**: Estimate A vs B treatment effect through population-adjusted indirect comparison (in the ALD population)

### Trial Population Generation
The study tests robustness to dependency structure misspecification by generating trials from structurally different source populations:

- **AC trial**: Generated using multivariate normal distribution, then transformed (X2 and X5 converted to binary via probit and logistic transformations)
- **BC trial**: Generated using `simstudy` package with sequential conditional definitions where variables depend on previously generated ones through different distributional forms (normal, binary, gamma)

This creates fundamentally different dependency structures between the two populations to test how doubly robust estimators handle violations of population homogeneity assumptions.

### Simulation Conditions
- **Sample sizes**: 100, 200, 600 patients per trial
- **ESS reduction levels**:
  - Moderate (~50% effective sample size reduction)
  - Severe (~82% effective sample size reduction)

The oracle scenario, obtained by marginalizing over the true ALD population, serves as a reference to investigate bias from population
misspecification. Results are analyzed using the `rsimsum` package to assess bias, coverage, and mean squared error when dependency structures in the ALD population are mis-specified.
