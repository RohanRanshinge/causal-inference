# Zero to Hero in Causal Inference
In my previous role, I was once asked to find the lift associated with a promotional event. The promotion had already run on multiple products nationwide and I was asked to find the effectiveness of it retrospectively.

As I researched the best way to solve this, I came across the package CausalImpact() and the world of Causal Inference, Bayesian Statistics and Bayesian Structural Time Series models. As these fields were a bit foreign to me, I did what any sane person would do - just run the package and move on. 
 
## What is this Causal Impact?
CausalImpact is a package created by researchers at Google, that enables you to do lift analysis in the absence of A/B tests. It creates counterfactual predictions and estimates the lift, along with generating useful charts and reports. It’s very easy to implement and requires executing only four lines of code. I was very fascinated by this package and realized if I was to truly understand it, I needed to have a deeper look under the hood. 

## But why bother learning the basics when I already had the answer? 
Running the package as a blackbox and not fully understanding it bothered me. I thought I understood the package but in a more real sense I didn’t really understand the package. It still felt a little like magic. And so after much thought, unnecessary planning and a whole lot of procrastination later, I finally decided to spend some time learning the basics.

## The challenges I faced
Causal Impact uses a Bayesian Structural Time Series (BSTS) model to make counterfactual predictions - don’t worry if you don’t understand this yet, I’ll explain this in detail down the road. As I read the technical paper, I came across various fancy mathematical notations and fun words such as **the Kalman filter, posterior distributions, Gibbs sampler, spike and slab priors, MCMC simulations etc.** ; words that seemed frightening and complex! Surely, I needed a PhD to understand this - or at least that’s what I thought. To my surprise, learning about this was actually fun! I read multiple books, watched far too many YouTube videos and signed up for various courses online all the while cursing myself for not starting sooner!

## Why am I writing about this? Why should you care about reading?
I’m no expert in this field and am learning new concepts as I type this. 
Writing forces me to
1. identify gaps in my own understanding (see Feynman technique)
2. see my own progress over the weeks and months and
3. be of use to someone else on the internet (hopefully) who is also interested in Causal Impact but does not know where to start. 
If I can save even one person’s time then it would be fantastic. If not, I get a fun reference to read later on which is still great. Win-win!

## Why listen to me and not an expert?
The thing about experts is, they do not know the problems faced by beginners. My journey stumbling through these various topics might be useful to the next beginner starting here.

**“The master cannot see with the student’s eyes”**

I plan on keeping this a running document. As I progress through this journey I expect to come across various concepts that are familiar and many that are not so. The plan is to cover topics that are directly applicable as well as foundational concepts that - though might not be directly relevant - are still important to improve our understanding of the core subject. I’ll be documenting this as I come across them and will curate my learning plan accordingly. I’ll also be linking all the sources that I find helpful.

And so I invite you on this adventure, to go from a beginner to an expert (sort of), and learn all kinds of fun things along the way!

## Next Steps
To fully understand CausalImpact() there are three big topics we need to cover:
1. Bayesian Statistics
2. Time Series Analysis + State Space Models
3. Causal Inference

In the next post, I’ll dive into the basics of Bayesian Statistics which is foundational and will explain some of the core concepts mentioned earlier!

Alright then, enough with the pitch, let's jump straight into Bayesian Statistics next!

## Recommended Readings
Causal Impact
1. [YouTube demo](https://www.youtube.com/watch?v=GTgZfCltMm8) 
2. Github [repo](https://github.com/google/CausalImpact)
3. Technical [paper](https://projecteuclid.org/journals/annals-of-applied-statistics/volume-9/issue-1/Inferring-causal-impact-using-Bayesian-structural-time-series-models/10.1214/14-AOAS788.full)

Bayesian Statistics
 1. Bayesian Statistics the Fun Way [book](https://www.amazon.com/dp/B07J461Q2K?ref=cm_sw_r_ffobk_cp_ud_dp_JXKX7NGTARGC7W81DJWR&ref_=cm_sw_r_ffobk_cp_ud_dp_JXKX7NGTARGC7W81DJWR&social_share=cm_sw_r_ffobk_cp_ud_dp_JXKX7NGTARGC7W81DJWR&bestFormat=true&previewDoh=1) (Will Kurt)
 2. Bayesian Statistics for Beginners [book](https://www.amazon.com/dp/B083FYDBGZ?ref=cm_sw_r_ffobk_cp_ud_dp_DBCM0M833P7T2KASXRRT&ref_=cm_sw_r_ffobk_cp_ud_dp_DBCM0M833P7T2KASXRRT&social_share=cm_sw_r_ffobk_cp_ud_dp_DBCM0M833P7T2KASXRRT&bestFormat=true&previewDoh=1) (Donovan and Mickey)
 3. Doing Bayesian Data Analysis [book](https://a.co/d/7l2mfs1) (John Kruschke)
