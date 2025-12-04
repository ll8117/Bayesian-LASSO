# Bayesian Lasso from Scratch (R & Python)

A full reimplementation of Park & Casella (2008) with Gibbs sampling, diagnostics, and modern documentation.

---

## 🎯 Project Goal

This project rebuilds my MSc dissertation on **Bayesian Lasso**, a Bayesuab sparse regression method that imposes a Laplace prior on regression coefficients.  
The original work was not completed to the standard I hoped for.  
This repository serves as a **complete and refined re-implementation**:  

- Full mathematical derivation  
- A working Gibbs sampler in **R** (primary version)  
- A second implementation in **Python**  
- Simulation, diagnostics, and visualisations  
- A rewritten dissertation-style report

---

## 📁 Repository Structure

```bash
docs/               -> project documentation & theory summary
notebooks/          -> analysis & experiments in R
src/R/              -> core R implementation (Gibbs sampler etc.)
src/python/         -> python implementation
data/               -> datasets (simulated or real)
figures/            -> plots for the report
```

---

## 📌 Features

### 🎲 Bayesian Lasso Hierarchical Model

Including:

- Normal likelihood  
- Laplace prior as a scale mixture of normals  
- Hierarchical prior on $\lambda^2$  
- Inverse-Gaussian latent variables $\tau_i^2$  

### 🎲 Full Gibbs Sampling Implementation

Sampling steps:

1. $\beta_0$ (intercept)  
2. $\beta$ (regression coefficients)  
3. $\sigma^2$  
4. $\tau_i^2$ latent scales (inverse Gaussian)  
5. $\lambda^2$ hyperparameter  

### 🎲 Diagnostics

- Trace plots  
- R-hat  
- Posterior densities  
- Posterior modes used for variable selection  

### 🎲 Comparison

- Classical Lasso (glmnet)  
- Bayesian Lasso behaviour under different $\lambda^2$ priors  

---

## 📔 References

- Park & Casella (2008), The Bayesian LASSO  
- Tibshirani (1996), Lasso Regression  

