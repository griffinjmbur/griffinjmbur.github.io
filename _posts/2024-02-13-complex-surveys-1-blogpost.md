---
title: "Complex surveys I: introduction to finite population statistics"
date: 2024-02-21
---

In the following series of posts, I want to provide a short introduction to complex survey design. I have not found, anywhere on the internet, a short (\\(\leq 30\\) page) document that introduces the concepts of stratified and cluster random sampling from a finite population clearly and which derives point estimators for the mean and true and estimated sampling variance of the mean. In principle, all of this can be developed without need of much advanced math, and Kish (1965); Cochran (1977); Särndal, Swensson, and Wretman (SSW) (1992); and Lohr (1999) all provide accessible, good treatments. That said, each graduate-level textbook requires most of its 300 or 400 pages to get users to the point of being able to fully understand a complex survey. For many users, this is simply too much detail, and I have found that each of the textbooks above, while individually useful, typically omits certain important assumptions that one only finds fully spelled out in one of the others. 

# Finite population statistics

I will start with finite population statistics generally, assuming simple random sampling. The big takeaways are relatively straightforward: our sample mean is still unbiased, and its sampling variance is actually reduced in a way that is proportional to the *sampling fraction*, the relative size of a sample to the finite population from which it is taken. 

# Notation

Note that contemporary mathematical norms favor precise notation, something that I like. I lost interest in math after high school calculus, despite doing well in it, until midway through college in part because the increasing handwaving about notation that really ramps up with calculus got to me. I'll write more about this in a future post. For now, let's focus on what this means practically. 

Roman letters like \\(X, Y\\), and \\(Z\\) will indicate random variables, which you can think of as the most abstract form of a property we care about, the very concept itself. A realization of that variable, i.e. the property actually observed in some person or firm or what have you, will look like \\(x, y\\), and \\(z\\) for our variables respectively. Greek letters such as \\(\sigma\\) and \\(\mu\\) will refer to parameters, properties of population that we don't directly observe (hence, *para* + *meter*). I'll try to remind people of this convention and explain any others that I use.

Note, by the way, that the idea of an *abstract representation* of a *specific realization* seems a bit paradoxical. It isn't, but this can cause confusion at first. You might want to think about statistical objects as arranged on a scale from completely concrete (e.g., \\(10\\) might be someone year's of education) to completely abstract (e.g., \\(Y\\) might represent the very concept of education as a variable), with \\(y\\) being the concept of some specific value of education. Subscripts (e.g. \\(y_i\\) or \\(y_1\\)) just indicate that we are considering a specific *person's* education, with \\(i\\) being a index variable and \\(1\\) referring to person \\(1\\) (note that any actual assignment of index numbers to people is also arbitrary). 

# Finite population sampling properties

## The sample mean is unbiased

Let's first prove that the sample mean, \\(\hat{\mu}_Y\\), of a sample from a finite population is unbiased. We can use a new type of proof here (relative to what you might have seen before). 

### An interlude on mathematical expectations and notation

Let's begin with the fact that by definition, the expectation of the sample mean, denoted by \\(\mathbb{E}[\hat{\mu}_Y]\\), is equal to the sum of probability-weighted possible outcomes: \\(\mathbb{E}[\hat{\mu}_Y] = \sum_y \mathbb{P}[Y=y]y \\).

And, while we're at it, we should show a simple but incredibly useful property, that the expectation of the sum of two random variables (or more; extending the proof below by induction is so trivial that it is not worth showing), whether they are dependent or not, is the sum of their expectations, without the need for any kind of fancy convolutions (which we need in order to work out the probability mass function of their sum). In other words, while summing together two random variables might cause the values of the composite variable to become more and less probable in ways that aren't immediately obvious, their expectation is immediately obvious.

For the sake of complete clarity of notation, note that \\(X\\), a capital Roman letter, means a random variable, i.e. a function from outcomes of an experiment to some set of numbers (often the naturals or the reals, \\(\mathbb{N}\\) or \\(\mathbb{R}\\)). The tricky concept of "specific but unspecified values of a random variable"&mdash;the abstract concept of a specific realization of the variable, "some specific outcome that I actually do not find useful to specify here"&mdash;is denoted by the lowercase Roman letter \\(x\\). This represents *some* outcome of \\(X\\) that is possible. An expression such as \\(X=x\\) means "\\(X\\) takes on, in this specific case, the value \\(x\\)". Then, \\(\mathbb{P}[X = x]\\) is the probability of that occurring, while \\(\mathbb{P}[X = x \cap Y = y]\\) is the probability that \\(X = x\\) *and*  that \\(Y= y\\) (the symbol \\(\cap\\) means "intersection of two outcomes"). 

