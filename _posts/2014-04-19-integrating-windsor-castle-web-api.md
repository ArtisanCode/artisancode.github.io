---
title: Integrating Windsor.Castle with Web API
author: Tom Kuhn
excerpt: "Integrating Windor.Castle with Web API can seem like a daunting prospect, but with this article I'll demonstrate all the steps required to get the job done. This article takes you through creating a CastleDependencyResolver and a CastleDependencyScope that you can used to allow Castle to create your ApiControllers."
layout: post
permalink: /2014/04/integrating-windsor-castle-web-api/
categories:
  - 'c#'
  - Technical
tags:
  - 'c#'
  - Dependency Injection
  - Web API
  - Windsor.Castle
---
In my [last post][1] I described the steps required successfully integrate Windsor.Castle with MVC. This time around I&#8217;ll be giving the same treatment to Web API. Whilst MVC and Web API take a very similar approach to allowing you to integrate your favorite dependency injection framework, rather annoyingly Microsoft decided to alter the mechanism enough to make you scratch your head for a few minutes. Like the previous post, at the end of the article, there will be a link to the Artisan Code GitHub repository that contains a reference Web API solution with all the setup code required. This can be used as a basis for a new project, or simply as a guide to help you integrate Castle into an existing project. Without any further ado, lets start looking at some code.

This article makes the following assumptions:

1.  You already have a Web API project (either a new project or an existing project).
2.  You already have Castle.Windsor downloaded and referenced by your project (this can easily be achieved by using&nbsp;[NuGet][2]).
3.  You are relatively familiar with Visual Studio.

### Step 1 &#8211; Create a Castle specific Web API Dependency resolver

Native MVC uses the concept of a which seams to work quite nicely. However, when we&#8217;re talking about Web API, the way that we can configure the Web API pipeline to utilize a Dependency Injection framework is by using a . This interface allows you to write the code required to bridge the gap between the Web API pipeline and the Dependency Injection framework you have chosen to use. There are three main functions that you&#8217;ll need to implement in this class to satisfy the [`IDependencyResolver`][3] interface:

*   [`public object GetService(Type serviceType)`][4] &#8211; This function allows Web API to resolve a single object given the `serviceType` parameter.
*   [`public IEnumerable<object> GetServices(Type serviceType)`][5] &#8211; This function allows Web API to resolve a List of objects given the `serviceType` parameter.
*   `public IDependencyScope BeginScope()` &#8211; This allows the Web API pipeline to integrate with the Castle scoping behavior (more about this in step 2).

#### Example Castle Dependency Resolver

