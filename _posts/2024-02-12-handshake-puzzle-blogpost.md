---
title: 'The handshake puzzle: introduction to counting'
date: \date
tags:
  - dissertation math
  - category1
  - category2
---

Here is my inaugural blogpost. This is written in a way so that a talented middle-school student could make sense of it, but this might be useful to people who haven't needed to use this beautiful math in the long interval between high school and graduate school.

How many ways can $n$ people shake hands at a party, if \\(n\\) represents any old number? It is pretty obvious that if we have a small party of two people, the answer is \\(1\\), and if we have three people, a bit of careful counting&mdash;maybe we call the people \\(A, B\\), and \\(C\\)&mdash;shows that we have three ways: A&B, A&C, and B&C. Note that we don't count the handshakes twice; I didn't list, for example, B&A. It's just the same handshake. Anyways, this puzzle is interesting because the answer is not obvious if we have \\(n=30\\). 

It is also interesting because the different ways to solve this puzzle lead to a variety of beautiful maths. This is sometimes called "discrete mathematics" because the numbers involved in this special kind of math are *discrete*, which you can think about as being like "concrete": they are solid, familiar things, the numbers you already know. The difficult, and exciting, part of this math will be the techniques we use, but the numbers will be ones like you know: \\(0, 1, 2, 3\\), etc.; they are contained in the "set" (which is just fancy talk for a group) of natural numbers, written \\(\mathbb{N}\\). [^continuousmath] 

[^continuousmath]: Some other parts of advanced math use new kinds of numbers which you might have heard of: negative numbers (we call them *integers* and write them as \\(\mathbb{Z}\\), German for *Zahlen* or "numbers"), numbers that are between the counting numbers (we call them *real numbers* and write them $\mathbb{R}$), "imaginary" or "complex" numbers, which I don't have time to explain, but which you can think of as having two distinct parts (they are written \\(\mathbb{C}\\)), or even real numbers with multiple distinct parts, called vectors (and written \\(\mathbb{R}^n\\), where \\(n\\) means the number of distinct parts). 

# Solution 1: clever cancelling

Here is a very fun way to prove it that the great mathematician Carl Friedrich Gauss (1777-1855) came up with as a young boy, probably when he was nearly your age. 

First, notice that no one can shake hands with herself. So, if we start with person \\(1\\), she can shake hands with $n-1$ people. Now, let's move to person \\(2\\). He can shake hands with \\(n-2\\) people; we subtract two because he can't shake hands with himself and we've already counted the shake he made with person 1. If we imagine going to the last couple of people, person \\(n-1\\) can shake hands with just one person, and person $n$ can shake hands with no one who hasn't already shaken with him. 

So, we want to sum up the numbers between \\(1\\) and \\(n-1\\)! This is a big first step to the solution. But, we still need to work out how to do that. *Let's show how to do this for any old number \\(n\\); then, we can just plug in \\(n-1\\) into the formula.*

Here is a very general way to write the sum. In math, the ellipse (the meaning of that word *here* is the "three dots") indicates that we keep going in the same way. Let's let $S$ stands for "sum".

$$
\begin{align*}
        S = 1 + 2 + 3 + \ldots + (n-1) + n
\end{align*}
$$

For example, if $n=7$, the formula above is an *abbreviation* for $S = 1 + 2 + 3 + 4 + 5 + 6 + 7$. 

 **Our goal here is going to be to find a way to simplify the sum**. Our strategy is going to be finding a way to make those numbers follow a very simple pattern, even simpler than the fact that they go up by one each time. Here is the way to do that. First, we'll write out the sum once in *ascending* order (which means each term gets bigger); then, we'll do the same thing but in *descending* order (which means each term gets smaller). Then, if we write the sums on top of each other, we'll see a beautiful pattern. We can then add the sum to itself twice, simplify the pattern, and then divide by two to fix the fact that we doubled the sum.

$$
\begin{align*}
S &= 1 &&+ 2  ...  &&&+ n-1 &&&&+ n \cr
+ S &= n &&+ n-1 ... &&&+ 2 &&&&+ 1 \cr
\hline
2S &= n +1 &&+ (n+1) ... &&&+ (n+1) &&&&+ (n+1)
\end{align*}
$$

