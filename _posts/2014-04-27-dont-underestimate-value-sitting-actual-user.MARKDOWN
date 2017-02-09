---
id: 130
title: 'Don&#8217;t underestimate the value of sitting with an actual user.'
date: 2014-04-27T12:33:30+00:00
author: Tom Kuhn
excerpt: A story highlighting the necessity of sitting with actual users of the application to help determine and fix problems.
layout: post
guid: http://www.artisancode.co.uk/?p=130
slug: dont-underestimate-value-sitting-actual-user
categories:
  - Anecdote
  - Opinion
  - Support
---
A few years ago, I was working with a production support team looking after the core business CRM application. My primary role at the time was a DevOps position that had me mentoring the junior team members alongside babysitting the legacy application. I was called over by the Application Services Manager at the time and asked to help one of the less technical members of the team on a particularly insidious issue. The issue I was tasked to help with was:

  * Looking at was why the application _intermittently_ crashed on a Windows server 2008 machine but worked fine on a Win XP desktop box.
  * This only seemed to happen when opening a particular screen in the application.
  * The application never crashed if you had administrator access to the application.
  * No-one in production support could replicate the issue.

The actual steps taken to resolve the issue aren&#8217;t particularly important to the story but suffice to say we checked all the usual suspects e.g. environments, user permissions, etc. and nothing was helping the situation. At this point we had modified the application a number of times to try to help pinpoint he issue and allow the production support team to replicate the issue. Each time we modified the application I would ask the junior teammate to trek to the main office in town (about a 30 minute walk away) and sit with the users affected whilst they went through the process of replicating the issue, whilst I stayed in the satellite office monitoring the logs and the server to see if anything unusual was appearing. Each time this cycle happened the users could successfully replicate the issue but we couldn&#8217;t. This cycle happened a few times until I decided that I wanted to see first hand what the issue was. So after three weeks of getting nowhere I braved the trip up to the main office to sit down with the users and see the problem first hand. As soon as I saw the user replicate the issue, I felt like giving myself the biggest face-palm of all time. To fully appreciate the situation you&#8217;ll need to understand that the application in question had a number of methods to allow the user to open a new screen:

  1. Use the File >> Open menu
  2. Use a toolbar button
  3. Use a keyboard shortcut

Options 1&3 opened the screen directly, whereas option 2 called the same functions as 1&3 but within the toolbar click event handler.

When I sat down with the user and saw them click on the toolbar to open up the new screen, everything fell into place as to why no-one in production support could replicate the issue&#8230; everyone in production support always used the keyboard shortcuts. The keyboard was the quickest way of navigating the application so being the technically minded team we were, we all used the keyboard to navigate the application and never used the menu or the toolbar. This epiphany would never have come about without taking the time to sit down with the user to understand what they actually did and why they did things that way. Ultimately I was able to track the issue down to some re-entrant code trying to modify the toolbar state when it still dealing with the click event handler but the issue soon got fixed.

As a bonus, the reason that the issue wasn&#8217;t occurring when the user had administrative privileges was due to the fact that when the user had more rights to the system different options were appearing within the toolbar for all the new functions that could be accessed by an admin. This then caused the particular screen button to be in a different position on the toolbar which broke the user&#8217;s &#8216;muscle-memory&#8217; associated with opening the screen. The knock-on impact of the open button moving on the toolbar was that they had to fallback to using the slower &#8216;File >> Open&#8217; option which didn&#8217;t exhibit the crashing behavior and therefore allowed the screen to open correctly.

This support incident taught me two very valuable lessons:

  1. Always try to speak to the person actually using your software. You can only do so much through an intermediary such as a Business Analyst or a colleague. Face to face time in invaluable to help understanding the parameters of the problem you are trying to solve. In the example above, it should have happened sooner than it did but both myself and my colleague were busy juggling multiple issues at the same time which caused the delay.
  2. Never underestimate the behaviors of the user. This is to say that even the seemingly smallest change in surroundings e.g. additional buttons appearing within the toolbar caused the user&#8217;s behavior to change and therefore masked the issue at hand.

Don&#8217;t underestimate the value of sitting with actual users!
