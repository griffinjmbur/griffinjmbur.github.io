---
title: "Complex surveys III: cluster random sampling"
date: 2024-02-22
---

In this post, I briefly discuss the benefits and drawbacks of cluster random sampling. Much of the post is dedicated to some interesting transformations of the sampling variance of the cluster sample mean. (Updated 2024-05-10).

# Equal-probability cluster sampling (one-stage, equal-size clusters)

## Motivation for cluster sampling

Recall from the last post in this series that cluster sampling has two primary benefits, per Cochran. The first is *cost-effectiveness*: it is much easier to, for example, conduct the General Social Survey by drawing up a list of randomly-selected neighborhoods to canvas rather than doing the same thing for randomly-selected people, whom we'll call our *elements* (the chemistry metaphor is appropriate: the elements are the smallest meaningful units that we might care about, e.g. we do not consider "parts of people" in sociological surveys). The second is just an extreme version of the first, that of *feasibility*: a list of the elements may not exist at all. 

## Dimensions of variation of cluster sampling

Note that this section is titled "equal-probability cluster sampling (one-stage, equal-size clusters)", indicating the large number of possible types of cluster sampling. I will address these in future posts, but for now, simply note that clusters might be selected with unequal probabilities, that they might vary in size, and that we may or may not sample every element in a cluster.

## Notation and definitions

We have already noted above how cluster random sampling differs from stratified random sampling. **Let us begin with the simplest case, where every element in a cluster is surveyed (we have censuses within clusters) and all clusters are of equal sizes**. From here on out, we will use the variable \\(i\\) to index clusters and \\(j\\) and \\(k\\) to index individuals or *elements*, \\(\forall i \in \{1, 2, ... n\}, j \in \{1, 2, ... m\}\\). For now, the size \\(m_i\\) of our sample *from* any cluster \\(i\\) is \\(M_i \\), its size in the population, and since all population clusters are the same size for now, \\(m_i = M_i = m\\). Note that \\(N\\) indicates the total number of clusters in the population.

Let's see how we might estimate the population mean and total in a cluster random sample. I will generally avoid second-order subscripting or commas in subscripts, so something that really "should" be, e.g. \\(\mu_{Y, cl.}\\) refers to the *cluster method of estimating the individual-level or element mean of \\(Y\\)*. 

First, let's get a useful example going that we'll use throughout the text and define some key terms. 

Let's suppose that we are interested in college basketball. Suppose for simplicity that each team has a defined starting lineup of \\(5\\) players each. We'll call each team a cluster. We might be interested in estimating the average points scored by a player (over some time, say a season), which we could infer with a cluster sample. This is an element-level population mean, denoted simply \\(\mu\\) or sometimes \\(\mu_{el.}\\) for clarity, whose estimate is \\(\hat{\mu}\\). 

At times, we might need to calculate the estimated cluster-level mean, which is the mean points per team (again, over some determinate amount of time), or the points for every five players (the size of each starting lineup), written \\(\hat{\mu}_{pc}\\). The mean per cluster with *equal* cluster sizes is the just the element mean scaled by the size of the cluster, but it could also be found by taking the mean of the cluster-level means. This is *not* true with unequal cluster sizes; with different-size clusters, the mean per it represents the *grand mean*, as it is called in ANOVA (the unweighted group-level mean). 

We might also want to make use of the estimated total points scored in the league, \\(\hat{\tau}\\). In older texts (e.g. both Kish and Cochran), \\(Y\\) and \\(y\\) alone are used to refer to the total of a variable in the population and the sample, which is inconsistent with later notation, where a capital Roman letter from the end of the alphabet means a random variable and a lowercase Roman letter means a realization of \\(Y\\). This notation *does* have the benefit of making cluster sampling notation and element sampling notation similar: an element's score is technically a total (over one element), and a cluster's score is certainly a total. So, both can be written explicitly in terms of a second index \\(j\\) if needed (for elements, the size of the cluster is just one).

