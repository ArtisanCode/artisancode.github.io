---
id: 210
title: 'Slightly confusing F# error messages'
slug: slightly-confusing-f-error-messages
date: 2014-10-02T13:25:49+00:00
author: Tom Kuhn
excerpt: 'The confusion around the f# error: "This expression was expected to have type unit but here has type int". Exploring the unhelpfulness of an F# error message for someone new to the language.'
layout: post
guid: http://www.artisancode.co.uk/?p=210
categories:
  - .NET
  - 'f#'
tags:
  - Error messages
  - 'F#'
page.comments: true
---
Recently, I&#8217;ve been delving into F#. Not for any purpose other than blowing my mind with a new (functional) way of thinking. In order to facilitate my exploration, I&#8217;ve been using the [Project Euler](https://projecteuler.net/ "Project Euler maths problems") site as a way of focusing my learning on a finite and measurable set of problems. This also has the added benefit that I get to think and design fun little algorithms to solve the problem at hand. If you&#8217;re interested in checking out the F#, feel free to have a look at the code at [my Git Hub repository](https://github.com/Tom-Kuhn/Project-Euler "My personal github repository for project Euler solutions"). WARNING: I&#8217;m very much still learning F# and functional programming so I realise that there are probably much more idiomatic solution to the problems so please don&#8217;t and blindly use any snippets in any production code (not that I think you would anyway!). If there is anything that can be improved (i.e. you feel that you want to teach me some more idiomatic F#!) or you have any questions, then please don&#8217;t hesitate to get in touch with me in the comments.

Now onto the real meat of this post: F# has some mightily confusing (at least to a newbie) error messages. Take for example the following code:

```haskell
let threshold x =
    if x >= 0.5 then
        1
    0
```

generates the following message (referencing line 3):

> This expression was expected to have type<br/>
unit<br/>
but here has type<br/>
int

To my mind the threshold function should be analogous to the following C# function:

```csharp
public int threshold(decimal x)
{
    if (x >= 0.5)
    {
        return 1;
    }

    return 0;
}
```

&#8230; except its not! I&#8217;m sure that there&#8217;s a reason for this that I haven&#8217;t been able to ferret out, but F# requires you to have the else clause specified. So the correct version of the F# is actually:

```haskell
let threshold x =
    if x >= 0.5 then
        1
    else
        0
```

This is all fine and dandy, I can get on board with the necessity of specifying the else clause its not needed in C# but that&#8217;s fine. What I really struggle with is the fact that the error message is so unhelpful (at least to a self-confessed newbie). It took me more time then I&#8217;d like to admit in order to find the solution to this problem. Needless to say the action function I was debugging was a fair bit more complex than the simple example above, but the point still stands. What does not being able to convert a unit &#8211;> int have to do with a missing else clause? I&#8217;m aware that people are actually using F# in a production environment and if it works then great! But in order to help the wider spread adoption of the language, I can&#8217;t help but feel that some work needs to go into the error messages that get generated to help those who are new to the language and maybe new to functional programming. This isn&#8217;t me trying to throw stones (people living in glass houses and all), just trying to make a point! That said if I ever pluck up the courage to go and look at the F# repository (now that Microsoft has open sourced it) I&#8217;ll be sure to look at the [F# contribution page](http://fsharp.github.io/2014/06/18/fsharp-contributions.html "F# contribution page") and see if I can add anything to help improve the usability of the F# error messages.

#### Edits:

  1. [02-Oct-2014 @ 17:25] Thanks to Jeremy in the comments who correctly pointed out that &#8216;uint&#8217; is not the same as &#8216;unit&#8217;. I&#8217;ve amended the post to remove my mistake and make the post make (more) sense
