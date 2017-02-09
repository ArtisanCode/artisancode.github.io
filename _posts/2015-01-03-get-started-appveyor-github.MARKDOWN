---
id: 223
title: Get started with AppVeyor and GitHub
date: 2015-01-03T16:41:05+00:00
author: Tom Kuhn
excerpt: |
  Ever since I dipped my toes into the world of Open Source software, I have long been impressed with one seemingly simple tool that <a href="http://www.appveyor.com/" title="AppVeyor home page.">AppVeyor</a>. So many companies fail to get their build/CI tool spot-on, but I'm happy to report that AppVeyor is blindingly effortless to use delivering exactly what is needed without all the fuss. I first stumbled on this tool when I was looking for some way to use CI on my <a href="http://www.nuget.org/packages/Log4Net.MessageEncryptor/" title="Log4Net Message encryption on NuGet">Log4Net Message Encryption library</a>, as this library is ridiculously small I wanted a simple, lightweight tool to handle my builds... [AppVeyor enters stage right]. And the best thing about this tool, its free for open source projects! Just in case anyone is unsure about just how easy it is to get a build working with GitHub, I thought I'd write a quick article detailing the steps required to get started with AppVeyor and GitHub.
layout: post
guid: http://www.artisancode.co.uk/?p=223
permalink: /2015/01/get-started-appveyor-github/
categories:
  - 'Build &amp; Deployment'
  - CI
  - Open source
tags:
  - AppVeyor
  - Build process
  - Continious Integration
  - GitHub
