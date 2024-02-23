---
title: "Complex surveys III: cluster random sampling"
date: 2024-02-22
---

In this post, I briefly discuss the benefits and drawbacks of cluster random sampling. Much of the post is dedicated to some interesting transformations of the sampling variance of the cluster sample mean. 

# Equal-probability cluster sampling (one-stage, equal-size clusters)

## Motivation for cluster sampling

Recall from the last post in this series that cluster sampling has two primary benefits, per Cochran. The first is *cost-effectiveness*: it is much easier to, for example, conduct the General Social Survey by drawing up a list of randomly-selected neighborhoods to canvas rather than doing the same thing for randomly-selected people. The second is just an extreme version of the first, that of *feasibility*: a list of the elements may not exist at all. 

We have already noted above how cluster random sampling differs from stratified random sampling. **Let us begin with the simplest case, where every element in a cluster is surveyed (we have censuses within clusters) and all clusters are of equal sizes**. From here on out, we will use the variable \\(i\\) to index clusters and \\(j\\) and \\(k\\) to index individuals or *elements*, \\(\forall i \in \{1, 2, ... n\}, j \in \{1, 2, ... m\}\\). For now, the size \\(m\\) of our sample *from* any cluster \\(i\\) is \\(M\\), its size in the population, while \\(N\\) indicates the total number of clusters in the population.

The first point to note is that cluster sampling is typically less efficient than element sampling. The sampling variance for the sample mean of a cluster can be calculated as below. 

## Drawbacks of cluster sampling: increased sampling variance

It is fairly easy to show that cluster sampling is inefficient relative to element sampling. Recall from post I in this series we saw that the finite sampling context only slightly modified our "naïve" sampling variance, the traditional formula \\(\frac{\sigma^2}{n}\\). We essentially just stick the fpc in front of whatever the correct formula would be for the infinite case. And, for the infinite case, the essence of the proof was that we were summing up the variances of independent random variables. 

Now, note that in the cluster case, our actual random variables are the cluster means themselves. Since we take a census within each cluster, there is no actual variation at the individual level. And, to obtain the mean for the entire sample, we simply sum the cluster means and divide by the number of clusters, \\(n\\); you may remember the school rule to never take the average of average, but this is, in fact, allowed as long as the means are taken over the same size group, and here each cluster is indeed of size \\(m\\). So, we simply plug the variance of the cluster means in.

$$
\begin{align*}
\mathbb{V}[\hat{\mu}_Y] &= \frac{(1-f)}{n} \sigma^2_{cl.} \cr
&= \frac{(1-f)}{n} \sigma^2_{cl.} \hspace{0.1cm} \text{where...} \cr
\sigma^2_{cl.} &= \frac{1}{N-1} \sum_{i=1}^N (\mu_{Y_i} - \mu_Y)^2
\end{align*}
$$

Now, we can simply recall from the population ANOVA decomposition that the \\(SS_B\\) is equal to ...

$$
\begin{align*}
&\sum_{i=1}^N\sum_{j=1}^M (\mu_{Y_i} - \mu_Y)^2 \cr
&= \sum_{i}^N M (\mu_{Y_i} - \mu_Y)^2 \cr
&\text{and so...} \cr
&MS_B = \frac{M}{N-1} \sum_{i}^N  (\mu_{Y_i} - \mu_Y)^2
\end{align*}
$$

Note that the "mean square between" will often be larger than the "total mean square" (the variance) because of the smaller number ofdegrees of freedom. 

## The intracluster correlation coefficient 

The *intracluster correlation coefficient* (\\(ICC\\)) (or intraclass corrrelation coefficient), $\rho_{\text{cl}}$, is often used to express the variance of the cluster mean. 

By definition, the \\(ICC\\) is the correlation between all of the unique pairs of individuals *within* each cluster, summed over all clusters. The number of possible pairs is the number of possible pairs in each group times the number of groups, \\(N\cdot\binom{M}{2} = \frac{NM(M-1)}{2}\\). We will see that our formula below counts each *unique* pair twice; this explains the complex-looking denominator, which then simply becomes the number of "elements" here (we omit one degree of freedom from the total population size). 

$$
\begin{align*}
ICC = \frac{\sum_{i=1}^N \sum_{j=1}^M \sum_{k \neq j}^M (y_{ij} 
    - \mu_Y)(y_{ik} - \mu_Y)}{(NM-1)(M-1)\sigma_Y^2}
\end{align*}
$$

### The relationship of $\rho_{\text{cl.}}$ to the variance of the cluster sample mean

