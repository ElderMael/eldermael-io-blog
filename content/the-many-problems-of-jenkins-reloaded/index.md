---
title: "The Many Problems Of Jenkins, Reloaded"
description: "Having worked with different automation servers for the past 3 years, specifically Jenkins during the last one, I have come to realize several problems with Jenkins coming from experience in the…"
date: "2018-08-25T21:02:53.335Z"
categories: 
  - Jenkins
  - DevOps

published: true
canonical_link: https://medium.com/@eldermael/the-many-problems-of-jenkins-reloaded-371e2769320c
redirect_from:
  - /the-many-problems-of-jenkins-reloaded-371e2769320c
---

Having worked with different automation servers for the past 3 years, specifically Jenkins during the last one, I have come to realize several problems with Jenkins coming from experience in the trenches. In this article, I explain some of them.

### Steep Learning Curve and Frustrating Documentation

I honestly think that Jenkins is hard to learn. Part of this is that the documentation is hard to grasp from a beginner perspective. I attempted to learn Jenkins using the official documentation. I found that it lacked a coherent structure and the examples leave many of details outside. I decided that I was not making any progress trying to follow it after a month reading it.

Finally, I decided to read a book about Jenkins and ditch the documentation for a while. While the book helped me with a structure that I couldn’t find in the docs, my surprise is that even relatively new books leave many features outside while only touching some of them. With this I mean essential features such as how to not stop executors while waiting for user input! I also had the problem of having my Jenkins up to date and finding that the book won’t always match the current documentation or that the documentation has been adding new ways to do the same thing.

### How Do You Automate Jenkins Setup, Seriously?

Working in a cloud-like environment means that I need to create templates/golden images of the software that we use. Remember the whole cattle versus pets concept? How do you do this with Jenkins? Being as old as it is, Jenkins suffers from the design problems of the software designed before the cloud era. By this, I mean that it’s designed for manual installation and you can see for all the quirks that you have to go through when installing it with an automated script.

Creating a Salt Stack state for it was such a nightmare that ended giving up. On my own time, I tried to create first a Vagrantfile and then a Dockerfile to automate this. It is an awful process that involves things such as writing a file with a specific version number to disk so that Jenkins can skip its initial setup wizard altogether (later versions also require running a groovy script at startup to set the correct state). This is crazy, just looks at all the answers that you get in sites such as StackOverflow or Server Fault! I probably have read dozens of questions and articles with the same question.

Another problem is setting up credentials, configurations, and jobs themselves. Even if you have a Jenkinsfile helping you to define the structure of the job, you still need a way to recreate the jobs from scratch. Fortunately, I found that Jenkins has a CLI but the documentation about it hides several details. The CLI itself requires knowledge of how Jenkins stores its configuration (tip, it’s XML) and I would say that is pretty bad itself because there is no documentation about the XML structure of many settings in Jenkins. I also had to read the source code to find the Javaclasses that mapped to it.

Being disappointed by the CLI, I found an OpenStack plugin to create what I needed: job configuration outside Jenkins in source control and that could be automated. However, doing this was not an easy task, and sometimes I thought that it was too difficult to provide this using Jenkins.

I could continue on and on but I think the point is clear: a Jenkins setup is hard to create, maintain and automate. Even with CloudBees support you still have many problems ahead of you.

In conclusion, Jenkins is not built with automation for itself in mind. I hate namedropping so I will mention that there are automation servers that require a single configuration file in YAML to store their configuration (even with secret backends). Others will simply need to backup a directory.

### Jenkins Blue Ocean is Great but Not There Yet

I liked the interface provided by Blue Ocean and the visibility it gives you in contrast of just watching walls of text over and over. However, that is it, if you want to do something else you need to go back to the old UI which is terrible until you get used to it, again, coming fresh to Jenkins does not help, and nothing is intuitive or easy to discover.

I think this contrast with the overall Jenkins experience for beginners and here is my case: I had coworkers that had years of experience with Jenkins, and they all hated Blue Ocean. My opinion is that because they were already used to all the quirks I am talking about, they saw it as a dumbed down version of what they had. While I agree with them on this, this is a testament to how Jenkins was never designed with simplicity in mind.

### The Never-Ending Scripted Versus Declarative Pipeline Problems

Writing this is kind of thematic, old versus new. If you, like me, came to Jenkins just recently, you will find that there are two ways of writing the same pipeline. As with programming languages, the price of freedom is complexity. The freedom that comes with two ways of doing the same thing are many and here are some of them.

Having a mix of engineers with previous experience with Jenkins and people coming fresh to it is a tricky situation, to say the least. The scripted pipeline is full of idiosyncrasies and quirks that do not translate well to the new declarative pipeline. Concepts change names (e.g., Node and Agent), plugins do not have support for the declarative way.

For complex tasks, I had to create blocks of scripts that then we refactored later to pipeline libraries, but we used many plugins created before the new syntax, so there was no way out of script blocks because we created Groovy objects that were needed between stages. It is a hard pill to swallow because there is no clear, intended way to build Jenkinsfiles consistently and the documentation sometimes won’t tell you which syntax they are using and the examples many times do not translate well between them.

### Yes, The Plugin Ecosystem is Awful

Plugins for authentication, locking resources, tooling, Docker (it has many of them), SSH, et. al. You name it and Jenkins will have a plugin for it. Then you find that many plugins do not work well with each other or are complete abandonware.

Is the documentation any better? No, many plugins have only an example or two and many of my coworkers and I had to read the source code (fortunately we were well versed in Java, but other coworkers weren’t).

With the advent of CloudBees and the concept of blessed plugins I think that we are solving this problem but there is a lot of time until Jenkins works smoothly.

At the end of the day we could just replace many plugins with shell scripts but this comes with the penalty of using too much time for something that was promised to work.

### CI/CD Concepts Are Missing

I remember having a project before this one in which one of our principal DevOps engineer convinced managers of using other automation server because Jenkins did not provide concepts such as fan-in and fan-out out of the box.

At first I was skeptical but having worked in a project with a microservice-like architecture I can say that this is a real problem that will cause you headaches. We have to create independent architecture guidelines and tooling to deal with fan-in. This is the reason of projects such as Jenkins Docker and Spinnaker. I remember the now defunct page of Concourse CI comparing the design of servers which had pipelines as first class citizens and the state of Jenkins a year ago (when I started using it). It makes so much sense now because it described it as an afterthought, something that Jenkins was never planed to do but they ended up retrofitting it without thinking that the problem was bigger than that.

### The Conclusion

I wish that in my next project I won’t use Jenkins. Having spent a year doing CI/CD with it I can say that I am tired. Now my coworkers know that I can take any task and do it using Jenkins with success but this was a year long process that was full of frustration and hard lessons. I look forward for the competition to finally be as popular as Jenkins and force it to improve more than just changing its syntax for jobs. I want a Jenkins that does not have all the problems that I mentioned.
