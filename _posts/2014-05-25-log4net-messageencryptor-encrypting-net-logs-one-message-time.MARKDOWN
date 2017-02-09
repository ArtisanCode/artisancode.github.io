---
id: 150
title: 'Log4Net.MessageEncryptor &#8211; Encrypting .NET logs one message at a time'
date: 2014-05-25T20:30:55+00:00
author: Tom Kuhn
excerpt: Launching a tool that allows you to encrypt log messages whilst leaving all the log statement meta data intact. The Log4Net message encryptor does exactly what it says in the title... it encrypts the log messages and only the messages. This means that all the time-stamps, log levels, thread information and any other metadata remain in plaintext within your logs. The tool uses the concept of the log4net forwarding appender meaning that any appenders referenced by the MessageEncryptingAppender receive a logging object containing the encrypted message contents, therefore, this tool should be compatible with any other appenders used by your current solution. The upshot of this is that log message encryption should be able to be plumbed into an existing solution with minimal effort and rework.
layout: post
guid: http://www.artisancode.co.uk/?p=150
permalink: /2014/05/log4net-messageencryptor-encrypting-net-logs-one-message-time/
categories:
  - 'c#'
  - Open source
tags:
  - .NET
  - 'c#'
  - Encryption
  - Log4Net
  - Open source
---
This week I found that I had a spare evening, so I set myself a challenge: set-up and deploy my first ever open source project in one evening. Unfortunately this was a bit too much of a hard target to achieve, so after three evenings, a couple of lunch breaks, I&#8217;m proud to introduce: Log4Net.MessageEncryptor!

### What is Log4Net.MessageEncryptor?

The Log4Net message encryptor does exactly what it says in the title&#8230; it encrypts the log messages and only the messages. This means that all the time-stamps, log levels, thread information and any other metadata remain in plaintext within your logs. The tool uses the concept of the [log4net forwarding appender](http://logging.apache.org/log4net/release/sdk/log4net.Appender.ForwardingAppender.html "Log4net documentation: forwarding appender") meaning that any appenders referenced by the MessageEncryptingAppender receive a logging object containing the encrypted message contents, therefore, this tool should be compatible with any other appenders used by your current solution. The upshot of this is that log message encryption should be able to be plumbed into an existing solution with minimal effort and rework.

&nbsp;

### Why do we need it?

Recently there is a trend where developers are logging entire object states to file to help provide more information when investigating issues (for the past couple of years JSON has been my go to format for logging object state to file&#8230; if you are interested then I can certainly write another post to go through it in more detail). Typically this involves logging all the parameters passed to a function and the return value as well, this then acts as a snapshot of the system at that particular point. Whilst this can be invaluable within a production setting, there is one glaring drawback of this technique: you are potentially storing sensitive information within Log files. This may not be so much of a problem when log file access is treated the same as access to the production database. However, in today&#8217;s modern society in the cloud-based environment more and more developers are finding themselves exposed to, that these logs exist on non-company machines is starting to worry some enterprise security and compliance teams. As a developer I can certainly attest to how important having good logs are in helping to find, fix and diagnose issues quickly and efficiently, but the challenge remains: How can I keep the level of detail required within the logs without putting people&#8217;s information at risk? &#8230; By encrypting it! This is where the Log4Net.MessageEncryptor tool comes in. By treating the log message as sensitive information and applying AES 256 encryption to any data present, we can add another layer of security to help ensure the data is not compromised.

&nbsp;

### How can we use it?

Usage of the tool is really simple &#8230; simply install via NuGet and add the necessary configuration information. Detailed steps of this process can be found here: [https://github.com/ArtisanCode/Log4NetMessageEncryptor](https://github.com/ArtisanCode/Log4NetMessageEncryptor "GitHub repository for Log4Net.MessageEncryptor")

&nbsp;

### Key links and information

<table class="table">
  <tr>
    <td>
      <i class="icon-github "></i> Git Hub repository
    </td>
    
    <td>
      <a title="GitHub repository for Log4Net.MessageEncryptor" href="https://github.com/ArtisanCode/Log4NetMessageEncryptor">https://github.com/ArtisanCode/Log4NetMessageEncryptor</a>
    </td>
  </tr>
  
  <tr>
    <td>
      NuGet site
    </td>
    
    <td>
      <a title="NuGet hosted package for Log4Net.MessageEncryptor" href="https://www.nuget.org/packages/Log4Net.MessageEncryptor">https://www.nuget.org/packages/Log4Net.MessageEncryptor</a>
    </td>
  </tr>
  
  <tr>
    <td>
      CI server
    </td>
    
    <td>
      <a href="https://ci.appveyor.com/project/ArtisanCode/log4netmessageencryptor/"><img class="alignnone" src="https://ci.appveyor.com/api/projects/status/9dl0t3ty387no5my" alt="Log4Net.MessageEncryptor Build status" width="100" height="18" /></a> <a title="CI server for Log4Net.MessageEncryptor" href="https://ci.appveyor.com/project/ArtisanCode/log4netmessageencryptor/">https://ci.appveyor.com/project/ArtisanCode/log4netmessageencryptor</a>
    </td>
  </tr>
</table>

&nbsp;

In a later post I want to explicitly go through some of the awesome tools that I found to help with the process of building and deploying a .NET project from GitHub &#8230; namely [AppVeyor](http://www.appveyor.com/ "Continuous Integration for busy developers") a truly amazing service that made building, testing and deploying the message encryptor a breeze!