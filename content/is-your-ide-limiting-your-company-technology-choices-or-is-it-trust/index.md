---
title: "Is Your IDE Limiting Your Company Technology Choices? Or Is It Trust?"
description: "So far, I have worked in two places where I have found a sad problem that I think it’s more common than I thought: having your codebase tightly coupled to your IDE. I remember the first time I found…"
date: "2017-10-08T13:21:18.892Z"
categories: 
  - Java
  - Ide
  - Trust
  - Software Development

published: true
canonical_link: https://medium.com/@eldermael/is-your-ide-limiting-your-company-technology-choices-or-is-it-trust-c043b371ad5f
redirect_from:
  - /is-your-ide-limiting-your-company-technology-choices-or-is-it-trust-c043b371ad5f
---

So far, I have worked in two places where I have found a sad problem that I think it’s more common than I thought: having your codebase tightly coupled to your IDE.

---

I remember the first time I found this, I just had my first job as a tech lead for a team of Java developers. I remember that after the first day that mostly was paperwork, they showed me the place where I would sit and my machine. After that, someone sent me an email with the Standard Operation Procedure (SOP) to set up your environment.

Now, does having a SOP for something that I think should be trivial such as setting up your development environment triggers an alarm to you? At that time, with my experience at that point, it didn’t. But if I remember correctly, that document was 9 to 10 pages long removing parts like the title, table of contents and version tracking (yeah, at that time nobody used wikis to version their documents!). After reading it from top to bottom I just shook my head in disbelief.

They were running an ancient version of Eclipse at the time. And they also were tied to another very old version of Maven. They also had instructions to set up a few plugins that they had in zip format to import directly because they were discontinued. I also remember that you had to put a lot of stuff in your maven settings file in order to be able to download the dependencies of our only project… and a long list of etc.

---

The second time I had this problem was pretty recent, but I dare to say ten times worst. They had scripts that created the Eclipse configuration files and they saved those files to SCM. Now, of all the problems that you can imagine being the result of this, I can tell you a few:

-   Hardcoded paths: because you have to save your IDE configuration to SCM, you have to explicitly setup paths in special directories or your whole project won’t work.
-   Vendor Lock-in: The project won’t work on other IDEs because the configuration files are tied to the version of Eclipse that they were using, and because the version was provided by a vendor of a JEE server, the plugins also generate specific configurations that are not compatible with other versions of Eclipse.
-   Developer images of the operating system: the project won’t work at all if you use other than the specific image that they provide to developers because they need special directory structures for the project to work and special network drives setup for this.

Imagine working in such a project? I can tell you that it took me at least one week to have the applications working locally. Imagine the mental overhead that a build system like this is. From time to time I still get emails from other software developers asking how I managed to have my local environment working.

---

The consequences of this are that in both projects were stuck in this situation and if you wanted to upgrade, let’s say, your Java version, you were stuck to what the IDE supports. Quoting one of my coworkers, it’s a sad situation that your entire team or organization is limited to what your current IDE can do.

The problem with this kind of mistakes is that they force you to take more bad decisions. I can tell you that those developer images are the symptom of a deeply broken build process.

In my opinion, if you cannot build your entire application and run it with a single command I dare to say that you have problems.

#### The Underlying Problem

An IDE/Text editor is something personal, we as programmers do care about efficiency and our text editors or IDEs are personal choices that allow us to become more productive in our work. Some of us pick one because we see it as a bragging right.

To the point of this article, in both these projects, someone decided that developers should use the same IDE all the time and dictated what they should do every step of the build process. This may look a logical choice for many reasons such as consistency but the underlying problem is about trust. If you do not trust your programmers to take trivial decisions such as use their own IDE I would dare to say that your organization has problems beyond these technical details.

Both projects had technical leaders that came to the wrong conclusions (that everyone should use the same IDE) with the proper reasoning (that consistency is good). The problem here is that their process focuses on preventing errors because that sounds so good when you think about it. But this kind of process “[cripples innovation because it tries to prevent small mistakes](https://www.slideshare.net/stevenpappas3/netflix-organizational-culture)” and it becomes a burden that does not prevent or even generates big mistakes (such as having a complex build process).