Writing \\(\sum_x\\) without an upper index or a starting value simply means "sum over *all* \\(x\\)", where \\(x\\), again, means "a value of \\(X\\)". I try to be diligent about summation notation and to be as explicit as possible. Smarter people than me argue (e.g. Knuth *et al.*) that the value of summation notation is that it can be made very flexible, but I dislike conventions such as \\(\sum_{X=x, Y=y}\\) since they are only superficially more convenient; the discerning reader will eventually have to work out that this implies a double sum (usually the apparent point of such notation is avoiding things like double or triple summation). At some point soon, I will upload a post going through sums in some detail, but for now, just note that something like \\(\sum_x \sum_y (x+y) \\) simply means "sum over every possible sum of \\(x\\) and \\(y\\)". 

You can visualize this in two ways that are both useful. First, a way that I like to think of as "algorithmic" or "tallying up"&mdash;easier to understand but productive of fewer insights. Here, you fix the value of \\(x\\) and then run through all values of \\(y\\), and then you increment \\(x\\) and repeat until you go through all possible \\(x\\). The second method is more geometric: picture a three-dimensional space where the grid is a Cartesian plane with \\(x\\)- and \\(y\\)-values, and the height above any particular square on the grid is the sum of \\(x\\) and \\(y\\) at that point. Then, you can simply realize that because summing \\(x+y\\) over \\(y\\) doesn't directly involve the value of \\(x\\) (it just adds it to itself as a variable term), you can just do the sum over \\(y\\) and note how many times you add \\(x\\) to itself (this is given by the upper limit of the summation which I here omit). This is like looking "up" the grid at some specific value of \\(x\\), finding out what the \\(y\\)s sum to, and keeping track of how many times we add that fixed value of \\(x\\) to itself. Then, you multiply \\(x\\) by that number and take the sum of this quantity over \\(x\\), which represents looking "to the right" on the grid: we know what happened to \\(x\\) as we went up it and summarized that by multiplying, so now we just add that quantity to itself for however many values of \\(x\\) we have. Finally, we recall that when we do this "looking to the right", we are summing up the component of the height that comes from the \\(y\\)s. We already found out what that comes to when we looked "up", so now we just take that scaled value and multiply it by however many values of \\(x\\) we ran through (the upper index on \\(sum_x\\), assuming that it begins at \\(1\\)). 

$$
\begin{align*}
\mathbb{E}[X+Y] &= \sum_{x} \sum_{y} \mathbb{P}[X = x \cap Y = y] (x + y)
    && \text{definition} \cr
&= \sum_{x} \sum_{y} 
    \mathbb{P}[X = x \cap Y = y] x + \sum_{x} \sum_{y} \mathbb{P}[X = x \cap Y = y] y 
    && \text{distributive rule of algebra} \cr
&= \sum_{x} x \sum_{y} \mathbb{P}[X = x \cap Y = y]  + 
    \sum_{y} y \sum_{x} \mathbb{P}[X = x \cap Y = y]  y 
    && \text{see below} \cr
&= \sum_{x}  x \mathbb{P}[X = x ]  +  \sum_{y} \mathbb{P}[ Y = y]  y 
\end{align*}
$$

The last move is allowed because summing the joint probability of two specific outcomes of \\(X\\) and \\(Y\\) *over* all outcomes of \\(Y\\) (as we do in the first term) just leaves the probability of that specific value of \\(X\\). For example, if we are rolling two dice, one a regular six-sided die \\(X\\) and one a \\(20\\)-sided *D&D* geek \\(Y\\), \\(\sum_y \mathbb{P}[X = 1 \cap Y = y]\\) means summing over all joint probabilities where \\(X=1\\) and \\(Y\\) equals whatever, i.e. 

$$
\begin{align*}
\mathbb{P}[X = 1 \cap Y = 1] +  \mathbb{P}[X = 1 \cap Y = 2] + ...  \mathbb{P}[X = 1 \cap Y = 20]
\end{align*}
$$ 

This is obviously (I hope) just equal to the probability that \\(X=1\\) since there is no possible way to get that outcome outside of the joint outcomes \\([X = 1 \cap Y = 1] \cup [X = 1 \cap Y = 2] ... \cup [X = 1 \cap Y = 20]\\), where \\(\cup\\) means "union", basically a mathematical "inclusive or". Since our events are *disjoint* here (meaning, there is no overlap between each joint outcome, for obvious reasons: \\(X\\) cannot be both \\(1\\) and \\(2\\)), this just means "joint outcome \\(1\\) or \\(2\\) or...".

