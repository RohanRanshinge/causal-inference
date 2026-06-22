# Study Plan: Zero to Hero in Causal Impact

> **Project Goal:** Fully understand the internals of `CausalImpact()` and build a custom causal impact package from scratch in Python or R.
>
> **Topics Covered:** Bayesian Statistics · Time Series Analysis · State Space Models · Causal Inference

---
## Required Reading List

Anchor these papers to the phases below — do not save them all for the end.

| Paper / Book | Read Alongside |
|---|---|
| Brodersen et al. (2015) — *Inferring causal impact using Bayesian structural time-series models* | Phases 4 & 5 |
| Scott & Varian (2014) — *Predicting the Present with Bayesian Structural Time Series* | Phase 4 |
| Schmitt et al. — *Detecting changes in dynamic experiments using Bayesian online change point detection* (multi-intervention BSTS extension) | Phase 5 |
| Durbin & Koopman (2012) — *Time Series Analysis by State Space Methods* | Phase 3 |
| Abadie, Diamond & Hainmueller (2010) — *Synthetic Control Methods for Comparative Case Studies* | Phase 5 |

---
## Phase 1: Foundations of Bayesian Probability

*Goal: Understand how to think about probability as a measure of belief and solve simple models analytically.*

### Chapter 1: The Bayes’ Theorem

* **Core Concept:** Shifting from frequentist probability to conditional probability.
* **Key Topics:**
    * Mathematical definition of conditional probability.
    * Derivation of Bayes' Theorem: $P(A|B) = \frac{P(B|A)P(A)}{P(B)}$
    * Base rate fallacy and real-world intuition.

### Chapter 2: Bayesian Inference: Priors, Likelihoods, and Posteriors
* **Core Concept:** Updating updating belief space with data.
* **Key Topics:**
    * **The Prior ($P(\theta)$):** Representing uncertainty before seeing data.
    * **The Likelihood ($P(X|\theta)$):** How likely the data is, given a parameter.
    * **The Posterior ($P(\theta|X)$):** The updated belief distribution.
    * **The Normalizing Constant / Evidence ($P(X)$):** Why integration becomes the bottleneck.

### Chapter 3: Probability Distributions
* **Core Concept:** Mastering the mathematical building blocks of Bayesian models.
* **Key Topics:**
    * **Probability Mass Function (PMF) vs. Probability Density Function (PDF).**
    * **Bernoulli & Binomial Distributions:** Modeling binary outcomes and trials.
    * **Beta Distribution:** Modeling probabilities and proportions (ranges from 0 to 1).
    * **Normal (Gaussian) Distribution:** The foundation of measurement noise and state spaces.
    * **Poisson & Gamma Distributions:** Modeling counts, rates, and waiting times.
    * **Inverse-Gamma Distribution:** The natural prior over variance parameters — used directly in BSTS.

**Multivariate Normal Distribution (MVN)**

The Kalman Filter operates entirely in matrix algebra over multivariate Gaussians. This section is a prerequisite for all of Phase 3.

- Definition: $\mathbf{x} \sim \mathcal{N}(\boldsymbol{\mu}, \boldsymbol{\Sigma})$, where $\boldsymbol{\Sigma}$ is a covariance matrix.
- Joint, marginal, and conditional distributions.
- The Schur complement — deriving the conditional distribution $p(\mathbf{x}_1 | \mathbf{x}_2)$, which is the algebraic core of the Kalman update step.
- Linear transformations of MVNs: if $\mathbf{x} \sim \mathcal{N}(\boldsymbol{\mu}, \boldsymbol{\Sigma})$, then $A\mathbf{x} \sim \mathcal{N}(A\boldsymbol{\mu}, A\boldsymbol{\Sigma}A^T)$.

### Chapter 4: Conjugate Priors
* **Core Concept:** Solving Bayesian updating exactly using pen and paper when the prior and posterior share the same distribution family.
* **Key Topics:**
    * **Beta-Binomial Conjugacy:** Updating a coin-flip probability.
    * **Gamma-Poisson Conjugacy:** Updating rate parameters.
    * Mathematical proofs of updating equations (adding successes/failures to parameters).
    
**The conjugate pairs that appear in BSTS:**
- **Normal-Normal conjugacy:** Normal likelihood with a Normal prior on the mean → Normal posterior. Proof of the posterior mean as a precision-weighted average of prior and data.
- **Normal-Inverse-Gamma conjugacy:** Jointly updating a mean $\mu$ and variance $\sigma^2$ — the direct ancestor of the variance-sampling step in the BSTS Gibbs sampler.
- Mathematical derivation of posterior hyperparameter update equations for both.

