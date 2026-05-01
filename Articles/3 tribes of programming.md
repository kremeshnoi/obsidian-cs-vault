---
title: 3 tribes of programming
source: https://josephg.com/blog/3-tribes/
author:
  - "[[Joseph Gentle]]"
published: 2017-05-03
created: 2025-11-02
description: There's an old joke that computer science is a lie, because its not really about computers, and its not really a science. Funny joke. Everyone laughs, then someone says "Yeah but it sort of is about computers though, isn't it?". Feet shuffle awkwardly. Someone clears their throat and before you
---

#articles

# 3 Tribes of Programming

There's an old joke that computer science is a lie, because its not really about computers, and its not really a science.

Funny joke. Everyone laughs, then someone says "Yeah but it sort of is about computers though, isn't it?". Feet shuffle awkwardly. Someone clears their throat and before you know it you're talking about Category Theory and looking up the [history of the word algorithm](https://en.wikipedia.org/wiki/Algorithm#Historical_background).

Out in the wild, these arguments look like this:

The speaker was making the point that the whole modern stack in our computers (Kernel, OS, browser, VM) is written in C + ASM. So you should know C and ASM.

Is that really important? Serious question, are programs foremost lists of instructions, or expressions of logical ideas?

> [!note]
> This question — whether programs are fundamentally instructions, mathematical expressions, or human artifacts — is the axis along which the three tribes diverge. Most disagreements in software culture trace back to this split.

Or maybe its neither, and programs are just things we *make* for other *humans*. A message is fundamentally meaningless without an audience who reads it. Are programs meaningless without reference to the outside world they interact in?

A friend bragged to me once about how he could prove that most programs were correct and completely bug-free using Ada. I asked him if he could prove that this function was correct:

```
fn sub(a, b) { return a + b }
```

He said "Of course, thats easy". So I asked how his prover would discover that the function had the wrong name, and he got delightfully flustered.

## Tribes

Programs, obviously, hold all of these properties. But I think there's fundamentally 3 architypes of *programmers*, divided by which ideals we hold in highest esteem:

- You are a poet and a mathematician. Programming is your poetry
- You are a hacker. You make hardware dance to your tune
- You are a maker. You build things for people to use

We self-select into communities of our peers based on these ideals. We use coded language to express these ideals to our peers.

I think each group has its own conferences, and its own subreddits. Its own programming languages, heroes and its own villains.

### Programming as applied mathematics

The first camp views programming is fundamentally an expression of thought - a kind of mathematical poetry which we can gift with life. The fact that we execute them on von Neumann machines is an implementation detail.

With this mindset, these details are important:

- **Source code**: The source should read like poetry - dense, with very few lines of code needed to express an idea. Once understood, the terse program seems like a beautiful and obvious description of your program. It is more important that the source code is simple than the execution is simple or fast. High level languages are better than low level languages because they let you express your intent more clearly.
- **Execution**: How the program is executed by the computer is an implementation detail of the compiler. It is more important that the code is simple than the execution is fast.
- **Correctness**: A program is correct if it implements the spec exactly. The best programs use tools like Ada to formally prove correctness.
- **UI**: How the code interacts with humans is a separate consideration from its implementation. Beautiful code is more important than beautiful UI.

