---
title: "Counting for adults III: balls/bins, partitions, inclusion-exclusion, Stirling numbers"
date: 2024-05-25
---

In previous posts about counting, I have not directly tied these questions to the "balls into bins" framing that is ubiquitous. In this post, I connect what I have discussed in past posts to this specific framing, which leads to a discussion of some important new (to this blog) ideas: the inclusion-exclusion principle, Stirling numbers of the second kind, and Bell numbers. 

# Distinguishable balls into distinguishable bins

## Variable sizes of bins

This is pretty straightforward; it is just a variety of the coin-flip sequence problem or a "permutation without repetition". We have \\(k\\) bins to pick, and we pick \\(n\\) times, so this is just \\(k^n\\). Some sources write this as \\(n^k\\), where \\(n\\) is the number of options and \\(k\\) the number of choices to make, which makes the bins the objects and the balls the choices. I think this latter convention is misleading, but more on that another time. In general, I think one appeal of the balls-into-bins approach is that all of the basic metaphors for the different combinatorial identities use the same fundamental physical set-up of distributing \\(n\\) (labeled or unlabeled) objects into \\(k\\) (labeled or unlabeled) bins, so we will stick with that. 

## Fixed sizes of bins

If we require that each bin have a certain *number* of items in it, but we distinguish the items, this is just a multinomial problem. To use our ongoing metaphor involving selecting meats at a barbeque counter, if we put the scoops of meat into a distinct pattern on our plate (say, a ring), then they *are* distinguishable scoops in virtue of their order. Since we only distinguish order up to the type of meat, though, we divide by the spurious orders within each scoops, \\(\Pi_{j=1} k_j!\\). 

# Indistinguishable balls into distinguishable bins

This is a multiset problem. Here, the difference is that we are here focused on *allocation* to distinct groups. Note that this problem and the last problem are often called problems of combinations *with* and *without* replacement. When we set up the multiset problem as one of selecting \\(k-1\\) "barriers" between otherwise indistinguishable groups of items, this reflects that fact. 

Our scoops of meat are the indistinguishable balls (the scoops themselves are not different except by virtue of which meat they select) and the type of meat is the literal bin. 

# Distinguishable balls into indistinguishable bins

This is the problem of partitions, i.e. the unique groups of distinguishable objects that can be formed. In general, the *Stirling numbers of the second type*, count the number of ways to partition a group of \\(n\\) objects into \\(k\\) groups; the *Bell numbers*, \\(B(n)\\), sum the Stirling numbers for fixed \\(n\\) and variable \\(k : k=1, 2, ... n\\). 

## Stirling numbers

Stirling numbers for two groups are closely related to binomial coefficients. The reason is that if we have \\(n\\) people, we can select \\(k\\) of them for one group in \\(\binom{n}{k}\\) ways. Generically, this means finding the sum of the binomial coefficients over \\(k\\), \\(\sum_{k=0}^n \binom{n}{k}\\). This is equal to \\(2^n\\), which we can see by combinatorial reasoning: the number of ways to pick groups of variable size from \\(n\\) elements amounts to working the number of permutations of a "selection list" which indicates whether someone is included in a group of not. If we imagine those names on a list and then choose "present or absent" \\(n\\) times, this is simply \\(2^n\\). Two of those violate our rule about empty sets \\(\binom{n}{0}, \binom{n}{n}\\), both of which are, obviously, equal to one, so we subtract them off. Then, finally, we must realize that our regular binomial formula is also a means of selecting a "stay-behind" group: \\(\binom{n}{k} = \binom{n}{n-k}\\) (this is another way of saying that the binomial formula is just a special case of the multinomial formula). So, while \\(\sum_{k=0}^n \binom{n}{k}\\) counts every possible unique group of every possible size of our \\(n\\), it counts each partition twice since each possible selection of a group will also occur exactly once more when it is the "stay-behind" group. Hence, we divide \\(2^n - 2\\) by \\(2\\), giving \\(2^{n-1} - 1\\). 

Things get more complicated with Stirling numbers for more than two partitions. This is because, in short, with, say, four partitions, working out how many repeats there are gets tricky. Just as a quick example, if we were to try to sum over the multinomial coefficient (itself a complicated expression), we would have to consider how to undercount the number of partitions that involve two groups of fixed size and composition while the other two groups change their size and composition. 