---
Ever since I dipped my toes into the world of Open Source software, I have long been impressed with one seemingly simple tool that [AppVeyor](http://www.appveyor.com/ "AppVeyor home page."). So many companies fail to get their build/CI tool spot-on, but I&#8217;m happy to report that AppVeyor is blindingly effortless to use delivering exactly what is needed without all the fuss. I first stumbled on this tool when I was looking for some way to use CI on my [Log4Net Message Encryption library](http://www.nuget.org/packages/Log4Net.MessageEncryptor/ "Log4Net Message encryption on NuGet"), as this library is ridiculously small I wanted a simple, lightweight tool to handle my builds&#8230; [AppVeyor enters stage right]. And the best thing about this tool, its free for open source projects! Just in case anyone is unsure about just how easy it is to get a build working with GitHub, I thought I&#8217;d write a quick article detailing the steps required to get started with AppVeyor and GitHub.


### 1. Sign up to AppVeyor

<a href="http://i0.wp.com/www.artisancode.co.uk/wp-content/uploads/2015/01/2015-01-03_15h56_48.png" rel="lightbox" ><img src="http://i0.wp.com/www.artisancode.co.uk/wp-content/uploads/2015/01/2015-01-03_15h56_48.png?fit=739%2C685" alt="Schreenshot showing how to sign up to AppVeyor" class="aligncenter size-full wp-image-229" srcset="http://i0.wp.com/www.artisancode.co.uk/wp-content/uploads/2015/01/2015-01-03_15h56_48.png?w=739 739w, http://i0.wp.com/www.artisancode.co.uk/wp-content/uploads/2015/01/2015-01-03_15h56_48.png?w=300 300w" sizes="(max-width: 739px) 100vw, 739px" data-recalc-dims="1" /></a> <a href="http://www.artisancode.co.uk/wp-content/uploads/2015/01/2015-01-03_15h41_15.png" rel="lightbox" ></a>

This step is almost too easy, but I thought I&#8217;d include it for completeness-sake. So head on over to [AppVeyor](http://www.appveyor.com/ "AppVeyor home page.") and sign up now! For ease I would recommend signing in with your GitHub account, but feel free to use any credentials you want as it doesn&#8217;t really matter.

### 2. Create a new project

<a href="http://i0.wp.com/www.artisancode.co.uk/wp-content/uploads/2015/01/2015-01-03_15h41_15.png" rel="lightbox" ><img src="http://i0.wp.com/www.artisancode.co.uk/wp-content/uploads/2015/01/2015-01-03_15h41_15.png?fit=767%2C360" alt="Screenshot showing how to create a new build project in AppVeyor" class="aligncenter size-full wp-image-225" srcset="http://i0.wp.com/www.artisancode.co.uk/wp-content/uploads/2015/01/2015-01-03_15h41_15.png?w=767 767w, http://i0.wp.com/www.artisancode.co.uk/wp-content/uploads/2015/01/2015-01-03_15h41_15.png?w=300 300w" sizes="(max-width: 767px) 100vw, 767px" data-recalc-dims="1" /></a>

Once you&#8217;re all logged in, feel free to create a new project. Head on over to the projects page and hit the big area that says &#8216;+ NEW PROJECT&#8217;.

### 3. Select your repository

<div class="panel panel-info">
  <div class="panel-heading">
    <p class="panel-title">First time projects only</p>
  </div>

  <div class="panel-body">
    The first time you create a new project, you will need to authorize AppVeyor to with your Source Control provider, simply select GitHub and click the big blue [Authorize GitHub] button. Once AppVeyor is authorized with your GitHub repository, you should see the following screenshot&#8230;
  </div>
</div>

<a href="http://i2.wp.com/www.artisancode.co.uk/wp-content/uploads/2015/01/2015-01-03_15h41_30.png" rel="lightbox" ><img src="http://i2.wp.com/www.artisancode.co.uk/wp-content/uploads/2015/01/2015-01-03_15h41_30.png?fit=767%2C387" alt="Add Select the GitHub project to add to AppVeyor" class="aligncenter size-full wp-image-226" srcset="http://i2.wp.com/www.artisancode.co.uk/wp-content/uploads/2015/01/2015-01-03_15h41_30.png?w=767 767w, http://i2.wp.com/www.artisancode.co.uk/wp-content/uploads/2015/01/2015-01-03_15h41_30.png?w=300 300w" sizes="(max-width: 767px) 100vw, 767px" data-recalc-dims="1" /></a>

Once you&#8217;ve spotted the repository you&#8217;re interested in building, hit the &#8216;+ ADD&#8217; and you&#8217;re already most of the way there!

### 4. (Optional) Modify the build settings

If you feel it&#8217;s necessary to change your projects build settings, fire them up, peruse them and change away to your heart&#8217;s content.

<a href="http://i2.wp.com/www.artisancode.co.uk/wp-content/uploads/2015/01/2015-01-03_16h36_45.png" rel="lightbox" ><img src="http://i2.wp.com/www.artisancode.co.uk/wp-content/uploads/2015/01/2015-01-03_16h36_45.png?fit=811%2C162" alt="Screenshot showing how to modify the project&#039;s build settings" class="aligncenter size-full wp-image-236" srcset="http://i2.wp.com/www.artisancode.co.uk/wp-content/uploads/2015/01/2015-01-03_16h36_45.png?w=811 811w, http://i2.wp.com/www.artisancode.co.uk/wp-content/uploads/2015/01/2015-01-03_16h36_45.png?w=300 300w" sizes="(max-width: 811px) 100vw, 811px" data-recalc-dims="1" /></a>

One option that is useful for me (considering I intend to publish my library on NuGet) is to select the &#8216;Package NuGet projects&#8217; option. Although I believe for this to work effectivly you will need to have the pre-requisite &#8216;.nuspec&#8217; file with the correct information in the project being built. (more on this in a later post!)

<a href="http://i2.wp.com/www.artisancode.co.uk/wp-content/uploads/2015/01/2015-01-03_15h42_42.png" rel="lightbox" ><img src="http://i2.wp.com/www.artisancode.co.uk/wp-content/uploads/2015/01/2015-01-03_15h42_42.png?fit=767%2C760" alt="Modify the build options to package the library using NuGet" class="aligncenter size-full wp-image-228" srcset="http://i2.wp.com/www.artisancode.co.uk/wp-content/uploads/2015/01/2015-01-03_15h42_42.png?w=767 767w, http://i2.wp.com/www.artisancode.co.uk/wp-content/uploads/2015/01/2015-01-03_15h42_42.png?w=150 150w, http://i2.wp.com/www.artisancode.co.uk/wp-content/uploads/2015/01/2015-01-03_15h42_42.png?w=300 300w" sizes="(max-width: 767px) 100vw, 767px" data-recalc-dims="1" /></a>

### 5. Start your build and watch the magic happen

<a href="http://i2.wp.com/www.artisancode.co.uk/wp-content/uploads/2015/01/2015-01-03_15h41_52.png" rel="lightbox" ><img src="http://i2.wp.com/www.artisancode.co.uk/wp-content/uploads/2015/01/2015-01-03_15h41_52.png?fit=767%2C202" alt="Screenshot demonstrating how to start a new build" class="aligncenter size-full wp-image-227" srcset="http://i2.wp.com/www.artisancode.co.uk/wp-content/uploads/2015/01/2015-01-03_15h41_52.png?w=767 767w, http://i2.wp.com/www.artisancode.co.uk/wp-content/uploads/2015/01/2015-01-03_15h41_52.png?w=300 300w" sizes="(max-width: 767px) 100vw, 767px" data-recalc-dims="1" /></a>

That&#8217;s all there is to it, you can go from nothing to a fully built project in under 5 minutes, that&#8217;s certainly nothing to be sniffed at! I only wish that all enterprise build systems were as quick and easy as AppVeyor.

Next time around, I&#8217;ll demonstrate how easy it is to create a new deployment environment on NuGet and publish specific versions form AppVeyor to NuGet and let you code be free in the big wide world&#8230;