The code required for the Castle Dependency Resolver can be found within the Artisan Code GitHub repository:&nbsp;[WebApiCastleIntegration/WebApiCastleIntegration/Infrastructure/CastleDependencyResolver.cs][6] [csharp] using Castle.MicroKernel;

  
using Castle.Windsor;  
using System;  
using System.Collections.Generic;  
using System.Diagnostics;  
using System.Linq;  
using System.Web.Http.Dependencies;  
using IDependencyResolver = System.Web.Http.Dependencies.IDependencyResolver;</p> 
namespace WebApiCastleIntegration.Infrastructure  
{  
[DebuggerStepThrough] public class CastleDependencyResolver : IDependencyResolver  
{  
/// <summary>  
/// Indicates whether or not the class has previously been disposed.  
/// </summary>  
private bool _disposed;

/// <summary>  
/// Initializes a new instance of the <see cref=&#8221;CastleDependencyResolver&#8221;/> class.  
/// </summary>  
/// <param name=&#8221;container&#8221;>The Castle container used to resolve components.</param>  
/// <exception cref=&#8221;System.ArgumentNullException&#8221;>container;@The instance of the container cannot be null.</exception>  
public CastleDependencyResolver(IWindsorContainer container)  
{  
if (container == null)  
{  
throw new ArgumentNullException(&#8220;container&#8221;, @&#8221;The instance of the container cannot be null.&#8221;);  
}

this.Container = container;  
}

/// <summary>  
/// Finalizes an instance of the <see cref=&#8221;CastleDependencyResolver&#8221;/> class.  
/// </summary>  
~CastleDependencyResolver()  
{  
Dispose(false);  
}

/// <summary>  
/// Gets or sets the Castle container.  
/// </summary>  
public IWindsorContainer Container { get; protected set; }

/// <summary>  
/// Starts a resolution scope.  
/// </summary>  
/// <returns>  
/// A new instance of the <c>CastleDependencyScope</c>.  
/// </returns>  
public IDependencyScope BeginScope()  
{  
return new CastleDependencyScope(Container);  
}

/// <summary>  
/// Performs application-defined tasks associated with freeing, releasing, or resetting unmanaged resources.  
/// </summary>  
public void Dispose()  
{  
Dispose(true);  
GC.SuppressFinalize(this);  
}

/// <summary>  
/// Resolves a component from the Castle container.  
/// </summary>  
/// <param name=&#8221;serviceType&#8221;>The type of the component to be resolved.</param>  
/// <returns>  
/// The resolved component; otherwise <c>null</c> if no component could be resolved.  
/// </returns>  
public object GetService(Type serviceType)  
{  
try  
{  
return Container.Resolve(serviceType);  
}  
catch (ComponentNotFoundException)  
{  
return null;  
}  
}

/// <summary>  
/// Resolves a collection components from the Castle container.  
/// </summary>  
/// <param name=&#8221;serviceType&#8221;>The type of the component to be resolved.</param>  
/// <returns>  
/// A list of the resolved components; otherwise <c>new List()</c> if no components could be resolved.  
/// </returns>  
public IEnumerable<object> GetServices(Type serviceType)  
{  
// Not expected to throw an exception, rather return an empty IEnumerable.  
return Container.ResolveAll(serviceType).Cast<object>();  
}

/// <summary>  
/// Releases unmanaged and &#8211; optionally &#8211; managed resources.  
/// </summary>  
/// <param name=&#8221;disposing&#8221;>  
/// <c>true</c> to release both managed and unmanaged resources; <c>false</c> to release only unmanaged resources.  
/// </param>  
protected virtual void Dispose(bool disposing)  
{  
if (!_disposed)  
{  
if (disposing)  
{  
// Cleanup managed resources here, if necessary.  
if (Container != null)  
{  
Container.Dispose();  
Container = null;  
}  
}

// Cleanup any unmanaged resources here, if necessary.  
_disposed = true;  
}

// Call the base class&#8217;s Dispose(Boolean) method, if available.  
// base.Dispose( disposing );  
}  
}  
}  
[/csharp] 
### Step 2 &#8211; Create a Castle specific Web API Dependency scope

For those who are paying attention to the above code snippet, they might have noticed that within the I was creating a new `CastleDependencyScope`. If you&#8217;re wondering what this is and why we might need it, fear not for I will explain all.&nbsp;When you set up your Castle configuration and you specify a component as `LifestyleScoped()` the Web API pipeline needs to understand where a scope begins and ends. You could use a Http Module to explicitly start and stop a scope boundary, or you could have a class implement the [`IDependencyScope`][7] and return this object from the `BeginScope()` function. For this article, this is exactly what I&#8217;m doing. The [`IDependencyScope`][7] interface is very similar to the [`IDependencyResolver`][3] interface. In fact the `IDependencyResolver` interface derives from `IDependencyScope`

*   [`public object GetService(Type serviceType)`][8] &#8211; This function allows Web API to resolve a single object given the `serviceType` parameter.
*   [`public IEnumerable<object> GetServices(Type serviceType)`][9] &#8211; This function allows Web API to resolve a List of objects given the `serviceType` parameter.

If at this point you&#8217;re asking where the scoping element of the class comes into play, it&#8217;s primarily dealt with in the constructor and `Dispose(bool);` functions. The idea is that we create the castle scope within the constructor by calling `Container.BeginScope()` and then explicitly dispose of the scope within the `Dispose(bool);` function. If like me you&#8217;re also asking why we need two classes to manage the dependency resolution and the scoping separately, the answer is &#8220;I don&#8217;t know&#8221;. If I had to speculate, it could be due to the fact that different Dependency Injection containers might have different requirements when it comes to scoping and that Microsoft wanted a solution that would work for everyone.

#### Example Castle Dependency Scope

The code required for the Castle Dependency Scope can be found within the Artisan Code GitHub repository:&nbsp;[WebApiCastleIntegration/WebApiCastleIntegration/Infrastructure/CastleDependencyScope.cs][10] [csharp] using Castle.MicroKernel;

  
using Castle.MicroKernel.Lifestyle;  
using Castle.Windsor;  
using System;  
using System.Collections.Generic;  
using System.Diagnostics;  
using System.Linq;  
using System.Web.Http.Dependencies;

namespace WebApiCastleIntegration.Infrastructure  
{  
[DebuggerStepThrough] public class CastleDependencyScope : IDependencyScope  
{  
/// <summary>  
/// Indicates whether or not the class has previously been disposed.  
/// </summary>  
private bool _disposed;

/// <summary>  
/// The scope of the components being resolved.  
/// </summary>  
/// <remarks>  
/// This scope variable contains the castle scope object created when  
/// resolving the component with castle.  
/// </remarks>  
private IDisposable _scope;

/// <summary>  
/// Initializes a new instance of the <see cref=&#8221;CastleDependencyScope&#8221;/> class.  
/// </summary>  
/// <param name=&#8221;container&#8221;>The castle container used to resolve components.</param>  
/// <exception cref=&#8221;System.ArgumentNullException&#8221;>container</exception>  
public CastleDependencyScope(IWindsorContainer container)  
{  
if (container == null)  
{  
throw new ArgumentNullException(&#8220;container&#8221;);  
}

this.Container = container;  
_scope = Container.BeginScope();  
}

/// <summary>  
/// Finalizes an instance of the <see cref=&#8221;CastleDependencyScope&#8221;/> class.  
/// </summary>  
~CastleDependencyScope()  
{  
Dispose(false);  
}

/// <summary>  
/// The Castle container used to resolve components.  
/// </summary>  
public IWindsorContainer Container { get; protected set; }

/// <summary>  
/// Performs application-defined tasks associated with freeing, releasing, or resetting unmanaged resources.  
/// </summary>  
public void Dispose()  
{  
Dispose(true);  
GC.SuppressFinalize(this);  
}

/// <summary>  
/// Resolves a component from the Castle container.  
/// </summary>  
/// <param name=&#8221;serviceType&#8221;>The type of the component to be resolved.</param>  
/// <returns>  
/// The resolved component; otherwise <c>null</c> if no component could be resolved.  
/// </returns>  
public object GetService(Type serviceType)  
{  
try  
{  
return Container.Resolve(serviceType);  
}  
catch (ComponentNotFoundException)  
{  
return null;  
}  
}

/// <summary>  
/// Resolves a collection components from the Castle container.  
/// </summary>  
/// <param name=&#8221;serviceType&#8221;>The type of the component to be resolved.</param>  
/// <returns>  
/// A list of the resolved components; otherwise <c>new List()</c> if no components could be resolved.  
/// </returns>  
public IEnumerable<object> GetServices(Type serviceType)  
{  
// Not expected to throw an exception, rather return an empty IEnumerable.  
return Container.ResolveAll(serviceType).Cast<object>();  
}

/// <summary>  
/// Releases unmanaged and &#8211; optionally &#8211; managed resources.  
/// </summary>  
/// <param name=&#8221;disposing&#8221;>  
/// <c>true</c> to release both managed and unmanaged resources; <c>false</c> to release only unmanaged resources.  
/// </param>  
protected virtual void Dispose(bool disposing)  
{  
if (!_disposed)  
{  
if (disposing)  
{  
// Cleanup managed resources here, if necessary.  
if (_scope != null)  
{  
_scope.Dispose();  
_scope = null;  
}  
}

// Cleanup any unmanaged resources here, if necessary.

_disposed = true;  
}

// Call the base class&#8217;s Dispose(Boolean) method, if available.  
// base.Dispose( disposing );  
}  
}  
}  
[/csharp] 
### Step 3 – Create a Castle installer for the Web API Controllers

Just like the [last post][1], we&#8217;ll need an installer to configure Castle to resolve the projects dependencies correctly. Rather than just repeating what I wrote previously I&#8217;ll let you go to that article and read the appropriate section. One difference this time round is that I&#8217;m going to be using Castles build in Fluent interface in order to reflect through the assembly and automatically register all the ApiControllers, using the following code: [csharp] // Register all the WebApi controllers within this assembly

  
container.Register(Classes.FromAssembly(typeof(ValuesController).Assembly)  
.BasedOn<ApiController>()  
.LifestyleScoped());  
[/csharp] 
An example Castle Web API Installer can be found within the Artisan Code GitHub repository: [WebApiCastleIntegration/WebApiCastleIntegration/Infrastructure/ApplicationCastleInstaller.cs][11]

### Step 4 – Wire everything up

At this point you should have the `CastleDependencyResolver`, `CastleDependencyScope` and the `CastleApplicationInstaller` ready to go. All that&#8217;s required to successfully get Web API to both these components is to add a little wiring up code to the application start-up. The following code needs to be added to the [`Application_Start`][12] function within the &#8216;Global.asax.cs&#8217; file. Here&#8217;s an example `Application_Start` function:  
[csharp] protected void Application_Start()  
{  
AreaRegistration.RegisterAllAreas();

FilterConfig.RegisterGlobalFilters(GlobalFilters.Filters);  
BundleConfig.RegisterBundles(BundleTable.Bundles);

// Initialize Castle & install application components  
var container = new WindsorContainer();  
container.Install(new ApplicationCastleInstaller());

// Configure WebApi to use a new CastleDependencyResolver as its dependency resolver  
GlobalConfiguration.Configuration.DependencyResolver = new CastleDependencyResolver(container);

// Configure WebApi to use the newly configured GlobalConfiguration complete with Castle dependency resolver  
WebApiConfig.Register(GlobalConfiguration.Configuration);  
RouteConfig.RegisterRoutes(RouteTable.Routes);  
}  
[/csharp] <div class="panel panel-danger">
  <div class="panel-heading">
    <p class="panel-title">
      WARNING: If using Web API v2.x
    </p></p>
  </div>
  
