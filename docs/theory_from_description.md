# Theory Summary Extracted from Project Description (Bayesian Lasso)

This document summarises and structures all theory related to the **Bayesian Lasso** as described in the provided project description PDF.  
It is organised in a dissertation-friendly format, ready to be used as the theory foundation for the full report.

---

## 1. Linear Regression Basics

We begin with the standard linear regression model:

\[
y = X\beta + \varepsilon,
\]

where  
\(\varepsilon \sim N(0, \sigma^2 I)\).

In component form:

\[
y_i = x_i^\top \beta + \varepsilon_i.
\]

This is the base framework upon which Bayesian Lasso is built.

---

## 2. Bayesian Linear Regression (Background)

Bayesian regression treats parameters as random variables and assigns priors.

A common conjugate prior is:

- \(\beta \mid \sigma^2 \sim N(m, \sigma^2 M)\)
- \(\sigma^2 \sim \text{Inv-Gamma}(a, b)\)

Under these priors, the posterior \((\beta, \sigma^2)\) remains a **Normal–Inverse-Gamma** distribution.

After marginalising out \(\sigma^2\), the posterior of \(\beta\) becomes a **multivariate t distribution**.

This section serves as theoretical foundation and contrast to the Bayesian Lasso.

---

## 3. Variable Selection Motivation

In many applications, only a subset of predictors is relevant.  
Frequentist Lasso achieves sparsity using the \(\ell_1\) penalty:

\[
\frac{1}{2}\|y - X\beta\|^2 + \lambda \|\beta\|_1.
\]

In a Bayesian context, we desire priors that induce sparsity.  
Spike-and-slab priors are theoretically ideal but computationally expensive.

The **Bayesian Lasso** offers a more tractable compromise.

---

## 4. Bayesian Lasso Key Idea (Park & Casella, 2008)

The Bayesian Lasso imposes **Laplace (double-exponential) priors** on regression coefficients:

\[
p(\beta_i \mid \lambda) = \frac{\lambda}{2} \exp(-\lambda |\beta_i|).
\]

The Laplace prior induces shrinkage toward zero, promoting sparsity.

However, the Laplace prior is non-conjugate and not directly usable in Gibbs sampling.  
The key trick is expressing the Laplace prior as a mixture of normal distributions.

---

## 5. Laplace Prior as Scale Mixture of Normals (Core Result)

The Laplace distribution can be represented as a **normal variance mixture**:

\[
\beta \mid \sigma^2, \tau^2 \sim N(0, \sigma^2 \, \mathrm{diag}(\tau_1^2, ..., \tau_p^2)),
\]

\[
\tau_i^2 \mid \lambda^2 \sim \text{Exponential}\left(\frac{1}{2}\lambda^2\right).
\]

Implications:

- Each coefficient \(\beta_i\) has its own scaling parameter \(\tau_i^2\).
- Small \(\tau_i^2\) ⇒ strong shrinkage.
- Conditional distributions become conjugate ⇒ Gibbs sampling possible.

This mixture representation is the mathematical core enabling the Bayesian Lasso.

---

## 6. Full Bayesian Lasso Hierarchical Model

The complete model given in the project description is:

### Likelihood
\[
y \mid \beta_0, \beta, \sigma^2
\sim N(\beta_0 1_n + X\beta, \sigma^2 I).
\]

### Priors
- \(\sigma^2 \sim \text{Inv-Gamma}(a, b)\)
- \(\beta_0 \sim\) flat prior  
- \(\beta \mid \sigma^2, \tau^2 \sim N(0, \sigma^2 \mathrm{diag}(\tau_1^2, ..., \tau_p^2))\)
- \(\tau_i^2 \mid \lambda^2 \sim \text{Exponential}\left(\frac{1}{2}\lambda^2\right)\)
- \(\lambda^2 \sim \text{Gamma}(a_\lambda, b_\lambda)\)

This is a full **hierarchical Bayesian model**.

Because of the mixture structure and multiple latent levels,  
the posterior does **not** have a closed form.

---

## 7. Why the Posterior Has No Closed Form

Reasons:

- Laplace prior is non-conjugate.
- The scale-mixture introduces latent variables \(\tau_i^2\).
- The shrinkage parameter \(\lambda^2\) has its own hyperprior.
- \(\sigma^2\) also has a prior.
- Multiple layers of hierarchy → multidimensional integration impossible in closed form.

Thus, **MCMC is required**.

---

## 8. Gibbs Sampling for Bayesian Lasso

The full conditional distributions are well-behaved, enabling Gibbs sampling.

Parameters sampled in each iteration:

1. **\(\beta_0\)** – Normal
2. **\(\beta\)** – Multivariate Normal
3. **\(\sigma^2\)** – Inverse-Gamma
4. **\(\tau_i^2\)** – Inverse Gaussian
5. **\(\lambda^2\)** – Gamma

All full conditionals are known, making the model computationally tractable.

This is the standard inference method for Bayesian Lasso
and the approach you will implement.

---

## 9. Variable Selection in Bayesian Lasso

The project description suggests:

- Use the **posterior mode** of \(\beta_i\) for variable selection.
- If the mode is close to zero, exclude the variable.
- This is a decision rule, not a probabilistic guarantee.

Limitations:

- This method ignores the uncertainty (posterior variance).
- The Bayesian Lasso cannot produce exact zeros (unlike spike-and-slab).

This should be discussed in your dissertation's "limitations" section.

---

## 10. Bayesian Lasso vs. Spike-and-Slab (Summary)

**Advantages:**
- Much easier to implement.
- Stable Gibbs sampling.
- Works well in high dimensions.

**Disadvantages:**
- Does not produce true zeros.
- Selection based only on posterior mode.
- Ignores parameter uncertainty in selection.

---

## End of Theory Summary

This file now contains the complete theoretical backbone extracted from the project description PDF.  
You can directly integrate this into the literature review and methodology sections of the dissertation.

