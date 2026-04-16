# STT_812_project
# Bayesian Analysis of Lung Cancer Survival Patterns

This project performs a comprehensive survival analysis of lung cancer patients using SEER data, combining classical survival methods with Bayesian modeling.

##  Project Overview

This analysis investigates survival outcomes for lung and bronchus cancer patients using data from the SEER (Surveillance, Epidemiology, and End Results) program.

The project progresses through three levels of modeling:

1. **Kaplan-Meier Estimation** (non-parametric baseline)
2. **Cox Proportional Hazards Model** (frequentist baseline)
3. **Bayesian Weibull Survival Model (Stan)** (primary model)

The goal is to understand how demographic and clinical factors influence survival while leveraging Bayesian inference to quantify uncertainty.

---

## Dataset

- Source: SEER Program (National Cancer Institute)
- Size: ~1.1 million patient records  
- Time span: 2000–2020  
- Each row represents a single patient

### Key Variables
- Survival time (months)
- Vital status (dead vs censored)
- Cancer stage
- Age, sex, race
- Tumor characteristics

---

## Data Processing

The raw dataset required significant preprocessing:

- Converted survival time from zero-padded strings to numeric
- Created event indicator (death vs censored)
- Combined multiple grade variables into a single feature
- Simplified stage into:
  - localized
  - regional
  - distant
- Extracted numeric age from categorical ranges
- Removed invalid or missing survival observations

---

## Exploratory Data Analysis

### Survival Distribution
- Median survival: ~9 months  
- Strong right-skew in survival times  
- High event rate (~84%)

### Kaplan-Meier Analysis

Survival curves were stratified by:

- **Stage**
  - Clear separation between localized, regional, and distant groups
  - Highly significant differences (p < 0.0001)

- **Sex**
  - Females show consistently higher survival probabilities

- **Race**
  - Observable disparities across racial groups

These results confirm that stage, sex, and race are meaningful predictors of survival.

---

## Modeling Approaches

### 1. Cox Proportional Hazards Model

Baseline frequentist model using:

- Age (standardized)
- Sex
- Stage
- Race

**Key Results:**
- Stage is the strongest predictor of mortality
- Older age increases hazard
- Females have lower risk than males
- Model performance:
  - Concordance index ≈ 0.70

---

### 2. Bayesian Weibull Survival Model (Primary Model)

Implemented in **Stan using MCMC**.

#### Why Weibull?
- Flexible hazard function (increasing/decreasing)
- Handles censored data naturally
- Suitable for medical survival data

#### Model Features:
- Separate likelihood for observed vs censored cases
- Covariates:
  - age
  - sex
  - stage
- Priors:
  - Weakly informative (Normal, Gamma)
  - Incorporate domain knowledge without overpowering data

---

## Bayesian Inference Results

### Convergence Diagnostics
- R-hat ≈ 1.0 for all parameters
- Good chain mixing (“fuzzy caterpillar” trace plots)

### Posterior Insights
- Age and stage strongly decrease survival
- Posterior distributions are well-behaved and unimodal
- Shape parameter (~0.8) suggests decreasing hazard over time

### Key Advantage
Unlike Cox:
- Provides **full posterior distributions**
- Enables **credible intervals**
- Quantifies uncertainty in estimates

---

## Model Validation

### Posterior Predictive Checks
- Simulated survival times closely match observed distribution

### LOO Cross-Validation
- All Pareto k < 0.7 (no influential outliers)
- Good out-of-sample predictive performance

---

## Final Results

- Cancer stage is the dominant predictor of survival
- Age and sex significantly influence outcomes
- Bayesian Weibull model provides:
  - Accurate survival estimates
  - Robust uncertainty quantification
  - Strong predictive performance

---

## Future Work

- Hierarchical Bayesian modeling
- Subgroup analysis across demographic groups
- Incorporation of additional clinical variables

---
## Repository Structure
```bash
project-name/
├── data/
│ ├── seer 17/ # Original dataset
├── source/
│ ├── lung_cancer_survival_1/ # R Markdown file (.Rmd)
├── Ouputs/
│ ├── lung_cancer_survival_1/ # Rendered output (.pdf)
│ └── lung_cancer_survival_1/ # Rendered output (.html)
├── README.md
```
---
## How to Run
- Clone the repo
- Install dependencies
- Open notebook or RMarkdown
- Run all cells to reproduce analysis
---
## Tools & Libraries
- R / Python (depending on your version)
- survival, survminer
- rstan / Stan
- tidyverse
- ggplot2
- loo
---
## Authors
- Nyla Blagrove
- Randal Burks
- Madison Honore