  <div class="panel-body">
    If you are using Web API version 2.x, then the above wiring up code needs one small modification. The line<br /> [csharp]WebApiConfig.Register(GlobalConfiguration.Configuration);[/csharp] needs to be changed to:<br /> [csharp]GlobalConfiguration.Configure(WebApiConfig.Register);[/csharp] <p>
      Thanks <a href="https://twitter.com/bertyJobbo" title="Link to Rob Johnson's Twitter profile">@bertyJobbo</a> for picking this up and telling me about it!<br /> I&#8217;m going to leave the sample code and the article as they are for the moment, I might get round to writing a separate WebApiArticle for version 2.1 later. </div> </div> <p>
        You can find an example Global.asax.cs file within the Artisan Code GitHub repository: <a title="Example Globabl.asax.cs file" href="https://github.com/ArtisanCode/WebApiCastleIntegration/blob/master/WebApiCastleIntegration/Global.asax.cs">WebApiCastleIntegration/WebApiCastleIntegration/Global.asax.cs</a>
      </p>
      
      <h3>
        Step 4 &#8211; That&#8217;s all there is!
      </h3>
      
      <p>
        It&#8217;s not quite as simple as integrating Castle with MVC, but in four easy steps you should have Windsor.Castle integrated into the Web API pipeline and serving correctly configured Api Controllers with any dependencies already injected. If you would like to have a look at an example solution that has Castle integrated with MVC, check out the <a title="Example project containing functioning Castle integration with Web API" href="https://github.com/ArtisanCode/WebApiCastleIntegration">WebApiCastleIntegration</a> solution hosted on GitHub.
      </p>
      