Fortunately, there is a systematic way to work this out indirectly. First, we'll consider how to allocate \\(n\\) objects to distinguishable bins where we allow some bins to be empty. Then, we'll use the principle of the complement and subtract the number of ways that this ends up with at least one bin zero, and divide by the number of spurious rearrangements of bins (generally, \\(k!\\)). In the case of just one bin empty, things are pretty simple: if we have one bin empty, this leaves \\(k-1\\) bins left to distribute \\(n\\) balls to. We could leave any of the \\(k\\) bins empty, so we just subtract out \\(k(k-1)^n\\). 

Now, the task is to work out how many assignments involve \\(2, 3, ... k\\) empty bins and then to substract that from the number of "unadjusted assignments". We can use a bit of set notation here to help us out. First, note that on this view of combinatorics&mdash;again, where we are generally distributing \\(n\\) (labeled or unlabeled) objects into \\(k\\) (labeled or unlabeled) bins&mdash;we are *generally* doing things like finding proper disjoint subsets of our set of objects. 

Various combinatorial identities can be seen in this set-theoretic light. Let's establish some notation; this will get a little thorny, but that is OK. Note that we have "sets" all over the place here, so technically these bins are a set, any particular grouping of some of the \\(n\\) items into a given bin is a set, a total grouping of the items into the bins is a set of sets, a set of relevant groupings is (of course) a set, and a set of all relevant groupings is again of course a set. Heady! 

So, let's use some slightly differentiated notation. First, let's call our bins \\(b_1, b_2...b_k\\). Then, let's call any assignment of \\(n\\) items to the bins (that is, some possible grouping of items into bins) a **collection** \\(B_j\\). Next, the **set of collections that satisfy some criterion** is \\(\mathcal{B}_j\\); we'll focus here on criteria such as "bin \\(j\\) is empty". Finally, then, \\(\mathscr{B}\\) is the set of those criterion-sets. 

Note that all of our standard combinatorial identities can be seen this way. For example, the "permutation with repetition" coefficient, although we usually think about it as counting sequences, is also a way of subsetting a set. To see this, consider that we can represent every possible sequence of \\(n\\) things with \\(k\\) outcomes possible at each go as every way to take the \\(n\\) trials and place them into various "bins" representing the possible outcomes. We are, therefore, finding each possible assignment of \\(n\\) items to our bins \\(b_1, b_2, ... b_k\\). Any such sequence is an assignment of assignments to sets \\(B_j\\), and we count all of them. This notion of the size of a set would *normally* be represented using "pipe" operators, but Jekyll (the platform I use to post these weblogs) has a violent dislike of them when they are used in paragraphs, so I will simply use the computer science notation, `len` (for "length"). Thus, we are finding \\(len(\mathcal{B})\\) (I use pipes in the indented blocks). 

The "combination without repetition" coefficient just fixes the size of the bins \\(b_j\\) and forces \\(k=2\\).[^notation] The multinomial is a simple extension where \\(k\\) can be anything. Permutations even can be fit into this context: they represent all exhaustive, dijoint possible subsets where \\(k\\) of the sets must be singletons and all other elements must go into a \\(k+1\\)th set. Combinations with repetition count all possible exhaustive, disjoint subsets of variable size where we do not distinguish the objects. 

[^notation]: Unfortunately, the standard notation for binomials (where \\(k\\) is the number of things to put in one of two bins) violates the meaning we otherwise give to \\(k\\) in these problems, where it indicates the *number of bins*. This is really only a problem with the standard way of writing the binomial, since \\(k_1, k_2, ... k_k\\) in the multinomial case isn't entirely execrable notation, but the slightly awkward \\(\binom{n}{n_1, n_2, ... n_k}\\) used by *WolframAlpha* is much preferable if we want to be eact. 

Now, let's consider a sets of collections such \\(\mathcal{B_1}\\) that satisfy criteria such as "\\(b_1\\) is empty". 

Then, a quantity such as \\(len(\mathcal{B_1})\\) is the number of collections \\(B_1, B_2...\\) with \\(b_1\\) empty. A quantity such as... 

$$
\begin{align*} 
\mathcal{B}_1 \cup \mathcal{B}_2
\end{align*}
$$

is the union of sets with both bin \\(1\\) and bin \\(2\\) empty.  **Our goal is going to be to count the size of the union of sets that contain all collections where a given bin is empty**. The complement of this number with the total number of ways to chuck \\(n\\) balls into \\(k\\) bins, no restrictions, is what we are after.  We therefore want to find the size of the complement of the size of that set of sets of collections...