A compromise that I use is what is sometimes called "dot notation" (e.g. Casella 2008 or Marden 2003), where a realization that *could* have two subscripts, e.g. \\(y_{ij}\\), is written as, e.g., \\(y_{i*}\\), if we are implicitly summing over, say, elements in the cluster indexed by \\(j\\)&mdash;so, this particular symbol means "some particular cluster's total". I follow the convention of using an asterisk instead of a literal dot because it is simply easier to read. So, then, the total for the league could also be written \\(y_{**}\\), especially useful since this is very different the actual estimate of the population total. 

Note that while we calculate quantities like the sample mean in basically the same way, the sampling variance is different, so I will write things like \\(\mathbb{V}[\hat{\mu}_{cl.}]\\). The sample mean can still be calculated in fundamentally the same way as before, but our notation will change and so will our sampling variances. 

For element means, we have the following...

$$
\begin{align*}
\mu_{el.} &= \frac{\sum_{i=1}^N \sum_{j=1}^M y_{ij}}{NM} && \text{with equal-size clusters} \cr
&= \frac{\sum_{i=1}^N \sum_{j=1}^{M_i} y_{ij}}{\sum_{i=1}^N M_i} && \text{equal or unequal-size clusters} \cr
\hat{\mu}_{el.} &= \frac{\sum_{i=1}^n \sum_{j=1}^M y_{ij}}{nM} && \text{with equal-size clusters} \cr
&= \frac{\sum_{i=1}^n \sum_{j=1}^{M_i} y_{ij}}{\sum_{i=1}^n m_i} && \text{equal or unequal-size clusters} 
\end{align*}
$$

... and for cluster-level means, we have...

$$
\begin{align*}
\mu_{pc} = \frac{\sum_{i=1}^N \sum_{j=1}^{M_i} y_{ij}}{N} && \text{equal or unequal-size clusters}  \cr
\hat{\mu}_{pc}= \frac{\sum_{i=1}^n \sum_{j=1}^{M_i} y_{ij}}{n}
\end{align*}
$$
    
Note that we will need to use different formulae at different moments. 

The first point to note is that cluster sampling is typically less efficient than element sampling. The sampling variance for the sample mean of a cluster can be calculated as below. 

## Drawbacks of cluster sampling: increased sampling variance

It is fairly easy to show that cluster sampling is inefficient relative to element sampling. Recall from post I in this series we saw that the finite sampling context only slightly modified our "naïve" sampling variance, the traditional formula \\(\frac{\sigma^2}{n}\\). We essentially just stick the *fpc* in front of whatever the correct formula would be for the infinite case, and, for the infinite case, the essence of the proof was that we were summing up the variances of independent random variables. 

Now, note that in the cluster case, our actual random variables are the cluster totals themselves. In other words, while some of our formulae above are written with summations over elements, we don't need actually need any individual-level information once we know the cluster total since we are, by construction, taking censuses within clusters. So, to find the variance of our cluster estimator of the element mean, we simply plug the population variance of the cluster totals into our formula for the variance of the sample mean. 

$$
\begin{align*}
\mathbb{V}[\hat{\mu}_{pc}] &= \frac{(1-f)}{n} \sigma^2_{cl.} \cr
& \text{...where...} \cr
\sigma^2_{cl.} &= \frac{1}{N-1} \sum_{i=1}^N (y_{i*} - \mu_{pc})^2 \cr
&= \frac{1}{N-1} \sum_{i=1}^N (m\mu_{i} - m\mu)^2 = 
    \frac{m^2}{N-1} \sum_{i=1}^N(\mu_{i} - \mu_{el.})^2
\end{align*}
$$

Note, in the last line, that we are able to write the population total of each cluster as \\(m\mu_i\\) since each cluster has the same size and has a total that is definitionally equal to the number of elements in it times the mean. Similarly, because each cluster is the same size here, the mean *per cluster* is just the mean *per element* divided by the size of each cluster. 

Now, we can simply compare this to the population ANOVA decomposition for elements. Then, the \\(SS_B\\) is equal to...

$$
\begin{align*}
&\sum_{i=1}^N \sum_{j=1}^{M_i} (\mu_i - \mu_{el.})^2 \cr
&= m\sum_{i=1}^N (\mu_i - \mu_{el.})^2 \cr
&\text{and so...} \cr
&\text{mean square between} = \frac{m}{N-1} \sum_{i}^N  (\mu_i - \mu_{el.})^2 
\end{align*}
$$