Cochran gives an elegant proof of this formula. We need to introduce new notation here. First, let \\(\mu_{\text{pc}}\\) indicate the mean *per cluster*. This is the average score on our variable of interest divided by the number of clusters in our population. For example, if we are considering a population of all eight-packs of protein shakes in a refrigerated warehouse, \\(\mu_{\text{pc}}\\) would be the *protein content per pack*. Let us also simply note that, although it is not a new concept, the counterpart to the per-cluster mean is the regular per-element mean. Although it is not strictly necessary, let us use \\(\mu_\text{el.}\\) where necessary.

In general, if each cluster contains \\(M\\) elements, the per-cluster mean divides the total by \\(M\\) fewer elements than the per-element mean, so \\(\mu_{\text{pc}}\\) will be \\(M\\) times the *per element* mean. In our protein shake example, the protein content *per element* needs to be divided by eight. 

Generalizing, we have...

$$
\begin{align*}
\mu_{\text{el.}} = \frac{\sum_{i}^N \sum_{j}^M y_{ij}}{NM} \cr
\hat{\mu}_{\text{el.}} = \frac{\sum_{i}^n \sum_{j}^M y_{ij}}{nM}
\end{align*}
$$
... and the mean *per cluster* is 

$$
\begin{align*}
\mu_{\text{pc}} = \frac{\sum_{i}^N \sum_{j}^M y_{ij}}{N} \cr
\hat{\mu}_{\text{pc}}= \frac{\sum_{i}^n \sum_{j}^M y_{ij}}{n}
\end{align*}
$$
    
Now, we can calculate the variance of the per-cluster sample estimator of the mean. We can simply pretend that we do not know about the granular, element-level data. We can estimate the per-cluster population mean by simply taking each cluster's total and dividing by the number of clusters. So, the population variance that we plug into the sampling variance formula is the variance of the group totals about the mean per cluster.[^notation]