$$
\begin{align*}
\neg|\mathcal{B}_1 \cup \mathcal{B}_2...\mathcal{B}_k| = \neg | \mathscr{B} |
\end{align*}
$$

Counting this requires the very useful principle of inclusion-exclusion (AKA the "sieve"). 

## The principle of inclusion-exclusion

One way to think about this is to take the typical image of sets as Venn diagram circles into three dimensions by imagining them as stacked pancakes. We want to find the area of a complex shape formed by overlapping discs. If the pancakes were totally non-overlapping (disjoint), we could just sum up the areas; but, the areas will likely overlap. If we have two pancakes, it is very straightfoward: we add the areas of the two pancakes, which counts their intersection twice, and then subtract the intersection once, so that is is only counted once. With three pancakes, it is still pretty straightforward to figure this out: we add the three pancakes' areas and take out the pairwise intersections. This second move removes any area that is an intersection of just two pancakes exactly one time, which we want, but it subtracts any intersection of the three pancakes three times, which actually nets out that intersection. So, we add it back in. 

I'll now write this with set notation. This is a very *general* principle; it works if our elements are truly atomic sorts of things or if they are themselves sets. I want to use separate notation so that we can just see the general principle before we use it in this somewhat-special use-case. 

$$
\begin{align*}
\bigcup_{j=1}^2 A_j &= A_1 + A_2 - (A_1 \cap A_2) \cr
\bigcup_{j=1}^3 A_j &= A_1 + A_2 + A_3 - 
    (A_1 \cap A_2) - (A_2 \cap A_3) - 
    (A_1 \cap A_3) + (A_1 \cap A_2 \cap A_3) \cr
\end{align*}
$$

Further, if we are only interested in the size of the sets, it is obvious that we can write this as follows. 

$$
\begin{align*}
|\bigcup_{j=1}^2 A_j| &= |A_1| + |A_2| - |A_1 \cap A_2| \cr
|\bigcup_{j=1}^3 A_j| &= |A_1| + |A_2| + |A_3| - 
    |A_1 \cap A_2| - |A_2 \cap A_3| - 
    |A_1 \cap A_3| + |A_1 \cap A_2 \cap A_3| \cr
\end{align*}
$$

Now, to generalize about this process, let us think about what is really happening here. We might *suspect* that the pattern here is that, for \\(j : 1 \leq j \leq n\\), we find all possible intersections of \\(j\\) of the \\(n\\) elements and their sizes, give them *alternating signs*, and sum them. We could represent this generically as ... 

$$
\begin{align*}
|\bigcup_{j=1}^n A_j| &= \sum_{i} |A_i| 
    - \sum_{i\neq j} |A_i \cap A_j | 
        + \sum_{i\neq j \neq h} |A_i \cap A_j \cap A_h| ... \cr
\end{align*}
$$

This is in fact the correct algorithm. Let us show why. First, let \\(m\\) indicate the number of sets which include some element \\(\epsilon\\), with \\(1 \leq m \leq n\\). Then, we can rewrite, for example, our correct tallying of the union of three sets as follows. 

$$
\begin{align*}
| \bigcup_{j=1}^3 A_j | &= \underbrace{|A_1| + |A_2| + |A_3|}_
    {\text{$\epsilon$ counted $\binom{m}{1}$ times}}
    - \underbrace{|A_1 \cap A_2| - |A_2 \cap A_3| - 
        |A_1 \cap A_3|}_
        {\text{$\epsilon$ counted $\binom{m}{2}$ times}} 
    + \underbrace{|A_1 \cap A_2 \cap A_3|}_
        {\text{$\epsilon$ counted $\binom{m}{3}$ times}} \cr
\end{align*}
$$

Let's unpack what's happening here. In the first count, the count of the "marginal" sets on the left, we count element \\(\epsilon\\) as many times as it is a member of a different set, \\(m\\). We can always write, for any \\(m \in \mathbb{N}, m = \binom{m}{1}\\), of course, but it may not immediately be clear why we would want to do this. However, if we examine the second term, it is clearer: for intersections of size \\(j\\), there are \\(m\\) sets with our elements, so there are \\(\binom{m}{j}\\) intersections that involve \\(\epsilon\\). For example, if we have \\(20\\) sets and some element appears in seven of them, there are \\(1716\\) intersections of dimension \\(13\\) that contain that element. 