Note that the "mean square between" will often be larger than the "total mean square" (the variance) because of the smaller number of degrees of freedom. So, if the mean square between is large, so too will be the sampling variance of the mean of a cluster random sample.

## The intracluster correlation coefficient 

The *intracluster correlation coefficient* (\\(ICC\\)) (or intraclass corrrelation coefficient), \\(\rho_{cl.}\\), is often used to express the variance of the cluster mean. 

By definition, the \\(ICC\\) is the correlation between all of the unique pairs of individuals *within* each cluster, summed over all clusters. The number of possible pairs is the number of possible pairs in each group times the number of groups, \\(N\cdot\binom{M}{2} = \frac{NM(M-1)}{2}\\). We will see that our formula below counts each *unique* pair twice; this explains the complex-looking denominator, which then simply becomes the number of "elements" here (we omit one degree of freedom from the total population size). 

$$
\begin{align*}
ICC = \rho_{cl.} = \frac{\sum_{i=1}^N \sum_{j=1}^M \sum_{k \neq j}^M (y_{ij} 
    - \mu_Y)(y_{ik} - \mu_Y)}{(NM-1)(M-1)\sigma_Y^2}
\end{align*}
$$

### The variance of the element mean estimated from cluster random sampling is proportional to \\(\rho_{cl.}\\)

An important fact is the following one: 

$$
\mathbb{V}[\hat{\mu}_{cl.}] \approx \frac{(1-f)}{Mn} \sigma_Y^2[1 + \rho_{cl.}(M-1)]
$$

Cochran gives an elegant proof of this formula. Recall, again, that with one-stage sampling, the cluster totals are the random variables (since we do not know which totals we will observe), rather than the element scores (which are non-random conditional on the presence of a cluster in the sample since we survey everyone). So, the population variance that we plug into the sampling variance formula is the variance of the group totals about the mean per cluster.

$$
\begin{align*}
\mathbb{V}[\hat{\mu}_{pc.}] = 
    \frac{(1-f)}{n} \frac{\sum_{i=1}^N (y_{i*} - \mu_{pc})^2}{N-1}
\end{align*}
$$

Again, recall that because of the simple relationship between \\(\hat{\mu}_{pc}\\) and \\(\hat{\mu}\\)&mdash;the former is just the latter multiplied by \\(M\\)&mdash;we can simply divide the variance we just obtained by \\(M^2\\) (constants factor out of variances as squares) to find another way to write the sample estimate of the element-level population mean.

$$
\begin{align*}
\mathbb{V}[\hat{\mu}_{cl.}] = 
    \frac{(1-f)}{nM^2} \frac{\sum_{i=1}^N (y_{i*} - \mu_{pc})^2}{N-1}
\end{align*}
$$

Now, let's write out the inside of the summation above as another summation.

$$
\begin{align*}
\mathbb{V}[\hat{\mu}_{el.}] = 
    \frac{(1-f)}{nM^2(N-1)} 
    \sum_{i=1}^N \left\{ \sum_{j=1}^M (y_{ij} - \mu) \right\} ^2\cr
\end{align*}
$$

Notice that the replacement of \\(\mu_{pc}\\) with \\(\mu\\) is justified because summing the latter \\(M\\) times gives the former. 

Now, expanding and squaring the inside, we have... 

$$
\begin{align*}
(y_{i1} - \mu)^2 + (y_{i2} - \mu)^2 + ... (y_{iM} - \mu)^2 
\end{align*}
$$

...plus all of the cross-product terms of the general form...

$$
\begin{align*}
(y_{i1} - \mu)(y_{i2} - \mu) + (y_{i1} - \mu)(y_{i3} - \mu) + ...
\end{align*}
$$

We can write the entire numerator now more concisely using summation notation as ...

$$
\begin{align*}
\sum_{i=1}^N \sum_{j=1}^M (y_{ij}-\mu)^2 + 
    2\sum_{i=1}^N \sum_{j>k}^M (y_{ij}-\mu)(y_{ik}-\mu) 