      <h4>
        Edits:
      </h4>
      
      <ol>
        <li>
          [14-Jun-2014 @ 17:43] Updated the github repository to be <a title="New GitHub repository" href="https://github.com/ArtisanCode">https://github.com/ArtisanCode</a>
        </li>
        <li>
          [14-Jun-2014 @ 18:16] Added a warning about using Web API 2.1 and the example Global.asax.cs file
        </li>
      </ol>

 [1]: http://www.artisancode.co.uk/2014/04/integrating-windsor-castle-mvc/ "Integrating Windsor.Castle with MVC"
 [2]: http://www.nuget.org/packages/Castle.Windsor/ "Use NuGet to retrieve the latest version of Castle.Windsor"
 [3]: http://msdn.microsoft.com/en-us/library/system.web.mvc.idependencyresolver.aspx "MSDN - IDependencyResolver"
 [4]: http://msdn.microsoft.com/en-us/library/system.web.mvc.idependencyresolver.getservice.aspx "IDependencyResolver.GetService Method"
 [5]: http://msdn.microsoft.com/en-us/library/system.web.mvc.idependencyresolver.getservices.aspx "IDependencyResolver.GetServices Method"
 [6]: https://github.com/ArtisanCode/WebApiCastleIntegration/blob/master/WebApiCastleIntegration/Infrastructure/CastleDependencyResolver.cs "Example Castle Dependency resolver"
 [7]: http://msdn.microsoft.com/en-us/library/system.web.http.dependencies.idependencyscope.aspx "MSDN - IDependencyScope"
 [8]: http://msdn.microsoft.com/en-us/library/system.web.http.dependencies.idependencyscope.getservice.aspx "MSDN - IDependencyScope.GetService Method"
 [9]: http://msdn.microsoft.com/en-us/library/system.web.http.dependencies.idependencyscope.getservices.aspx "IDependencyScope.GetServices Method"
 [10]: https://github.com/ArtisanCode/WebApiCastleIntegration/blob/master/WebApiCastleIntegration/Infrastructure/CastleDependencyScope.cs "Example Castle Dependency scope"
 [11]: https://github.com/ArtisanCode/WebApiCastleIntegration/blob/master/WebApiCastleIntegration/Infrastructure/ApplicationCastleInstaller.cs "Example Application Castle Installer"
 [12]: http://msdn.microsoft.com/en-US/library/ee255103.aspx "MSDN Application_Start documentation"