Let us now *verify* that this method of counting will correctly count any given element exactly once. This is surprisingly easy. For any element \\(\epsilon\\) which occurs in \\(m\\) sets, if we find all possible intersections of \\(j\\) of the \\(n\\) sets for \\(j : 1 \leq j \leq n\\), give them *alternating signs*, and sum them, we will conduct the following summation. 

$$
\begin{align*}
\sum_{j=1}^n \binom{m}{j} \cdot (-1)^{j-1}
\end{align*}
$$

Since \\(\binom{m}{j}, j>m = 0\\) by definition (and combinatorial good sense), this reduces, for all elements in any set and for any \\(1 \leq m \leq n\\) , to...

$$
\begin{align*}
\sum_{j=1}^m \binom{m}{j}\cdot (-1)^{j-1}
\end{align*}
$$

There are a couple of ways to work out this sum. Either way, let's handle a simpler case first, where we sum alternate signs with no arbitrary restriction on the lower index, keeping in mind that we're working with a sum temporarily which is actually one smaller than the real sum: the unrestricted sum adds a term worth \\(\binom{m}{0} \cdot (-1)^{-1} = 1 \cdot -1 \\). So, at the end of our calculations, we'll just add back one. 

$$
\begin{align*}
\sum_{j=0}^m \binom{m}{j}\cdot (-1)^{j-1}
\end{align*}
$$

What is this sum really counting? Well, it is *adding up* the binomial coefficients for a fixed \\(m, 0 \leq j \leq m, j \mod 2 = 0\\) and *subtracting* from that the binomial coefficients \\(m, 0 \leq j \leq m, j \mod 2 = 1\\). More prosaically, we fix a row of Pascal's triangle and sum up the terms where we pick an even number of things and subtract the terms where we pick an odd number of things. 

### Alternating sums of rows of Pascal's triangle

So, what is the sum of every other term in a row of the triangle? Well, if we have an even number of elements in the row, we're in an odd row because of the zero-indexing of the Triangle. So, if we were to, say, sum up the seventh row, we'd pair \\(\binom{7}{0}, \binom{7}{7}; \binom{7}{1}, \binom{7}{6}\\), etc. Because of the symmetry of the triangle, this means that the sum of every other item must be a clean *half* of the whole sum, \\(2^{n-1}\\), and the alternating sum is zero. If we have an *odd* number of elements in the row, the argument is slightly trickier but also more fun. Imagine that our binomial coefficients represent the selection of people for a tennis tournament with Swiss rules, which works better with even numbers of participants. We can select \\(0, 2, 4, ... n\\) people. This is the same, in a way, as either selecting person \\(1\\) or not, person \\(2\\) or not, etc. We lose one degree of freedom with respect to person \\(n\\), though, since we are forced to pick an even number of people, though; based on our decisions about the previous \\(n-1\\) people and the fact that we are only selecting even numbers of people, our choice is fully determined. So, our forking garden paths are \\(2^{n-1}\\) in number.   

An alternate method, if we're clever, is that we can simply recognize this as a form of the binomial *expansion*, not just a sum of binomial coefficients. 

$$
\begin{align*}
\sum_{j=0}^m \binom{m}{j}\cdot (-1)^{j} &= \sum_{j=0}^m \binom{m}{j}\cdot (-1)^{j}1^{n-j} \cr
&= (1-1)^m = 0
\end{align*}
$$

Again, if we flip the sign on each term, the sum logically cannot change, so...

$$
\begin{align*}
\sum_{j=0}^m \binom{m}{j}\cdot (-1)^{j-1} &= \sum_{j=0}^m \binom{m}{j}\cdot (-1)^{j} \cr
&= \sum_{j=0}^m \binom{m}{j}\cdot (-1)^{j}1^{n-j} \cr
&= (1-1)^m = 0
\end{align*}
$$

Finally, we simply recall that the actual sum we conducted omitted the first term, \\(-\binom{m}{0}\\), so our actual sum is one larger than zero. What is the payoff of all of this trouble? Well, we in fact *proved* that our method of working out the size of the union of \\(n\\) elements correctly counts any element exactly once. 

Summarizing our findings in words, we found that the size of the union of \\(n\\) sets is the alternating sum of the size of each \\(j\\)-way intersection of the sets, for all \\(1 \leq j \leq n\\). This is a rare case in math where words might serve to communicate the idea more clearly than symbols, but we should try to put the idea symbolically as well. 