**💻 Coding Exercise 1** *(after completing this chapter):*
Implement Normal-Normal Bayesian updating from scratch in Python/R. Start with a prior $\mu_0, \sigma_0^2$, generate synthetic data, and plot how the posterior mean and variance evolve as you add observations one at a time. Verify analytically.

---

## Phase 2: Approximate Inference & Computation
*Goal: Learn how to sample from complex posteriors when exact mathematical equations are impossible to calculate.*

### Chapter 5: Markov Chain Monte Carlo (MCMC) & Gibbs Sampling
* **Core Concept:** Simulating paths through a probability distribution to map out the posterior.
* **Key Topics:**
    * The limits of analytical calculation in high dimensions.
    * **Metropolis-Hastings Algorithm:** Accept/reject mechanics based on probability ratios.
    * **Gibbs Sampling:** Iteratively sampling each parameter conditioned on current values of all other parameters (the core engine of BSTS).
    * **MCMC Diagnostics:** Trace plots, $\hat{R}$ (Gelman-Rubin) statistic, autocorrelation, and burn-in periods.
    * Why Gibbs sampling is a special case of Metropolis-Hastings with acceptance probability = 1.

### Chapter 6: Bayesian Linear Regression & Feature Selection
* **Core Concept:** Running a linear model with priors on coefficients — the building block of the BSTS regression component.
* **Key Topics:**
    - Bayesian OLS setup: prior on $\beta$ and $\sigma^2$, likelihood, posterior.
    - Using the Normal-Inverse-Gamma conjugate pair to derive the posterior analytically.
    - Comparison to frequentist OLS: posterior mean collapses to OLS estimate as prior weakens.
    - MCMC for Bayesian regression when conjugacy does not apply.
    - Posterior predictive distribution: not just a point estimate, but a distribution over future $y$ values.

**💻 Coding Exercise 2** *(after completing this chapter):*
Implement Bayesian linear regression from scratch using Gibbs sampling. Compare posterior coefficient distributions to OLS estimates on a synthetic dataset. Verify the posterior predictive interval against held-out data.
---

## Phase 3: State Space Models & The Kalman Filter
*Goal: Model time-series data where parameters change dynamically at every time step.*

### Chapter 7: Introduction to State Space Models (SSMs)
* **Core Concept:** Separating observed, noisy data from the true, hidden underlying states.
* **Key Topics:**
    * **Observation Equation:** Mapping the hidden state $\alpha_t$ to the observed data $y_t$:
      $$y_t = Z_t^T \alpha_t + \varepsilon_t, \quad \varepsilon_t \sim N(0, H_t)$$
    * **Transition (State) Equation:** Defining how the hidden state evolves over time:
      $$\alpha_{t+1} = T_t \alpha_t + R_t \eta_t, \quad \eta_t \sim N(0, Q_t)$$

**Warm-up: The Local Level Model**

Before implementing the full BSTS, implement the simplest possible SSM. This is the essential stepping stone between abstract SSM equations and the full Gibbs sampler:

- State $\alpha_t$ is a scalar random walk: $\alpha_{t+1} = \alpha_t + \eta_t$
- Observation: $y_t = \alpha_t + \varepsilon_t$
- Only two variance parameters: $\sigma^2_\eta$ (state noise) and $\sigma^2_\varepsilon$ (observation noise).
- Solve the Kalman Filter on this model analytically first, then extend.

**Key SSM concepts:**
- What $Z_t$, $T_t$, $R_t$, $H_t$, $Q_t$ each represent and how they are specified for different model structures.
- The Markov property of states: $\alpha_{t+1} \perp \alpha_{1:t-1} \mid \alpha_t$.
- How local trend, seasonality, and regression components are each encoded as blocks of the state vector $\alpha_t$.

### Chapter 8: The Kalman Filter and Smoother
* **Core Concept:** Recursively tracking and smoothing hidden states over time.
* **Key Topics:**
    * **The Filter Loop:** Alternating between *Prediction* (moving forward one step in time) and *Update* (correcting the prediction based on the new observation error).
    * **Kalman Gain:** Determining how much weight to give the prediction versus the new noisy measurement.
    * **Kalman Smoothing:** Running the algorithm backward from time $T$ to time 1 to estimate historical states using the full dataset.

