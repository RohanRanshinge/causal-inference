# Chapter 2: Bayesian Inference: Priors, Likelihoods and Posteriors
Today’s chapter is a fun one and on which everything in the future will rely on. We’re covering Bayesian inference!

At its core, Bayesian inference revolves around understanding and quantifying uncertainty. 

It provides a framework for updating our beliefs in light of new evidence (recall the case of the flat tire we had seen in the first chapter).   
Bayes' theorem, is a wonderful mathematical expression that formalizes this process: 

$P(A|B) =\frac{P(B|A) * P(A)}{P(B)}$   

It expresses the conditional probability of event A given that event B has occurred. 

Let's rewrite it in different terms:  

$Posterior Probability =\frac{Likelihood * Prior}{Normalized Probability}$ 

## Deconstructing the Equation
* **P(A|B) or Posterior probability:** This represents our updated belief about event A after considering the evidence B. It's the probability we are interested in finding.
* **P(B|A) or Likelihood:** This is the probability of observing the evidence B given that our hypothesis A is true. It measures how well the data supports our hypothesis.
* **P(A) or Prior probability:** This reflects our initial belief about the probability of event A before observing any evidence. This is our existing knowledge or assumptions.
* **P(B) or Normalized probability:** This acts as a normalizing constant, ensuring that the posterior probability distribution is properly scaled. It represents the overall probability of observing the evidence B.

## Priors
An important part of the above equation is the prior. The prior is used to quantify our belief even before observing the data. They allow us to use background information to adjust the likelihood.  
There are 2 types of priors:
* **Non-informative priors:**
    * When a prior adds little to no information its called a non-informative prior
    * When a non-informative prior is used, the analyst wants to the posterior distribution to be primarily shaped by the likelihood of the data
    * eg: assigning 1/2 priors to two events (heads or tails). assigning 1/6 priors to a value when rolling a fair six-sided die 
* **Informative priors:**
    * An informative prior is one that adds information
    * When an informative prior is used, the analyst wants the posterior distribution to be shaped by both the likelihood and the prior.

## The Importance of Normalization ie. the denominator
The denominator, plays an important role in normalizing the data. The denominator ensures that the posterior probabilities sum to one. 

Recall the previous cancer example we went through. There, we had two hypotheses based on the question (cancer or no cancer) and we wanted to find the probability of one of the hypotheses given a positive result.  
This is the equation from earlier:  

$P(BC|+) = \frac{P(+|BC) * P(BC)}{P(+)}$

the denominator P(+) expanded to  

$P(BC|+) = \frac{P(+|BC) * P(BC)}{P(+ \cap BC) + P(+ \cap \bar{BC})}$  

$P(BC|+) = \frac{P(+|BC) * P(BC)}{P(+|BC) * P(BC) + P(+|\bar{BC}) * P(\bar{BC})}$

### So what’s going on in the denominator?
In the denominator, we are looking at both hypotheses $P(+ \cap BC)$ and  $P(+ \cap \bar{BC})$ and summing them. ie, we are finding the $likelihood*prior$ for both hypotheses and summing them in the denominator.

### What happens with multiple hypotheses?
The example earlier was easy because we were just interested in two hypotheses (cancer and no cancer) but, there could be situations where there are multiple hypotheses at play.   
In that case, we have to sum over **ALL** hypotheses in the denominator.   
The equation then becomes:  

$P(H_i|data) = \frac{P(data|H_i) * P(H_i)}{\sum_{j=1}^{n}P(data|H_j) * P(H_j)}$

This might look a bit more complex, but the principle remains the same. Here's what each component represents:
* $P(H_i|data)$: This is the posterior probability. It represents the updated belief in hypothesis Hi after considering the observed data. It's what we want to calculate.
* $P(data|H_i)$: This is the likelihood. It's the probability of observing the data given that hypothesis Hi is true. In other words, how well does this hypothesis explain the data?
* $P(H_i)$: This is the prior probability. It represents our initial belief in hypothesis Hi before we saw any data. It's what we thought was true based on existing knowledge.  
* Σ $P(data|H_j) * P(H_j)$: This is the evidence or marginal likelihood. It is the sum across all hypotheses of the product of the likelihood of the data given each hypothesis, and that hypothesis prior. It acts as a normalizing constant, ensuring that the posterior probabilities across all hypotheses sum up to 1.

Calculating the denominator can pose significant challenges, especially when it requires considering evidence across multiple conditions. Addressing these challenges is tricky and sometimes impossible. To solve this, statisticians often use:
* **Conjugate priors:** Using conjugate priors simplifies calculations and avoids direct computation of the denominator. Conjugate priors are mathematical conveniences that we will explore later on.
* **Monte Carlo simulations:** When conjugate priors are not feasible, Monte Carlo simulations provide alternative methods to approximate the posterior distribution and circumvent calculating the complex denominator.

## Bayesian Inference
Bayesian inference revolves around the foundational Bayes' Theorem. At the end of the previous chapter, we encountered the simplified version for two outcomes:

$P(BC|+) = \frac{P(+|BC) * P(BC)}{P(+|BC) * P(BC) + P(+|\bar{BC}) * P(\bar{BC})}$

We also saw how we can generalize this equation for multiple hypotheses. If we have 'n' different hypotheses we want to evaluate. Bayes' Theorem translates to:

$P(H_i|data) = \frac{P(data|H_i) * P(H_i)}{\sum_{j=1}^{n}P(data|H_j) * P(H_j)}$

In simpler terms, the posterior probability (what we're interested in) is proportional to the likelihood times the prior, all divided by the evidence.

By applying this formula, we can calculate the posterior probability for each hypothesis in our set. This lets us quantify how much our belief in each hypothesis should change given the evidence. The hypothesis with the highest posterior probability is considered the most probable given the data.
With multiple hypotheses at play sometimes calculating the denominator can be hard and to solve this we often use conjugate priors or monte carlo simulations.

However, we can compare the posterior probabilities of two or more hypotheses directly without needing to solve the denominator. For instance, comparing hypotheses H1 and H2, we can write:

$\frac{P(H_1|data)}{P(H_2|data)} = \frac{P(data|H_1) * P(H_1)}{P(data|H_2) * P(H_2)}$

Notice that the denominator, which is the evidence term, is the same for both and hence cancels out when performing this ratio. This simplification highlights the core of Bayesian comparison: the ratio of posterior probabilities is directly proportional to the ratio of likelihoods times priors.

The result of this comparison tells us which hypothesis provides a better explanation for the data. A ratio greater than 1 suggests that H1 is more probable than H2, and a ratio less than 1 suggests the opposite.

Bayesian inference provides a powerful framework for reasoning under uncertainty, updating beliefs, and comparing hypotheses.  

That's all for this chapter folks! Next we cover Probability Distributions!

## Skill Tree Extensions: Recommended Readings
1. Bayesian Statistics for Beginners [book](https://a.co/d/0mdF1pj) (Donovan and Mickey)
2. Bayesian Statistics the Fun Way [book](https://a.co/d/dLdYfSo) (Will Kurt)
3. Yudkowsky’s Bayes [article](https://www.yudkowsky.net/rational/bayes)
4. Doing Bayesian Data Analysis [book](https://a.co/d/7l2mfs1) (John Kruschke)
5. Bayesian Statistics: From Concept to Data Analysis ([Coursera](https://www.coursera.org/learn/bayesian-statistics/home/module/1))
