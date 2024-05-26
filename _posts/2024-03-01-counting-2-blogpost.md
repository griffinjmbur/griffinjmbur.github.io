---
title: "Counting for adults II: the binomial and multinomial distribution"
date: 2024-03-01
---

In this post, I introduce two very useful distributions in statistics, the binomial and multinomial distribution. What follows is not a full lecture on these topics; it merely introduces what is most important about these quantities for the ongoing series in survey statistics. 

# The uses of combination in statistics

In probability and statistics, we often deal with distributions of variables that are hard to summarize. However, the binomial and multinomial coefficients allow us to directly characterize some very important probability distributions without even needing calculus. By the way, note that there are plenty of situations, not that directly relevant here, where we *do* use these counting techniques to work out probabilities. 

## Binomial random variables 

A variable is *binomially distributed* if it is a count of some outcome which can be broken down into the sum of one binary or Bernoulli trial repeated many times. For example, the count of people in a sample who are married is the result of drawing \\(n\\) people who could each be married or not (notice that here I am using the traditional conception of an infinite population and, relatedly, one where someone's status is not considered fundamentally a constant as in the Cornfield model above). The count of heads in \\(n\\) flips of a fair coin is a simpler, if less interesting, example. 

Note that the binomial distribution is thus, for the most part, a sampling distribution: it describes group-level outcomes (although it is often true that we can telescope statistics in both directions: we might consider groups to be elements or consider even someone's age to be a binomial variable counting their number of birthdays). 

## The binomial distribution: PMF, expectation, variance

If we denote by the random variable \\(X\\) the number of successes in \\(n\\) trials, the probability mass function (pmf) is given by \\(\mathbb{P}[X=k] = \binom{n}{k}\pi^k(1-\pi)^{n-k}\\). The reasoning is as follows. The probability of any specific arrangement of independent trials resulting in \\(k\\) successes and \\(n-k\\) failures (e.g., \\(HHHHTTTTTT\\) to get four heads in 10 flips) is written in easiest form as \\(\pi^k(1-\pi)^{n-k}\\). The resulting probability[^caution] is the same for any other arrangement with the same number of successes; so, to sum up the probabilities, we simply ask "how many ways can \\(n\\) trials result in \\(k\\) successes?", and the answer is, of course, \\(\binom{n}{k}\\). 

[^caution]:  Note that in this case, since the probability of success and failure are equal, *all* sequences are equiprobable, but this won't be true in general. 

The expected value of a binomially-distributed variable is simple: we sum together \\(n\\) Bernoullis, each with expected value \\(\pi\\) (discussed in the main text), so it is \\(n\pi\\). The variance of such a variable is also simple; per the Köning-Huygens formula, we merely have the sum of the individual variances (as the trial outcomes have no covariance), i.e. \\(n \pi(1-\pi)\\), where the variance of a single Bernoulli has been discussed above.  

### Covariance between any two sums of binomials derived from a random vector \\(\mathbf{Y}\\)

In the situation at hand, we are considering \\(N\\) binomial random variables representing the number of times that each person in our universe of \\(N\\) people is included in a sample with replacement. Each of those \\(N\\) variables is the sum of \\(n\\) binary random variables, each representing whether some specific person was selected into the sample for some specific trial; all such \\(Nn\\) binary variables are independent of one another. 

However, the covariance of two binomial random variables representing the number of times persons \\(i\\) and \\(j\\) respectively were selected is *not* zero: intuitively, the more times a set of \\(n\\) trials results in person \\(i\\) being selected, the fewer times that it results in person \\(j\\) being selected.

The covariance of any two of the binomial variables above can be developed as follows. We wish to find the covariance between two binomial variables \\(T_i\\) and \\(T_j\\), where the subscripts represent individuals. Our first step below is to write each binomial random variable *as* a sum of \\(n\\) Bernoullis, with the trials indexed by \\(k\\) and \\(r\\). 

To complete this proof, we just need one more intuitive property that is worth knowing. 

Assume that the random variables \\(T_i\\) and \\(T_j\\) are each the sum of some \\(n\\) variables \\(T_{ik}\\) and \\(m\\) variables \\(T_{jr}\\), respectively. Note that in the step where \\(\mu_{T_i}\\) is replaced by \\(\sum_k \mu_{T_{ik}}\\) is justified by the linearity of the mean: while we typically should not take an average of averages, this is allowed when each average is calculated over the same number of observations. When we are dealing with an abstract population of people, we can say that the true mean of, say, mothers' and fathers' education is indeed the mean of the individual means for each parent, even if it would be unwise (most likely) to calculate it this way in the sample (the missing observations for each parent would probably be different in number). 

$$
\begin{align*}
\mathbb{COV}[T_i, T_j] &= \mathbb{COV} \left\{ 
    \sum_{k=1}^n T_{ik}, \sum_{r=1}^m T_{jr} \right\} \cr
&= \mathbb{E}[(T_{i1} + T_{i2} + ... T_{in} - \mu_{T_i})
    (T_{j1} + T_{j2} + ... T_{jm} - \mu_{T_j})] \cr
&= \mathbb{E} \left\{ (\sum_k T_{ik} - \sum_k\mu_{T_{ik}})
    (\sum_r T_{jr} - \sum_r\mu_{T_{jr}}) \right\} \cr
&= \mathbb{E} \left\{ \sum_k (T_{ik} - \mu_{T_{ik}})
    \sum_r (T_{jr} - \mu_{T_{jr}}) \right\} \cr
&= \sum_k \sum_r \mathbb{COV}[T_{ik}, Z_{jr}]
\end{align*}
$$

Now we can resume the proof of the covariance of two binomial variables which each represent the number of times persons \\(1\\) and \\(2\\) are selected. Again, I'm using the subscripts \\(1\\) and \\(2\\) for the sake of not having multiple abstract indices. 

$$
\begin{align*}
&1. \mathbb{COV}[T_i, T_j] = \mathbb{COV} \left\{ 
    \sum_{k=1}^n T_{ik}, \sum_{r=1}^n Y_{jr} \right\} \cr
&2. = \sum_{i=1}^n \sum_{j=1}^n \mathbb{COV}[T_{ik}, T_{jr}] 
    && \text{see above} \cr
&3. =\sum_{k=r}^n \mathbb{COV}[T_{1i}, T_{2j}]  
    && \text{trials are independent, so no covar.} \cr
&4. =\sum_{k=1}^n \mathbb{E}[T_{ik} T_{jk}] - \mathbb{E}[T_{ik}]\mathbb{E}[T_{jk}] 
    && \text{definition of covariance, simplification of summation index} \cr
&5. = -\mathbb{E}[T_{ik}]\mathbb{E}[T_{jk}] 
    && \text{outcomes of trials are mutually exclusive} \cr
&6. = -n \pi_i\pi_j = -n \left\{ \frac{1}{N} \right\}^2
    && \text{expectation of a Bernoulli; last equality true with epsem} \cr
\end{align*}
$$

## Multinomial random variables

The remaining material here is not necessary to what comes above, but it is a case where we "might as well" round out some useful remaining properties. 

If we collect the results of multiple binomial variables into a single vector, we refer to this as a *multinomial* variable. For example, if we're considering rolling a six-sided die \\(n\\) times, the number of times each face is rolled can be represented by a binomial variable taking a value between between \\(0\\) and \\(n\\); the random vector \\(\mathbf{T} \in \mathbb{N}^6\\) collects these six random variables, each of which is a binomial random variable.

More sociologically, we might be interested in the PMF of the number of occurrences of married, divorced, separated, single, and widowed people; this yields a vector of five binomial random variables. Or, more technically, we might be interested in the number of times \\(N\\) people in the population show up in a sample, as noted above; this is a (massive) random vector of \\(N\\) binomial variables.

## The multinomial coefficient

The multinomial distribution is essentially an extension of the binomial distribution. We can quickly extend the binomial coefficient \\(\rightarrow\\) binomial PMF analogy. Recall the binomial PMF, i.e. \\(\mathbb{P}[Y=k] = \binom{n}{k} \pi^k (1-\pi)^{n-k}\\). This says that the probability that a random count variable \\(Y\\) takes on \\(k\\) successes is the probability of success to the \\(k\\), multiplied by the probability of failure to the \\(n - k\\), mulitplied the number of ways to select \\(k\\) of the \\(n\\) trials to be successes. We basically find the probability of a specific permutation of trials that are successes and failures (the probability of any such permutation is identical, so we simplify and find the probability that the first \\(k\\) were successes); there are actually \\(\binom{n}{k}\\) ways for that to occur, so we just multiply the probability of one way by \\(\binom{n}{k}\\). 

## The multinomial distribution's PMF

The multinomial distribution, from here, is a very simple combination of the ideas above: 

$$
\begin{align*}
\mathbb{P}[\mathbf{\vec{Y}}=\mathbf{\vec{y}}] = \frac{n!}{\prod_{j=1}^m k_j!} \prod_{j=1}^m \pi_j^{k_j}
\end{align*}
$$

We read this as saying that the probability that the random vector \\(\mathbf{Y}\\), with \\(m\\) outcomes, has \\(k_1\\) successes for outcome \\(1\\), \\(k_2\\) successes for outcome \\(2\\), ... \\(k_m\\) successes for outcome \\(m\\) (note that each of these outcomes is a marginally-binomial RV) is the probability of any specific sequence of \\(k_1\\) successes on the first outcome ... and \\(k_m\\) successes on the \\(m\\)th outcome, multiplied by the the number of ways to pick \\(k_1\\) outcomes to take the first outcome, ... and \\(k_m\\) outcomes to be outcome \\(m\\). Again, we basically have the task of finding the probability of any particular sequence of the \\(m\\) different counts of successes times the number of ways that this specific distribution of successes could come about. 

## Examples of multinomially-distributed variables

Let's examine a couple of examples since the above discussion might be a little too abstract on its own. Suppose we draw a sample of \\(100\\) people and ask them their marital status. We are interested in the following set of outcomes \\(j\\) with population probabilities given on the right:


$$
\begin{array}{|c|c|c|}
\hline
j & {k_j} & \pi_j \\
\hline
\textbf{married} & 4 & 0.45 \\
\hline
\textbf{widowed} & 1 & 0.05\\
\hline
\hline
\textbf{divorced} & 1 & 0.15 \\
\hline
\textbf{single} & 3 & 0.25\\
\hline
\textbf{separated} & 1 & 0.1 \\
\hline
\end{array}
$$

First, we calculate the probability of any specific set of trials resulting in this outcome. This specific way of calculating it implies that we observed all successes for each class in order: first, the four married people, then the one widowed person, etc. But, note that this calculation represents the probability of any sequence of independent trials that results in these *sums* of people in each class.

$$
\begin{align*}
\prod_{j=1}^m \pi_j^{k_j} &= 0.45^{4} \cdot 0.05^1
    \cdot 0.15^{1} \cdot 0.25^{3} \cdot 0.1^1
\end{align*}
$$

This number is very small so we won't directly calculate it for the sake of precision. Now, we consider the following problem. What if we had observed the sequence in, say, exactly reverse order? In fact, most of the typical sequences we would observe could happen in more than one way (an exception would be a bizarre situation such as getting \\(10\\) widowers). 

So, one way to think about this is to imagine that we have \\(10\\) marbles laid out which represent our ten people, colored so that each status is associated with a different color. The reason for this conceit is that since we are only interested in marital status here, we want it to be clear that people are interchangeable to the extent that they share a marital status here. Now, it is more obvious that we should find all the unique sequences of those \\(10\\) marbles but then divide by the number of spurious rearrangements in each group (e.g., swapping two red marbles; whether or not the human *could* distinguish them, we are here saying that *by construction* we do not wish to distinguish them). This is given by...

$$
\begin{align*}
\frac{10!}{4!1!1!3!1!}
\end{align*}
$$

Now, if we multiply the two together, we get the small but non-negligible probability of this outcome, \\(\mathbb{P}=0.012\\). 

Finally, here is how this applies to the unusual situation of selecting people or clusters into the sample in the first place, a problem relevant to sampling design.

First, let \\(T_i\\) be a random variable representing the number of times cluster \\(i\\) is selected and \\(t_i\\) be some specific number of times. To work out the probability of any particular configuration where \\(T_1 = t_1, T_2 = t_2, ... T_N = t-N\\), we multiply out the probability of selecting the first cluster \\(t_1\\) times, \\(\pi_1^{t_1}\\)..times the the probability of selecting the \\(N\\)th cluster \\(t_N\\) times, \\(\pi_N^{t_N}\\). *This provides the probability of drawing this exact sequence of clusters with replacement*.  Notice that many of our probabilities will be taken to the power of \\(0\\), thus working out to \\(1\\), since many clusters will not appear in the sample at all. 

Then, secondly, we consider the possibility of getting these exact numbers of successes for each cluster \\(1, 2, ... N\\). You might want to consider the marbles example again: suppose that for each cluster, we have a distinctive hue/swirl pattern of marble. Now, we have \\(n!\\) rearrangements possible physically. Let's be *very* clear about what this represents: a color of marble represents a cluster, and a spot on the table represents a trial. If we simply count all the permutations, we are taking as *valid* the difference between cluster \\(i\\) being selected for trials, say, \\(j, k, r\\) and...well, the same thing, but with the order of the marbles changed. *This does not actually correspond to anything we care about*, so we divide through by the number of ways to rearrange \\(t_i\\) copies of each cluster, \\(\prod_{i=1}^N t_i!\\)&mdash;but, note again that many of these factorials will be \\(0!\\) since many clusters will not appear even once. 

### Some subtleties in the interpretation of multinomial coefficients

So, notice here that it is indeed, as mentioned in "Counting I", very often useful to think about the binomial and multinomial coefficients as giving the number of possible sequences of classes&mdash;here, the classes are just people, but since we can observe someone multiple times, it makes sense to think about a person as a class and one specific instance of them being chosen as an indistinguishable element drawn from this class. 

In fact, it is sometimes almost difficult to *avoid* this interpretation. E.g., when I teach the binomial distribution, talented students often balk at the use of the binomial coefficient to count the number of ways for \\(k\\) trials out of \\(n\\) to be successes because this appears to be using a *combination* to count a *permutation*. But, on the other hand, there is some confusion about the fact that we are, after all, numbering the trials, so perhaps a permutation *is* appropriate.

Well, in fact, a permutation of classes is precisely one way to think about what combinations count. We care about *order* to the extent that trial \\(1\\) is not trial \\(2\\), but we do not care about any possible *re*-ordering of the trials, something like "what if trial \\(1\\) and trial \\(2\\) could swap places", which is what we would be counting if we did not divide by \\(k_1!k_2!...k_m!\\). In fact, we could just give the trials letter names, and there would be no difference in how we should count the quantity of interest. In other words, we care about order here to the extent that something like "trials \\(5, 8, 13\\)" is a *class* of objects. This provides the link between combinations as permutations of classes and combinations as assignments to classes. Sometimes a class can be thought of as a set of spots in a sequence. However, it is not a *true* permutation in that some possible permutations are not counted(swapping the order of items in the same class is not counted). 

# Hypergeometric random variables

Many times in sociology, we use the binomial distribution to model situations where we in fact sample people *without* replacement. This makes the use of the binomial pmf technically incorrect since the probability of a success changes with each draw, and the distribution is actually one known as the *hypergeometric*. However, this approximation is often a very useful shortcut. If we have a large population, removing one person from it hardly changes the probability of a success or failure. Furthermore, calculating exact probabilities is often challenging. 

That said, I will show the formula for the hypergeometric. It is very simple. If we let \\(N\\) and \\(n\\) index the population and sample number of individuals and \\(K\\) and \\(k\\) index the population and sample number of successes, we simply find the number of ways to have \\(k\\) successes and \\(n-k\\) failures directly and then divide by the total number of events. 

$$
\begin{align*}
\mathbb{P}[X=k] = \frac{\binom{K}{k} \binom{N-K}{n-k}}{\binom{N}{n}}
\end{align*}
$$

It is instructive to compare this to our binomial pmf. In that setting, we calculate the probability of any possible sequence of successes on \\(n\\) trials and only directly count the number of possible group-level permutations of \\(k\\) successes and \\(n\\) failures, which usually is not prohibitive computationally. In the hypergeometric setting, however, we directly calculate the number of possible samples from populations of size \\(N, K\\) and \\(N-K\\), which is, of course, impossible computationally in most large populations. 


```python
import math

binom = math.comb(20, 11) * (0.6)**11 * 0.4**9
hypergeom = math.comb(60, 11) * math.comb(40,9) / math.comb(100, 20)
print(f"The binomial probability is {binom}, while the hypergeometric probability is {hypergeom}")
```

    The binomial probability is 0.15973847800416832, while the hypergeometric probability is 0.1748329213409969