[^notation]: Notation can be tricky here. Older texts (e.g. Kish and Cochran) take the approach of using a capital letter of the random variable to mean "the total taken over it", which is not consistent with contemporary usage (where capital letters mean "the random variable as opposed to a realization". I use what is sometimes called "dot notation" (e.g. Casella 2008 or Marden 2003), where a realization that *could* have two subscripts, e.g. \\(y_{ij}\\), is written as, e.g., \\(y_{i*}\\), if we are implicitly summing over the cluster&mdash;so, this particular symbol means "some particular cluster's total". I follow the convention of using an asterisk instead of a literal dot because it is simply easier to read.

$$
\begin{align*}
\mathbb{V}[\hat{\mu}_\text{pc}] = 
    \frac{(1-f)}{n} \frac{\sum_{i=1}^N (y_{i*} - \mu_\text{pc})^2}{N-1}
\end{align*}
$$

Now, notice that because of the simple relationship between \\(\hat{\mu}_\text{pc}\\) and \\(\hat{\mu}\\)&mdash;the former is just the latter multiplied by \\(M\\)&mdash;we can simply divide the variance we just obtained by \\(M^2\\) (constants factor out of variances as squares) to find another way to write the sample element mean's sampling variance. 

$$
\begin{align*}
\mathbb{V}[\hat{\mu}_{\text{cl.}}] = 
    \frac{(1-f)}{nM^2} \frac{\sum_{i=1}^N (y_{i*} - \mu_\text{pc})^2}{N-1}
\end{align*}
$$

Now, we just want to explicitly write out the sum within clusters. We have...

$$
\begin{align*}
\mathbb{V}[\hat{\mu}_{\text{cl.}}] = 
    \frac{(1-f)}{nM^2} \frac{\sum_{i=1}^N 
    \left\{ \sum_{j=1}^M (y_{ij} - \mu) \right\} ^2}{N-1} \cr
\end{align*}
$$

Notice that the replacement of \\(\mu_\text{pc}\\) with \\(\mu\\) is justified because summing the latter \\(M\\) times gives the former. 

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
&(NM-1)(M-1)\sigma_Y^2(ICC) + (NM-1)\sigma^2_Y \cr
\sum_{i=1}^N (y_{i*} - \mu_\text{pc})^2 &= (NM-1)\sigma_Y^2[1 + (ICC)(M-1)]
\end{align*}
$$

Now, let's go back to the sampling variance of the element-level sample mean. 

$$
\begin{align*}
\mathbb{V}[\hat{\mu}_{\text{cl.}}] &= 
    \frac{(1-f)}{nM^2} \frac{\sum_{i=1}^N (y_{i*} - \mu_\text{pc})^2}{N-1} \cr
&= \frac{(1-f)}{n} \frac{(NM-1)}{M^2(N-1)}\sigma_Y^2[1 + (ICC)(M-1)] && 
    \text{substituting in previous results} \cr
&= \frac{(1-f)}{Mn} \frac{(NM-1)}{(NM-M)}\sigma_Y^2[1 + (ICC)(M-1)] &&
    \text{arithmetic}
\end{align*}
$$

Notice that if \\(N\\) is quite large relative to \\(M\\)&mdash;and the two are inversely related, since \\(NM = |\mathscr{U}|\\)&mdash;this is approximately equal to... 

$$
\begin{align*}
\frac{(1-f)}{Mn} \sigma_Y^2[1 + (ICC)(M-1)]
\end{align*}
$$

In other words, all else equal, the sampling variance of a mean from a cluster random sample is proportional to the intracluster correlation coefficient. 

## The intracluster correlation and the ANOVA decomposition

A useful rewrite of the intracluster correlation coefficient uses the ANOVA population decomposition to show that the \\(ICC\\) is very close to the coefficient of determination for the regression of the outcome on the clusters as a set of binary predictors, \\(R^2 = \frac{SS_W}{SS_T}\\). 

First, note that the population variance of the cluster totals found above is quite close to the mean square between groups in the population ANOVA decomposition with individual-level data. Since using Roman-letter acronyms in math has a potential to be extremely confusing&mdash;I have tried to limit myself to it in the case of the sums of squares since that is a pretty general convention even though details vary[^sumsq]&mdash;I will use \\(\sigma^2_B\\) and \\(\sigma^2_W\\)  to indicate the mean squares between and within since these are essentially variances anyways. 

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
&\sum_{i=1}^N (y_{i*} - \mu_{\text{pc}})^2 = \sigma^2_BM(N-1) \cr
& =(NM-1)\sigma_Y^2[1 + (ICC)(M-1)] && \text{}
\end{align*}
$$

...using the information from our last derivation. Finally, 

$$
\begin{align*}
\sigma^2_BM(N-1) &= (NM-1)\sigma_Y^2[1 + (ICC)(M-1)] \cr
\sigma^2_BM(N-1) - (NM-1)\sigma_Y^2 &= (NM-1)\sigma_Y^2(ICC)(M-1) \cr
\frac{\sigma^2_BM(N-1) - (NM-1)\sigma_Y^2}{(NM-1)(M-1)\sigma_Y^2} &= (ICC)
\end{align*}
$$

If we again assume that \\((NM - M) \approx (NM -1) \approx NM\\) (if the number of clusters is large and the subpopulations within them small), then we have it that...

$$
\begin{align*}
ICC \approx \frac{\sigma^2_B - \sigma_Y^2}{(M-1)\sigma_Y^2}
\end{align*}
$$

One final connection is worth noting. Keeping in mind that \\(SS_T = SS_B + SS_W\\) and that the degrees of freedom for the mean squares are, respectively, \\((NM-1), (N-1)\\), and \\(N(M-1)\\), we can write...

$$
\begin{align*}
(NM-1)\sigma^2_Y &= (N-1)\sigma^2_B + N(M-1)\sigma^2_W \cr
&= \frac{1}{M(N-1)} \sum_{i=1}^N (y_{i*} - \mu_\text{pc})^2
    +  N(M-1)\sigma^2_W  \cr
    &= \frac{1}{M}(NM-1)\sigma_Y^2[1 + (ICC)(M-1)] + N(M-1)\sigma^2_W
\end{align*}
$$

So, we have...
$$
\begin{align*}
&(NM-1)\sigma^2_Y = \frac{1}{M}(NM-1)\sigma_Y^2[1 + (ICC)(M-1)] + N(M-1)\sigma^2_W \cr
&(NM-1)\sigma^2_Y - \frac{(NM-1)\sigma_Y^2}{M}
    - \frac{(NM-1)\sigma_Y^2 (ICC)(M-1)}{M} = N(M-1)\sigma^2_W \cr
&(NM-1)\sigma^2_Y \left\{\frac{M - 1
    - (ICC)(M-1)}{M} \right\} = N(M-1)\sigma^2_W \cr
&\frac{(NM-1)\sigma^2_Y}{M} [M - 1 - (ICC)(M-1)] = N(M-1)\sigma^2_W \cr
&\frac{(NM-1)\sigma^2_Y(M-1)}{M} [1 - (ICC)] = N(M-1)\sigma^2_W \cr
&\frac{(NM-1)\sigma^2_Y}{NM} [1 - (ICC)] = \sigma^2_W \cr
\end{align*}
$$

And since \\(NM-1 \approx NM\\), we have...

$$
\begin{align*}
\sigma^2_W \approx \sigma^2_Y(1 - (ICC))
\end{align*}
$$