\end{align*}
$$

The second term is just another way of writing the numerator of the ICC...

$$
\begin{align*}
&\sum_{i=1}^N \sum_{j=1}^M \sum_{k \neq j}^M (y_{ij} - \mu_Y)(y_{ik} - \mu_Y) \cr
&= (NM-1)(M-1)\sigma_Y^2(ICC)
\end{align*}
$$

...while the first term is the element variance multiplied by its \\(df, (NM-1)\sigma^2_Y\\)

Writing the sum out again as all as one expression, we have ...

$$
\begin{align*}
\sum_{i=1}^N (y_{i*} - \mu_\text{pc})^2  &= (NM-1)(M-1)\sigma_Y^2(ICC) + (NM-1)\sigma^2_Y \cr
&= (NM-1)\sigma_Y^2[1 + (ICC)(M-1)]
\end{align*}
$$

Now, let's go back to the sampling variance of the element-level sample mean. 

$$
\begin{align*}
\mathbb{V}[\hat{\mu}] &= 
    \frac{(1-f)}{nM^2} \frac{\sum_{i=1}^N (y_{i*} - \mu_\text{pc})^2}{N-1} \cr
&= \frac{(1-f)}{n} \frac{(NM-1)}{M^2(N-1)}\sigma_Y^2[1 + (ICC)(M-1)] && 
    \text{substituting in previous results} \cr
&= \frac{(1-f)}{Mn} \frac{(NM-1)}{(NM-M)}\sigma_Y^2[1 + (ICC)(M-1)] &&
    \text{arithmetic}
\end{align*}
$$

Notice that if \\(N\\) is quite large relative to \\(M\\)&mdash;and the two are inversely related, since \\(NM\\) must be our total population size&mdash;this is approximately equal to... 

$$
\begin{align*}
\frac{(1-f)}{Mn} \sigma_Y^2[1 + (ICC)(M-1)]
\end{align*}
$$

In other words, all else equal, the sampling variance of a mean from a cluster random sample is proportional to the intracluster correlation coefficient. 

## The intracluster correlation and the ANOVA decomposition

A useful rewrite of the intracluster correlation coefficient uses the ANOVA population decomposition to show that the \\(ICC\\) is closely related to the traditional mean squares calculated in ANOVA.  

First, note that the population variance of the cluster totals found above is quite close to the mean square between groups in the population ANOVA decomposition with individual-level data. On that note, please keep in mind that the use of Roman-letter initialisms in math has a potential to be extremely confusing&mdash;I have tried to limit myself to it in the case of the sums of squares since that is a pretty general convention even though details vary[^sumsq]quite a bit&mdash;I will use \\(\sigma^2_B\\) and \\(\sigma^2_W\\), following Cochran, to indicate the mean squares between and within, respectively.

[^sumsq]: Although note that the actual terms used are extremely variegated. E.g., in the general regression context, some people use \\(SS_E\\) to indicate "sum of squares explained" while others use it (somewhat sloppily) to mean the "sum of squared errors", i.e. a vector perfectly orthogonal to the explained sum of squares! 

$$
\begin{align*}
\sigma^2_B &= \frac{1}{N-1} \sum_{i=1}^N \sum_{j=1}^M (\mu_{i} - \mu)^2 \cr
&= \frac{1}{N-1} \sum_{i=1}^N \sum_{j=1}^M 
    \left\{\frac{y_{i*}}{M} - \frac{\mu_{pc}}{M}\right\}^2 \cr
&= \frac{1}{N-1} \sum_{i=1}^N M 
    \left\{\frac{y_{i*}}{M} - \frac{\mu_{pc}}{M}\right\}^2 \cr
&= \frac{1}{N-1} \sum_{i=1}^N \frac{M}{M^2} (y_{i*} - \mu_\text{pc})^2 \cr
&= \frac{1}{M(N-1)} \sum_{i=1}^N (y_{i*} - \mu_\text{pc})^2 \cr
\sigma^2_B &= \frac{\sigma^2_{\text{cl.}}}{M}
\end{align*}
$$

