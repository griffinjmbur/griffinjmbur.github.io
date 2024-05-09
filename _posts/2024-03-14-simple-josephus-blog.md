---
title: "A straightforward discussion of the Josephus problem"
date: 2024-03-14
---

In this post, I offer a straightforward discussion of the Josephus problem, discussed  in Graham, Knuth, and Patashnik's *Concrete Mathematics*. I follow their discussion but present in a more straightforward manner, foregrounding a simpler and brilliant proof found in one of the infamous marginal comments.

# The basic set-up of the problem

This is a puzzle with a long history. I won't recap it here; Daniel Erman (of my own University of Wisconsin) gives a bit of history in this *Numberphile* [video](https://www.numberphile.com/videos/the-josephus-problem), which also has an animation, if you like that. The basic idea is that members of a captured army are committing suicide rather than be taken as slaves by the conquerers. They commit group suicide by having some members of the group kill every \\(k\\)th person; this ultimately leaves survivors, however. Josephus, an early Jewish historian (who is evidently famous for many other reasons), allegedly survied just such a situation by calculating who the survivors would be. The simplest case and one you'll often see online is for \\(k=2\\), i.e. every other person is killed. We'll use that one.

# How to solve it

This problem, like the Tower of Hanoi and the lines in a plane problem (classics from the first chapter of *Concrete Mathematics*), is actually easier the *less* you get absorbed in the details. In other words, you need to focus on very abstract parts of the process; you should actually trace out how this would work for a few small cases, get a feel for the problem, but then realize that you need to actually look at this from an eagle's view and blur some of the details. 

We can work out a simple case such as \\(n=10\\), letting \\(J(n)\\) indicating the survivor number for a group of \\(n\\). Again, \\(k=2\\). In this case, persons \\(2, 4, 6, 8,\\) and \\(10\\) go in the first round; our survivor set is \\(1, 3, 5, 7, 9\\). Now, we have to skip person \\(1\\) and go to person \\(3\\); we skip person \\(5\\) (since he's not *second* from person \\(3\\) anymore: person \\(4\\) is dead); person \\(7\\) goes. Our survivor set is now \\(1, 5, 9\\); we skip ahead now to person \\(1\\) then person \\(9\\), and so person \\(5\\) is the survivor. 

One insight here is that we obviously will knock out all even numbers on the first run through. But there are some complications. For example, if \\(n=7\\), we omit \\(2, 4, 6\\), but then \\(1\\) *is* the second person in this example. The [*Wikipedia*](https://archive.ph/XSHxf) page for this problem shows some very detailed, computation-heavy ways to visualize how people's index number changes after each round. This is 1) complex and 2) although not impossible to generalize about, difficult.

# The clever solution

As a marginal comment in Knuth *et al.* notes, however, the problem is much simpler than it is often made. There is no obvious way to continue with that (fairly tedious) style. There are a lot of interesting patterns and problem-solving lessons in longer-winded solutions of the problem, but there is, I think, a lot of merit in showing how the problem might be solved quickly. 

Here is the simplest proof for the right answer. The problem is actually quite easy with powers of \\(2: J(2^m) = 1\\). How? The key is that the first round knocks out half of the participants any time there is an even number of people \\(n\\) to begin with; if \\(n = 2^m\\), this is true for *every* round. This is good news because now the various rounds are in some sense isomorphic: if we have \\(n=64\\), after the first round, we will have \\(n=32\\) people, and each surviving person's first-round index number \\(j\\) yields a second-round index number \\(k=\frac{j+1}{2}\\); e.g., lucky survivor \\(j=7\\) is now, unfortunately, person \\(k=4\\). 

The key to seeing why \\(J(2^m) = 1\\) is to note that we never have an odd number of people remaining after any given round when \\(n=2^m\\), so person \\(j=1\\) is never second from the person killed last in round \\(p\\)&mdash; any round \\(p\\) ends with the final person to survive to that round being killed because we have an even number of people. Another way to see this is to work upwards. \\(J(2^0)\\) is trivially \\(1\\); \\(J(2^1)\\) is, also obviously, \\(1\\)...but then, less obviously at first, \\(J(2^2) = 1\\) (persons killed, in order, are \\(2, 4, 3\\)). The reason is actually that, after round \\(1\\) of \\(n=4\\), we have half the people left...so this is just \\(J(2)\\)&mdash;the index numbers change, but since it doesn't change for person \\(1\\), they are still the winner. 

This might seem like a nice fact that falls short of a solution. \\(\mathfrak{O, ye, of little faith}\\). We can represent any number \\(n\\) in pseudo-binary as \\(2^m + r\\). The proof is now startlingly simple. Once \\(r\\) people have died in round \\(1\\), we have \\(2^m\\) left, so we simply imagine shifting our frame of reference so that the cycle starts *now*; since the number of people standing is even and our group can now be halved evenly each time (as it is a perfect power of \\(2\\)), that person will never be next-after-the-final-person. So, who is this person exactly? To kill \\(r\\) people during the first round, we have to move to person \\(2r\\) (since we skip every other person) and then add \\(1: J(n) = 2r + 1\\). That is all there is to the problem.

This solution, much like the general solutions to the Tower of Hanoi or the partition of the plane, does not make it any more obvious how the intermediate steps might look, even if those are soluble (here, when person \\(k\\) will go; with the Tower, how we might, say, write a loop that solves the Tower, i.e. writing down the general form of the intermediary steps). But, fortunately, in both cases, that is not the problem we wish to solve. And, as a problem-solving lesson, it is instructive: this is a lot like the "fly puzzle" (found, first by me, in Jacobs' *Puzzler*, though I saw it in Epstein's *Thinking Physics* in the next year) or the Moscow puzzle involving two trains. Legendarily, John von Neumann solved the fly puzzle the "smart" way (with an infinite series), which is really not all that hard, but it is, in one sense, kind of dumb to not see the simple way (I tried to solve it von Neumann's way). 