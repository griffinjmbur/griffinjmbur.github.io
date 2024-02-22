---
title: "Complex surveys II: stratified random sampling"
date: 2024-02-21
---

# Stratified random sampling

## Stratifying vs. clustering: two very different approaches

In the next couple of posts, I will introduce the general concepts of stratified and cluster random sampling. Rather than begin with the difference in definitions&mdash;in fact, I have never seen a generalist textbook which answers this question precisely (e.g. the sloppy Bryman 2012)&mdash;let me start with the problems that these approaches solve. 

Suppose that we have one of the following goals. 

1. To ensure that we have large enough subsamples for small groups whose representation we want to guarantee, or to make standard errors for various groups equal (requiring the taking of non-proportional samples from subsets of our population, strategically).
2. To ensure that we sample every group with the same probability, so that the sample looks like the population (requiring that we take exactly proportional numbers from subsets of our population into the sample, rather than leaving this up to chance).
3. To reduce variance generally: we could use, as our subsets, the group score on some variable which also predicts the outcome of interest. We could potentially do so *so* exactly that there is no variance within subsets (and thus no variance across samples), and we would only need to worry about bias in our sample. More realistically, the variance might be very small.

If any of these is our aim, we should *stratify* our sample in advance (or after it is taken). Simply put, a stratum is an outcome on some grouping variable that is relevant to some particular sample. For example, the variable may be "one's state of residence", a single stratum is a specific state \\(H=h\\) such as Wisconsin or North Carolina, and this might be relevant to the study of income or wealth. Once a population is chopped into strata, we carefully ensure that someone from every stratum is sampled (I'll explain why in a moment). There is a wide variety of approaches to selection within strata that I will mention later; *the essential element of stratification is that we split our population up into groups that are relevant to the outcome and then systematically include people from **every** group*. Stratification reduces (typically) the sampling variance because we can exploit the fact that we have every stratum sampled and it is presumably related to some of the outcomes. By treating members of our sample as draws from a subsample, defined by levels of a grouping variable related to one of the outcomes, we can use . In that sense, stratification is a *sampling design* that uses techniques usually considered to be related to *analysis*&mdash;in particular, ANOVA will be central. 

Suppose, instead, that we have the goal of making a sample easy to administer. We don't want to stratify a sample by state, randomly dial up people in North Carolina, and end up with a 1000 person sample with someone in every single ZIP code. Instead, we might hope to visit a small number of large city blocks and a few rural population centers, perhaps including a handful of people from truly isolated areas. In this case, we would instead try to achieve a certain amount of randomness in our sample by making the choice of *which* of these *clusters* to include at random. Thus, we reduce the administrative burden of interviewing people across wide geographic areas, but we still include the extremely important random aspect. Clustering tends to increase sampling variance because we don't observe every cluster, so any given sample will be more unlike other samples than in the case of simple or stratified random sampling.

Strata are often confused with clusters, in part because nothing *necessarily* makes something a cluster or a stratum *except* how they are treated in sampling. ***At least some people from every stratum are sampled; not every cluster is included***. They share the property that we have some kind of sampling procedure at a level more abstract than the actual elements. The difference is that we have essentially a census *of* strata (and random selection within) and a random sample *of* clusters. Within a cluster, we can have random sampling or a census; these are referred to, respectively, as two-stage or one-stage cluster sampling for obvious reasons. I'll say more about this later. Now, we'll focus on stratified sampling. 

## Stratified random sampling: proportional and optimal allocation

Generally, there are two methods of stratifying which we'll consider here. The first is that of **proportional allocation**, which means to use the sampling fraction \\(f_h = f = \frac{n}{N} \forall h \in \mathscr{H} = \{1, 2, ... L\}\\), where \\(\mathscr{H}\\) is the set of all elements comprising the random variable \\(H\\) and \\(h\\) is a realization of \\(H\\). This ensures that, if we know the population sizes of all the strata, we have an exactly proportional sample. 

How do we actually *use* strata in the analysis? To find the equation for a sample mean, we need to recall what a mean really *is*, at least according to one approach. 

We can introduce the notation for a stratified mean and prove that it is unbiased *if* the method of choosing subsample means is unbiased. The actual proof itself is somewhat trivial, but the first few steps, though not necessary for the proof if already known, help show what the stratified mean really *is*. 

In one sense, a sample mean is an estimate of the expectation of a variable. This is not obvious because we often work with continuous random variables (whose expectation is given by \\(\mathbb{E}[Y_{continuous}] = \int_{-\infty}^\infty y \cdot f(y)\hspace{0.1cm} dy\\), where \\(f(y)\\) is the probability density function). Even if our variable is discrete, we may not have repeated values in the sample; for example, if \\(i\\)'s wealth is recorded to the cent (unlikely but possible), we probably won't have any repeated values of observations on \\(Y :=\\) income in the sample. However, note that in any case...

$$
\begin{align*} 
&1. \hat{\mu}_\text{Y, simple} = \frac{1}{n} \sum_{i=1}^{n} y_{i} \cr
&2. = \sum_{i=1}^{n} \underbrace{\left\{\frac{1}{n}\right\}}_
    \text{estimated probability} 
    \underbrace{y_{i}}_\text{value of observation}
\end{align*} 
$$

This becomes less trivial and more resembles the expectation formula if we have repeat values. For example, if we have a sample of size \\(n=10\\) where every observed value happens to occur twice, we could, rather than counting all ten values when calculating the mean, simply write \\(\hat{\mu}_\text{Y} = \sum_{j=1}^{5} \frac{1}{5} y_{j}\\), where \\(j\\) indexes the *unique* values. This is more obviously a form of the expected value formula (for discrete variables). That said, for a simple random sample, the computational advantage of the expected value is moot: we would have to first catalogue repeat values before actually applying this, which would waste time rather than save it. 

**However**, applying these probabilities is extremely important *if* we know that we have sampled people with different probabilities, depending on their group membership. In general, if the *simple* sample mean formula is (in pseudo-algebra) \\(\sum_{\text{unique outcomes}} \text{\{estimated probability of outcome}\} \text{\{value of outcome}\}\\), a stratified random sample with different sampling probabilities needs to be adjusted so that we have an unbiased estimate of the expected value of the random variable. In other words, if we treat a stratified random sample like a simple one, what we really have is \\(\sum_{\text{unique outcomes}} \text{\{biased probability of outcome}\} \text{\{value of outcome}\}\\). 

To fix this, we apply *weights*, which are simply the inverse of the biased probability. The biased probability is simply the known probability of inclusion in the sample; "biased" here need not imply "bad" or "unintended" as much as simply "unnatural". 

*In nuce*, we want our observed values in the stratified sample to be treatable as equally-probable realizations of the random variable; they are *not* treatable this way because for every person from an (under-)over-sampled group, the probability of their outcome on \\(Y\\) is not well-estimated by \\(\frac{1}{n}\\) as it is in a simple random sample. 

Another way to understand weights that will lead us more directly to their use is the following rewrite of the sample mean ***which will seem pretty silly at first***.

$$
\begin{align*}
&1. \hat{\mu}_\text{Y} = \frac{1}{n} \sum_{i=1}^{n} y_{i} \cr
&2. = \sum_{i=1}^{n} \frac{N}{n} \frac{y_i}{N} \cr
&3. = \sum_{h=1}^{1} \frac{N_h \hat{\mu}_y}{N} && \text{Let $h$ index subsets of the population} \cr
&4. = \frac{1}{N} \sum_{h=1}^{1} N_h \hat{\mu}_y \cr
\end{align*} 
$$

Now, this way of writing the mean seems perhaps silly at first. We begin with the mechanical definition of the mean, insert two \\(N\\)s that cancel, and then sum over \\(i\\); this all works out, as can be seen in the third line, but it might seem pointless. However, in line \\(4\\), we actually see a useful example of a way to conceptualize the mean: for some subset of our sample (possibly the whole sample!), we find the sample mean. Then, we scale this to the population size of that subset; this represents an estimate of their subpopulation-level total. In this case, with just one subset of the population, this results in a trivial rewrite: we could simply rewrite \\(N_h\\) as \\(N\\) and then have \\(\hat{\mu}_y\\).

*But*, what if we have multiple subpopulations of different sizes? Then, the formula on line \\(4\\) is useful because it provides a less trivial way of taking the mean: we simply scale the mean by the size of each group in the population&mdash;which *nets out* any over- or under-sampling because the mean is a *per capita* measure to begin with&mdash;and then treat the result estimated total like the actual population total; to find a good estimate of the population mean, we simply divide by \\(N\\). Notice that this is another way of dealing with the fact that our groups might have been sampled at different rates (which will, by definition, produce a biased estimator if we use simple techniques). The means for each sub-group are not affected by the subsample size, however; the sampling variance *is* but not the sampling *bias*. So, as long as we scale them into estimates of the total correctly with the use of \\(N_h\\), our estimate has eliminated the the problem. 

Thus, we arrive at the formula for the unbiased estimate of the stratified random sample mean, for \\(h \in \mathscr{H} = \{1, 2, 3, ... L\}\\).  

$$
\begin{align*}
&1. \hat{\mu}_\text{Y, str.} = \frac{1}{N} \sum_{h=1}^{L} N_h \hat{\mu}_{Y, h} \cr
&2. = \sum_{h=1}^{L} \frac{N_h}{N} \hat{\mu}_{Y, h} \cr
&3a. = \sum_{h=1}^{L} \omega_h \hat{\mu}_{Y, h} \cr
\end{align*}
$$

where \\(\omega_h := \frac{N_h}{N} \\) is the weight for each stratum. Here, the *weight* specifically means the *known* share of the population of a stratum.

Note that this definition of weights is consistent with Cochran (1977). Some authors prefer to write the weights as the inverse of the probability of selection. Note that this is simply a re-partitioning of one-and-the-same formula....

$$
\begin{align*}
&2. = \sum_{h=1}^{L} \frac{N_h}{N} \hat{\mu}_{Y, h} \cr
&3b. = \sum_{h=1}^{L} \sum_{i=1}^{n_h} \frac{N_h}{N} \frac{y_{ih}}{n_h} \cr
&4b. = \sum_{h=1}^{L} \sum_{i=1}^{n_h} \frac{N_h}{n_h} \frac{y_{ih}}{N} \cr
&5b. = \sum_{h=1}^{L} \sum_{i=1}^{n_h} \omega_{\text{prob.},h} \frac{y_{ih}}{N} \cr
&6b. = \frac{\sum_{h=1}^{L} \sum_{i=1}^{n_h} \omega_{\text{prob.},h} y_{ih}}{N} \cr
&7b. = \frac{\sum_{h=1}^{L} \sum_{i=1}^{n_h} \
    \omega_{\text{prob.},h} y_{ih}}{\sum_{h=1}^{L} N_h} \cr
&8b. = \frac{\sum_{h=1}^{L} \sum_{i=1}^{n_h} \omega_{\text{prob.},h} y_{ih}}
    {\sum_{h=1}^{L} \sum_{i=1}^{n_h} \frac{N_h}{n_h}} \cr
&9b. = \frac{\sum_{h=1}^{L} \sum_{i=1}^{n_h} \omega_{\text{prob.},h} y_{ih}}
    {\sum_{h=1}^{L} \sum_{i=1}^{n_h} \omega_{\text{prob.},h}} \cr
\end{align*}
$$

This alternative definition of weights is somewhat complex for the formulation of the sample estimate of the mean. Fortunately, it is simple for the estimate of the population total: we simply omit the denominator, leaving \\(\hat{\tau}_Y = \sum_{h=1}^{L} \sum_{i=1}^{n_h} \omega_{\text{prob.},h} y_{ih}\\). Correspondingly, the formula involving the "expected value" weights needs to be modified to be multiplied by \\(N : \hat{\tau}_Y = N \sum_{h=1}^{L} \omega_h \hat{\mu}_{Y, h}\\). 

Note an important fact: when the sampling fraction is the same in all groups (i.e., we use the stratification method to simply ensure an exactly proportional sample), we have a *self-weighting* sample. The standard error, to be derived momentarily, should still be calculated with weights, but the point estimate is the simple random sample mean. The basic idea is that with proportional sampling, the overall sampling fraction \\(\frac{n}{N}\\) is also the sampling fraction for every group, \\(\frac{n_h}{N_h}\\). If this is true, \\(\frac{n}{N}=\frac{n_h}{N_h}\\), and so \\(\frac{N_h}{N}=\frac{n_h}{n}\\): because the sampling fraction is the same for each group, the fraction that a group is of the population is also the fraction that it is of the sample. 

$$
\begin{align*}
&1. \hat{\mu}_\text{Y, prop} = \sum_{h=1}^{L} \frac{N_h}{N} \hat{\mu}_{Y, h} 
    && \text{proportional sampling is a subset of stratified sampling} \cr
&2. = \sum_{h=1}^{L} \frac{n_h}{n} \hat{\mu}_{Y, h} 
    && \text{see above} \cr
&3. = \sum_{h=1}^{L} \frac{1}{n} n_h\hat{\mu}_{Y, h}
    && \text{algebra} \cr
&4. = \frac{1}{n} \sum_{h=1}^{L} \sum_{i=1}^{n_h} y_{ih}
    && \text{algebra, definition of summation} \cr
&5. = \frac{1}{n} \sum_{i=1}^{n} y_{i}
    && \text{definition of summation} \cr
\end{align*}
$$

## The variance of the stratified random sample and its improvement on the simple random sample

The sampling variance of a stratified random sample mean is easy to find from the above, fortunately. 

$$
\begin{align*}
&1. \mathbb{V}[\hat{\mu}_\text{Y, str.}] = \sum_{h=1}^L \mathbb{V}[\omega_h \hat{\mu}_{Y, h}]
    + 2\sum_{h>j}^L \sum_{j=1}^L \mathbb{COV} 
    [\omega_h \hat{\mu}_{Y, h}, \omega_j\hat{\mu}_{Y, j}] 
    && \text{Bienaymé’s identity; see previous post} \cr
&2. = \sum_{h=1}^L \mathbb{V}[\omega_h \hat{\mu}_{Y, h}]
    && \text{cov. between subpopulation means is zero here} \cr
&3. = \sum_{h=1}^L \omega_h^2  \mathbb{V}[\hat{\mu}_{Y, h}]
    && \text{constants factor out of variances as squares} \cr
&4. = \sum_{h=1}^L \omega_h^2  \left\{ 1 - \frac{n_h}{N_h} \right\} 
    \frac{\sigma^2_{Y, h}}{n_h}
    && \text{fpc applied to each subpopulation}
\end{align*}
$$

### The ANOVA proof

Before going further, it is important to have the analysis of variance (ANOVA) decomposition results handy. What the ANOVA decomposition tells us is essentially that the variance of a variable can be partitioned cleanly into variation within groups and variation between groups. This is true at both the population- and sample-level; here, I show the decomposition at the population level. 

$$
\begin{align*} 
&1. SS_T = \sum_{h=1}^L \sum_{i=1}^{N_h} (y_{ih} - \mu_\text{Y})^2 
        && \text{definition, summation} \cr
&2. = \sum_{h=1}^L \sum_{i=1}^{N_h} (y_{ih} - 
    (\mathbf{\mu_\text{Y, h}} - \mathbf{\mu_\text{Y, h}})
    - \mu_\text{Y}^2)
    && \text{we just add and subtract the same value} \cr
&3. = \sum_{h=1}^L \sum_{i=1}^{N_h} (y_{ih} - 
    \mathbf{\mu_\text{Y, h}} + \mu_\text{Y, h} - \bar{y})^2 \
    && \text{rearrangement} \cr
&4. =\sum_{h=1}^L \sum_{i=1}^{N_h} (\text{within-group deviation} + \text{between-group deviation})^2 &&
    \text{definition of terms} \cr
&5. = \sum_{h=1}^L \sum_{i=1}^{N_h} (\text{within deviation})^2 + 
    \sum_{h=1}^L \sum_{i=1}^{N_h} (\text{between deviation})^2 + &&  \text{binomial expansion} \cr
    & 2\sum_{h=1}^L \sum_{i=1}^{N_h} (\text{between deviation} * \text{within deviation})
\end{align*} 
$$

The rightmost term disappears because, while we sum within a group (within deviation), the group deviation is constant. So, we treat it as a constant, and the within-deviations from the group-level mean sum to zero for any group as proved earlier. Then...

$$
\begin{align*}
&6. = \sum_{h=1}^L \sum_{i=1}^{N_h} (y_{ih} - \hat{\mu}_\text{Y, h})^2 + 
    \sum_{h=1}^L \sum_{i=1}^{N_h} (\hat{\mu}_\text{Y, h} 
    - \hat{\mu}_\text{Y})^2 \cr
&7. = \underbrace{\sum_{h=1}^L \sum_{i=1}^{N_h} (y_{ij} - 
    \hat{\mu}_\text{Y, h})^2}_\text{within-groups variation}  + 
    \underbrace{\sum_{h=1}^L \sum_{i=1}^{N_h}  (\hat{\mu}_\text{Y, h} - \hat{\mu}_\text{Y})^2 }_\text{between-groups variation} \cr
\end{align*}
$$

We often see this simplied to \\(SS_T = \sum_{h=1}^L \sum_{i=1}^{N_h} (y_{ih} - \hat{\mu}_\text{Y, h})^2 + 
N_h \sum_{h=1}^L (\hat{\mu}_\text{Y, h} - \hat{\mu}_\text{Y})^2\\) since the sum over \\(i\\) of the second expression is a sum over a constant (from the perspective of \\(i\\), which does not index any term in the expression). 

### Stratified random sampling as an improvement on simple random sampling

Now we can move to the demonstration that this is an improvement on simple random sampling. We'll start by writing the TSS as a scalar multiple of the population standard deviation. 

\begin{align*} 
&1. (N-1)\sigma^2_\text{Y, adj.} = \sum_{h=1}^L \sum_{i=1}^{N_h} (y_{ih} - \hat{\mu}_\text{Y, h})^2 + 
    \sum_{h=1}^L \sum_{i=1}^{N_h} (\hat{\mu}_\text{Y, h} - \hat{\mu}_\text{Y})^2 && \text{definition} \cr
&2. (N-1)\sigma^2_\text{Y, adj.} = \sum_{h=1}^L (N_h-1) \sigma^2_{Y, h} + 
    \sum_{h=1}^L N_h (\hat{\mu}_\text{Y, h} - \hat{\mu}_\text{Y})^2 && \text{summation simplification} \cr
&3. \sigma^2_\text{Y, adj.} \approx \sum_{h=1}^L \omega_h \sigma^2_{Y, h} + 
    \sum_{h=1}^L \omega_h (\hat{\mu}_\text{Y, h} - \hat{\mu}_\text{Y})^2 
    && \text{assuming $N_h >> 1$} \cr
&4. \mathbb{V}[\hat{\mu}_\text{Y, SRS}] = (1-\frac{n}{N})\frac{1}{n} \sigma^2_{Y, \text{adjusted}}
    && \text{found above} \cr
&5. \mathbb{V}[\hat{\mu}_\text{Y, SRS}] = \frac{1-f}{n} \left\{ \sum_{h=1}^L \omega_h \sigma^2_{Y, h} + 
    \sum_{h=1}^L \omega_h (\hat{\mu}_\text{Y, h} - \hat{\mu}_\text{Y})^2\right\} 
    && \text{combining steps 1-4} \cr
\end{align*}

Now, finally, note that when we have proportional allocation, $n_h = \frac{nN_h}{N}$, meaning that the variance of the mean is $\sum_{h=1}^L (1 - f_h) \omega^2_h \frac{\hat{\sigma}_{Y, h}}{n} = \sum_{h=1}^L (1 - f_h) \frac{N_h^2}{N^2} \frac{N\hat{\sigma}_{Y, h}}{nN_h}= \sum_{h=1}^L (1 - f_h) \frac{N_h}{N} \frac{\hat{\sigma}_{Y, h}}{n}= \sum_{h=1}^L (1 - f) \omega_h \frac{\hat{\sigma}_{Y, h}}{n}$. 

Thus, from 5) above, we have ... 
\begin{align*}
\mathbb{V}[\hat{\mu}_\text{Y, SRS}] &= \frac{1-f}{n} \left\{ \sum_{h=1}^L \omega_h \sigma^2_{Y, h} + 
    \sum_{h=1}^L \omega_h (\hat{\mu}_\text{Y, h} - \hat{\mu}_\text{Y})^2\right\} \cr
&= \mathbb{V}[\hat{\mu}_\text{Y, prop}] + \frac{1-f}{n} \left\{
    \sum_{h=1}^L \omega_h (\hat{\mu}_\text{Y, h} - \hat{\mu}_\text{Y})^2\right\}
\end{align*}

## Optimal allocation 

I will probably post at some point in the future a proof using Lagrange multipliers.