**Missing Data — The Counterfactual Mechanism:**
- When $y_t$ is missing (i.e., during the post-intervention period), skip the update step and propagate the prediction only.
- This is the core mechanism by which CausalImpact generates counterfactual forecasts. The post-period observations are withheld and the filter runs forward on state predictions alone.

**Log-Likelihood via Prediction Error Decomposition:**
- The Kalman Filter produces a sequence of prediction errors (innovations) and their variances as a by-product.
- These can be assembled into the model log-likelihood, which is how observation variances are estimated.
- This connects the filtering step to model fitting — a link the original plan omits entirely.

**Kalman Smoothing (RTS Smoother):**
- Running the algorithm backward from time $T$ to time $1$ to estimate historical states using the full dataset.
- Why smoothed state estimates are more accurate than filtered ones for historical analysis.

**💻 Coding Exercise 3** *(after completing this chapter):*
Implement a univariate Kalman Filter and RTS Smoother from scratch on a synthetic random walk plus noise series. Verify your filtered and smoothed state estimates against `statsmodels.tsa.statespace`. Then simulate a "post-intervention period" by withholding the last 20 observations and plotting the counterfactual forecast with uncertainty bands.

---

## Phase 4: Bayesian Structural Time Series (BSTS)
*Goal: Combine structural time-series components with Bayesian feature selection into a unified forecasting engine.*

### Chapter 9: Decomposing Time Series Components
* **Core Concept:** Breaking a time series down into additive state vectors.
* **Key Topics:**
    * **Local Linear Trend:** Modeling shifting levels and slopes over time.
    * **Seasonality:** Modeling evolving seasonal effects (e.g., day-of-week effects) using dummy variables or trigonometric states.
    * **Regression Component:** Adding contemporaneous external predictors (selected via Spike and Slab) directly into the state equation.

### Chapter 10: Spike and Slab Priors & Bayesian Variable Selection

**Core Concept:** Selecting the most predictive control markets from a large pool, directly within the BSTS model.

**Key Topics:**

**The Sparsity Problem:**
- Selecting 5 useful control markets out of 100 potential options.
- Why standard MCMC over all $2^{100}$ subsets is computationally intractable.

**The Spike and Slab Prior:**
- Each coefficient $\beta_j$ has a mixture prior:
  - **Spike:** Point mass at zero — excludes the variable from the model.
  - **Slab:** A wide Normal prior — includes the variable with uncertainty.
- A binary inclusion indicator $\gamma_j \in \{0, 1\}$ governs which component applies.

**Gibbs Sampling for Variable Selection:**
- Jointly sampling $(\boldsymbol{\beta}, \boldsymbol{\gamma})$ — coefficients and inclusion indicators — in the Gibbs loop.
- The posterior inclusion probability (PIP) for each predictor as a diagnostic.
- Connection to model averaging: BSTS does not hard-select one set of variables; it averages over models.

### Chapter 11: The BSTS Gibbs Sampler
**Core Concept:** The master algorithm that ties state tracking, structural components, and regression together into one joint MCMC loop.

**Key Topics:**

**Forward Filtering Backward Sampling (FFBS):**
- The algorithm for drawing *sample paths* of hidden states given current model parameters — not just point estimates.
- Forward pass: run the Kalman Filter to get filtered distributions at each $t$.
- Backward pass: sample a complete state trajectory $\alpha_{1:T}$ from the joint smoothing distribution.
- Why this is more powerful than the RTS Smoother: FFBS gives you full posterior draws over entire state paths, not just point estimates.

**The Full Gibbs Loop:**

Each iteration of the sampler cycles through:

1. **Sample hidden states $\alpha_{1:T}$** given current parameters using FFBS.
2. **Sample structural variances** ($\sigma^2_\eta$, $\sigma^2_\delta$, etc.) given the sampled state path, using Normal-Inverse-Gamma conjugacy.
3. **Sample regression coefficients $\boldsymbol{\beta}$** and inclusion indicators $\boldsymbol{\gamma}$ given states and variances, using Spike and Slab Gibbs steps.
4. **Repeat** until convergence.

**Posterior Predictive Checks:**
- After fitting, simulate data from the posterior predictive distribution and compare to observed pre-period data.
- Using $\hat{R}$, trace plots, and effective sample size to verify the sampler has converged before drawing conclusions.