As a consequence of the last two steps above, we have ... 

$$
\begin{align*}
\sigma^2_B M(N-1) &= \sum_{i=1}^N (y_{i*} - \mu_{\text{pc}})^2  \cr
& =(NM-1)\sigma_Y^2[1 + \rho_{cl.}(M-1)] 
\end{align*}
$$

...using the information from our last derivation. By the way, note in passing for later that this all implies that, since...

$$
\begin{align*}
SS_B = (N-1)\sigma^2_B && \text{by definition} \cr 
SS_B = \frac{1}{M}\sum_{i=1}^N (y_{i*} - \mu_\text{pc})^2
\end{align*}
$$

Finally, we can rearrange this last equation to get it resembling the ICC.

$$
\begin{align*}
\sigma^2_B M(N-1) &= (NM-1)\sigma_Y^2[1 + \rho_{cl.}(M-1)] \cr
\sigma^2_BM(N-1) - (NM-1)\sigma_Y^2 &= (NM-1)\sigma_Y^2\rho_{cl.}(M-1) \cr
\frac{\sigma^2_BM(N-1) - (NM-1)\sigma_Y^2}{(NM-1)(M-1)\sigma_Y^2} &= \rho_{cl.}
\end{align*}
$$

If we again assume that \\((NM - M) \approx (NM -1) \approx NM\\) (if the number of clusters is large and the subpopulations within them small), then we have it that...

$$
\begin{align*}
\rho_{cl.} \approx \frac{\sigma^2_B - \sigma_Y^2}{(M-1)\sigma_Y^2}
\end{align*}
$$

One final connection is worth noting. Keeping in mind that \\(SS_T = SS_B + SS_W\\) and that the degrees of freedom for the mean squares are, respectively, \\((NM-1), (N-1)\\), and \\(N(M-1)\\), we can write...

$$
\begin{align*}
(NM-1)\sigma^2_Y &= (N-1)\sigma^2_B+ N(M-1)\sigma^2_W 
    && \text{mean squares multiplied by df = sums of squares}\cr
&= \frac{1}{M} \sum_{i=1}^N (y_{i*} - \mu_\text{pc})^2
    +  N(M-1)\sigma^2_W  
    && \text{see above for $SS_B$}\cr
    &= \frac{1}{M}(NM-1)\sigma_Y^2[1 + \rho_{cl.}(M-1)] + N(M-1)\sigma^2_W
    && \text{again, we substitute in from above}\cr
\end{align*}
$$

So, we have...
$$
\begin{align*}
&(NM-1)\sigma^2_Y = \frac{1}{M}(NM-1)\sigma_Y^2[1 + \rho_{cl.}(M-1)] + N(M-1)\sigma^2_W \cr
&(NM-1)\sigma^2_Y - \frac{(NM-1)\sigma_Y^2}{M}
    - \frac{(NM-1)\sigma_Y^2 \rho_{cl.}(M-1)}{M} = N(M-1)\sigma^2_W && \text{expansion/subtraction} \cr
&(NM-1)\sigma^2_Y \left\{\frac{M - 1
    - \rho_{cl.}(M-1)}{M} \right\} = N(M-1)\sigma^2_W 
    && \text{clever factoring; expand LHS} \cr
&\frac{(NM-1)\sigma^2_Y}{M} [M - 1 - \rho_{cl.}(M-1)] = N(M-1)\sigma^2_W 
    && \text{factoring out $M$} \cr
&\frac{(NM-1)\sigma^2_Y(M-1)}{M} [1 - \rho_{cl.}] = N(M-1)\sigma^2_W 
    && \text{factoring out $(M-1)$ in blocks} \cr
&\frac{(NM-1)\sigma^2_Y}{NM} [1 - \rho_{cl.}] = \sigma^2_W
    && \text{divide both sides by $N(M-1)$} \cr
\end{align*}
$$

And since \\(NM-1 \approx NM\\), we have...

$$
\begin{align*}
\sigma^2_W \approx \sigma^2_Y(1 - \rho_{cl.})
\end{align*}
$$
