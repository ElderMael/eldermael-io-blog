---
title: "Notes On Mental Overhead And Build “Heroes”"
description: "Previously I wrote about problems presented due to a lack of trust between software developers, giving some examples of complex build systems. One topic I did mention is that having a complicated…"
date: "2017-11-28T07:30:47.100Z"
categories: 
  - Software Development
  - Extreme Programming
  - Cognitive Load

published: true
canonical_link: https://medium.com/@eldermael/notes-on-mental-overhead-and-build-heroes-91a29ee46a78
redirect_from:
  - /notes-on-mental-overhead-and-build-heroes-91a29ee46a78
---

Previously I wrote about problems presented due to a lack of trust between software developers, giving some examples of complex build systems. One topic I did mention is that having a complicated build process can create unbearable mental overhead for the programmers working on such a project.

So, what is mental overhead in software development? Simplifying the meaning a bit: it’s the amount of effort you have to put in order to hold the mental model of a software system in your working memory. We also call it cognitive load.

This load is the reason we use such things as abstractions in software: they allow us to offload entire parts of a system of our working memory. It’s also the reason we, as programmers, value simplicity.

#### The Heroes

Going back to my comment about the build process in the previous article, it became so complicated over time, that they had to separate two programmers from the rest to maintain it. Now, I could only deduce those programmers were very good at offloading that problem from others, and they eventually became the“heroes” of the build system. The problem here is not trying to make things simpler but keep things the way they are for the sake of stability; they became so accustomed to such complexity that they started to see it as something ordinary.

I honestly think such thing is never going to be good. This kind of environment makes people very much stubborn due to the perceived stability because when people do not know how to lead, they try to control.

The second problem is that because heroes are mostly regarded in such high esteem, contradicting their opinions is something seen as heretical. While I agree on being opinionated about software development practices, being opinionated while appealing to fear is something I think it’s not productive or useful because it will probably stop disruption and innovation and it’s not the right reason at all.

#### The Two Real Problems: Complexity and Fear

I have seen that complexity becomes fear eventually. These heroes are there to make things safe and stable at the cost of flexibility. I do agree that stability is a good thing, but I also argue that to move forward you must make the trade-off between this and flexibility.  
Recently I had to retract the use of a tool that generated code for our project. This particular CLI became a fantastic accelerator to our team, but unfortunately, the CLI is retired because the company behind it decided to move to a SaaS model. That model did not fit our client’s needs.

Back to when I added this to our project, I also had this fear that the tool was still in milestone phase, but I had to make the trade-off between stability and making our team go fast in a critical moment.  
While now I have to look for a replacement for this tool.

To me, the fact that it allowed us to deliver more quickly on our project at a critical moment, says that it’s worth the trade-off while it lasted.

The problem is that if you focus only on stability and error prevention, acceleration and velocity are not something you give enough credit. Do you remember when DevOps did not exist and now everyone thinks they are the best thing in the world? We as industry realized that DevOps teams are accelerators; they may not be productive in the sense that they do not deliver the product per se, but they do make things happen faster.

Now, if you compare these build hero and DevOps you can see that the first ones have stability as a priority and the later are all about moving forward faster but with the trade-offs in mind so you can get the best of both worlds.

#### Values

In chapter four of “Extreme Programming Explained,” Kent Beck covers the following values: communication, simplicity, feedback, and courage. I believe that If you apply those, you will regularly avoid the problems described in this text. I hopefully will write about those later.
