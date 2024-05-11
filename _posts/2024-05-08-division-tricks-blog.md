---
title: "Some useful division tricks"
date: 2024-05-09
---

Working out which multiple-digit numbers are divisible by the single-digit numbers is a very handy arrow to have in one's quiver. In this post, I show how tricks for dividing multi-digit numbers by \\(2, 3, 4, 5, 6, 7, 8, 9,\\) and \\(11\\). I also use this occasion to introduce a bit of modular arithmetic. 

# Division tricks, simply stated

The simple rules we'll start with are as follows. To see if a number is divisible by \\(2\\) or \\(5\\), check if the last digit of the number is divisible by that number, respectively. To see if a number is divisible by \\(3\\) or \\(9\\), see if the digits sum to that number. To see if a number is divisible by \\(6\\), note that it must obey the rules for \\(2\\) and \\(3\\) both. To check \\(4\\) and \\(8\\), see if the final two and three digits of the number are, respectively, divisible by those numbers. The divisibility tricks for \\(7\\) and \\(11\\) are a little more complicated, so I'll show them towards the end. They are informative from the standpoint of modular arithmetic. 

All of the simple tricks can be seen easily just writing out the decimal expansion of a number. Below is how this works for a three digit number without heavy-duty notation.

$$
\begin{align*}
ABC &= 100\cdot A + 10 \cdot B + C \cr
\end{align*}
$$

But, let's generalize. We can write an \\(n\\)-digit number in base \\(b\\) as follows. 

$$
\begin{align*}
a &= (a_{n-1}a_{n-2}...a_0)_{b} \cr
&= \sum_{j=0}^{n-1} a_j b^j
\end{align*}
$$

Note that we only go up to \\(n-1\\) because in all bases, it is conventional to have a "ones place" where the "face value" of a number is its place value also. 

For example, in base or *radix* \\(2\\) and \\(10\\) below, we have...

$$
\begin{align*}
(a_{n-1}a_{n-2}...a_0)_{2} &= \sum_{j=0}^{n-1} a_j 2^j, a \in \{0, 1\} \cr
(a_{n-1}a_{n-2}...a_0)_{10} &= \sum_{j=0}^{n-1} a_j 10^j, a \in \{0, 1, ...9\} \cr
\end{align*}
$$

Now, in base-\\(10\\), the tricks are all pretty self-evident. For \\(2\\) and \\(5\\), we note that the decimal expansion can be written with the last term cleaved off. 

$$
\begin{align*}
(a_{n-1}a_{n-2}...a_0)_{10} &= \sum_{j=0}^{n-1} a_j 10^j, a \in \{0, 1, ...9\} \cr
&= \sum_{j=1}^{n-1} a_j 10^j + a_0 
\end{align*}
$$

Since the first \\(n-1\\) terms to the left are guaranteed to be a multiple of \\(2\\) and \\(5\\) (because each term is multiplied by a power of \\(10\\), which they exactly divide), the number overall will be a multiple of those numbers *iff* \\(a_0\\) is also a multiple of them. 

We can also quickly observe the trick for \\(4\\) and \\(8\\). Since these numbers are \\(2^2\\) and \\(2^3\\), respectively, we know that in base-\\(10\\), the part of the number which is a sum of digits attached to a powers of \\(10^3 = (2\cdot5)^3 = (2^3\cdot5^3)\\) or higher will all be divisible by \\(8\\). So, we just need to worry about powers of \\(10\\) which are too small to be divisible by \\(8\\). This is illustrated below. 

$$
\begin{align*}
&= \sum_{j=3}^{n-1} a_j 10^j + 100a_2 + 10a_1 + a_0 
\end{align*}
$$

So, *iff* the last three digits form a number which is divisible by \\(8\\), the entire number will be as well. 

A similar trick applies to \\(3\\) and \\(9\\). We simply rewrite our previous expression a bit.

$$
\begin{align*}
&= \sum_{j=0}^{n-1} a_j 10^j \cr
&= \sum_{j=0}^{n-1} a_j (10^j - 1) +  \sum_{j=0}^{n-1} a_j  \cr
\end{align*}
$$

Since numbers of the form \\(10^p - 1\\) are always just a string of \\(9\\)s, these numbers are always divisible by \\(3\\) and \\(9\\) both, so the number will be divisible by each number, respectively, *iff* the sum of the face value of the digits is divisible by it. 

Looking at division in this way actually brings us to an interesting point, which is that our standard algorithm for division is only *one* way of doing division. It is computationally efficient and requires minimal knowledge of multiplication facts, but it can hide, in some ways, what happens under the hood when we divide large numbers by smaller ones. 

