# Chapter 2: Bayes’ theorem

Alright! Now that we have some understanding of probability, let’s get into one of the most important theorems!

The almighty Bayes’ theorem!

Bayes theorem is based on conditional probability. It allows us to find the probability of event A happening, given we know event B happened.

Let’s see an example to understand the core concept!
## Example 1: Sorting the Staff
Q) There are 20 employees at a company, 12 of whom are female.
There are 10 PhDs, of which 6 are female.
What is the probability of being a female given they have a PhD?

We can solve this directly using Bayes’ theorem (revealed later), but first let’s understand how to solve this in a simple table format.

Based on the data given to us we can create the following table:
| | Female | Male | Total
|:--:| :-------:|:------:|:------:|
**PhDs**|6|?|10 |
**Not PhDs**|?|?|? |
**Total**|12|?|20 |

Our job is to find the missing data values. As you can see this is fairly easy to solve


| | Female | Male | Total
|-| -------|------|------|
**PhDs**|6|4|10 |
**Not PhDs**|6|4|10 |
**Total**|12|8|20 |


Given just this table, we can solve the question! \
We are asked what is P(F|PhD)? \
The total number of PhDs = 10. Of which the number of females = 6.\
Thus, the universe size is 10 and we want to find the probability of being female. That equates to 6/10

We can arrive at the same conclusion in a different way using conditional probabilities which is given as,

$P(A|B) = \frac{P(A ∩ B)}{P(B)}$ 

Substituting the above equation with our example we get,

P(F|PhD) = $\frac{P(F ∩ PhD)}{P(PhD)}$ 

Translating the above equation in English you will see it matches our earlier statement: \
From the universe of PhDs (denominator) we want to find all those that are female AND have a PhD (numerator).

Let’s find each of the elements in this equation: \
First, let’s get the probability of being a female on this team. This is known as marginal probability. 

For P(F) we find total number of females and divide by total count.\
P(F) = $\frac{12}{20} = \frac{3}{5}$ … marginal probability

Similarly, the probability of having a PhD is,\
P(PhD) = $\frac{10}{20} = \frac{1}{2}$ 						… marginal probability


Next, to find the probability of being female and having a PhD,  
find the cell that corresponds to female and PhD (= 6), divide this with the total number (=20). 
This is known as the joint probability.\
P(F ∩ PhD) = $\frac{6}{20} = \frac{3}{10}$  ... joint probability

Now that we have all these elements in place we can find P(F | PhD). This is known as conditional probability and is given by,\
P(F|PhD) $= \frac{P(F ∩ PhD)}{P(PhD)}$ 

P(F|PhD) $= \frac{3/10}{1/2} = \frac{6}{10}$  ... conditional probability

This matches the answer we had seen earlier. This was just a different way to get to it.

## Deriving the Bayes Theorem
Okay, now that we have seen that example let’s look at some equations and derive the Bayes theorem

In mathematical form, the conditional probability is given as:

$P(A|B) = \frac{P(A \cap B)}{P(B)}$ ... eqn(1)

i.e. The probability of A given B is the probability that A and B happen together relative to the probability that B happens at all.