**💻 Coding Exercise 4** *(after completing this chapter):*
Implement FFBS on the Local Level Model from Chapter 7. Draw 1,000 state path samples and visualize the posterior distribution over $\alpha_t$ at several time points. Compare the mean of your samples to the RTS Smoother estimate from Exercise 3.
---

## Phase 5: Causal Inference & The Capstone
*Goal: Use your custom time-series engine to construct counterfactuals and measure treatment effects.*

### Chapter 12: The Counterfactual Framework & Synthetic Controls
* **Core Concept:** Framing impact analysis as a missing data problem.
* **Key Topics:**
    * The Rubin Causal Model and Potential Outcomes framework.
        * Defining the treatment effect as $\tau_t = Y_t(1) - Y_t(0)$, where $Y_t(0)$ is the unobserved counterfactual.
        * The fundamental problem of causal inference: you can only observe one potential outcome.
        * How BSTS solves this by predicting $Y_t(0)$ via the control series.
    * **Synthetic Controls:** Building an optimized, unexposed version of your target market using unaffected control markets.

Core Identifying Assumptions:
* **SUTVA (Stable Unit Treatment Value Assumption):** Each unit's outcome is unaffected by other units' treatment assignment. In practice: your control markets were not exposed to the campaign. If they were, your counterfactual is contaminated.
* **Parallel Trends / Pre-period Fit:** The relationship between the target and controls in the pre-period generalizes to the post-period in the absence of treatment. The quality of the pre-period fit is your primary diagnostic.
* **No Confounding:** No other event affected the target during the post-period that did not also affect the controls.

### Chapter 13: Capstone — Coding Your Custom Package

**Core Concept:** Writing the pure Python/R code to build the full CausalImpact package from scratch, integrating every component from prior chapters.

**Implementation Architecture:**

```
CustomCausalImpact/
├── data/
│   └── ingestion.py         # Input validation, train/test split
├── model/
│   ├── components.py        # Trend, seasonality, regression state specs
│   ├── kalman.py            # Kalman Filter + RTS Smoother + FFBS
│   ├── spike_slab.py        # Spike and Slab Gibbs step
│   └── gibbs.py             # Master BSTS Gibbs sampler loop
├── inference/
│   ├── counterfactual.py    # Post-period projection via missing-data filter
│   └── effects.py           # Pointwise, cumulative, credible intervals
├── validation/
│   └── placebo.py           # Placebo test runner
└── output/
    └── plots.py             # Summary visualization functions
```

**Step-by-Step Implementation Plan:**

**Step 1 — Data Ingestion:**
- Accept a target time series $Y$ and a matrix of control time series $X$.
- Enforce pre/post split at the intervention date.
- Validate alignment of time indices.

**Step 2 — Model Specification:**
- Build the state vector $\alpha_t$ by stacking chosen components (trend, seasonality, regression).
- Construct system matrices $Z_t$, $T_t$, $R_t$, $H_t$, $Q_t$ for the chosen specification.
- Initialize diffuse priors on the initial state.

**Step 3 — Pre-Period Training (Gibbs Sampler):**
- Run the full BSTS Gibbs sampler on pre-intervention data only.
- At each iteration: FFBS → variance sampling → Spike and Slab coefficient sampling.
- Store posterior draws of $\boldsymbol{\beta}$, $\boldsymbol{\gamma}$, variances, and state paths.
- Verify convergence via $\hat{R}$ and trace plots before proceeding.

**Step 4 — Counterfactual Projection:**
- For each posterior draw of model parameters, run the Kalman Filter forward into the post-period with observed $y_t$ withheld (missing data mode).
- Each draw produces one sample counterfactual path $\hat{Y}_t(0)$.
- The ensemble of draws gives the full posterior predictive distribution over the counterfactual.

**Step 5 — Causal Effect Extraction:**
- **Pointwise Impact:** $\tau_t = Y_t - \hat{Y}_t(0)$ at each post-period time step $t$.
- **Cumulative Impact:** $\sum_{t=T_0+1}^{T} \tau_t$ — the running total.
- **Credible Intervals:** Extract 2.5th and 97.5th percentiles across MCMC draws for each quantity.
- **Tail-Area Probability:** Compute $P(\text{cumulative effect} > 0 \mid \text{data})$ from the posterior sample.

**Step 6 — Validation:**
- Run placebo tests on in-sample pre-period windows.
- Check posterior predictive fit in the pre-period.
- Compare output to `CausalImpact` package on a shared dataset to verify correctness.

