---
id: 135
title: 'Architecture anti-pattern: &#8216;one architecture to rule them all&#8217;'
date: 2014-05-14T11:29:32+00:00
author: Tom Kuhn
excerpt: Exploring the need to constantly refine software architecture and in some cases start from scratch by looking at some architecture/system design anti-patterns. This post explores the situation where there is one overriding system design that all projects must work within irrespective of the actual project requirements.
layout: post
guid: http://www.artisancode.co.uk/?p=135
permalink: /2014/05/architecture-antipattern-one-system-architecture-rule/
categories:
  - Enterprise architecture
  - Opinion
  - System design
tags:
  - Anti-patterns
  - Design
  - Enterprise archtiecture
---
Software systems can be complicated, and I mean **really** complicated&#8230; the kind of complicated that would make an otherwise sane developer want to run out of the room screaming. Unfortunately this is a world a lot of enterprise developers experience on an almost daily basis. We can try to console ourselves by mumbling something about the necessary complexities due to business processes or the wildly disparate systems needing to communicate with each other through some form of arcane hand-waving and astrological signals. These platitudes doesn&#8217;t really help us when we have to go to battle with the system on a daily basis to try and shore-up ailing processes or implement the nice new shiny features being requested by the business. To cope with this complexity, what we need is action&#8230; decisive action that will eliminate all these cognitive land-mines and make our lives bearable again: we need a system design to bring order to the chaos. Enter the enterprise architects, our saviours!

Despite such a seemingly flippant opening paragraph, I would agree that some form of system design is required to help form a basis for evaluating options. Very often these enterprise solutions have to work around legacy systems, immovable security restrictions, local laws and a myriad of other small but not inconsequential stumbling blocks. The enterprise architect&#8217;s role in understanding all these is often vital to help a project successfully deliver. The importance of starting a system design off on the right foot cannot be understated, bad decisions at this point are compounded the further down the development route you go, right up until the cliff edge at which point there is no going back. So why are software systems still so complicated? Why are the designs adding more components into the solution rather than making things simpler?

Unfortunately the answer to these questions is neither simple, nor easy to articulate in just one blog article (sorry about that!). What I want to do is explore the aspects of system design by using anti-patterns as a method to highlight practices that can be improved. Today I want to cover the “One architecture to rule them all” design anti-pattern. This design pattern is a particularly insidious and pervasive practice as it usually stems from the best intentions, the desire to simplify and consolidate all the numerous complexities of a system into one unifying architecture/design. At first glance this seems like a great thing, right? Unfortunately, like so many great ideas and suggestions there are a number of pitfalls that can really end up negatively impacting the future of the system.

### Why should we care?

This type of dogmatic approach to system design can have some undesirable consequences, from the annoying to the harmful. In this section I hope to take you through some of the outcomes that can arise from the blind adoption and unthinking utilization of a &#8216;master architecture&#8217;.

#### Stifled innovation and lack of creativity

Motivation is often a very intangible subject, there is a whole load of theories and literature surrounding the subject. Rather than trying to place all developers into the same category, I’m going to talk about my own motivations and desires and hope that it resonates with you. I am highly motivated by having the ability to positively affect my work environment. This means that I desire some level of control over the solutions being developed. For me, this manifests itself in a need to be able to experiment and be creative with new tools and technologies to try and come up with new ways of working that provide real benefits. By having one monolithic system design, the scope for innovation is greatly reduced as the areas that can be affected by creative new solutions are smaller and more disparate as they aren’t covered by the master architecture. By encouraging an environment that suppresses innovation, companies run the risk of demotivating employees who thrive on creating new ways of working to help the company.

#### Culture of &#8216;sub optimal&#8217; solutions: Always use the same approach even if there is no compelling reason to do so.

