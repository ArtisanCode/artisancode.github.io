---
id: 161
title: 'Dependency Injection: When is it appropriate to use?'
date: 2014-06-05T18:16:38+00:00
author: Tom Kuhn
excerpt: "A colleague of mine recently wrote a very nice article about knowing when to use Dependency Injection (DI) in a project. There are a whole load of great ideas and suggestions within this post and I strongly recommend you spending 5 minutes reading the article in its entirety. There were so many points that were raised that could spawn individual discussions but one leapt out at me which I thought warranted special attention: 'Practice makes perfect'!"
layout: post
guid: http://www.artisancode.co.uk/?p=161
slug: appropriate-dependency-injection
categories:
  - Dependency Injection
  - Opinion
  - System design
  - Technical
  - Uncategorized
tags:
  - Companion article
  - Dependency Injection
  - Design
---
A colleague of mine recently wrote a very nice article about knowing when to use Dependency Injection (DI) in a project. Here&#8217;s a link to the [original article](http://flat9.wordpress.com/2014/06/04/when-should-i-use-dependency-injection/ "Blog article: When should I use Dependency Injection?"). There are a whole load of great ideas and suggestions within this post and I strongly recommend you spending 5 minutes reading the article in its entirety. There were so many points that were raised that could spawn individual discussions but one leapt out at me which I thought warranted special attention: &#8216;Practice makes perfect&#8217;!

&nbsp;

Whilst the article never explicitly mentions the word &#8216;practice&#8217; the concept is certainly touched upon several times.