Examples: [Rich Hickey](https://www.youtube.com/watch?v=f84n5oFoZBc), [Brett Victor](https://vimeo.com/36579366)

These programmers are probably the least common, although that might be because its hard to get a job working like this. [Haskell](https://stackoverflow.blog/2017/02/07/what-programming-languages-weekends/?cb=1) has the highest weekend/weekday usage ratio of all languages on stackoverflow.

Most (arguably all) of the modern advancements in programming languages come from people in this camp. If you've used React to make a website, you should know that the model of immutability and expressing your view as a pure function from data to DOM came from functional programming. Actually, most modern language features are invented by people who think of programming as thought. Years (or decades) later, those features get copied into the more popular languages and get treated as new ideas.

I have a friend who spent months loving [J](http://www.jsoftware.com/). He eventually wrote a little game in J. He described his code as this perfect, beautiful crystal. Later he wanted to make it multiplayer - but to do that he would have to deal with lag. And that would require ripping apart some of the beautiful internal flow. He couldn't stomach it, so instead he abandoned the project entirely.

That story is *funny*, but I'm a little jealous of my friend. I bet he learned a heap and had a great time. Experiences like that make us better programmers.

I did a Haskell short course late last year and I challenged the main instructor. I told him "this is all well and good, but I bet I can still make useful software using my practical languages faster than you can". He said no way - using haskell he was convinced he could implement anything I could implement, faster and better and with less code. We didn't test the claim - but I still wonder - is he right?

**Favorite languages**: Haskell, Lisp, ML (Ocaml, etc), Closure, ADA

**Hangouts**: FP meetups, [Lambda the ultimate](http://lambda-the-ultimate.org/), Strange Loop, [Research](https://www.data61.csiro.au/).

And of course, [Steve Yegge making fun of this tribe](http://steve-yegge.blogspot.com.au/2010/12/haskell-researchers-announce-discovery.html)

### Programming as hardware hacking

The second camp views programming as fundamentally tied to the *machinery* of the computer. No program is run without a computer, therefore to program effectively we must keep the computer in mind at all times - hardware and software.

Elegance and beauty come not just from a simple code base, but by that codebase using the hardware in an elegant and efficient manner.

Thus, elegance like this:

- **Source code**: The code should be clean, but clean code is less important than a clean execution. Low level languages are often better than high level languages because you can be more explicit about *what the computer will do* when it executes your code. (Thus you have more room to optimize).
- **Execution**: How the computer executes your code is paramount. Programming without thinking about execution is just begging for slow performance.
- **Correctness**: A program is correct if it runs the way you expect it to run, given normal parameters. Execution elegance is more important than correctness. And if a theoretical issue can't happen in practice due to how the machine works, [its not a real bug](https://twitter.com/pomeranian99/status/858856994438094848). A program must be adequately fast to be considered correct.
- **UI**: How the code interacts with humans is a separate consideration from its implementation. Its ok to let the constraints of the hardware guide the user experience.

Example: [Poul-Henning Kamp](http://queue.acm.org/detail.cfm?id=1814327), [Michael Steil](http://www.pagetable.com/), [The 8-Bit guy](https://www.youtube.com/watch?v=Tfh0ytz8S0k)

The key here is thinking about the entirety of the computer and your running program, together. According to this community, the best (only?) way to write good software is to think holistically about how it will run, and how our program will interact with the rest of the hardware and software. Doing that well achieves [mechanical sympathy](https://groups.google.com/forum/#!forum/mechanical-sympathy) and everything runs like a well oiled clock. The joy is like that of driving a manual car that you can *hear* and *understand*.

Anything that obfuscates how the computer will execute your program is dangerous for the implementor - because it adds more variables to consider. Thus, people in this camp often deride garbage collectors, or the churn from JS performance benchmark results changing how we should write our code. Undefined behaviour in C compilers is an [ongoing point of contention](https://github.com/sandstorm-io/capnproto/blob/master/security-advisories/2017-04-17-0-apple-clang-elides-bounds-check.md).

In modern app development our computers are fast enough that this kind of thinking isn't really important any more. A few decades ago you needed a deep understanding of how the computer works to write software. But now basically any language you use is fast enough, so why bother learning C? Most web developers I know don't know any C, and have no interest in learning about pointers or manual memory management.

But this sort of work is still hugely valuable in lots of areas. The game development community still writes most code in C++ (though unity is slowly changing that). Security engineers need a systematic understanding to find vulnerabilities. Embedded systems engineers can't afford to waste cycles and RAM, and once backend systems get big enough performance starts mattering again.

And even when its not practical, but being forced to think about the machine can be really *fun*! For example, the [PICO-8](https://www.lexaloffle.com/pico-8.php) imposes arbitrary 'hardware' limits to force you to be clever when designing your games.

To this community we owe almost all performance improvements in our computers, above and beyond what is demanded by customers. Nobody else cares about performance quite like people who think about the hardware all day. But if you're thinking about your computer as a machine, what greater ugliness can you inflict than pointless work?

I'm really curious if Rust will take off amongst this community. Rust is essentially a language designed *by* language nerds in the first camp above, *for* people who care about runtime efficiency. Will they take to it? Will future game engines be ported to rust?

Conflicts with the first group:

- Mutability (memory is fundamentally mutable / but it makes our programs harder to understand)
- GC (it makes your program slow and janky / but less buggy!)
- Abstraction (you're making your program harder / easier to reason about)

**Fav languages**: C, C++, Assembly.

**Hangouts**: Hackerspaces, Game dev shops, database companies, CCC, Defcon.

And here's Brett Victor [making fun of this tribe](https://vimeo.com/71278954).

### Programming as a tool to make things

The final group see programming as a means to a beautiful end, not something made beautiful through its construction. The way people in this camp describe themselves is fundamentally pragmatic. They write software because software is useful to other people.

- **Source code**: The code should be clean, but only because cleaner code is easier to iterate on. Code cleanliness is less important than most other considerations.
- **Execution**: The program only has to be fast enough for the users. If you make it even faster, you're taking time away from adding features that people care about more.
- **Correctness**: Bugs are bad only in proportion to their impact. The program should act the way the users expect it to act.
- **UI**: The UI is more important than anything else. Every other part of the program only exists in service to the user interface.

I think most professional software engineers are in this tribe - which makes sense, because this is the place where it is easiest to make money writing software.

In my experience people in this camp are better at community. They seem to be much more positive and encouraging of new members, and willing to help. I guess its because you can tell if you're doing a good job in the other two camps by simply taking a look yourself. If you make software for other humans, satisfaction comes from making the people around you happy.

I can't help but feel that this place is a touch soulless. Taken to the extreme, this world view doesn't value the beauty in the engineering itself. Although you could probably make the opposite criticism against the other groups - they don't value how their software can impact the world.

There's a lot of tension between this camp and the other two camps I've talked about. And it can get a bit mean. I know many product people who feel self conscious about their lack of knowledge of traditional data structures and algorithms. They feel judged by "real" programmers because they can't implement obscure algorithms and binary framing protocols. The way people in this tribe see it, other people have already implemented all that stuff anyway. So who cares?

Thats fair, but its also true that lots of issues are caused by the lack of technical skill amongst frontend engineers. This is mostly self correcting - if your program is too slow, you know about it and can fix it. But security engineering is a real danger. If you don't know how to secure the software you write against hackers, [its probably not secure](https://www.troyhunt.com/reckon-youve-seen-some-stupid-security-things-here-hold-my-beer/?utm_source=hackernewsletter&utm_medium=email&utm_term=fav). And you might not know its a problem *even if you get hacked*.

Here's an example of this conflict playing out on twitter:

For context, Jonathan Blow (famous indie game developer) is saying that if you can't reverse a binary tree you're not a good developer, even if you write useful software every day.

Is he right? Well it depends on what 'good developer' means, and that depends on which tribes you care about. I think Blow [is in camp 2](https://www.youtube.com/watch?v=gWv_vUgbmug), so you're judged based on how much you know. @jdan is in camp 3, so he's judged based on what he's made. Jonathan Blow certainly writes useful software, but one of the reasons his last game (The Witness) took so long to write was that he wrote his own engine instead of using something off the shelf. When [asked about it](http://the-witness.net/news/2011/06/how-to-program-independent-games/comment-page-2/#comment-3655) (emphasis mine):

> I don’t know very much about Unity. However, it’s clear that one could not build The Witness in Unity without rewriting a lot of Unity (or adding a lot of things that are not there, and declining to use most of what Unity provides). And we already had a simple graphics engine anyway. So **when building our own systems, we can ensure that they are really what the game needs to be its best**.

I suspect he's wrong about the first part. But I'm mostly in camp 2 myself, so I understand wanting to write your own engine anyway. I probably would have done the same thing.

**Fav languages**: Whatever gets the job done. JS, Ruby, Python, Swift, C#.

**Hangouts**: Twitter, SydJS, StackOverflow, A Company Near you!

And of course, Gary Bernhardt [making fun of this camp](https://www.destroyallsoftware.com/talks/the-birth-and-death-of-javascript).

## A quiet war

I think a lot of the conflicts and disagreements in our community can be expressed in these terms. And a lot of the misunderstandings between programmers.

For example, what should your programming language do when an integer overflows? If you think of programming like mathematical poetry, above all else it should give you the mathematically correct result.

Haskell (first camp):

```
λ: 23^23
20880467999847912034355032910567 :: Num a => a
```

Vs C (second camp):

```
printf("%llu\n", 1 << 100); // overflows. Prints 0.
```

And if you just want to ship a product, you don't care. In javascript (third camp), there is no integer type at all. JS just uses floats for everything. And if they overflow, tough luck.

Rust is trying to put one foot in each of the first two camps - be a language made by programming language nerds but which compiles to efficient code. And unsurprisingly, this problem generated a long argument in the rust community. The final solution [was this](http://huonw.github.io/blog/2016/04/myths-and-legends-about-integer-overflow-in-rust/), where by default overflows cause panics to be thrown in debug mode, but silently work in production mode.

Rob Pike (author of Go) was confused about which tribe his language is trying to appeal to. He [wrote this](http://commandcenter.blogspot.com.au/2012/06/less-is-exponentially-more.html) a couple years after Go was released:

> I was asked a few weeks ago, "What was the biggest surprise you encountered rolling out Go?" I knew the answer instantly: Although we expected C++ programmers to see Go as an alternative, instead most Go programmers come from languages like Python and Ruby. Very few come from C++.

Why? Well C++ programmers are largely in camp 2 above. They want to know how their code will run. But Go has a garbage collector, and a fast compiler. Really, Go cares about getting out of your way so you can just make stuff. Its a language for people in the last camp, who want to build products. What languages do people who care about that currently use? Python, Ruby and Javascript. So of course they're who Go is converting.

## Closing

Next time you see an argument about whether Javascript is a cancer or a boon to our industry, or you see someone like me getting angry about [modern apps being crap](https://josephg.com/blog/electron-is-flash-for-the-desktop/), ask yourself which camp is speaking. Are they championing beautiful code? Performance and a "deep understanding"? Or do they just want to get work done and ship product?

Ultimately code is code. Even though we have different reasons for writing software, what we write is (usually) compatible. And even when its not (looking at you, Haskell) - there's always a lot of ideas we can learn from and steal.

We all owe each other a lot, after all. Without language wonks we would still be writing assembly. Without systems programmers we wouldn't have operating systems, and haskell and javascript would be unusably slow. And without product engineers, everyone else would be forced to write CSS. And trust me, nobody wants that.

Rear Admiral Grace Hopper managed to bridge machine understanding and product thinking, and in doing so invented the idea of a machine-independant computer language. Without being able to think both about what the computer can do and what we *want* the computer to do, that wouldn't have been possible.

> [!tip]
> When you find yourself in a heated debate about tools, languages, or architecture, ask: which tribe is each person speaking from? Recognizing this often dissolves the argument — the disagreement is about values, not facts.

But personally I think we should aspire to be like [Alan Kay](https://www.youtube.com/watch?v=YyIQKBzIuBY) and do all three. Him and his team regularly crosses multiple tribal lines. As an example, he invented object-oriented programming from watching children learn Squeak and Logo. He thinks there's ways we can have our cake and eat it too - using modern techniques to engineer much simpler systems that are faster, more elegant and more useful for humans. If you haven't done it already, you should watch every talk he's ever given. Do it *slowly*.

Thats certainly what I aim for. And hopefully I'll still be blowing people's minds past the age of 70.