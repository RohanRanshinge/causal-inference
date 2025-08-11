# Chapter 5: Conjugate Priors
Earlier we covered a few distributions. In this section we will see how we can actually apply them to solve problems!  
Remember the Bayes theorem we had seen earlier?  

$PosteriorProbability =\frac{Likelihood * Prior}{Normalized Probability}$  
Therefore,

$Posterior Probability  ∝   Likelihood * Prior$ 

## What is a Conjugate Prior?
A conjugate prior is a special prior distribution that when multiplied with the likelihood function, results in a posterior distribution that belongs to the same family as the prior.
* This allows us to easily incorporate new data, as the new posterior can serve as the prior for the next update.

This special prior is a conjugate only for the specified likelihood function.

## Why do we need conjugate priors?
* Conjugate priors are a shortcut that allow us to directly find the posterior distributions without needing to integrate the denominator in the Bayes Theorem. 
* The denominator normalizes the data. It accounts for the prior * likelihood for all possible hypotheses and then sums them. 
* This integration is very hard and sometimes impossible. 
* To solve this we use conjugate priors and when that fails we use MCMC simulations. 

For example, let's say we are observing coin flips and we have some prior distribution which is of the form $p^a * (1-p)^b$  
We flip a coin and get heads. This follows the bernoulli distribution: $p^k *(1-p)^{1-k}$  
When we multiply the prior with the likelihood we get: $p^{k+a} * (1-p)^{1-k+b}$

Notice the resulting posterior function is exactly similar to the prior we had defined earlier. 
* If  you recall the beta distribution we had seen earlier you will notice that it exactly has the mathematical form of the prior defined! 
* The Bernoulli distribution also is a special case of the binomial distribution!

Thus we can combine the two distributions to get a **beta-binomial conjugate**!
