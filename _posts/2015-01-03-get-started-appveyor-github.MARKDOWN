---
_id: 223
title: Get started with AppVeyor and GitHub
date: 2015-01-03T16:41:05+00:00
author: Tom Kuhn
excerpt: |
  Ever since I dipped my toes into the world of Open Source software, I have long been impressed with one seemingly simple tool that <a href="http://www.appveyor.com/" title="AppVeyor home page.">AppVeyor</a>. So many companies fail to get their build/CI tool spot-on, but I'm happy to report that AppVeyor is blindingly effortless to use delivering exactly what is needed without all the fuss. I first stumbled on this tool when I was looking for some way to use CI on my <a href="http://www.nuget.org/packages/Log4Net.MessageEncryptor/" title="Log4Net Message encryption on NuGet">Log4Net Message Encryption library</a>, as this library is ridiculously small I wanted a simple, lightweight tool to handle my builds... [AppVeyor enters stage right]. And the best thing about this tool, its free for open source projects! Just in case anyone is unsure about just how easy it is to get a build working with GitHub, I thought I'd write a quick article detailing the steps required to get started with AppVeyor and GitHub.
layout: post
guid: http://www.artisancode.co.uk/?p=223
slug: get-started-appveyor-github
categories:
  - build-&amp;-deployment
  - CI
  - open-source
tags:
  - appveyor
  - build-process
  - continious-integration
  - github
---
Ever since I dipped my toes into the world of Open Source software, I have long been impressed with one seemingly simple tool that [AppVeyor](http://www.appveyor.com/ "AppVeyor home page."). So many companies fail to get their build/CI tool spot-on, but I'm happy to report that AppVeyor is blindingly effortless to use delivering exactly what is needed without all the fuss. I first stumbled on this tool when I was looking for some way to use CI on my [Log4Net Message Encryption library](http://www.nuget.org/packages/Log4Net.MessageEncryptor/ "Log4Net Message encryption on NuGet"), as this library is ridiculously small I wanted a simple, lightweight tool to handle my builds&#8230; [AppVeyor enters stage right]. And the best thing about this tool, its free for open source projects! Just in case anyone is unsure about just how easy it is to get a build working with GitHub, I thought I'd write a quick article detailing the steps required to get started with AppVeyor and GitHub.

### 1. Sign up to AppVeyor

<a href="#" class="blog-image" data-featherlight="/content/2015/Getting started with AppVeyor.png" ><img src="/content/2015/Getting started with AppVeyor.png" alt="Screenshot showing how to sign up to AppVeyor"></a>

This step is almost too easy, but I thought I'd include it for completeness-sake. So head on over to [AppVeyor](http://www.appveyor.com/ "AppVeyor home page.") and sign up now! For ease I would recommend signing in with your GitHub account, but feel free to use any credentials you want as it doesn't really matter.

### 2. Create a new project

<a href="#" class="blog-image" data-featherlight="/content/2015/New AppVeyor project.png" ><img src="/content/2015/New AppVeyor project.png" alt="Screenshot showing how to create a new build project in AppVeyor"></a>

Once you're all logged in, feel free to create a new project. Head on over to the projects page and hit the big area that says &#8216;+ NEW PROJECT'.

### 3. Select your repository

<div class="panel panel-info">
  <div class="panel-heading">
    <p class="panel-title">First time projects only</p>
  </div>

  <div class="panel-body">
    The first time you create a new project, you will need to authorize AppVeyor to with your Source Control provider, simply select GitHub and click the big blue [Authorize GitHub] button. Once AppVeyor is authorized with your GitHub repository, you should see the following screenshot&#8230;
  </div>
</div>

<a href="#" class="blog-image" data-featherlight="/content/2015/Select repository.png" ><img src="/content/2015/Select repository.png"  alt="Add Select the GitHub project to add to AppVeyor" ></a>

Once you've spotted the repository you're interested in building, hit the &#8216;+ ADD' and you're already most of the way there!

### 4. (Optional) Modify the build settings

If you feel it's necessary to change your projects build settings, fire them up, peruse them and change away to your heart's content.

<a href="#" class="blog-image" data-featherlight="/content/2015/Modify build settings.png" ><img src="/content/2015/Modify build settings.png"  alt="Screenshot showing how to modify the project's build settings" ></a>

One option that is useful for me (considering I intend to publish my library on NuGet) is to select the &#8216;Package NuGet projects' option. Although I believe for this to work effectivly you will need to have the pre-requisite &#8216;.nuspec' file with the correct information in the project being built. (more on this in a later post!)

<a href="#" class="blog-image" data-featherlight="/content/2015/Modify build options for nuget.png" ><img src="/content/2015/Modify build options for nuget.png"  alt="Modify the build options to package the library using NuGet" ></a>

### 5. Start your build and watch the magic happen

<a href="#" class="blog-image" data-featherlight="/content/2015/new build.png" ><img src="/content/2015/new build.png"  alt="Screenshot demonstrating how to start a new build"></a>

That's all there is to it, you can go from nothing to a fully built project in under 5 minutes, that's certainly nothing to be sniffed at! I only wish that all enterprise build systems were as quick and easy as AppVeyor.

Next time around, I'll demonstrate how easy it is to create a new deployment environment on NuGet and publish specific versions form AppVeyor to NuGet and let you code be free in the big wide world&#8230;