> Rather than thinking in terms of when a project is big enough I’d recommend starting on a small simple project. Whenever I learn something new I try to minimise complexity elsewhere so that I can focus my effort on the thing I’m trying to learn.
>
> [_Marc Ziman_](http://flat9.wordpress.com/2014/06/04/when-should-i-use-dependency-injection/ "Blog article: When should I use Dependency Injection?")

As software developers is always so easy to fall into the trap of just following the predecessor project or falling back on what you know, love and are comfortable with. These defaults are not bad, far from it. By having these comfortable norms it allows developers to focus on the problems the software is trying to solve rather than wrestling with the code architecture as well. However, once in a while it can pay to reassess their own (and the businesses) defaults to evaluate new techniques and tools&#8230; this is where practice can make the difference between success and failure. Vary rarely do we as software developers get the opportunity to dedicate our time to practising new techniques. There&#8217;s a famous quote which reads

> [&#8230;] it takes roughly ten thousand hours of practice to achieve mastery in a field
>
> [_Malcolm Gladwell_](http://www.wisdomgroup.com/blog/10000-hours-of-practice/ "Blog article: Mastering a skill in ten thousand hours.")

By picking a technique to practice (in this case Dependency Injection) it can really help you appreciate the finer details of DI containers, lifestyles, [Aspect Orientated Programming](http://en.wikipedia.org/wiki/Aspect-oriented_programming "Wikipedia entry on AOP") and good old-fashioned SOLID design principles. Dependency Injection is a tool that can help your code become cleaner, conciser and much more maintainable, so it&#8217;s often a handy addition to your default tool set.

> [&#8230;] any friction you encounter using your DI container is part of the learning process. Once you’re familiar with the tool you shouldn&#8217;t really notice it.
>
> [_Marc Ziman_](http://flat9.wordpress.com/2014/06/04/when-should-i-use-dependency-injection/ "Blog article: When should I use Dependency Injection?")

Whenever you are learning a new technique or tool there will be some bumpy spots along the road, Dependency Injection is no exception! There will be times when you&#8217;re looking at your code, pulling your hair out wondering why one of your dependencies is `null`. I promise you that this happens to everyone, even those who are supposed &#8216;experts&#8217; in DI! All this talk of practice is fine and dandy but it doesn&#8217;t answer the question at the top of this post&#8230;

&nbsp;

### So when is it appropriate to use Dependency Injection?

The answer, like so many thinks in life is not black and white. Very unhelpfully I&#8217;ll answer this question by saying &#8220;it depends&#8221;. Like Marc, I am a big fan of DI and I think that it can add a lot of value to most projects if adopted correctly. Personally, I would always start with the premise that DI should always be used unless there is a justification for not doing so.

What are some of the valid reasons might there be to consider not using DI:

  1. **Large Legacy application** &#8211; Retrofitting dependency injection into a pre-existing application can be a nightmare. The pain generally scales with the size and age of the application as older applications often have years of organic growth along with a spaghetti-like approach to managing dependencies. If you are in this position, it might be more cost-effective to simply live without DI. However, this isn&#8217;t to say that moving over is impossible, it just requires a concerted effort to migrate small portions of the application over at a time.
  2. **Software written in a language that doesn&#8217;t have a readily available Dependency Injection framework** &#8211; Unless you are willing to write your own Dependency Injection framework, then you are pretty much blocked at the first hurdle here. However, this doesn&#8217;t mean to say that you shouldn&#8217;t be thinking about the &#8216;D&#8217; in SOLID and design your code accordingly. you never know when someone might write a DI container for your language and if the the code is designed an implemented correctly plumbing in a DI container at a latter stage can be made a easier task.
  3. **Simple applications that run for a short period and then exit** &#8211; Using a DI container often has an associated overhead, usually associated with the set-up and configuration of the container. If you have an application that runs for a very short period and then exits then the overhead of bringing in a DI container could be too much. However, you need to be careful as soon as your application starts to bring in more than 3 dependencies, then seriously consider using DI.
  4. **Ultra performance sensitive applications** &#8211; Most software can be tolerant of a few ms delay in creating an object from the DI container, however there are certain classes of applications e.g. high frequency trading platforms in which 20 ms is an eternity! Know your domain, know what delays you can tolerate and then decide if Dependency Injection is for you.

&nbsp;

If there are any other justifications you can think of, please feel free to add them to the comments below. Since we have covered some of the valid reasons for not adopting DI, less look at some of the less valid justifications people give for not wanting to use DI:

  1. **My application is too large** &#8211; Large often equals complex, so why wouldn&#8217;t you be looking to bring in anything that can help you minimise that complexity? There does seem to be a misconception running around stating that DI doesn&#8217;t handle large complex scenarios very well and to be honest I&#8217;m not sure where this comes from. Sure in a large application your DI configuration can become unruly an monolithic, this is where conventions can be your friend. By looking at introducing conventions within the application, you can look to eschew complex configuration for a default set of configurations that will handle everything in your application. Marc touched upon this in his article when he talked about automatic registration.
  2. **My application is too small** &#8211; Some people seem to think that just because an application is small doesn&#8217;t mean that it needs the same rigorous development process applied. Just because an application is small gives you the perfect platform to develop and hone your abilities using the same techniques you would employ on larger projects. If you repeat the process enough (see above for comments on practising!) then this process can become exceedingly quick and painless. Indeed I have a simple NuGet package at work that allows me to pick and choose from a suite of business-wide core functionality e.g. logging, connection/transaction management, exception handling etc. so no matter how small an application is, I always know that I have the same features used within the larger business applications. Finally, people who use this excuse seem to forget that there is an almost universal constant in software which dictates that applications are almost always larger and more complex then you initially conceive them to be. Applications also tend to grow and change over time, so by making sure that you have a good solid foundations means that the growth and change can be delivered quicker and with minimal disruption to the rest of the application.
  3. **My application is too mid-sized** &#8211; Well I&#8217;ve never _actually_ heard this excuse, but it wouldn&#8217;t surprise me!
  4. **We&#8217;ve never written software that way before/That&#8217;s not how we develop software** &#8211; Unfortunately this is an excuse I&#8217;ve heard all too often. I&#8217;m certainly not in the habit of being dogmatic with using DI for all applications, but I have certainly felt the benefits whenever I have made the decision to use it. This justification can be the hardest one to try and counteract as it is so inter-twinned with people&#8217;s personal beliefs and attitudes to software development. Changing people&#8217;s opinions and believes is a huge topic and one that deserves it&#8217;s very own post. Suffice to say that just because you haven&#8217;t been using DI in the past doesn&#8217;t mean that you won&#8217;t benefit for it in the log run. As has been previously mentioned, there might be some initial friction in getting up to speed, but once you are in the DI habit, you can start to reap all the benefits that are associated with Dependency Injection.
  5. **It takes too long to develop** &#8211; This point is twinned with #4 above. Once you get practised with this technique, there is no reason why using a DI container wouldn&#8217;t be as quick or even quicker than the more traditional methods of managing the creation, visibility and destruction of objects within your application.
  6. **It&#8217;s too difficult to understand what&#8217;s going on/Our developers won&#8217;t understand it** &#8211; By using Dependency Injection you are allowing a 3<sup>rd</sup> party component to take responsibility for the creation and destruction of your objects. If developers are used to a more procedural view on the code i.e. with all object creation in-line (and generally mixed in with all the business logic!) then it can take a while to get used to allowing a library to manage this for you.This You can make life easier by implementing some conventions within your applications e.g. all DI configuration occurs in the same fashion e.g. installers co-located in a single place. I&#8217;m afraid comes down to practice again!
  7. **Dependency Injection costs us too much performance** &#8211; How much is too much performance? This is a question only you can answer. As was mentioned in the valid justifications list, you need to know your own domain and be aware of the limitations that are imposed. My general view on this is that anything developers can do to minimise complexity and the time taken to understand and modify the code after the fact should be encouraged wholeheartedly. Generally most software solutions can handle the small amount of latency introduced by using a DI container. However, this is something that you may need to pay special attention to by profiling you application when it is running to truly understand where the time is going and not just assume that its the DI&#8217;s fault.,

&nbsp;

### What are you waiting for?

Pick a framework and start practising, there are certainly plenty to choose from!