Do you see the pattern? Each term simplifies to $n+1$. So, how many times did we write $n+1$? We wrote it out $n$ times, so $2S = n(n+1)$ and $S = \frac{n(n+1)}{2}$. Beautiful, right?

Now, all we need to do is to remember that we're actually only adding up the numbers $1$ through $n-1$, so we plug in $n-1$ for $n$ and get $\text{number of handshakes} = \frac{n(n-1)}{2}$. If we have $30$ people, my example of a difficult problem above that we wouldn't want to count by hand, our formula gives us $\frac{30(29)}{2} = 435$ handshakes, which is a very big number, indeed: this is much more than the number of people with whom the ordinary person has any kind of personal relationship (that number is usually thought to be between 150 and 300). 

# Solution 2: counting for big kids

## Introduction

Another way to solve the handshake puzzle is to ask "how many ways can we pick unique groups of two people from a group of $n$ that contains them?". This actually leads us into a whole exciting branch of math called *combinatorics* that is extremely useful. It is a kind of math that is concerned with counting. Counting things might seem like a task for students much younger than you, but when you get older, you'll learn that much of higher-level math involves just this very task (most of the rest deals with cases where have things moving at different speeds over time, something called *calculus*). 

The question "how many ways are there of choosing items from a group of $n$" is a question that comes up *constantly* in mathematics. Remember from above that $n$ means "any old number". In math, we also typically assign the letter $k$ or (for people who speak British or British-influenced English) $r$ to the number of items we pick. 

To answer this question, we need to realize that there are actually several general situations where we can think of what we're doing as "choosing $k$ items from a set of $n$". We're going to focus on situations that are common when we count people or physical things, cases where *once we pick someone to be in our group, they can't be picked again* (if it seems like there couldn't be any other way to do it, imagine throwing treats to dogs: you could pick one dog that you like the most over and over, even though that's not very nice to the other dogs). So, we'll take a little detour into this kind of counting before we arrive at the answer. 


## Counting without replacement

To understand the two most important situations&mdash;where we do *not* have replacement&mdash;let's suppose that we return to our party. Let's start with some concrete numbers; then we will generalize. Let's say that we have $n=6$ people. 

### Permutations without replacement

Let's suppose that we are first interested in a silly situation, taking a photo of the party. How many ways can everyone line up in a row? After a little reflection, we see that in the first slot, we could put any of six people, but once we do that, there are only five people available for the next slot, four for the next, and so on. We want to multiply $30\cdot29\cdot28...\cdot3\cdot2\cdot1$ (although we don't really need to multiply by the one, but it does no harm). There is a simple way to represent this kind of multiplication; it is called the *factorial operator* and it has the exciting symbol "!". So, $30! = 30\cdot29\cdot28...\cdot3\cdot2\cdot1$, and $n! = n \cdot (n-1) \cdot (n-2) \hspace{0.1cm} ...  3 \cdot 2 \cdot 1$. 

So, our answer to this puzzle of the photo lineups is $30! = 265,252,859,812,191,058,636,308,480,000,000$ (which is mindblowingly large, I think), which we can abbreviate as $2.65 \cdot 10^{32}$. No-one uses ordinary names for numbers this large, but technically, this is called *265 nonillion* or *265 thousand billion billion billion*. That's bigger than the number of stars in the entire universe, which NASA estimates to be $1 \cdot 10^{24}$. It is truly incredible that we can "make" such massive numbers with just a simple party trick. 

We generally refer to this situation as a *permutation*, where this means that the order of selections matters. The way to remember this is a dumb joke: your bike's combination lock is actually a *permutation lock*. 

Now, we took a very easy case just now. We had $n = 30$ people and selected all $k=30$ of them. What if ask after how many ways we could line up any three attendees and take their photo (for whatever reason)? Then, it is clear that we just want the first three terms of the factorial. This turns out to be easy to write more simply, even though factorial arithmetic is generally tricky to simplify. We can simply divide $30!$ by $28!$, leaving only $30*29$, a big number, but a much smaller one, $870$, which is probably smaller than the number of people you'll go to high school with. 