Company culture is important, so important that I want to write a wholly separate blog post on it in the future. In a nutshell, it sets a precedence for the quality of the work being undertaken, it establishes the social norms that are followed within the company and it sets the boundaries of what is acceptable when delivering solutions. If there is a de facto approach to all systems irrespective of the actual requirements this can lead to development teams getting lazy and relying on the pre-existing design rather than assessing ‘is it the right solution for this problem’

#### One person’s nirvana is another’s pain

This is quite a controversial point, but one that I think is worth making anyway. When designing one architectural way for all systems, often it is only one visionary or a very small set of people who generally get to decide on the way forward. This _can_ lead to a beautifully well focussed design that (at least to the designer) is elegant and exactly what is required. However, it can also lead to developers feeling isolated and marginalised if they don’t necessarily share the same viewpoints as the architects. Having been on both sides of this point, it wasn’t until I had designs enforced on me that I truly realised how stressful it can be working within the confines of someone else’s vision. One architecture may help the company in a purist’s sense, but it can also cause undue stress within the development team. Let’s face it, we all live, breathe and work in a stressful enough environment without looking for any more!

#### High upfront costs to implement and even more to replace

Typically with large scale system designs, they are imagined before any actual development can take place. Unfortunately this means that the designs are often created with ‘scalability’, ‘asynchrony’, ‘durability’ and ‘reliability’ in mind without any appreciation for the real world usages. Now I’m not saying that these concepts are not important, but not all of these capabilities need to be present in all applications. I’m very much an advocate of “right tool for the right job at the right time”, additional capabilities very well might be required but let’s build and spec them when we know that we definitely need them and not waste money building unnecessary features. In order to build designs that have unnecessary features often costs a lot of money. In fact very often most of the architecture needs to be fully specified and built in order to allow projects to actually utilise the infrastructure.

When it comes round to replacing legacy systems, the costs associated with re-writing/re-architecting parts of or the whole of a legacy system can be prohibitively expensive. This usually comes down to the fact that no-one really understands how the whole system operates in its entirety. Also, software systems tend to grow over time as they inherit little features and bug fixes which can also add to the overhead of maintainability. Most people when designing solutions fail to adequately consider the scrappage costs associated with decommissioning old legacy systems. This is partly why we see old line of business applications roll on and on and on without any thought to replace or upgrade. By having only one architecture means that the whole system needs to be torn down before starting afresh otherwise aspects of the ‘old way’ start bleeding into any newer designs and thus the legacy lives on.

#### Slow reaction to changes within the world of technology

Within the technology world, innovation and change is the status quo. The rate of this change is phenomenal and never ceases to amaze me. It is precisely this innovation that drives bigger, better, faster, more agile, cheaper solutions (the list goes on and on). Without the ability to take time to regularly review the current progress of the technology world and look to refresh your systems, there is a very real possibility of being left behind the curve. Currently it is getting harder to differentiate one company from another based solely on their IT/system merits, rather customers start noticing poor functionality, design or performance and use the negatives as their differentiator. Innovation and creativity is the lifeblood of a healthy development/IT department and can’t be underestimated.

### What can we do about it?

So now that I have highlights some of the negative outcomes that can arise from having only one approved architecture to work within, let’s talk about how we can work to improve things a bit. After all that I’ve said above, sometimes (just sometimes) things don’t need improving and a big all-encompassing design is just what is required. However, these occasions are likely to be fairly rare so it’s still worth bearing some of the following in mind.

#### Right tool for the right job at the right time

Like most aspects of developing software, there is (usually) never a 100% correct solution and a 100% incorrect solution, we live in a world of compromises. The job of an architect / developer is to understand the compromises and make the best choices available with the information available. With this ambiguity comes the need to assess and evaluate the tools available e.g. do I _really_ need a ServiceBus here, or can I just call a WebAPI endpoint directly? The answer might not always be the same for all problems, indeed the answer may change over time when the throughput of the system is measured and analysed. Sometimes as developers we need to stop ourselves from taking the easy option and blindly following the previous projects, which leads me on to …

#### Review often and avoid big upfront designs (waterfall)

