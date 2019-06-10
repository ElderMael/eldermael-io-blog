---
title: "A (Very) Tiny HTTP Server With Ceylon"
description: "Finally Ceylon 1.2 has been released and I am pleased to start coding more and more with it. If you have not take a look at Ceylon, let me tell you that it is one of best designed programming…"
date: "2015-11-13T17:57:44.344Z"
categories: 
  - Programming
  - Ceylon Lang
  - Http

published: true
canonical_link: https://medium.com/@eldermael/a-very-tiny-http-server-with-ceylon-b7c6f56c0a16
redirect_from:
  - /a-very-tiny-http-server-with-ceylon-b7c6f56c0a16
---

Finally [Ceylon 1.2](http://ceylon-lang.org/) has been released and I am pleased to start coding more and more with it. If you have not take a look at Ceylon, let me tell you that it is one of best designed programming language that I have seen. The type system is simply unmatched by any contemporaneous programming language.

Now, as a web developer on both the front end and backend I really was expecting an easy way to create a web server to return files and JSON data to create a little web application for fun and starting to learn more of the language. And also use Ceylon on the client side because it is not just targeting the JVM in the backend but it also transpiles to JavaScript.

Fortunately, Ceylon 1.2 comes with different modules and one of them is ceylon.net.http.server which uses JBoss Undertow behind the scenes. For now I was trying to make a quick program to return a single string by HTTP, afterwards I will build on top of this to create what I want.

#### The Server And The Endpoints, First Glance At The Code

Code is king, so let’s start, here is a three file gist of a HTTP Server with only one endpoint:

<Embed src="https://gist.github.com/ElderMael/21e902deead0eccda5cb.js" aspectRatio={0.357} caption="" />

If this code looks familiar to you, it is because it is a version of the current code snippet that is shown on the official Ceylon web site. But let’s analyze it:

The first two files are self explanatory, one is a Ceylon module definition. Here you can see that our module will import the module ceylon.net to the specific version.The second file is a Ceylon package descriptor telling that our package will be shared i.e. visible to other modules if they import our own

#### How to create a server

Next is the juicy code, here you can see various lines that do the following:

First, we import the factory function newServer which will help us to create a HTTP Server that is defined by the interface ceylon.net.http.server.Server. This interface, as you expect it, handles the lifecycle of our HTTP server and allow us to define Endpoints.

Now, all this function needs is a stream (list, sort of) of ceylon.net.http.server.Endpoint instances that will define what your server will respond to.

#### Endpoints

Now, to create an Endpoint you need three parameters:

-   A _path_ defined by a ceylon.net.http.server.Matcher, i.e. the URIs that the endpoint will handle. For simplicity I am using the factory function called equals that will return me a Matcher that will make the endpoint respond to only URIs that match exactly the string I am using. You can compose these Matchers for greater flexibility.
-   An _acceptMethod_ that is an stream of ceylon.net.http.Method instances. These translate into HTTP verbs, and as you can see I only needed a single instance for the verb GET.
-   A _service function_, this will be the code that will be executed when a request to our server is matched by the path we specified and the verb(s) that we passed in the parameter acceptMethod. It takes as arguments a ceylon.net.http.server.Request and ceylon.net.http.server.Reponse. These interfaces allow you to obtain data from and send data to the client respectively. This through HTTP specific things such as parameters, cookies, headers, etc. I will talk about these later.

#### It’s Running!

Now, if you execute the code (I am using Ceylon IDE to execute the run function) you will have a running HTTP server that will listen to requests to [http://localhost:8080/](http://localhost:8080/). That was easy!

Now, talking about the language itself, I want you to take note of the named parameters that we are using all over the place (endpoints, path, acceptMethod, service). They make our code very explicit and readable without the caveats of other languages like passing objects/hashes of options that will not be checked by the compiler (here it does!).

Also note that the endpoints parameter in the function newServer: it is an iterable. This means that if you add a comma after the Endpoint instance that we are creating, you can add more Endpoints.

#### What I will do next

Currently I am working to make this server respond files, afterwards I really need to start working to respond with JSON and finally I will try to use Ceylon on the browser putting it all together (remember, Ceylon does transpiles to JavaScript). I hope you can read the coming posts about this.

#### Notes for the experienced Java programmer

Yes, I almost can see the grin in your face while reading the code. If you are like me and you have been using Java and the Servlet API for a long time, I think you get the idea behind the ceylon.net.http.server API. And if you compare this code, you can see that it allows you to define things very quickly and concisely with no unneeded verbosity. This somehow reminds me of Spring Boot but here you use less annotations and more code.
