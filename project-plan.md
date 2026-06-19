# Study Plan: Zero to Hero in Causal Impact

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

### Chapter 4: Conjugate Priors
* **Core Concept:** Solving Bayesian updating exactly using pen and paper when the prior and posterior share the same distribution family.
* **Key Topics:**
    * **Beta-Binomial Conjugacy:** Updating a coin-flip probability.
    * **Gamma-Poisson Conjugacy:** Updating rate parameters.
    * Mathematical proofs of updating equations (adding successes/failures to parameters).

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

### Chapter 6: Bayesian Linear Regression & Feature Selection
* **Core Concept:** Running a linear model with priors and isolating the most predictive control variables from a massive pool.
* **Key Topics:**
    * Setting up Bayesian OLS with priors on coefficients ($\beta$) and variance ($\sigma^2$).
    * **The Sparsity Problem:** Selecting 5 useful control markets out of 100 potential options.
    * **Spike and Slab Priors:** The mathematical framework for feature selection:
        * *Spike:* Point mass at zero (excluding the variable).
        * *Slab:* A wide prior distribution (including the variable).
    * Navigating variable inclusion dynamically using a Gibbs sampler.

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

### Chapter 8: The Kalman Filter and Smoother
* **Core Concept:** Recursively tracking and smoothing hidden states over time.
* **Key Topics:**
    * **The Filter Loop:** Alternating between *Prediction* (moving forward one step in time) and *Update* (correcting the prediction based on the new observation error).
    * **Kalman Gain:** Determining how much weight to give the prediction versus the new noisy measurement.
    * **Kalman Smoothing:** Running the algorithm backward from time $T$ to time 1 to estimate historical states using the full dataset.

---

## Phase 4: Bayesian Structural Time Series (BSTS)
*Goal: Combine structural time-series components with Bayesian feature selection into a unified forecasting engine.*

### Chapter 9: Decomposing Time Series Components
* **Core Concept:** Breaking a time series down into additive state vectors.
* **Key Topics:**
    * **Local Linear Trend:** Modeling shifting levels and slopes over time.
    * **Seasonality:** Modeling evolving seasonal effects (e.g., day-of-week effects) using dummy variables or trigonometric states.
    * **Regression Component:** Adding contemporaneous external predictors (selected via Spike and Slab) directly into the state equation.

### Chapter 10: The BSTS Gibbs Sampler
* **Core Concept:** The master algorithm that ties computation, state tracking, and regression together.
* **Key Topics:**
    * **Forward Filtering Backward Sampling (FFBS):** Efficiently drawing sample paths of hidden states given model parameters.
    * The joint MCMC loop:
        1. Sample hidden states given parameters and coefficients.
        2. Sample structural variances ($\sigma^2$).
        3. Sample regression coefficients ($\beta$) and model inclusion indicators.

---

## Phase 5: Causal Inference & The Capstone
*Goal: Use your custom time-series engine to construct counterfactuals and measure treatment effects.*

### Chapter 11: The Counterfactual Framework & Synthetic Controls
* **Core Concept:** Framing impact analysis as a missing data problem.
* **Key Topics:**
    * The Rubin Causal Model and Potential Outcomes framework.
    * **Synthetic Controls:** Building an optimized, unexposed version of your target market using unaffected control markets.

### Chapter 12: Capstone — Coding Your Custom Package
* **Core Concept:** Writing the pure Python/R code to build the package from scratch.
* **Key Implementation Steps:**
    * **Data Ingestion:** Accepting a target time series $Y$ and a matrix of control time series $X$.
    * **Pre-Period Training:** Fitting the BSTS model (Trend + Seasonality + Regression with Spike & Slab) strictly on data *prior* to the intervention date.
    * **Counterfactual Projection:** Forecasting the model forward into the *post-intervention* period to simulate what *would* have happened without the marketing intervention.
    * **Causal Effect Extraction:**
        * *Pointwise Impact:* Actual minus Counterfactual at each time step $t$.
        * *Cumulative Impact:* The running total of the pointwise impact.
        * *Uncertainty Intervals:* Extracting credible intervals from the distribution of MCMC forecast draws.
    * **Reporting:** Writing functions to generate custom plot summaries (Pointwise, Cumulative, Original vs. Counterfactual).