$$
\begin{align*}
|\bigcup_{j=1}^n A_j| &= \sum_{j=1}^n (-1)^{j-1} \sum_{1 \leq h_1 < h_2 < ... h_j \leq n} 
    | A_{h_1} \cap A_{h_2} ... \cap A_{h_j}|
\end{align*}
$$

The notation here is gnarly. If we were just trying to sum over all possible \\(w\\)-way intersection for a fixed \\(w\\), we could omit the first summation and simply write the second sum as a sum over several dummy indices \\(\alpha, \beta, \gamma\\), etc.&mdash;summing over sets \\(A_\alpha, A_\beta, A_\gamma\\) indexed by \\(1 \leq \alpha < \beta < \gamma \leq n \\) is another way of saying "find all combinations (strict sense) of these sets and sum them". This could be written as multiple sums if we want; it is really a triplet of sums, after all. However, in the actual case under consideration, we do not even have a fixed number of intersections, and there is not a natural way to explicitly write the growing number of summations using multiple summation sigmas. 

# Applying the principle of inclusion-exclusion

Now, we use this to work out the number of ways to distribute \\(n\\) distinguishable items into \\(k\\) indistinguishable bins.

*Here, our sets are actually sets of collections*. So, the intersection of, say, \\(\mathcal{B}_1\\) and \\(\mathcal{B}_2\\) would be all collections \\(B_p, B_q, B_r...\\) that have both bin \\(b_1\\) and bin \\(b_2\\) empty.

$$
\begin{align*}
&k^n  && \text{# ways to distribute $n$ distinct items into $k$ distinct bins} \cr
|\mathcal{B}_j| &= \binom{k}{1} (k-1)^n 
    && \text{size of set of collections with a given bin empty} \cr
|\mathcal{B}_j \cap \mathcal{B}_h| &= \binom{k}{2} (k-2)^n 
    && \text{size of set of collections with two given bins both empty} \cr
| \bigcup_{j=1}^k \mathcal{B}_j | &= 
    \sum_{j=1}^k (-1)^{j-1} \binom{k}{j} (k-j)^n = \mathscr{B}
        && \text{set of collections with any of $k$ boxes empty} \cr
& k^n -  
    \sum_{j=1}^k (-1)^{j-1} \binom{k}{j} (k-j)^n
    && \text{number of partitions with no box empty, distinct bins} \cr
\end{align*}
$$

Now, let's do a bit of clever algebra on that last expression to get it more tractable. First, we rewrite it as an addition of negative terms, simplifying the exponent; then, we just write the whole expression as one summation.

$$
\begin{align*}
k^n - \sum_{j=1}^k (-1)^{j-1} \binom{k}{j} (k-j)^n 
    &= k^n +  \sum_{j=1}^k (-1)^{j} \binom{k}{j} (k-j)^n \cr
&= \sum_{j=0}^k (-1)^{j} \binom{k}{j} (k-j)^n 
\end{align*}
$$

The last trick is to get this in a slightly nicer form. Of course, \\((k-j)^n\\) for a fixed \\(n, k\\), variable \\(0 \leq j \leq k\\) is just a sequence of integers to some power; with natural ordering, this is just \\(0, 1, 2^n, 3^n, ... k^n\\). We might suspect that we could simply re-parameterize this term as \\(j^n\\) to the same effect. In fact, we can. If we are moving along the element of a row of Pascal's Triangle, we are taking equal-sized steps up and down a hill. If we move through the old expression, we have terms such as...

$$
\begin{align*}
\binom{k}{0} k^n + \binom{k}{1} (k-1)^n + ... + \binom{k}{k-2} 2^n + \binom{k}{k-1}  + \binom{k}{k} \cr
\end{align*}
$$

If we rewrite each term, we simply have...


$$
\begin{align*}
\binom{k}{k} k^n + \binom{k}{k-1} (k-1)^n + ... + \binom{k}{2} 2^n + \binom{k}{1}  + \binom{k}{0} \cr
\end{align*}
$$

The only problem is that the sign should be flipped. We also just divide out by \\(k!\\) to take care of the fact that the boxes are, in fact, indistinct, so any permutation of the labeled boxes is immaterial.

$$
\begin{align*}
\left\{\begin{matrix} n \\ k \end{matrix}\right\} &= \frac{1}{k!}\sum_{j=0}^k (-1)^{j} \binom{k}{j} j^n 
\end{align*}
$$

