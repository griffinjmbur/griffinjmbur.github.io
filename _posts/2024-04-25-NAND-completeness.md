---
title: "The universality of NAND gates"
date: 2024-04-25
---

Let's show a useful theorem in computer science, that \\(\text{NAND}\\) gates are all one needs to be able to express any Boolean function, and thus extremely useful in computing. 

# Boolean functions

First, what *is* a Boolean function? It is a function on \\(n\\) binary variables to one binary output. In the simplest case of one binary variable, there are only two Boolean functions possible: one that returns \\(\tiny{\text{TRUE}}\\) if the input is \\(1\\) (and \\(\tiny{\text{FALSE}}\\) otherwise) and one that does the opposite. 

In the more common example of two binary variables, we have \\(2^2 = 4\\) possible configurations or sequences of the variables, which we might call a "state of the world". Since a Boolean function could return a \\(1\\) for any combination of the states of the world, we have \\(2^{4} = 4\\) Boolean functions possible with two variables. 

Let’s see this concretely. The \\(\text{NAND}\\) function for two variables \\(X_1, X_2\\) returns \\(1\\) *iff* \\(\neg(X_1 =1 \land X_2 = 1)\\); so, of our four possible sequences using \\(X_1, X_2\\), we have selected three to return \\(\tiny{\text{TRUE}}\\). Here that is in truth table form.

$$
\begin{aligned}
\begin{array}{|c|c|c|}
\hline 
X_1 & X_2 & \text{NAND}(X_1, X_2) \cr
\hline
0 & 0 & 1 \cr
\hline
1 & 0 & 1 \cr
\hline
0 & 1 & 1 \cr
\hline
1 & 1 & 0 \cr
\hline
\end{array}
\end{aligned}
$$

More generally, we can say that for \\(n\\) Boolean variables, there are \\(2^{2n}\\) unique Boolean functions defined on them. The reason is that if each variable can take one of two values, there are \\(2^n\\) possible sequences (sometimes referred to as a "permutation with repetition"). On top of that, the number of unique Boolean functions is given by the number of unique groups of sequences that trigger a \\(1\\) or a \\(\tiny{\text{TRUE}}\\); the sum of all variable-length sub-groups of a group of \\(n\\) is also \\(2^n = \sum_{j=0}^n \binom{n}{j} \\). To see why, we can simply realize that each elements's inclusion is a variable: it is or is not included. So, to count the total number of independent options, we simply multiply the numbers of outcomes for each variable together. 

# The proof

The proof can proceed in three parts. 

First, note that any truth table can be expressed using disjunctions, conjunctions and negations. For example, here we could write \\(\text{NAND}(X_1, X_2) = \neg{(X_1 \land X_2)} \\). Another function such as \\(\text{XOR}\\) can be written \\(\text{XOR}(X_1, X_2) = (X_1 \land \neg X_2) \lor (\neg X_1 \land X_2)\\).

Second, note that we don't actually need the \\(\lor\\) operator. This is because of De Morgan's laws, one of which tells us that \\(X_1 \lor X_2 = \neg(\neg X_1 \land \neg X_2)\\). In words, "either \\(X_1\\) on its own, \\(X_2\\) on its own, or both is the same as *not* neither-of-them". Technicaly speaking, this law is usually written more directly as \\(\neg(X_1 \lor X_2) = \neg X_1 \land \neg X_2\\); simply adding a negation to both sides produces the formulation I gave. Incidentally, De Morgan's other major law is that \\(\neg(X_1 \land X_2) = \neg X_1 \lor \neg X_2\\): the opposite (a slightly imprecise term) of both \\(X_1\\) and \\(X_2\\) being true is that *at least* one is false.

Third, having whittled down our necessary logical connectives to \\(\land, \lor, \neg\\), and then just \\(\land, \neg\\), let us show that both of the latter can be expressed using \\(\text{NAND}\\) alone. 

First, we can express \\(\neg X_1\\) as \\(\text{NAND}(X_1, X_1)\\): after all, the only way for it to false that "both" \\(X_1\\) "and" \\(X_1\\) are true is for one to be false, the other to be false, or both. But, if either is false, so is the other, and this reduces to the case that \\(\text{NAND}(X_1, X_1)=\text{NOT}(X_1)\\). 

Second, we can express \\(X_1 \land X_2\\) as follows. First, note that \\(X_1 \land X_2 = \neg\text{NAND}(X_1, X_2)\\): both \\(X_1\\) and \\(X_2\\) being true is the logical complement of only one-or-the-other or neither being true. What comes next is a little tricky, but we can now combine our rules to show that ...

$$
\begin{equation}
X_1 \land X_2 = \
    \text{NAND}\left\{\text{NAND}(X_1, X_2), \text{NAND}(X_1, X_2)\right\}
\end{equation}
$$

We can reason through this logically, moving inside-out. First, \\(\text{NAND}(X_1, X_2)\\) means "not-both". Then, we saw that applying this function to two identical inputs, a split signal, means "not the input"; so, this reduces to "not(not-both)", i.e., both. 
