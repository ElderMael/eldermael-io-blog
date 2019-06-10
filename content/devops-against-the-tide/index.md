---
title: "DevOps Against The Tide"
description: "In retrospect, I can say that I have worked on only one software development project that was successful. Only once have I tasted the victory of deploying code to production (and it was excellent…"
date: "2018-08-16T07:06:18.846Z"
categories: 
  - DevOps

published: true
canonical_link: https://medium.com/@eldermael/devops-against-the-tide-cb545767fe44
redirect_from:
  - /devops-against-the-tide-cb545767fe44
---

In retrospect, I can say that I have worked on only one software development project that was successful. Only once have I tasted the victory of deploying code to production (and it was excellent code!). Most of my career consists of working for different consulting companies of different sizes, and most of my work has been in software delivery contracts, i.e., coding software for clients of different sizes and cultures.

Many things have happened in those projects. One project ran out of budget. Another one was shut down after a year of development because it had constant delays and had 300 people working on it. On another occasion we had an excellent project that was doing very well, but the client decided to change from creating a new platform to “refactoring” (not really) the existing project.

Nonetheless, the defeat that has weighed on me the most was a recent project in which we had to swim against the tide in a DevOps team.

The thing with experience is that it teaches you many things that you shouldn’t do. Moreover, having worked with many different people as a consultant, I have also learned behaviors in people and teams that lead to disaster, antipatterns in social interaction and problems that arise between teams inside the same organization.

In this particular project, I could identify early on many things that immediately raised the red alert.

### Having Separate Teams for Infrastructure and Development

Infrastructure development and DevOps are tightly coupled. I would like to say that DevOps is a philosophy and we need to create a term for infrastructure development, i.e., creating servers, managing clouds, all those things. However, I like to be pragmatic, and most DevOps roles in the industry are seen as infrastructure development first with a touch of tooling for delivering software pipelines.

I think that the worst problem of them all was that the team in charge of the infrastructure and its tooling was a separate team. That team also had very strict idiosyncracies about virtual machines that I can only attribute to a very systemic, operational way of thinking.

For example, as ridiculous as it sounds, we did not have access to create virtual machines ourselves. We were the team in charge of creating and provisioning the infrastructure and platform for several applications yet restricted to the most bureaucratic system I have seen. We had to create tickets for creating servers in bulk. However, even calling this creating them in bulk would be generous, as those servers were snowflakes created manually with very particular and irreproducible processes that were unknown to us.

Because of all of this, reverting the state of those virtual machines was also very challenging. Handoffs and updates of those servers frequently broke functionality.

I have seen the benefits of immutable servers and phoenix servers, but these practices simply eroded under the control of the team in charge of the infrastructure.

### Too Many Chefs in the Kitchen and the Lack of Accountability

The role of Architect in this project lead to many problems during our development. As the team in charge of software delivery pipelines and platform development, we were the glue among teams requiring consensus for delivering working software. There is no way to achieve such a thing with more than 17 software architects with clashing decisions.

We had fruitless, long lasting meetings that lead nowhere. For a place where they value authority so much as to have so many architects, it was impossible to have them accountable for working software.

Conway’s Law applies once more to this project. Most software produced by this organization was a chain of delegation from the top of the hierarchy, in which nothing is achieved, to finally realize the functionality hidden by layers upon layers of indirection and senseless delegation created by developers. This hierarchy is the most rigid pyramid of bureaucracy that I have seen in all my years of software development.

One more thing that I would like to describe is how many architects became the proverbial “Ivory Tower Architect.” They have been away from coding so long that things such as git were alien to them. I also can attest that some of them are the classic example of the Peter principle: promoted not because of software development and architecture proficiency, but because they have been in the same place for years.

### Tools Considered Harmful

Imagine that you are a carpenter. To work efficiently, you need to use specific tools that are intrinsic to your trade. Can you imagine being hired to do some work in a house, arrive and then being stripped of these tools because they are considered harmful by the owners of the house? It sounds ridiculous, but these kinds of policies were in place due to security constraints and some architectural decisions of the software development practice in place.

Many of us know the corporate IT world where the git protocol is considered harmful, so we have to rewrite our URLs to use HTTP instead. However, this is not even the tip of the iceberg; access to Github also was banned because it could contain malicious software. You had to do everything behind the software repository server, so we had to proxy several things such as the central Maven repo, the NPM registry, and Red Hat repositories. We also had to store binaries of different projects for provisioning our CI/CD server with those tools.

More extreme cases required us to repackage software ourselves so we could install it, ask vendors to customize installers for other software so it could be proxied through our repository server. Many packages also had to download binaries from the internet, requiring us to spend several weeks looking for workarounds to download those dependencies, rewrite the software hardcoded URLs, updating them to binaries in the repository server.

I can say with confidence that we spent several weeks customizing software for the sake of security. Unfortunately, most software assumes that the internet is available to download dependencies, but this was not the case in this organization.

### Certificates and PKI, Done Poorly

If there is something that almost any tool uses universally nowadays is Public Key Infrastructure. If you go back to the previous section, you can see that most tools I mentioned require valid certificates to work correctly.

Would you like to make almost everything fail while trying to create a software pipeline or an infrastructure platform? Simply deploy your own PKI, using your own Certificate Authority and require every connection to use an intermediate certificate.

The problem is not PKI itself, the problem is that if you enforce connections to go through this intermediate certificate, you need to have an excellent automated process to provide such certificates. We did not have such nicety. We had to create certificates manually for each server created.

To make things worse, at least in Red Hat, even if you provide certificates in the system-wide repository, some software rolls its own repository or ignores it altogether. For example, we have Java and its Key and Certificate Management Tool. Python also has the certifi bundle of certificates. Jenkins may install its own versions of the JRE (again, Java). You also have the quirks of Red Hat and install certificates. HashiCorp Consul and much other software require certificates from the same CA explicitly set in the configuration.

I think we wasted much time provisioning certificates for every new tool in our hands. I cannot even recount how many times we first had to configure our tools to bypass SSL and then figure a way to provide the required certificate to connect to whatever you needed.

Finally, we had a tool to generate those certificates automatically but we did not use it: HashiCorp Vault. The problem was that it was not under our control, but I will address why this is a problem later.

### The Result Is Low Quality

Software development and the DevOps practice require effort to be successful but when you have all of these technical and organizational problems you are doomed to fail.

One thing that keeps bothering me is that I never had the authority to push best practices and technical decisions forward. Even if I know better, places with such strict hierarchies are impossible to navigate if you are seen as a resource, awaiting orders.

The low quality of the software I had to write, full of workarounds, visibly counterintuitive desitions, is laughable at best. I can only end this saying that technical debt as this, which is not paid in time, is not technical debt at all. It’s low-quality software.