Now, let's return to the topic at hand. With equal probability sampling, the expectation is equal to the simple average of all possible samples. Let's have \\(S\\) be a random variable representing any possible sample with \\(s\\) being a realization (i.e., some specific sample), avoiding the use of \\(n\\) here since it is the size of each sample. Then, for a finite population, each sample has \\(\mathbb{P}[S=s] = 1/\binom{N}{n}\\), and this constant can be factored out. We'll let "the pretentious *S*", \\(\mathscr{S}\\), represent the universe of all possible samples. 

Then, we have ...

$$
\begin{align*}
\mathbb{E}[\hat{\mu}_Y] &= \frac{1}{N \choose n} \sum_{s \in \mathscr{S}} 
    \hat{\mu}_Y && \text{definition} \cr
&= \frac{n!(N-n)!}{N!} \sum_{s \in \mathscr{S}} \left\{\frac{y_1 + y_2 + ... y_n}{n}\right\} && \text{We write out the sum long-hand}
\end{align*}
$$

Note again that the \\(y\\)s in the second line are *abstract concepts of specific values*. They aren't actual observations.

Each observation in the finite population, \\(y_i \in \mathscr{U}\\), appears in exactly \\(N-1 \choose n-1\\) samples. This is a combinatorial fact: if we insist on \\(i\\)'s inclusion in a given subset of samples, we can pick \\(n-1\\) people to appear in any given sample from a remaining neo-universe of \\(N-1\\) people. Thus, summing all values of all samples means multiplying each *concrete* person's score by \\(N-1 \choose n-1\\). Note that we're already abusing notation here a little bit since now our notation below shows the \\(y\\)s as referring actual members of the universe&mdash;carefully note that the final value is now \\(y_N\\) where it was before \\(y_n\\). Unfortunately, I think it just ends up being clunky to be too precise here. Sometimes a numbered subscript means a specific member of the sample (which itself could be different) and sometimes it means a specific member of the finite population (the numbering could be different, but the members are fixed). 

$$
\begin{align*}
\mathbb{E}[\hat{\mu}_y] &= \frac{n!(N-n)!}{N!} \binom{N-1}{n-1} \frac{y_1 + y_2 + ... y_N}{n} \cr
&= \frac{n!(N-n)!}{N!} \mathbf{\frac{(N-1)!}{(N-1 - [n-1])!(n-1)!}} \frac{y_1 + y_2 + ... y_N}{n} && \text{definition of binomial coefficient} \cr
&= \frac{n!(N-n)!}{N!} \frac{(N-1)!}{(N-n)!(n-1)!} \frac{y_1 + y_2 + ... y_N}{n} 
    && \text{simplification} \cr
&= \frac{n!}{N!} \frac{(N-1)!}{(n-1)!} \frac{y_1 + y_2 + ... y_N}{n} 
    && \text{cancellation} \cr
&= \frac{n}{N} \frac{y_1 + y_2 + ... y_N}{n} 
    && \text{simplification of factorials} \cr
&= \frac{1}{N} (y_1 + y_2 + ... y_N) && \text{simplification} \cr
&= \frac{1}{N} (N\mu_Y) = \mu_Y && \text{definition} \cr
\end{align*}
$$

## The sampling variance of \\(\hat{\mu}_Y\\) is \\((1-f)\frac{\sigma_Y^2}{n}\\)

We can now derive the formula for the variance when sampling from a finite population, which involves the use of the finite population correction (fpc), \\(1 - f = 1 - \frac{n}{N}\\) (\\(f\\) is called the sampling fraction). 

You may want to first recall for comparison how we derive the sampling variance for the infinite population case. 

### The infinite population sampling variance

Assume that \\(Y_1 + Y_2 + ... Y_n = Y\\).

$$
\begin{align*}
\mathbb{V}[\hat{\mu}_{Y}] &= \mathbb{V}[\frac{(Y_1 + Y_2 + ... Y_n)}{n}] 
    && \text{Each trial is an iid random variable} \cr
&= \frac{1}{n^2} \mathbb{V}[(Y_1 + Y_2 + ... Y_n)] 
    && \text{Constants factor out of variances as squares} \cr
