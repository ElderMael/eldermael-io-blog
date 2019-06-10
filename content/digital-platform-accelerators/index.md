---
title: "Digital Platform Accelerators"
description: "A Digital Platform is described as the set technologies that function as the context in which the company applications run. A combination of middleware that serves as the foundation for the different…"
date: "2019-05-17T21:27:04.383Z"
categories: 
  - Digital Platforms

published: true
canonical_link: https://medium.com/@eldermael/digital-platform-accelerators-a897ec4b92f4
redirect_from:
  - /digital-platform-accelerators-a897ec4b92f4
---

A Digital Platform is described as the set technologies that function as the context in which the company applications run. A combination of middleware that serves as the foundation for the different products and APIs that are part of an organization.

We have had Platforms as a Service (PaaS) for a while and I have worked on three digital transformations whose objective is to deliver applications and the platform to sustain them.

During those transformations, I have observed _Digital Platform Accelerators_ emerging as patterns and tools that help you to reduce the friction created by deploying and managing many new services into a Digital Platform. The objective is clear: they provide faster access to use the platform itself.

Here are some types of accelerators I have implemented and used many times during these projects.

### Project Kickstarters

In order to create services fast, enterprises have created mechanisms to build software applications using conceptual templates that can be deployed to a platform. Such templates are the aggregate of the technologies, best practices, and integrations required to run inside the Digital Platform.

I have implemented a few Project Kickstarters using different mechanisms and patterns and here are some of those:

#### Project Templates

The idea is to have a project that serves as a blueprint of the typical application deployed in your platform. This project is cloned and after some modifications by hand, you get a repository that can be used to create new services in your platform.

Many times these templates are divided by technology stack. Maven archetypes are a good example of this pattern but a significant difference is that the project templates _contain integrations that are required for the application to run inside the platform_.

These integrations differ from company to company but the usual cases are authentication, authorization, database integrations, caching. As you can deduce from these, we are talking about cross-cutting concerns that a Digital Platform should provide.

#### Project Generators

Eventually, you will have as many templates as technology stacks. And these templates will evolve as the platform starts to change to integrate more cross-cutting concerns. The logical next step is to provide platform users with a way to generate applications without the error-prone task of modifying a Project Template by hand.

Enter Project Generators. These applications will take a set of parameters that change between applications in your Platform and produce a repository ready for deployment.

Another benefit is that a generator is software too so it can be used to remove or add features from the generated projects. For example, if the new application does not require a database, the generator can remove the code that provides database access from the codebase.

Feature generation is a complex topic and it’s very hard to implement. Generators tend to grow complex if many features are provided thus is always better to keep them simple or have template projects to pull code from and transform them.

My dislike for tools such as Yeoman is that they work on a very low level by mostly manipulating text files as templates or low-level string replacement. This had lead to many bugs in which replacing a string in many files and over many layers can lead to very nasty bugs that are hard to resolve.

Atomist provides the next step of this kind of tooling in my opinion as it allows you to use AST transformations for code and also string replacements if your needs are not that sophisticated.

