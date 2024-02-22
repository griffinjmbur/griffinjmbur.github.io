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

Let's begin with the fact that by definition, the expectation of the sample mean, denoted by \\(\mathbb{E}[\hat{\mu}_Y]\\), is equal to the sum of probability-weighted possible outcomes: \\(\mathbb{E}[\hat{\mu}_Y] = \sum_y \mathbb{P}[Y=y]y \\).

With equal probability sampling, the expectation is equal to the simple average of all possible samples. Let's have \\(S\\) be a random variable representing any possible sample with \\(s\\) being a realization (i.e., some specific sample), avoiding the use of \\(n\\) here since it is the size of each sample. Then, for a finite population, each sample has \\(\mathbb{P}[S=s] = 1/\binom{N}{n}\\), and this constant can be factored out. We'll let the "pretentious *S*", \\(\mathscr{S}\\), represent the universe of all possible samples. 

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

First, we demonstrate that the probability of any individual being selected into the sample, whether we sample with or without replacement. This turns out to be \\(\frac{n}{N}\\), which is obvious when we sample with replacement (which is often the tacit assumption in introductory statistics, with the justification that this leads only to minor bias with sufficiently large populations). But it is also true without replacement!

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

Cornfield's approach is simple: we write the sample total as \\(n\hat{\mu}_Y = \sum_{i=1}^N A_i y_i \\), where \\(A_i, i \in \{1, 2, ... N\}\\) are dummy random variables such that \\(A_i = 1 \iff i \in S\\) and \\(A_i = 0 \iff i \not\in S\\), where \\(S\\) again indicates the sample set considered as a random vector. In words, this means that a sample total is the result of multiplying the (fixed, non-random) score of each of the \\(N\\) elements in the universe by a dummy random variable. If the dummy is \\(1\\), we multiply their score by one and it is added to the sample total; if it is \\(0\\), their score effectively does not count. The result is that we can write the sample total or sample mean as a sum over all elements of the population. 

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
                && \text{expectation of a constant is just that constant, definition of square} \cr
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

[^notation]: Note that, below, I use the convention, different to Cochran and Lohr, of using \\(\sigma\\) for the population standard deviation instead of \\(S\\). I do so because, in my view, the use of Greek letters permits greater clarity in distinguishing parameters and random variables, and it is jarring in their texts to see Greek letters mostly eschewed but used in certain specific contexts (e.g. \\(\pi\\) for "probability of inclusion"). On the rare occasions where the difference between \\(\frac{\sum_{i=1}^N (Y_i-\mu_Y)^2}{N-1}\\) and \\(\frac{\sum_{i=1}^N (Y_i-\mu_Y)^2}{N}\\) becomes important, I will denote these as \\(\sigma^2_{Y, \text{adjusted}}\\) and \\(\sigma^2_{Y, \text{unadjusted}}\\).

$$
\begin{align*}
&1. \mathbb{V}[\hat{\mu}_Y] = \mathbb{V} \left\{ \frac{\sum_{i=1}^n Y_i}{n} \right\} 
    \cr 
&2. = \frac{1}{n^2} \sum_{i=1}^n \sigma^2_{Y_i} 
    + 2\sum_{i>j}^n\sum_{j=1}^n \sigma^2_{Y_i, Y_j}
    && \text{KH identity, factoring out \\(n\\)} \cr
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
    && \text{rewrite one \\(N\mu_Y\\) as \\(\sum_{i=1}^N y_i\\)} \cr 
&16. = (1-f)\frac{1}{n} \left\{\frac{\sum_{i=1}^N (y_i
    -\mu_Y)^2}{N-1} \right\}
    && \text{reverse binomial expansion} \cr 
&17. \mathbb{V}[\hat{\mu}_Y] = (1-f)\frac{1}{n} \sigma^2_{Y, \text{adjusted}}
    && \text{recognizing \\(\sigma^2\\)} \cr 
\end{align*}
$$

Note that with sampling *with* replacement, the formula is simpler, although the results require good knowledge of the multinomial coefficient and distribution. I restrict a full discussion to the appendix. More important than the mechanics of the multinomial is the new conceptualization. Before, our random variables indicating sample inclusion were dummy (AKA Bernoulli, indicator, binary) variables. Now those random variables are true binomial variables which are the sum of sets of \\(Nn\\) binary variables (\\(N\\) persons who could be in the sample, \\(n\\) trials). We begin from step three above, replacing the \\(A_i\\) with \\(T_i\\). Those \\(T_i\\) have the property that, for epsem sampling, \\(\mathbb{E}[T_i] = n\pi_i = \frac{n}{N}, \mathbb{V}[T_i] = n\pi_i(1-\pi_i) = n\frac{1}{N}(1-\frac{1}{N})\\), and \\(\mathbb{COV}[T_i, T_j] = -n(\frac{1}{N})^2\\). 

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