&= \frac{1}{n^2} \mathbb{E}[\left\{(Y_1 + Y_2 + ... Y_n) - 
    \mu_{Y} \right\}^2] && 
    \text{Definition of variance} \cr
&= \frac{1}{n^2} \mathbb{E}[\left\{(Y_1 + Y_2 + ... Y_n) - 
    (\mu_{Y_1} + \mu_{Y_2} + ... \mu_{Y_n})\right\}^2] && 
    \text{Expectation is linear} \cr
&= \frac{1}{n^2} \mathbb{E}[\left\{(Y_1 - \mu_{Y_1}) + (Y_2 - \mu_{Y_2}) + ... 
    (Y_n-\mu_{Y_n})\right\}^2] && 
    \text{Commutativity of addition} \cr
&= \frac{1}{n^2} \mathbb{E}[(Y_1 - \mu_{Y_1})^2 + (Y_1 - \mu_{Y_1})(Y_2 - \mu_{Y_2}) + 
    ... \cr
&(Y_2-\mu_{Y_2})^2 + (Y_1 - \mu_{Y_1})(Y_2 - \mu_{Y_2})...] && 
    \text{Polynomial expansion} \cr
&= \frac{1}{n^2} \sum_{i=1}^n \sigma^2_{Y_i} + 2\sum_{i>j} \sigma_{Y_i, Y_j} && 
    \text{Definition of co/variance} \cr
&= \frac{1}{n^2} \sum_{i=1}^n \sigma^2_{Y_i} && 
    \text{Covariance is zero for independent draws} \cr
&= \frac{1}{n^2} n \sigma^2_{Y} && 
    \text{The $Y$s are iid random variables} \cr
&= \frac{1}{n} \sigma^2_{Y} && 
    \text{Arithmetic} \cr
\end{align*}
$$

Now, to move onto the finite population case, we demonstrate that the probability of any individual being selected into the sample, whether we sample with or without replacement. This turns out to be \\(\frac{n}{N}\\), which is obvious when we sample with replacement (which is often the tacit assumption in introductory statistics, with the justification that this leads only to minor bias with sufficiently large populations). But it is also true without replacement!

### The probability of inclusion when sampling from a finite population without replacement

It turns out that any element has probability \\(\frac{n}{N}\\) of being selected into the sample from a finite population. This is obvious when sampling with replacement. We have \\(n\\) mutually exclusive or disjoint events, the selection of *n* elements into our sample. Each time, we have probability \\(\frac{1}{N}\\), so we simply add up the probabilities \\(n\\) times. 

This is also the correct answer for a sample *without* replacement, but this is, in some respects a coincidence since the probability of inclusion *appears* to change each time since the population does (it loses a member). Here is the proof (this is the developed form a proof telegraphed in the classic Kish [1965]). 

$$
\begin{align*}
&1. \text{number of ways to pick any given sample} = {N \choose n} \
    && \text{definition of binomial coefficient} \cr
&2. \text{num. ways select sample definitely incl.} j \
    = {N-1 \choose n-1} && \text{see above} \cr
&3. \mathbb{P}(\text{select element} j) = \frac{\text{step 2}}{\text{step 1}} \
    && \text{probability =} \frac{n_{\text{ways to succeed}}}{n_{\text{outcomes}}} \cr
&4. \mathbb{P}(\text{select element} j) = \frac{\binom{N-1}{n-1}}{\binom{N}{n}} \
    && \text{substitution} \cr
&5. = \frac{\frac{(N-1)!}{(n-1)!(N-1 - [n-1])!}}{\frac{N!}{n! (N-n)!}} \
    && \text{definition} \cr
&6. = \frac{\frac{(N-1)!}{(n-1)! (N-n)!}}{\frac{N!}{n!(N-n)!}} \
    && \text{simplification} \cr
&7. = \frac{\frac{(N-1)!}{(n-1)!}}{\frac{N!}{n!}} \
    && \text{more simplification} \cr
&8. = \frac{(N-1)!n!}{N!(n-1)!} \
    && \text{more simplification} \cr
&9. = \frac{n}{N} \
    && \text{definition of factorials} \cr
\end{align*}
$$

### Cornfield's approach

We use here an elegant argument from Cornfield (1944), which has the bonus of answering a question that even bothers many undergraduates: aren't all populations dealt with by sociology actually finite and therefore no person's income is really a "trial"? The outlines of this answer occurred to me as a graduate student learning statistics belatedly, but I cannot claim to have worked out the logic in any detail. The basic answer is that the random quantity is actually someone's selection into the sample; we do not have to pretend that someone's income is actually unknown until observed (of course, there is also a subjectivist, Bayesian answer to this question).