For example, if you divide \\(867\\) by \\(3\\) using the standard algorithm, you'll first find out how many *hundred*-times \\(3\\) goes into \\(800\\) without going over, which is \\(2\\), and find the remainder, which is \\(200\\). Then, you *implicitly* write that as \\(20 \cdot 10\\) and pair that with \\(60\\) written as \\(6 \cdot 10\\) and find the number of times \\(3\\) goes into it, which is the number of *ten*-times it goes into \\(260\\) without going over, which is \\(8\\). Finally, you take the difference \\(260-240 = 20\\), written as \\(2\cdot 10\\) and pair that with plain old \\(7\\) before dividing and getting an even \\(9\\). 

The advantage of the standard algorithm is that it only requires us to know the standard times table (actually, not even the whole thing!). It does have the effect of *hiding* what we're actually doing, though, which isn't always good. To investigate this a little more, let's learn some modular arithmetic. We'll come back around to the standard algorithm at the end. 

# Modular arithmetic

Modular arithmetic can be thought of informally as the study of remainders. A common example is the math of European clocks. If it is \\(11 \text{PM}\\) when I fall asleep, and I plan to wake up \\(8\\) hours later, I will wake up at \\(7 \text{AM}\\), not \\(19 \text{PM}\\) or something like that. Another useful example is the "sum bit" in a half-adder circuit in electronics: it is the sum not of two binary inputs exactly, but actually their remainder when divided by \\(2\\). 

Let's define the *modulo* operation like this: \\(a \mod m\\) means "find the remainder when \\(a\\) is divided by \\(m\\). The number we divide by, \\(m\\), is called the *modulus*. So, for example, \\(7 \mod 2 = 1\\); \\(99 \mod 5 = 4\\); \\(10 \mod 5 = 0\\). The integer part of the quotient (the result when a dividend is divided by a divisor) is, surprisingly, something that did not have a name in math for a long time and is actually borrowed directly from computer science; Kenneth Iversen introduced it in his highly-influential programming language. We write it as \\(\lfloor \frac{a}{m} \rfloor\\); it is called the "floor" of the fraction, the largest number without going over.[^metaphor]

[^metaphor]: The metaphor seems to be something like this: you might want to reach a point on the wall to hang a picture frame; to do so, you would most commonly, in math and life, start at the floor, then reach up, using a "remainder"; but, you could start at the ceiling and move down, too). 

When two numbers are *congruent*, \\(a \equiv b \pmod m\\), this means that the remainder of each number, when divided by \\(m\\), is the same. An important equivalence here is that this also means the numbers only differ by a multiple of \\(m\\). You can see that algebraically or intuitively. Intuitively, if two numbers *are* multiples of \\(m\\), this means that we can skip between them by hopping some discrete number of steps of size \\(m\\) on the number line. If both numbers \\(a\\) and \\(b\\) are the same distance away from a multiple of \\(m\\) (which is what it means to share a remainder), we can think of this as "we get each number by going to a multiple of \\(m\\), then hopping ahead \\(r\\) hops". Since we get to the first number this way, we don't actually need to take \\(r\\) hops to get the second, only \\(m\\) big steps. 

Algebraically, we write this as...

$$
\begin{align*}
a &= jm + r_1 \cr
b &= km + r_1 \cr
a - b &= jm + r_1 -km - r_1 \cr
&= (j-k)m
\end{align*}
$$

Most of regular algebra works for modular arithmetic as long we keep in mind that congruence replaces equality. For example, we can add the same thing to both sides of the congruence we have. Let \\(a\\) and \\(b\\) be defined as before. Then, define \\(c\\) and \\(d\\) as follows. 

$$
\begin{align*}
c &= pm + r_2 \cr
d &= qm + r_2 \cr
c &\equiv d \cr
a + c &= jm + pm + r_1 + r_2 \cr
b + d &= km + qm + r_1 + r_2 \cr
a + c &= (j+p)m + (r_1 + r_2) \cr
b + d &= (k + q)m + (r_1 + r_2) \cr
a + c &\equiv b + d
\end{align*}
$$

Since \\(a + c\\) and \\(b + d\\) are some multiple of \\(m\\) plus the same remainder, \\((r_1 + r_2)\\), they are congruent \\(\pmod m\\) by definition. Note that it does not matter that the pairs of congruences themselves produced different remainders. The remainders of the new congruences add. Here is a quick example. 

$$
\begin{align*}
12 \equiv 7 \mod 5 \cr
13 \equiv 8 \mod 5 \cr
25 \equiv 15 \mod 5 \cr
\end{align*}
$$

We started with remainders of \\(2\\) and \\(3\\) and ended up with a remainder of \\(0\\), but in each line, the congruence holds true. 

The same trick holds with multiplication. Use the same definitions of \\(a, b, c, d\\) above. 

