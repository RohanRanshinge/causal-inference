# Chapter 2: Bayesian Inference: Priors, Likelihoods and Posteriors
Today’s chapter is a fun one and one on which everything in the future will rely on. We’re covering Bayesian inference!

At its core, Bayesian inference revolves around understanding and quantifying uncertainty. 

It provides a framework for updating our beliefs in light of new evidence (recall the case of the flat tire we had seen in the first chapter).   
Bayes' theorem, is a wonderful mathematical expression that formalizes this process:  
$P(A|B) =\frac{P(B|A) * P(A)}{P(B)}$   
It expresses the conditional probability of event A given that event B has occurred. 

Let's rewrite it in different terms:  
$Posterior probability =\frac{Likelihood * Prior}{Normalized Probability}$ 

## Deconstructing the Equation
* **P(A|B) or Posterior probability:** This represents our updated belief about event A after considering the evidence B. It's the probability we are interested in finding.
* **P(B|A) or Likelihood:** This is the probability of observing the evidence B given that our hypothesis A is true. It measures how well the data supports our hypothesis.
* **P(A) or Prior probability:** This reflects our initial belief about the probability of event A before observing any evidence. This is our existing knowledge or assumptions.
* **P(B) or Normalized probability:** This acts as a normalizing constant, ensuring that the posterior probability distribution is properly scaled. It represents the overall probability of observing the evidence B.
