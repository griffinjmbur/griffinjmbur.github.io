If you are an undergraduate or graduate student in the social sciences, you will probably at some point be asked to learn some statistical software. And, you might be surprised to learn that Microsoft Excel is usually not understood to meet that criterion. Instead, you'll be asked to learn Stata, R, Python, or maybe SPSS or SAS.

New users of those languages often experience initial frustration in learning them. The data are so *visible* in Excel, so tactile. Writing scripts in those other languages can be frustrating and time-consuming. Why should anyone bother? And, even if *some people* should bother, why should you, a non-computer scientist? Here is a list of reasons, spanning from the very practical to the humanistic. 

0. **Excel and similar programs (e.g. Sheets) are extremely error prone**. 

    The flexibility of these "point-and-click" languages is also their downside. It is *very* easy to modify data in them&mdash;too easy! The horror stories are legion. The most famous is the Reinhart-Rogoff disaster, a tale that has been told many times. You can read good write-ups many places (I suggest [here](https://archive.ph/OGzrG)), but TL;DR: Nobel Prize-winning economists made a disastrously-wrong analysis of the relationship between debt-load and GDP growth that changed global policy. The culprit? Simple Excel mistakes. Are you more interested in clinical research? Head [here](https://www.youtube.com/watch?v=64paAOI17rc&t=413s) (timestamped) to hear about a major scandal in cancer research cause, in part, by Excel errors. Matthew Zeitlin catalogued a very entertaining [list](https://archive.ph/cLyX5) of "data disasters" *more embarrassing than Reinhart and Rogoff*!
    
    There is a direct relationship between having the data in front of you in a way that makes them *effortless* to manipulate and embarrassing blunders such as these. Data manipulation should be *easy*, not *effortless*. 
    
    Beyond that, there aren't just *data-management* goofs. People also tend to forget 
    
1. **Excel and similar programs (e.g. Sheets) are extremely inefficient**. 

    Even if you are an excellent typist and don't make many data-entry errors, script based languages are vastly more efficient. Once you figure out a procedure that you want to carry out, you just add it to your script. At a basic level, you may not have much need to use custom statistical procedures, but at a minimum, correctly titling and scaling graphs
    
2. **Script-based languages teach logical thinking useful in many jobs**. 

    Learning to code is the process of *learning to communicate unambiguously*. Much of what we think of as digital technology is only *possible* in the first place because it is possible to reduce our beautiful, complex human languages to boring, binary statements about the world. This might sound like a pessimistic view, and life would be grim were this the *only* kind of communication possible, but it is an essential component of our world. 
    
    Many, many white collar jobs require you to be able to problem solve in a way that computational thinking generally promotes: to solve computer problems, you need to work step-by-step, isolate and debug problems while keeping what works, reverse engineer, and picture solutions in an abstract way before implementing them. These are precious skills. Once you see the structure of a `for` loop, you'll find yourself picturing all kinds of problems this way: "I want to iterate over all the customer names in our database, multiply their credit score by $100$, add it to a list, but skip people if they are duplicates". This is a common sort of problem that is easy to solve if you have language for it, but can take hours
    
    Moreover, many jobs require specific facility with data: being able to understand the structure of your information.
    
3. **Script-based languages help you think clearly about math**. 

    For the sake of a math class, even if you intend to never take one again, a programming language can help you get through the course. Many operations in math that frighten novices, e.g. summation notation, are the equivalent of basic and fun-seeming operations in programming, such as [loops](https://archive.ph/0zKoy). It is also very quick to check properties you might suspect to be true using programming. "Does the variance change when I center a variable?" You can prove to yourself that it *doesn't* pretty easily, but before you waste time doing so, why not check and see whether it appears to be true *empirically* first? 

4. **Computers are essential to our world; from simple file management to AI, they are part of our every day life. Script-based languages force you to learn the basics**. 

    Before we even talk about AI, we need to discuss something a little uncomfortable: many people *in the technical sciences* don't even understand the basics of how computers work, even if they are very proficient users of the internet or certain software,[^twt] and that's (at least potentially) a problem. 
    
    [^twt]: A PhD student in computer science recently had a tweet go viral, with some $8000$ likes and $1000$ times as many views, that they "didn't know that the entire internet is literally wired physically with undersea cables". This actually isn't as absurd as it seems; a big part of computer science is "black box" thinking, where you can be proficient at one level of technical abstraction while not needing to know the details of another. But, predictably, this can get you into trouble at some points.
    
    I'm not talking about understanding deeply, like knowing exactly how programming statements in a language (like Python or C) get turned into binary which gets turned into a bunch of tiny little switches flipping on and off. By the way, if you do care about that, this [series](https://www.youtube.com/watch?v=tpIctyqH29Q&list=PL8dPuuaLjXtNlUrzyH5r6jN9ulIgZBpdo&ab_channel=CrashCourse) produced by Carrie Anne Philbin is an excellent introduction, and for the brave, see Nisan and Schocken's *Elements of Computing Systems*. 
    
    I'm talking about things like "what on my computer is 'the internet' and what is just 'on my computer'", something that you really need to know in *any white-collar job* and something which many people today do not know. I'm talking about things like "how can I even begin to find files on my computer". I'm [far from the first person](https://archive.ph/8come) to notice this decline. In a way, this is simply a predictable consequence of computers getting *better*, in the same that many people today probably know *less* about cars than they used to (which is also, I think, probably not good; at best, it's netural). 
    
    What does all of this have to do with learning a script-based language? It's a pretty general but robust connection: you simply have to get a crash course in this stuff in order to begin thinking about scripts. Where do I pull data from? Where do I save my results? These are questions that you have to answer every time you write a new script. These are things that you simply have to learn in order to do well with basic programming.

    A brief word on AI: artificial "intelligence" is not the eschaton. I don't even think it is *intelligent*. But, it is very useful. You might be surprised to learn that souped-up versions of the statistics and computing that you learn in a basic social science statistics class are basically all that AI really is. Of course, that's a big oversimplification, but it's more right than wrong. 
    
5. **It's fun and can be useful to you recreationally**. 
    
    Information is *everywhere*. This is a double-edged sword; the presence of "big data" in some ways corrodes our folkways. At the same time, you can make the most of it. Your phone, for example, records all kinds of data on your movements, and many of you may use watches or other biometric feedback devices. Perhaps you track your macros, map your runs and walks, or track your sleep. Perhaps your phone automatically counts your steps without you even *realizing*. Now, again, there are some Foucauldian reasons to be very fearful of all of this, but if you can't fully opt out...why not have some fun with it? Of course, actually manipulating these data with any facility beyond just what you can do in the app (typically extremely limited) requires programming. 
    
    Here's an example: I got back into running outside during the pandemic and tracked most of my runs. I also 
    
![""](./images/running.png)
![""]("./images/running.png")