# Binomial appendix

## Binomial variables 

A variable is *binomially distributed* if it is a count of some outcome which can be broken down into the sum of one binary or Bernoulli trial repeated many times. For example, the count of people in a sample who are married is the result of drawing \\(n\\) people who could each be married or not (notice that here I am using the traditional conception of an infinite population and, relatedly, one where someone's status is not considered fundamentally a constant as in the Cornfield model above). The count of heads in \\(n\\) flips of a fair coin is a simpler, if less interesting, example. 

Note that the binomial distribution is thus, for the most part, a sampling distribution: it describes group-level outcomes (although it is often true that we can telescope statistics in both directions: we might consider groups to be elements or consider even someone's age to be a binomial variable counting their number of birthdays). 

## The binomial distribution: PMF, expectation, variance

If we denote by the random variable \\(X\\) the number of successes in \\(n\\) trials, the probability mass function (pmf) is given by \\(\mathbb{P}[X=k] = \binom{n}{k}\pi^k(1-\pi)^{(n-k)}\\). The reasoning is as follows. The probability of any specific arrangement of independent trials resulting in \\(k\\) successes and \\(n-k\\) failures (e.g., \\(HHHHTTTTTT\\) to get four heads in 10 flips) is written in easiest form as \\(\pi^k(1-\pi)^{(n-k)}\\). That specific probability is the same for any other arrangement; so, to sum up the probabilities, we simply ask "how many ways can \\(n\\) trials result in \\(k\\) successes?", and the answer is, of course, \\(\binom{n}{k}\\). 

The expected value of a binomially-distributed variable is simple: we sum together \\(n\\) Bernoullis, each with expected value \\(\pi\\) (discussed in the main text), so it is \\(n\pi\\). The variance of such a variable is also simple; per the KH formula above, we merely have the sum of the individual variances (as the trial outcomes have no covariance), i.e. \\(n \pi(1-\pi)\\), where the variance of a single Bernoulli has been discussed above.  

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
&= \mathbb{E} \left\{ (\sum_k T_{ik} - \mu_{T_{ik}})
    (\sum_r T_{jr} - \mu_{T_{jr}}) \right\} \cr
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
    && \text{trials are independent, so no cov. for \\(k\neq r\\)} \cr
&4. =\sum_{k=1}^n \mathbb{E}[T_{ik} T_{jk}] - \mathbb{E}[T_{ik}]\mathbb{E}[T_{jk}] 
    && \text{definition of covariance, simplification of summation index} \cr
&5. = -\mathbb{E}[T_{ik}]\mathbb{E}[T_{jk}] 
    && \text{outcomes of trials are mutually exclusive, 
    so \\(\mathbb{E}[T_{ik} T_{jk}]=0\\)} \cr
&6. = -n \pi_i\pi_j = -n \left\{ \frac{1}{N} \right\}^2
    && \text{expectation of a Bernoulli; last equality true with epsem} \cr
\end{align*}
$$

# Multinomial appendix

The remaining material here is not necessary to what comes above, but it is a case where we "might as well" round out some useful remaining properties. 

If we collect the results of multiple binomial variables into a single vector, we refer to this as a *multinomial* variable. For example, if we're considering rolling a six-sided die \\(n\\) times, the number of times each face is rolled can be represented by a binomial variable taking a value between between \\(0\\) and \\(n\\); the random vector \\(\mathbf{t} \in \mathbb{N}^6\\) collects these six random variables, each of which is a binomial random variable.

More sociologically, we might be interested in the PMF of the number of occurrences of married, divorced, separated, single, and widowed people; this yields a vector of five binomial random variables. Or, more technically, we might be interested in the number of times \\(N\\) people in the population show up in a sample, as noted above; this is a (massive) random vector of \\(N\\) binomial variables.

## The multinomial coefficient

The multinomial distribution is essentially an extension of the binomial distribution, and we should similarly derive the multinomial PMF by first motivating the multinomial *coefficient*, namely: 

$$
\begin{align*}
\binom{n}{k_1, k_2, ... k_p} = \frac{n!}{\prod_{j=1}^p k_j!}
\end{align*}
$$

What does the multinomial coefficient actually tell us? It essentially gives the number of ways to partition a set of \\(n\\) items into \\(p\\) distinct buckets where the order of items in the buckets are not relevant. A classic example is the question "how many *unique* anagrams are there of a word?" For example, the word *naan* has four letters, but it should be obvious that it does not have \\(4! = 24\\) (linguistically) unique arrangements. In fact, simple hand inspection would tell us that it has only six: nnaa, nana, naan, aann, anan, anna. This is also equal to \\(\frac{4!}{2!2!}\\), which is what our formula tells us to count. 

The reason that our formula gives the correct answer is that we can consider the problem like so: what if all four letters *were* different? Then, our formula would work: we would have a pure permutation, given by the formula \\(\frac{n!}{(n-k)!}\\), where \\(n = k\\) and so we simply have \\(n!\\). Now, all we have to do is deflate this answer by the number of ways that our old formula overcounts things. This is simply, for each class of letter \\(k_i\\) (e.g., the \\(a\\)s), the number of spurious rearrangements of the repeat letters, which is \\(k_i!\\) (this is actually, in itself, a question of how many ways we can permute the items in each group \\(k_j\\)). 

In general, we are simply extending the binomial coefficient to cover more than two groups. Note that the *naan* case actually just *is* the binomial coefficient since we only have two groups; the binomial formula, \\(\frac{n!}{(n-k)!k!}\\) somewhat sneakily hides the fact that we could call our two groups \\(k = k_1\\) and \\((n-k) = k_2\\), plug them into the multinomial formula, and get the same answer. The reason is simple: if we only have two groups, every way of picking \\(k\\) items is also a way of picking the other \\(n - k\\) items; it is a way of partitioning the set. So, instead of thinking about the binomial as a way of ordering \\(k\\) items of a set of \\(n\\), then undoing the spurious rearrangements of any group of \\(k\\), we can think about it as the number of ways to permute all the items, then undoing two spurious rearrangements within groups. Then, the multinomial follows simply: what if we have more than two groups? 

Conversely, the multinomial can also be derived as the product of multiple binomials. Let's try a harder word like *barbell*. We first ask how many ways we can choose \\(k_1\\) slots from a set of \\(n\\) to be, say, \\(l\\)s. Then, we ask how many ways from a set of \\(n - k_1\\) we can let be the \\(k_2 b\\) s. In general, the pattern looks like this (eschewing the formal statement)... 

$$
\begin{align*}
\binom{n}{k_1}\binom{n-k_1}{k_2}...\binom{n-{(k_1 + k_2 + ... k_{p-1})}}{k_p} \cr
= \frac{n!}{k_1!(n-k_1)!} \cdot \frac{(n-k_1)!}{k_2!(n-[k_1+k_2])!} \cdot \frac{n-(k_1+k_2)!}{k_3!(n-[k_1+k_2+k_3])!}...
\end{align*}
$$

At each step of selecting some specific \\(k_m\\), we divide by \\(k_m!\\) to get rid of the spurious permutations of identical items within that class, and we also divide by \\((n - \sum_{j=1}^{m} k_j)!\\) so that the permutation of the \\((n - \sum_{j=1}^{m-1} k_j)\\) available objects only goes through the first \\(k_m\\) steps (just like how we derive the binomial). 

Finally, we can quickly extend the binomial coefficient \\(\rightarrow\\) binomial PMF analogy. Recall the binomial PMF, i.e. \\(\mathbb{P}[Y=k] = \binom{n}{k} \pi^k (1-\pi)^{n-k}\\). This says that the probability that a random count variable \\(Y\\) takes on \\(k\\) successes is the probability of success to the \\(k\\), multiplied by the probability of failure to the \\(n - k\\), mulitplied the number of ways to select \\(k\\) of the \\(n\\) trials to be successes. We basically find the probability of a specific permutation of trials that are successes and failures (the probability of any such permutation is identical, so we simplify and find the probability that the first \\(k\\) were successes); there are actually \\(\binom{n}{k}\\) ways for that to occur, so we just multiply the probability of one way by \\(\binom{n}{k}\\). 

## The multinomial distribution's PMF

The multinomial distribution, from here, is a very simple combination of the ideas above: 

$$
\begin{align*}
\mathbb{P}[\mathbf{\vec{Y}}=\mathbf{\vec{y}}] = \frac{n!}{\prod_{j=1}^m k_j!} \prod_{j=1}^m \pi_j^{k_j}
\end{align*}
$$

We read this as saying that the probability that the random vector \\(\mathbf{Y}\\), with \\(m\\) outcomes, has \\(k_1\\) successes for outcome 1, \\(k_2\\) successes for outcome two, ... \\(k_m\\) successes for outcome \\(m\\) (note that each of these outcomes is a marginally-binomial RV) probability of any specific sequence of \\(k_1\\) successes on the first outcome, ... times the number of ways to pick \\(k_1\\) outcomes to take the first outcome, ... and \\(k_m\\) outcomes to be outcome \\(m\\). Again, we basically have the task of finding the probability of any particular sequence of the \\(m\\) different counts of successes times the number of ways that this specific probability could come about. 