Cornfield's approach is simple: we write the sample total as a sum over the population, \\(\sum_{i=1}^N A_i y_i \\), where \\(A_i, i \in \{1, 2, ... N\}\\) are dummy random variables such that \\(A_i = 1 \iff i \in S\\) and \\(A_i = 0 \iff i \not\in S\\), where \\(S\\) again indicates the sample set considered as a random vector. In words, this means that a sample total is the result of multiplying the (fixed, non-random) score of each of the \\(N\\) elements in the universe by a dummy random variable. If the dummy is \\(1\\), we multiply their score by one and it is added to the sample total; if it is \\(0\\), their score effectively does not count. The result is that we can write the sample total or sample mean as a sum over all elements of the population. 

We start with two basic results. First, \\(\mathbb{E}[A_i] = \pi_{i} = \frac{n}{N}\\) (the last equality follows only when we have equal probability sampling or *epsem*). 

This is as a consequence the formula which we developed above for the expected value (or expectation):

$$
\begin{align*}
\mathbb{E}[A_i] &= 0\cdot\mathbb{P}[A_i = 0] + 1\cdot\mathbb{P}[A_i = 1] \cr
&= 0(1 - \frac{n}{N}) + 1(\frac{n}{N}) \cr
&= \frac{n}{N}
\end{align*}
$$

Second, the variance of the indicator inclusion variable is, like that of any Bernoulli variable, easily derived from the general variance of binary random variables. 

### The ubiquitous König-Huygens formula for the variance

First, we derive an extremely useful alternative expression for the variance. This does not seem to have an English-language literature name, but I have seen it referred to in French as the König-Huygens formula, and it is helpful to have a name for it, so I will call it that. 

$$
\begin{align*}
            &1. \mathbb{V}[Y] = \mathbb{E}[(Y - \mu_Y)^2] && \text{definition} \cr
            &2. = \mathbb{E}[Y^2 - 2*\mu_Y*Y + \mu_Y^2] && \text{binomial expansion AKA "FOILing"} \cr
            &3. = \mathbb{E}[Y^2] - \mathbb{E}[2*\mu_Y*Y] + \mathbb{E}[\mu_Y^2] 
                && \text{expectation operator is linear} \cr
            &4. = \mathbb{E}[Y^2] - 2*\mu_Y*\mathbb{E}[Y] + \mathbb{E}[\mu_Y^2]
                && \text{constants can be factored out} \cr
            &5. = \mathbb{E}[Y^2] - 2*\mu_Y*\mu_Y + \mathbb{E}[\mu_Y^2]
                && \text{definition of expectation} \cr
            &6. = \mathbb{E}[Y^2] - 2*\mu_Y^2 + \mu_Y^2
                && \text{expectation of a constant is itself; definition of square} \cr
            &7. = \mathbb{E}[Y^2] - \mu_Y^2
                && \text{arithmetic} \cr
            &8. \mathbb{V}[Y] = \mathbb{E}[Y^2] - \mathbb{E}[Y]^2
                && \text{definition of expectation} \cr
\end{align*}
$$
        
Now, we simply plug in our expectations for the particular random variable \\(A\\), letting \\(\pi\\) stand for the *probability of inclusion* (nothing directly to do with circles)\\(: \mathbb{E}[A^2] - \mathbb{E}[A]^2 = \pi - \pi^2 = \pi(1-\pi)\\). The first term remains simply \\(\pi\\) because the expectation of the square of the random variable itself is the weighted mean ofthe square of every outcome, but squaring 0 and 1 leave them unchanged, so the value of every outcome of \\(A\\) is unchanged. But, squaring the expectation itself results in the square of \\(\pi\\). 

### The variance of a sum of random variables (Bienaymé's identity)

Next, we show the formula for the variance of a sum of random variables. Once again, this seems to have a name in the French literature but not in English. This property is easier to refer to without appending someone's name, but for the sake of searchability, I will also refer to it as Bienaymé's identity. Here, \\(Y\\) is a composite variable, created by adding together \\(Y_1, Y_2...Y_k\\). 

$$
\begin{align*}
&1. \mathbb{V}[Y] = \mathbb{E}[(Y - \mu_Y)^2] && \text{definition of variance} \cr
&2. = \mathbb{E}[([Y_1 + Y_2 + ... Y_k] - \mu_Y)^2] && \text{definition of variable 
        Y} \cr