$$
\begin{align*}
ac &= r_1r_2 + (jmr_2 + pmr_1 + jmpm) \cr
&= r_1r_2 + m(jr_2 + pr_1 + jpm) \cr
bd &= r_1r_2 + (kmr_2 + qmr_1 + kmqm) \cr
&= r_1r_2 + m(kr_2 + qr_1 + kqm) \cr
ac &\equiv bd
\end{align*}
$$

So, we conclude that the two are congruent because they are multiples of \\(m\\) with the same remainder (and the remainders here multiply just as the previous ones added.

This also leads us to the power rule. This is really simple: since we can multiply each side of a congruence by respective sides of any other congruence with the same modulus, we can just multiply a congruence by itself. So, if \\(a \equiv b\\), \\(a^n \equiv b^n\\).

This lets us conclude that since \\(10 \equiv 1 \pmod 3\\), \\(10^n \equiv 1^n \equiv 1 \pmod 3\\), a fact that we actually saw above when we concluded that any number which is a string of nines is just one below the nearest power of \\(10\\). Then, finally, this lets us see the "real" reason for the divisibility rule for \\(3\\). 

$$
\begin{align*}
a_j 10^j &\equiv a_j \pmod 3 \cr
\sum_{j=0}^{n-1} a_j 10^j &\equiv \sum_{j=0}^{n-1} a_j \pmod 3 \cr
\end{align*}
$$

So, this means that the remainder from dividing our number \\(a\\) by \\(3\\), if there is any, is the same as dividing the sum of the digits by \\(3\\). 

## The tricks for seven and eleven

I'll briefly show the trick for \\(11\\). First, note that \\(10 \equiv -1 \pmod {11} \\). Now, this creates a slight problem since the power rule is now \\(10^n \equiv -1^n\\), and the sign alternates. But, this isn't too bad. All we need to do is sum the digits of our number \\(a\\) but attached alternating signs, where the ones digit begins as positive (since \\(-1^0 =1\\). Then, if that number is divisible by \\(11\\), we are in the clear. 

$$
\begin{align*}
a_j 10^j &\equiv -a_j^j \pmod 11 \cr
\sum_{j=0}^{n-1} a_j 10^j &\equiv \sum_{j=0}^{n-1} -a_j^j \pmod 11 \cr
\end{align*}
$$

For example, is \\(561\\) divisible by \\(11\\)? Adding up, we have \\(1 - 6 + 5 = 0\\), which is, indeed, technically a multiple of \\(11\\) (and the answer is correct if we check by hand)&mdash;this case is actually not unusual. But, an example where the sum is a "regular" multiple of \\(11\\) might be \\(209,021,505\\), which has a sum of \\(22\\) when its digits are tallied (it is \\(11\\) multiplied by \\(19,001,955\\).

Finally, let's show the trick for dividing by \\(7\\). This one is a little complicated to state. For a number \\(a\\), lop off the units digit \\(a_0\\) and double it. Then, shift the remaining digits over to the right by one place; call this number \\(a_{\text{short}}\\). Subtract \\(a_{\text{short}} - 2a_0\\); if the resulting number is divisible by \\(7\\), our original number is, too. 

Although we can show this proof without modular arithmetic, the proof without them is just as "gnarly". Notably, this is the only trick that I think is probably slower than actually dividing unless the numbers are very big. Suppose our number *is* divisible by \\(7\\). Then...

$$
\begin{align*}
a &= 10\sum_{j=1}^{n-1} a_{j} 10^{j} + a_0 \cr
\sum_{j=1}^{n-1} a_{j} 10^{j} &:= a_{\text{short}} \cr
a &= 10a_{\text{short}} + a_0 \cr
-21 a_0 &\equiv 0 \pmod 7 \cr
a &\equiv 10a_{\text{short}} - 20a_0 \pmod 7 \cr
a &\equiv 10(a_{\text{short}} - 2a_0) \pmod 7 \cr
\end{align*}
$$

From the last line, we conclude that, although we cannot actually divide out the \\(10\\) immediately according to the laws of modular arithmetic, it is clear that the multiplication by it cannot change the divisibility by \\(7\\). So, if \\((a_{\text{short}} - 2a_0)\\) is a multiple of \\(7\\), the number is. 

An example is \\(7^4 = 2401\\). Lopping off the final digit, doubling, and subtracting it off the shortened number, we have \\(240 - 2 = 238\\). We can always iterate the algorithm if we like. Here, that is probably not necessary for many students, but just to show that it works, we can find \\(23 - 16 = 7\\). 

## The standard long division algorithm

Let's return to example we used the standard algorithm to solve, but now from a different angle, using just a bit of algebraic simplification.

$$
\begin{align*}
867 \div 3 &= (8\cdot10^2 + 6\cdot10^1 7\cdot10^0) \div 3 \cr
&= \frac{8\cdot10^2 + 6\cdot10^1 + 7\cdot10^0}{3} \cr
&= \frac{8\cdot10^2}{3} + \frac{6\cdot10^1}{3} + \frac{7\cdot10^0}{3} \cr
&= \frac{8}{3}\cdot10^2 + \frac{6}{3}\cdot10^1 + \frac{7}{3}\cdot10^0 \cr
&= \left\{\lfloor\frac{8}{3}\rfloor + \frac{8 \mod 3}{3} \right\} \cdot10^2 + 
    \left\{ \lfloor \frac{6}{3}\rfloor + \frac{6 \mod 3}{3} \right\} \cdot10^1 
    + \left\{\lfloor \frac{7}{3} \rfloor + \frac{7 \mod 3}{3} \right\} \cdot10^0 \cr
\end{align*}
$$

So, we can see that division of multidigit numbers by a single digit number can really just be represented as the sum of divisions of the multidigit number written as a linear combination in base-\\(10\\).

Starting from the penultimate line above, the standard algorithm works by working left-to-right, finding the floor and the remainder (the result of the *modulo* operation). The floor is "multiplied" by the correct power of \\(10\\) by simply writing it as the left-most digit in the standard algorithm. The remainder is "multiplied" by the correct power of \\(10\\) by (behind the scenes, as it were) first multiplying through by \\(10\\) *once* to turn it into a number larger than the divisor and then allocating the second \\(10\\) so that we can move the remainder into the next fraction. 

$$
\begin{align*}
&= \lfloor \frac{8}{3} \rfloor \cdot10^2 + \frac{20}{3}\cdot10^1 +
    \frac{6}{3}\cdot10^1 + \frac{7}{3}\cdot10^0 \cr
&= 2 \cdot10^2 + 
    \left\{ \lfloor \frac{20 + 6}{3}\rfloor + \frac{26 \mod 3}{3} \right\} \cdot10^1 
    + \frac{7}{3}\cdot10^0 \cr
&= 2 \cdot10^2 + 8 \cdot 10^1 + \frac{20}{3} \cdot 10^0 
    + \frac{7}{3}\cdot10^0 \cr
&= 2 \cdot10^2 + 8 \cdot 10^1 + 9 \cdot10^0 \cr
&= 289
\end{align*}
$$

Notice that we could, however, just find the floor of each simple division, multiply it and the remainders out, sum them, and then divide at the very end. Using our running example, here is how that would look. 

$$
\begin{align*}
&= \lfloor \frac{8}{3} \rfloor \cdot10^2 + 
    \lfloor \frac{6}{3}\rfloor \cdot10^1 
    + \lfloor \frac{7}{3} \rfloor 
    + \frac{8 \mod 3 \cdot10^2 + 6 \mod 3 \cdot 10 + 7 \mod 3}{3}
\end{align*}
$$

The actual intuition here is worth noting. The standard algorithm may be more efficient, but it distracts, to some extent, from what division really does. Here, what we do is go to each digit in the number, calculate the floor and the remainder for the simple one-digit-by-one-digit problem (while noting the power of ten to which they're attached), sum up the floors, and then gather up the remainders and divide them by our divisor. Notice that we could also, if we knew things offhandedly such as \\( \lfloor \frac{800}{3} \rfloor \\), find the remainders without even needing to multiply by powers of \\(10\\). 

The main reason that this is useful, however, is in connecting modular arithmetic to our informal rules given above. Crucially, we will see that if there is no remainder \\(r\\) for the division of some dividend \\(d\\) by some divisor \\(m\\) at each place in the decimal, there is (obviously, now) no . So if we can show that each term in the decimal expansion leaves no remainder, this is enough to show that it is a perfect division. We can express that like so. 

$$
\begin{align*}
\frac{(a_{n-1}...a_0)_{10}}{d} &= \sum_{j=0}^{n-1} \frac{a_j}{d} 10^j \cr
&= \sum_{j=0}^{n-1} \left\{ \lfloor \frac{a_j}{d} \rfloor + \frac{a_j \mod d}{d} \right\} 10^j \cr
&= \sum_{j=0}^{n-1} \lfloor \frac{a_j}{d} \rfloor + \sum_{j=0}^{n-1} \frac{a_j \mod d}{d}10^j \cr
\end{align*}
$$

Notice that in the final expression, the floor operator can *only* produce integers, so we only need to look at the sum of the remainders of each term in the linear combination view to determine whether a number \\(a\\) is divisible by \\(d\\) or not. This means, importantly, that if we can somehow determine that each term in the expansion has a remainder of \\(0\\), the division works.

# Further reading (AKA whence I swiped this)

Arthur Benjamin's *Magic of Math* (a perfect review of all the math you forgot, had explained poorly, or never learned but should have, from arithmetic to single variable calculus)

Martin Weissman's *Illustrated Theory of Numbers* (a beautiful text about number theory)