We can generalize this easily! Defining $nPk$ as the number of ways to *permute*, or order, $k$ items taken from a group of $n$ (this is pronounced *$n$ permute $k$*, we have:

$$
\begin{align*}
nPk = \frac{n!}{(n-k)!}
\end{align*}
$$

You can plug in the actual numbers we have into this formula and see that it works!

$$
\begin{align*}
30P2 &= \frac{30!}{(30-2)!} \\
&= \frac{30!}{(28)!} \\
&= 30\cdot29 = 870
\end{align*}
$$

### Combinations (without replacement)

Now, let's change our question slightly to directly answer the question we started with. Let's examine the number of ways to select a *group* of, say, two people from our party. The number of those groups is also the number of handshakes! 

There are several good ways to think about this question. The first is that we could reason as follows. Our old formula $nPk$ works quite well to count the number of ways we could pick a conga leader and a DJ because, in that scenario, we distinguish the two. However, if we only care about the *group* of people who are shaking hands, note that our formula overcounts! If we have a group of two leaders, Person A who is the conga leader and Person B who is the DJ, this is not the same as if we flip the roles. But if we think about them shaking hands, there is no "order" of any kind: a handshake is a mutual act. So if we use our "conga leader/DJ" strategy, we overcount&mdash;but, as we often do in higher math, we can pretty easily work out by what factor it overcounts, and then we just fix our formula to take that into account! 

Well, we can reason that, because we'll eventually get every possible ordering of any distinct group of $k$ people, it overcounts each *group* of people by however many ways that they can be rearranged, which is $k!$ as we saw from before.

This is easy to generalize. Defining $nCk$ as the number of ways to pick $k$ items taken from a set of $n$ where we don't care about the order, and noting that this is often written as $\binom{n}{k}$ and pronounced *$n$ choose $k$*, we have:

$$
\begin{align*}
nCk = \binom{n}{k} = \frac{n!}{(n-k)!k!}
\end{align*}
$$

Now we can finally solve our puzzle. 

First, overcount the number of ways we can order two people out of the $n$ at the party. This is easy; we can start with each person, and then we end up with one person fewer with whom to pair them, so we have $n\cdot(n-1)$, which can be written for the sake of generalizing as $\frac{n!}{(n-2)!} = n(n-1)$. 

Then, we realize that this formula overcounts by the number of ways to order the group of size $2$ which we select: each permutation, or ordered listing, of size $2$ given by the formula $\frac{n!}{(n-2)!}$ is a member of a group with all the same elements, just arranged in a different order; since each permutation is of the same "length", every unique group is overcounted by a factor of $2! = 2$, so the formula for the sum of permutations overcounts the sum of groups by that exact factor. So, if we just divide by the old formula by $2! (=2)$, we'll have our answer. 

Here, we obtain $\frac{n(n-1)}{2}$, which is the same formula we found above for the sum of the first $n-1$ positive integers! If we try our example situation of a party with $30$ people, we get $\frac{30(29)}{2} = 435$, the same as before. Wow! Math is magical. 

Note, by the way, that we could also see the problem a bit more generally in a way that will be useful later in life. Imagine that we simply set out 30 chairs and label two of them "handshaker" chairs and put them together with 28 regular partygoer chairs. Now, we just find all the rearrangements of the party into the 30 chairs ($30!$), and then we deflate our count by the unnecessary rearrangements of the handshakers ($2!$) and non-handshakers ($28!$). This yields the same answer, although doing this would take virtually infinite lifetimes&mdash;not very practical!

Finally, you might wonder about some special cases. What if, for example, we try to pick *everyone* at the party to be part of a group that goes out back to the bonfire? Logic tells us that there should be only one way to do this. Does our formula work? 

$$
\begin{align*}
\binom{30}{30} = \frac{30!}{(30-30)!30!} = \frac{30!}{0!30!}
\end{align*}
$$

Hm, this is puzzling; we have $0!$, and we definitely know that we cannot divide by zero: any such operation is *undefined*.[^divzero] Well, it turns out that $0!$ is *defined* and it is equal to $1$. The reason is simple, but it is sometimes alarming since it makes us realize the fact that many mathematical operations appear to be *descriptive* of the real world (e.g., the Greeks could show that squares and cubes of numbers corresponded to the finding of areas and volumes of figures), but only up to a point. Beyond that, we have some choice of how to define certain operations; there are sometimes choices that appear almost forced, but it is a choice. One simple reason to define it this way is that if we have two rooms of people, one with $n$ people and the other with $m$, the total number of ways for everyone in both rooms to stand in a row is of couse $n!m!$, as we would expect from our previous work; but, if all the people in room two leave, and we use a *naive* definition whereby $0! = 0$, this then means that the total number of arrangements of room 1 is $0$ (?!) even though it is clearly $n!$ 

[^divzero]: It is not $0$ because dividing something into "zero" parts shouldn't make it any smaller; sometimes people think it is $\infty$ because dividing by very small numbers makes for a very big answer, but $\infty$ is not really a number. Also, dividing $n$ into "half a part" basically means finding the number that our $n$ is half of, while there is *no* number that our $n$ is "zero parts of". The simple answer is that it just makes no sense to say what "dividing into zero parts" means&mdash. The reason that we can *multiply* by zero is that if we think about multiplication as being like taking a foot-long slinky and stretching it to two or three feet (that is, multiplying it by $2$ or $3$), it *does* make sense to compress a slinky out until it has basically no width.

If this still doesn't satisfy you, you can think about it like this: we know the right answer should be 1. We can either say that $0! = 0$, and there is a special case where our formula breaks down, or we can say that $0! = 1$ because it makes sense (there's only one way to have nothing!) and lets our formula work generally. Neither option is fully satisfying or "obviously" correct; the second one is easier. 