&3. = \mathbb{E}[([Y_1 + Y_2 + ... Y_k] - [\mu_{Y_1} + \mu_{Y_2} + ... 
    \mu_{Y_k}])^2] && \text{expectation operator is linear} \cr
&4. = \mathbb{E}[(Y_1 - \mu_{Y_1} + Y_2 - \mu_{Y_2} + ... Y_k - \mu_{Y_k})^2]
    && \text{commutativity of addition} \cr
&5. = \mathbb{E}[(Y_1-\mu_{Y_1})^2 + (Y_1 -\mu_{Y_1})(Y_2 -\mu_{Y_2}) \ + ... 
    (Y_2-\mu_{Y_2})^2 ... ]
                && \text{multinomial expansion} \cr
&6. = \mathbb{E} \left\{ \sum_{j=1}^k\sum_{i=1}^k (Y_i - \mu_{Y_i}) (Y_j - 
    \mu_{Y_j}) \right\} && \text{generalizing the pattern} \cr
&7. = \sum_{j=1}^k\sum_{i=1}^k \sigma^2_{Y_i, Y_j}
    && \text{definition of (co)variances} \cr
&8. = \sum_{j=1}^k \sigma^2_{Y_j} + 2\sum_{i>j}^k\sum_{j=1}^k \sigma^2_{Y_i, Y_j}
    && \text{each covariance appears twice}
\end{align*}
$$
        
Finally, we move on to Cornfield's proof. First, we introduce a non-standard result (all of the results above have many common applications). This is the covariance of two indicators. First, note that the König-Huygens result applies to covariances, too. 

$$
\begin{align*}
&1. \mathbb{COV}[Y, Z] = \mathbb{E}[(Y - \mu_Y)(Z - \mu_Z] 
    && \text{definition of covariance} \cr
&2. = \mathbb{E}[YZ - \mu_YZ - \mu_ZY + \mu_y\mu_Z] 
    && \text{binomial expansion AKA "FOILing"} \cr 
&3. = \mathbb{E}[YZ] - 2\mu_Y\mu_Z + \mu_Y\mu_Z
    && \text{definition of expectation} \cr
&4. = \mathbb{E}[YZ] - \mu_Y\mu_Z
    && \text{arithmetic} \cr
&5. = \mathbb{E}[YZ] - \mathbb{E}[Y]\mathbb{E}[Z]
    && \text{definition...} \cr
\end{align*}
$$

Now, the covariance of the indicators follows readily, if tediously. 

$$
\begin{align*}
&1. \mathbb{COV}[A_i, A_j] = \mathbb{E}[A_iA_j] - \mathbb{E}[A_i]\mathbb{E}[A_j] 
    && \text{definition of covariance} \cr
&2. = \frac{n}{N}\frac{n-1}{N-1} - \left\{ \frac{n}{N} \right\} ^2
    && \text{probability of j's inclusion given i's is discussed above} \cr 
&3. = \frac{Nn(n-1)}{N^2(N-1)} - \frac{(N-1)n^2}{(N-1)N^2}
    && \text{algebra} \cr
&4. = \frac{Nn(n-1)-(N-1)n^2}{N^2(N-1)}
    && \text{algebra} \cr
&5. = \frac{Nn^2-Nn-Nn^2 + n^2}{N^2(N-1)}
    && \text{arithmetic} \cr
&6. = \frac{-Nn+n^2}{N^2(N-1)}
    && \text{arithmetic} \cr
&7. = \frac{-Nn}{N^2(N-1)} + \frac{n^2}{N^2(N-1)} 
    && \text{arithmetic} \cr
&8. = \frac{-n}{N(N-1)} + \frac{n}{N}\frac{n}{N(N-1)} 
    && \text{arithmetic} \cr
&9. = -\frac{n}{N(N-1)}(1-\frac{n}{N})
    && \text{arithmetic} \cr
&10. \mathbb{COV}[A_i, A_j] = -\frac{n}{N(N-1)}(1-f)
    && \text{definition} \cr
\end{align*}
$$

Intuitively enough, the probability of a given person \\(i\\)'s inclusion in the sample is slightly negatively related to the probability of person \\(j\\)'s inclusion with a finite population. 

Finally, we can now simply realize that the variance of the sample mean is the variance of a sum of random variables.[^notation]

[^notation]: Note that, below, I use the convention, different to Cochran and Lohr, of using \\(\sigma\\) for the population standard deviation instead of \\(S\\). I do so because, in my view, the use of Greek letters permits greater clarity in distinguishing parameters and random variables, and it is jarring in their texts to see Greek letters mostly eschewed but used in certain specific contexts (e.g. \\(\pi\\) for "probability of inclusion"). On some occasions above, the difference between \\(\frac{\sum_{i=1}^N (Y_i-\mu_Y)^2}{N-1}\\) and \\(\frac{\sum_{i=1}^N (Y_i-\mu_Y)^2}{N}\\) becomes important, I will call these the "adjusted" and "unadjusted" forms).