**Step 7 — Reporting:**
- Generate the three canonical summary plots:
  1. Original vs. Counterfactual (with posterior credible band)
  2. Pointwise Impact over time
  3. Cumulative Impact over time
- Output a summary table: estimated effect, 95% CI, tail-area probability.

---

## Phase Summary & Milestone Checklist

| Phase | Key Milestone |
|---|---|
| Phase 1 | Can derive the Normal-Normal posterior update by hand. Understand MVN conditional distributions. |
| Phase 2 | Can implement Gibbs sampling for Bayesian linear regression from scratch. |
| Phase 3 | Can implement a Kalman Filter and FFBS on a Local Level Model. Counterfactual forecast via missing data. |
| Phase 4 | Can run a full BSTS Gibbs loop with Spike and Slab on synthetic data. Diagnose convergence. |
| Phase 5 | Custom package produces output that matches `CausalImpact` on a held-out validation dataset. |

## Coding Exercises Summary

| Exercise | After Chapter | Task |
|---|---|---|
| 1 | Ch. 4 — Conjugate Priors | Normal-Normal Bayesian updating; plot posterior evolution with each new observation |
| 2 | Ch. 6 — Bayesian Linear Regression | Gibbs sampler for Bayesian regression; compare to OLS; posterior predictive intervals |
| 3 | Ch. 8 — Kalman Filter | Kalman Filter + RTS Smoother on synthetic data; counterfactual forecast via withheld post-period |
| 4 | Ch. 11 — BSTS Gibbs Sampler | FFBS on Local Level Model; 1,000 state path draws; compare mean to RTS estimate |
| 5 | Ch. 13 — Capstone | Full package; validate against `CausalImpact` on real dataset |

## Resources

### Causal Impact
1. [Causal Impact Paper - Brodersen et al. (2015)](https://projecteuclid.org/journals/annals-of-applied-statistics/volume-9/issue-1/Inferring-causal-impact-using-Bayesian-structural-time-series-models/10.1214/14-AOAS788.full) — the primary technical paper.
2. [Causal Impact YouTube demo](https://www.youtube.com/watch?v=GTgZfCltMm8) 
3. [CausalImpact R package GitHub](https://github.com/google/CausalImpact) — reference implementation.

### Bayesian Statistics
1. Bayesian Statistics the Fun Way [book](https://www.amazon.com/dp/B07J461Q2K?ref=cm_sw_r_ffobk_cp_ud_dp_JXKX7NGTARGC7W81DJWR&ref_=cm_sw_r_ffobk_cp_ud_dp_JXKX7NGTARGC7W81DJWR&social_share=cm_sw_r_ffobk_cp_ud_dp_JXKX7NGTARGC7W81DJWR&bestFormat=true&previewDoh=1) (Will Kurt)
2. Bayesian Statistics for Beginners [book](https://www.amazon.com/dp/B083FYDBGZ?ref=cm_sw_r_ffobk_cp_ud_dp_DBCM0M833P7T2KASXRRT&ref_=cm_sw_r_ffobk_cp_ud_dp_DBCM0M833P7T2KASXRRT&social_share=cm_sw_r_ffobk_cp_ud_dp_DBCM0M833P7T2KASXRRT&bestFormat=true&previewDoh=1) (Donovan and Mickey)
3. Doing Bayesian Data Analysis [book](https://a.co/d/7l2mfs1) (John Kruschke)
4. Think Bayes - Allen Downey [online book](allendowney.github.io/ThinkBayes2)

### State Space Models and Kalman Filter
1. Kalman and Bayesian Filters in Python - Roger Labbe [online book](https://github.com/rlabbe/Kalman-and-Bayesian-Filters-in-Python)
2. Forecasting: Principles and Practice [online book](https://otexts.com/fpp3/)

### BSTS and MCMC Computation
1. Bayesian Methods for Hackers - Cam Davidson-Pilon [online book](https://github.com/CamDavidsonPilon/Probabilistic-Programming-and-Bayesian-Methods-for-Hackers)
2. [Scott & Varian (2014)](https://people.ischool.berkeley.edu/~hal/Papers/2013/pred-present-with-bsts.pdf) - reference for BSTS pkg

### Causal Inference
1. Causal Inference for the Brave and True — Matheus Facure Alves [online book](https://matheusfacure.github.io/python-causality-handbook/landing-page.html)
2. Causal Inference: The Mixtape — Scott Cunningham [online book](https://mixtape.scunning.com/)