# Zero to Hero in Bayesian Structural Time Series Models
The first time I came across Bayesian Statistics and Bayesian Structural Time Series models I was completely at sea and had no idea what I was reading. After much thought, unnecessary planning and a whole lot of procrastination later, I finally decided to spend some real time learning about this field. As I went through various sources, I soon came across topics that were deeply fascinating and intellectually stimulating and cursed myself for not starting sooner! I read multiple books, watched far too many YouTube videos and completed various courses on Coursera all in the hopes of learning something of value. As I compiled my material, I thought it would be a shame if all my notes were kept private with me. That’s when I decided to publish them as online articles with the hope that someone else might find them useful! 

And so our adventure begins…

Welcome to the first post of our journey into Bayesian Statistics and Bayesian Structural Time Series (BSTS) models. The objective of this series is to go from a novice to a (kinda) expert in this domain and learn all kinds of fun things along the way. 
## Why learn Bayesian Stats and BSTS models?
Quick story time - At my previous role, I was once asked to find the lift associated with a certain promotional event. This promotion was run on a bunch of products on different dates and offered to all customers nationwide. The promotion was already live and I was asked to find the effectiveness of this promotion retrospectively. 

My first thought was to simply compare the average sales before and after the promotion. However, I quickly realized that this method won’t work. The difference in sales there could be due to natural fluctuations, seasonality or just random noise. How do I confidently say that any difference was truly due to the promotion? 

That’s when I came across **CausalImpact()** created by researchers at Google. CausalImpact is a package that enables you to do lift analysis in the absence of A/B tests. It’s very easy to implement and requires just four lines of code to get your output. I was fascinated by the package and realized that to truly understand it I needed to have a look under the hood.
## The challenges I faced
Causal Impact uses a Bayesian Structural Time Series model to make counterfactual predictions (don’t worry if you don’t understand this yet, I’ll explain all of this down the road). Now I had no idea what BSTS models were and to be honest I was not even sure I knew what Bayesian Statistics really is. As I read the technical paper I came across all sorts of mathematical notations and keywords such as the **Kalman filter, posterior distributions, Gibbs sampler, spike and slab priors** etc etc. Words that I couldn’t figure out for the life of me! I realized that if I wanted to understand the paper and the algorithm, I needed to start from the basics.
## Why am I writing about this? Why should you care about reading?
I’m no expert in this field and am learning everything from scratch. Writing forces me to keep learning and helps me identify gaps in my understanding.
Documenting this journey would be valuable to 
1. see my progress over the weeks and months 
2. be useful to someone else on the internet (hopefully) who also is interested in causal impact but does not know where to start. 

The problem with experts is, they are so knowledgeable, they do not know the issues faced by beginners. If I can save even one person’s time then it would be fantastic, if not, I get a fun reference to read later on which is still great. Win-win!


***“The master cannot see with the student’s eyes”***

As I progress through this journey I expect to come across various concepts that are familiar and many that are not so. The plan is to cover topics that are directly applicable as well as foundational concepts that - though might not be directly relevant - are still important to improve our understanding of the core subject. I’ll be documenting this as I come across them and will curate my learning plan accordingly. I’ll also be linking all the sources that I find helpful.
## Next Steps
To fully understand CausalImpact() there are four big topics we need to cover:
1. Bayesian Statistics
2. Time Series Analysis
3.  State Space Models
4.  Causal Inference

This might sound like overkill but who cares! I’m here to learn and no learning is ever wasted.

In the next post, I’ll dive into the basics of Bayesian Statistics which is the foundation on which everything is built and explain some of the core concepts mentioned earlier!

Alright enough with the pitch, let's jump straight into Bayesian Statistics next!
## Recommended Readings
Causal Impact
1. [YouTube demo](https://www.youtube.com/watch?v=GTgZfCltMm8) 
2. Github [repo](https://github.com/google/CausalImpact)
3. Technical [paper](https://projecteuclid.org/journals/annals-of-applied-statistics/volume-9/issue-1/Inferring-causal-impact-using-Bayesian-structural-time-series-models/10.1214/14-AOAS788.full)

Bayesian Statistics
 1. Bayesian Statistics the Fun Way [book](https://www.amazon.com/dp/B07J461Q2K?ref=cm_sw_r_ffobk_cp_ud_dp_JXKX7NGTARGC7W81DJWR&ref_=cm_sw_r_ffobk_cp_ud_dp_JXKX7NGTARGC7W81DJWR&social_share=cm_sw_r_ffobk_cp_ud_dp_JXKX7NGTARGC7W81DJWR&bestFormat=true&previewDoh=1) (Will Kurt)
 2. Bayesian Statistics for Beginners [book](https://www.amazon.com/dp/B083FYDBGZ?ref=cm_sw_r_ffobk_cp_ud_dp_DBCM0M833P7T2KASXRRT&ref_=cm_sw_r_ffobk_cp_ud_dp_DBCM0M833P7T2KASXRRT&social_share=cm_sw_r_ffobk_cp_ud_dp_DBCM0M833P7T2KASXRRT&bestFormat=true&previewDoh=1) (Donovan and Mickey)
 3. Doing Bayesian Data Analysis [book](https://a.co/d/7l2mfs1) (John Kruschke)