$$
\begin{align*}
&1. \mathbb{V}[\hat{\mu}_Y] = \mathbb{V} \left\{ \frac{\sum_{i=1}^n Y_i}{n} \right\} 
    \cr 
&2. = \frac{1}{n^2} \sum_{i=1}^n \sigma^2_{Y_i} 
    + 2\sum_{i>j}^n\sum_{j=1}^n \sigma^2_{Y_i, Y_j}
    && \text{KH identity, factoring out $n$ } \cr
&3. = \frac{1}{n^2} \sum_{i=1}^N \mathbb{V}[A_iy_i]
    + 2\sum_{i>j}^N\sum_{j=1}^N \mathbb{COV}[A_iy_i, A_jy_j]
    && \text{Cornfield approach} \cr
&4. = \frac{1}{n^2} \sum_{i=1}^N y_i^2\mathbb{V}[A_i]
    + 2\sum_{i>j}^N\sum_{j=1}^N y_iy_j\mathbb{COV}[A_i, A_j]
    && \text{factoring out of co/variance} \cr
&5. = \frac{1}{n^2} \sum_{i=1}^N y_i^2 \frac{n}{N}(1-f)
    -2\frac{n}{N(N-1)}(1-f)\sum_{i>j}^N\sum_{j=1}^n y_iy_j
    && \text{substituting from above} \cr
&6. = (1-f)\frac{n}{N}\frac{1}{n^2} \left\{ \sum_{i=1}^N y_i^2 
    - \frac{2}{N-1} \sum_{i>j}^N\sum_{j=1}^N y_iy_j \right\}
    && \text{factoring} \cr
&7. = (1-f)\frac{1}{nN} \left\{ \sum_{i=1}^N y_i^2 
    - \frac{2}{N-1} \sum_{i>j}^N\sum_{j=1}^N y_iy_j \right\}
    && \text{simplifying} \cr
&8. = (1-f)\frac{1}{nN} \left\{\frac{N-1}{N-1} \sum_{i=1}^N y_i^2 
    - \frac{2}{N-1} \sum_{i>j}^N\sum_{j=1}^N y_iy_j \right\}
    && \text{common denominator} \cr
&9. = (1-f)\frac{1}{nN} \left\{ \frac{N}{N-1} \sum_{i=1}^N y_i^2 
    - \frac{1}{N-1} \sum_{i=1}^N y_i^2 
    - \frac{2}{N-1} \sum_{i>j}^N \sum_{j=1}^N y_iy_j \right\}
    && \text{separating out} \cr
&10. = (1-f)\frac{1}{nN} \left\{\frac{N}{N-1} \sum_{i=1}^N y_i^2 
    + \frac{-1}{N-1} (\sum_{i=j}^N y_iy_j + 
    \sum_{i \neq j}^N\sum_{j=1}^N y_iy_j) \right\}
    && \text{consolidate, rewrite indices} \cr
&11. = (1-f)\frac{1}{nN} \left\{\frac{N}{N-1} \sum_{i=1}^N y_i^2 
    + \frac{-1}{N-1} (\sum_{i=1}^N\sum_{j=1}^N y_iy_j) \right\}
    && \text{consolidate} \cr
&12. = (1-f)\frac{1}{nN} \left\{\frac{N}{N-1} \sum_{i=1}^N y_i^2 
    + \frac{(-N\mu_Y)^2}{N-1} \right\}
    && \text{reverse multinomial expansion} \cr
&13. = (1-f)\frac{1}{n} \left\{\frac{\sum_{i=1}^N y_i^2 
    -N\mu_Y^2}{N-1} \right\}
    && \text{factoring out} \cr
&14. = (1-f)\frac{1}{n} \left\{\frac{\sum_{i=1}^N y_i^2 
    -2N\mu_Y^2 + N\mu_Y^2}{N-1} \right\}
    && \text{add/subtract a constant} \cr