So, here is our answer now. 

$$
\begin{align*}
\binom{30}{30} = \frac{30!}{0!30!} = 1
\end{align*}
$$

What if we picked $0$ people to go out back because it's raining? There is also only one way for zero people to do that (everyone stays inside), so we should get the same answer from the arithmetic. 

$$
\begin{align*}
\binom{30}{0} = \frac{30!}{30!0!} = 1
\end{align*}
$$

This is another little interesting pattern. Our $\binom{n}{k}$, which is also called the *binomial coefficient* for reasons that I don't have space to talk about, is symmetric. In other words, $\binom{n}{k} = \binom{n}{n-k}$. Let's see that algebraically first. 
 
$$
\begin{align*}
nCk = \binom{n}{n-k} = \frac{n!}{(n-[n-k])!(n-k)!} \cr
= \frac{n!}{(-[-k])!(n-k)!} \cr
= \frac{n!}{(k)!(n-k)!} \cr
\end{align*}
$$

So, to solve our handshake puzzle, we could have just found $\binom{30}{28}$. But *why*? Well, conceptually, if any way of choosing $k$ items from a set of $n$ leaves $n-k$ left over, then every single way of picking of $k$ items is a way of "picking" $n-k$, so they logically have to add up. I like to picture, say, an unsolved jigsaw puzzle on the table. One way to "pick" $k$ pieces is simply to lay a length of twine down the table, separating it into parts with $k$ and $n-k$ pieces (by definition since you must have $n$ pieces for some appropriate choice of $n$, e.g. 1000). 

So, in our party example, where it's a little less obvious, we can work out that any time we pick two people to shake hands, we're picking $28$ people to *not* shake hands. Maybe a silly little way to see it is to think about the $28$ people as audiences for the handshake&mdash;maybe the handshakes are arm-wrestling contests!! The audience changes each time the people arm-wrestling does, so we could have just counted the audiences, too. 

## Fun facts about the binomial coefficient: Pascal's triangle/paths

Have you ever seen or constructed by hand Pascal's triangle? This is easy to do by hand. We start with the arbitrary number one, a buoy floating atop a sea surface of zeros (not pictured below; I'm using the standard picture). Then, you can form any number in the triangle by adding together the two numbers directly above it. Remember that there are zeros everywhere outside the triangle. 

$$
\begin{array}{cccccccccccc}
& & & & 1 & & & & \\
& & & 1 & & 1 & & \\
& & 1 & & 2 & & 1 & \\
& 1 & & 3 & & 3 & & 1 \\
1 & & 4 & & 6 & & 4 & & 1 \\
\end{array}
$$

How is this in any way connected to the binomial theorem? Well, all of those numbers in the triangle are binomial coefficients for *n* = row number (starting at zero, please note!) and *k* = entry from the left-most element of the row. 

