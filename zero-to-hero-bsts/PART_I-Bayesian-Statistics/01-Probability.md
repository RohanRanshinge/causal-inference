# Chapter 1: Probability
We can’t do Bayesian Statistics without learning about probability first! Personally, I never found probability fun. I always struggled with those questions of finding the probability of landing 5 heads consecutively or the probability of picking one ace and two queens from three decks of cards etc. I know some of you may be thinking “oh, that’s so obvious, what’s so hard about probability”. If you are already familiar with probability and are comfortable with it feel free to skip this section, but for those of us that find it challenging, this might be worth a read. (I haven’t gone into great detail here though as the intention is to just introduce the concept).
## Super Basics
Starting from scratch, let’s first define probability: \
Probability is the chance of observing a certain event.

For example, when we toss a coin, how likely is it to come up a head or a tail?
> ½

Or what is the probability of rolling a 3 on a fair six-sided die?
> ⅙

Notation: P(X=3) = ⅙


An important characteristic of probability is that the probabilities of all events should always add to one. 
* ie, if the probability of X being 3 is 1/6 , then the probability of X not being 3 is ⅚ 
    * if P(X=3) = ⅙, then P(X!=3) = ⅚
* Sum(both events) = 1


Probabilities are a way of assigning numbers to a set of mutually exclusive possibilities.

Let’s look at two key terms that will come in handy in the future:
## Odds
Probabilities can also be expressed in terms of odds.

So what does this mean exactly?\
In our previous example of rolling a 3 on a fair six sided die, P(A) = ⅙ \
The odds for event A is then given as

$O(A) = \frac{P(A)}{P(A^c)} = \frac{P(A)}{1-P(A)}$ 

$O(A) = \frac{1/6}{5/6}= \frac{1}{5} $

This can also be written as 1:5 (or 5:1 odds against). \
So if an event has a ⅙ probability of happening its odds are 1:5


Let’s look at another example? \
Imagine the probability of seeing a shooting star is 3/100 

Therefore, its odds are $O(A) = \frac{3/100}{97/100}= \frac{3}{97}=3:97$

Or the odds against seeing a shooting star are 97:3

Similarly, an event with higher probability of ⅔ has 2:1 odds.

Using this information, we can also back calculate probabilities from odds. \
ie. X:Y odds can be rewritten as $\frac{X}{X+Y}$


In simpler terms, an event with 3:7 odds has a probability of $\frac{3}{10}$

Looking at our previous example of Leicester city winning the Premier League. \
They had a 5000-1 odds against winning, which means odds of winning were 1:5000 

$P(winning) = \frac{X}{X+Y} = \frac{1}{5000+1}$

$P(winning) = \frac{1}{5001}$

## Expected value
In simple terms, the expected value is the **average** of the variable.

In slightly more technical terms, the expected value of a random variable X is the weighted average of values X, where the weights are the probabilities of those values.


It's adding up each possible value, but each value is multiplied by its chance of happening first.

Taking the example of a fair sided die the expected value is \
$E(X) = \frac{1}{6}*(1+2+3+4+5+6) = 3.5$ \
Since each side is equally likely we sum up all the values and divide them by 6.

## Frameworks for Defining Probabilities
There are two main frameworks we need to be aware of for defining probabilities
1. Frequentist framework\
  a. It's all about probability in the long run.  You have a hypothetical infinite sequence and then look at the frequency of outcomes in that hypothetical infinite sequence (if a die is rolled infinitely, probability of rolling a 2 is 1 in 6)
2. Bayesian framework\
  a. Based on personal perspective. Takes into account what you know about a particular problem\

We will be using the Bayesian framework in our journey.
## Recommended Readings
Bayesian Statistics: From Concept to Data Analysis ([Coursera](https://www.coursera.org/learn/bayesian-statistics/home/module/1))