When developing software in an agile fashion, there is an expectation of constant introspection, review and rework. Why should designing a system be any different? By forcing designers to stop and evaluate the needs of the infrastructure vs. the needs of the project it&#8217;s possible to work within a &#8216;just-it-time&#8217; architecture. This is likely to better align to the actual needs of a solution rather than the perceived needs right at the start of the project. One of the other questions that you can ask yourself is: can I achieve a solution in a better way? This type of question is aimed at trying to ensure that the current status quo is always re-evaluated. This may not mean that if the answer is yes that you will always re-engineer the infrastructure, after all there are sometimes extenuating circumstances and you might decide that a bit of technical debt can be accumulated in order to deliver quicker, however, this shouldn&#8217;t stop you planning in some refactoring at a later date.

#### Be aware of ‘red flag statements’

When discussing the overall system design, I have found that there are a number of comments or statements that are given to help justify using only one monolithic architecture. No doubt you can think of your own additions to the list below, but here are some that I have certainly heard:

> If an architecture/design worked for project &#8216;X&#8217;, then surely it should be a great fit for project &#8216;Y&#8217;?”

> People are comfortable and familiar with technology &#8216;Z&#8217; and it should get the job done even if there are a few workarounds required.”

> &#8220;That&#8217;s how we have always done things&#8221;

> We just can&#8217;t afford to change the whole system right now”

Whilst these statements in themselves are not bad per-se, they can be an indicator that the solution design is just following the established pattern without any thought to reviewing what is currently in use and what is actually required.

#### Keep an open mind

As developers and designers, it is very easy to fall in love with a particular pattern or design, this very often forms the basis of your &#8216;go-to&#8217; or default solution. Whilst this isn&#8217;t necessarily a bad thing, it can colour your thinking and decision making process without you even realising it. This bias is a perfectly natural part of being a human developer/designer and is something that is very difficult to shake off, but by being aware that it may be occurring you can mentally take a step back and ask &#8216;is my opinion based on fact or a bias towards my favourite solution?&#8217;. By keeping an open mind within a design context, often this leads to unexpected and novel ideas that may have been dismissed before they can e fully explored.

#### When improving, lots of small incremental changes are preferable to big bang implementations.

When looking to redesign and architect a system, very often a &#8216;big-bang&#8217; approach is going to end up being very costly and give the redesign concept a poor reputation. Rather, I would favour an iterative approach that focusses on the biggest pain points first and then working outwards when the value of the redesign has been established.

#### Seek out feedback from people and analytics

When looking to make incremental updates or changes people often want hard and fast facts to be able to justify any changes or rework. This is where analytics and statistics can play an important part in informing and guiding the work to look at the areas with the best cost/benefit ratio. By being able to point to the actual characteristics of the solution developed, this then gives you an invaluable insight into what areas actually need improving rather than the areas that you think \*might\* need improving.

#### Become involved in the wider community

By becoming involved with other communities whether they are other development teams within the company, or local developer groups you will be exposed to a wide range of new and challenging ideas and concepts. By finding out what and why other people think differently to yourself you can really start to help break down the mental barriers that guide you automatically down a particular solution. Communities are also a great way to find out what is happening within the wider world of technology. Given the fact that new products, libraries and patterns are being published on a daily basis it&#8217;s often difficult to research all the new options available. This is where a community can come together and when sharing new ideas and concepts the best often find a way to bubble up to the top and can be disseminated throughout the whole group. This can only help to give you a more informed decision when evaluating the current system design and seeking improvements or alternatives.

### Conclusion

A system architecture, like software grows with time. What starts off being a well-intentioned fairly well structured design can often find itself spiraling out of control as soon as more functionality/projects are required. What we need to be aware of is the dangers associated with designing and utilizing one single system architecture for all purposes. In some cases, there will be no option but to use what has gone before, but there are certainly plenty of occasions where taking the time to stop and review the requirements and the current design with respect to its suitability can play dividends for the whole project.