&15. = (1-f)\frac{1}{n} \left\{\frac{\sum_{i=1}^N y_i^2 
    -2\mu_Y\sum_{i=1}^N y_i + \sum_{i=1}^N \mu_Y^2}{N-1} \right\}
    && \text{rewrite one $N \mu_Y$} \cr 
&16. = (1-f)\frac{1}{n} \left\{\frac{\sum_{i=1}^N (y_i
    -\mu_Y)^2}{N-1} \right\}
    && \text{reverse binomial expansion} \cr 
&17. \mathbb{V}[\hat{\mu}_Y] = (1-f)\frac{1}{n} \sigma^2_{Y, \text{adjusted}}
    && \text{recognizing $\sigma^2$} \cr 
\end{align*}
$$

Note that with sampling *with* replacement, the formula is simpler, although the results require good knowledge of the multinomial coefficient and distribution. I restrict a full discussion to a blogpost that I will put up shortly which covers basic combinatorics from the beginning. More important than the mechanics of the multinomial is the new conceptualization. Before, our random variables indicating sample inclusion were dummy (AKA Bernoulli, indicator, binary) variables. Now those random variables are true binomial variables which are the sum of sets of \\(Nn\\) binary variables (\\(N\\) persons who could be in the sample, \\(n\\) trials). We begin from step three above, replacing the \\(A_i\\) with \\(T_i\\). Those \\(T_i\\) have the property that, for epsem sampling, \\(\mathbb{E}[T_i] = n\pi_i = \frac{n}{N}, \mathbb{V}[T_i] = n\pi_i(1-\pi_i) = n\frac{1}{N}(1-\frac{1}{N})\\), and \\(\mathbb{COV}[T_i, T_j] = -n(\frac{1}{N})^2\\). 

$$
\begin{align*}
&1. \mathbb{V}[\hat{\mu}_{\text{Y, repl.}}] = \frac{1}{n^2} \left\{\sum_{i=1}^N \mathbb{V}[t_iY_i]
    + 2\sum_{i>j}^n\sum_{j=1}^n \mathbb{COV}[t_iY_i, t_jY_j]\right\}
    && \text{reconceptualizing an RV per the above} \cr
&2. = \frac{1}{n^2} \left\{\sum_{i=1}^N Y_i^2\mathbb{V}[t_i]
    + 2\sum_{i>j}^n\sum_{j=1}^n Y_iY_j\mathbb{COV}[t_i, t_j]\right\}
    && \text{factoring out of co/variance as appropriate} \cr
&3. = \frac{1}{n^2} \left\{\sum_{i=1}^N Y_i^2 \frac{n}{N}(1-\frac{1}{N})
    -2\frac{n}{N^2}\sum_{i>j}^n\sum_{j=1}^n Y_iY_j\right\}
    && \text{plugging in variances and covariances} \cr
&4. = \frac{1}{n} \left\{\sum_{i=1}^N Y_i^2 \frac{1}{N}(1-\frac{1}{N})
    -2\frac{1}{N^2}\sum_{i>j}^n\sum_{j=1}^n Y_iY_j\right\}
    && \text{factoring out} \cr
&5. = \frac{1}{n} \left\{\sum_{i=1}^N Y_i^2 \frac{N-1}{N^2}
    -2\frac{1}{N^2} \sum_{i>j}^n\sum_{j=1}^n Y_iY_j \right\}
    && \text{simplifying} \cr
&6. = \frac{1}{Nn} \left\{\sum_{i=1}^N Y_i^2 \frac{N-1}{N}
    -2\frac{1}{N} \sum_{i>j}^n\sum_{j=1}^n Y_iY_j\right\} 
    && \text{simplifying} \cr
&7. = \frac{1}{Nn} \left\{\sum_{i=1}^N Y_i^2 \frac{N-1}{N}
    -\frac{1}{N} \sum_{i \neq j}^n\sum_{j=1}^n Y_iY_j\right\} 
    && \text{simplifying fractions and indices} \cr
&8. = \frac{1}{Nn} \left\{\sum_{i=1}^N Y_i^2 
    -N\mu_Y)^2 \right\}
    && \text{multinomial expansion} \cr
&9. = \frac{1}{n} \left\{\frac{\sum_{i=1}^N Y_i^2 
    -N\mu_Y)^2}{N} \right\}
    && \text{bringing N back in} \cr
&10. = (1-\frac{n}{N})\frac{1}{n} \sigma^2_{Y, \text{unadjusted}}
    && \text{clever algebra [see above]} \cr 
\end{align*}
$$