$$
\begin{array}{cccccccccccc}
& & & & \binom{0}{0} & & & & \\
& & & \binom{1}{0} & & \binom{1}{1} & & \\
& & \binom{2}{0} & & \binom{2}{1} & & \binom{2}{2} & \\
& \binom{3}{0} & & \binom{3}{1} & & \binom{3}{2} & & \binom{3}{3} \\
\binom{4}{0} & & \binom{4}{1} & & \binom{4}{2} & & \binom{4}{3} & & \binom{4}{4} \\
\end{array}
$$

Now, *why* is this true? There are a few connections.

The first way is to reinterpret the "foundational numbers"&mdash;the initial $1$ and the $0$s outside the triangle&mdash;as ways to get to those spots. So, we take it for granted that the unity at the top of the pyramid is not *just* the number one but also the number of ways to get to that spot, a sort of *creatio ex nihilo*. Then, we recall that there are actually zeros outside of the triangle, representing the fact that there is no way to get to them (at least in this simple version of the triangle). 

Then, we can interpret each of the other numbers as being additions of the *number of ways to get to that spot*, starting with the next row ("row 1" since the first row is zero-indexed). After all, *if you took it for granted* that the two numbers above any number *were* the number of ways to get to those spots, then adding them is not only just the addition of two numbers but *the total number of ways to get to our spot*&mdash;after all, if all possible streets from the gym to my office at the corner of Observatory and Charter either end by coming down Observatory or down Charter, you can work how many ways there are to that corner so long as you know how many ways there are to get to the penultimate intersection on Observatory and the penultimate one on Charter. So, this is a recursive proof: it doesn't matter how we got the "parent numbers" (as in a family tree) for some arbitrary entry, as long as we know that they are the sums of ways to get to those two spots. Then, we just pursue this all the way back to the beginning and declare this true by fiat.

The second way is also very nice and works in the opposite direction using what is called "Pascal's law": $\binom{n-1}{k-1}+\binom{n-1}{k} = \binom{n}{k}$. In words, this says that to find the number of ways to pick $k$ items from a set of $n$, we simply find the number of ways to pick one fewer item from a set of one fewer item and add this to the number of ways to pick fully $k$ items from a set one size smaller. I'll prove this in a moment; for now, just notice that this means that the entries in the *binomial representation* of the triangle can be now interpreted as the sums of the "parent entries", at least before we get to the (arbitrary/mystical) first element. Once we pick that arbitrary first element to be one *and* to be $\binom{0}{0}$, every element below that is interpretable as the sum of the two entries above it, whether they are numbers or binomial coefficients (you can understand the zeros as being $n$ choose negative numbers or $k>n$, both of which logically have to be zero). 

So, why is Pascal's rule true? There is an algebraic proof, but it is far less interesting than the logical proof. Imagine that the Mystery Inc. gang&mdash;Fred, Daphne, Velma, Shaggy, and Scooby&mdash;decides to split up to go look for clues. They are trying to decide who will investigate the abandoned amusement park and decide to send three people no matter what. At first, Fred says "I'm reading on this map that there's some kibble there!" Scooby perks up: "count me in for sure!" Velma reasons clearly that now they only need to pick two people out of the remaining four: $\binom{4}{2}$. Then, however, she looks more carefully at Fred's map. "Fred, you dolt!" she exclaims. "It says that there's a nefarious cabal there!" "Zoinks!" says Scooby. "Count me *out* no matter what!" The gang now reasons that they now need to choose all three people from only four: $\binom{4}{3}$. Then, however, Daphne takes a look at the map, and notices that the map is all smudged and it's not really clear what it says, so that they might as well stick with the original plan. "Jinkies", says Velma. "This is getting hard to count". Shaggy, in a rare moment of lucidity, points out that if they are considering all search parties of size three *with* Scooby definitely included ($\binom{4}{2}$ = $\binom{n-1}{k-1}$ from before) and all search parties of size three with Scooby *definitely **ex**cluded* ($\binom{4}{3}$ = $\binom{n-1}{k}$ from before), their sum can't possibly differ from the number of search parties that can be formed of size three using all five members (some include Scooby, some don't; all five members are candidates): $\binom{5}{3} = \binom{n}{k}$. This completes the proof. 


```python

```