Examples of this are Yeoman, Atomist, JHipster, [https://start.spring.io/](https://start.spring.io/).

### Delivery Workflow Tools

In order to be deployed, generators need also a standardized pipeline from your lower environments to production. A typical problem in Digital Platforms is trying to establish this pipeline as your projects may differ in very little details that need to be captured by your delivery pipeline. Here are some useful patterns for achieving this:

#### Custom Build System Integrations And Tasks

You may have tasks small enough that can be integrated into your build automation tools such as Gradle/NPM. You encapsulate your steps in the tool instead of the pipeline and it gets executed by calling a task defined there.

These tasks usually conform to mandated designs in order to progress the applications through the platform. Examples of these tasks are running isolation and API tests, OWASP dependency checks, publishing to different platform environments, packaging the application in the preferred format for the servers/containers, etc.

A possible example of this tooling is implemented in Netflix Nebula. I also have created custom Gradle plugins that encapsulate many steps required to build applications and deploy them to a platform such as Kubernetes/Openshift.

#### Shared Pipeline Libraries

CI/CD tools such as Jenkins provide you with mechanisms to create shared libraries that can store common steps required to deploy applications. These libraries are pulled from your CI/CD server such as Jenkins and have definitions of shared steps that are used across your applications. They also can provide templates of fully-fledged pipelines that can consume your source code repositories and deliver artifacts up to production.

Unfortunately, shared pipeline libraries can sometimes be very complex and many developers I know think these are against the DevOps way of thinking as the platform is somehow mandating the structure of your pipeline.

On the bright side, Shared Pipeline Libraries avoid the problem of distributed refactoring inside applications within a Digital Platform. Imagine having to modify the pipeline definitions of every application if you introduce a new stage required for all of them. An example of this is security scanners mandated by your security engineers.

Unfortunately, creating these shared steps and pipelines requires a lot of effort because they need to be as general as possible but eventually you will have small edge cases. Maybe you need a different database in one project. Maybe you need to skip certain stages in another project.

Most sophisticated Shared Pipeline Libraries I have coded use _Convention Over Configuration_ and small DSLs to build standardized projects. This removes the friction from deploying as new teams can have a newly generated project delivered into production in minutes.

#### Delivery Workflow CLIs

Over time, delivery steps in a pipeline start to get more complex as bash commands/scripts are being abused. In order to use a fully fledged programming language, a CLI tool is created that is called from the pipeline instead to execute more complicated stages.

The workflow of the application delivery and integration is captured in these CLIs and they sometimes mirror the stages of the pipeline. The benefit of this is that you are decoupling your delivery process from the CI/CD thus you could potentially run the CLI locally and achieve the same result, helping you with debugging and get faster feedback than waiting for the pipeline to run.

Another benefit is that the CLI can accommodate different technology stacks while keeping the complexity low because you are not tied to shell scripts.

NEWT (Netflix Workflow Toolkit) is a possible example of this pattern.

#### Shared Build Images

If you have created a standardized pipeline or workflow CLI chances are that all the required tooling is scattered in different places. For example, if you are using Jenkins you will have Jenkins agents for each technology stack or even for each tool.

This becomes a maintenance burden sooner or later for your DevOps/Infrastructure team thus a pattern I have seen emerging is to accommodate the bulk of your tools into a single Docker image/VM Template. If you only have a single place to add tooling and a single image to use in your CI/CD server you only will have a place to modify to add tooling required for new platform features.

This is important as many tools such as security scanners and tools to deploy applications such as kubectl are required regardless of the technology stack. You only pay the price of building this tool once (even if it is a lot of time) but you can easily update agent definitions and orchestrate pipeline changes that use the required tooling.

### **Distributed Refactoring Tools**

Unless you are working with a monorepo, if you introduce a feature to your Digital Platform you will have to look for their dependents and update them accordingly to use your new features.

An example of this is typically when there is a need to introduce new stages in your delivery pipelines such as security scans. If you have a shared pipeline library or a CLI you might only need to update their versions but this has to be done across all the projects that use your pipeline and/or CLI.

Project Generators and Templates also only provide the first steps for your project. As they are updated with new features, projects that are already live won’t benefit from these features.

This implies touching several repositories and creating the commits to update existing projects with new cross-cutting concerts and platform features.

In the worst case scenario, this process has to be done manually by someone. Once a new feature is available in the underlying platform, many projects are mandated to use this feature thus their respective developers are tasked to do the required refactoring. If you have Workflow CLIs or Shared Pipeline Libraries it may only require you to update to the latest version.

There are a few tools that provide distributed refactoring:

#### Atomist

Atomist has the concept of editors which are pieces of code that transform code. While an oversimplification, these editors can produce pull requests to the required projects thus solving the distributed refactoring problem.

I have been working with Atomist for around three years and during this time the tool has been rewritten a couple of times. That kept me from adopting it for some projects but currently, the documentation and the tool are very powerful and complete.

#### Snyk

Snyk has auto fixes for security vulnerabilities in the Java ecosystem. This is a more focused type of refactoring but Snyk also manages multiple projects. The need to fix vulnerabilities is something obliged in an enterprise setting and Snyk has been my tool of choice for such requirements.
