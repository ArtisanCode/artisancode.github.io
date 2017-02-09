---
id: 72
title: Integrating Windsor.Castle with ASP.Net MVC
date: 2014-04-05T16:24:23+00:00
author: Tom Kuhn
layout: post
guid: http://www.artisancode.co.uk/?p=72
slug: integrating-windsor-castle-mvc
categories:
  - 'c#'
  - Technical
tags:
  - Asp.Net MVC
  - 'c#'
  - Dependency Injection
  - Windsor.Castle
published: true
---
Making sure that ASP.NET MVC and Windsor Castle play nicely together is surprisingly easy, just as long as you know what steps need to be performed. This post will hopefully take you through the process of integrating Castle (or any other Dependency Injection container) with the latest version of MVC. At the end of the article, there will be a link to the Artisan Code GitHub repository that contains a reference MVC solution with all the setup code required. This can be used as a basis for a new project, or simply as a guide to help you integrate Castle into an existing project. Without any further ado, lets start looking at some code.


This article makes the following assumptions:

  1. You already have a MVC project (either a new project or an existing project).
  2. You already have Castle.Windsor downloaded and referenced by your project (this can easily be achieved by using [NuGet](http://www.nuget.org/packages/Castle.Windsor/ "Use NuGet to retrieve the latest version of Castle.Windsor")).
  3. You are relatively familiar with Visual Studio.

### Step 1 &#8211; Create a Castle specific MVC Controller factory

MVC uses the concept of a &#8216;Controller factory&#8217; in order to generate the MVC Controllers when the HTTP request is process by the server. In order to allow Castle a chance to hook into the application pipeline and resolve the controller with all of it&#8217;s dependencies a new controller factory needs to be implemented and inserted into the MVC pipeline. Although this sounds very technical, thankfully Microsoft has made this pretty straightforward and simple. To create a new controller factory all that is required is to create a class that inherits from [`DefaultControllerFactory`](http://msdn.microsoft.com/en-us/library/system.web.mvc.defaultcontrollerfactory.aspx "MSDN - DefaultControllerFactory") and then override the required methods to allow Castle to resolve the controller rather than the default controller factory. The main methods that require overriding are: [`GetControllerInstance`](http://msdn.microsoft.com/en-us/library/system.web.mvc.defaultcontrollerfactory.createcontroller.aspx "MSDN - Default Controller Factory class, CreateController method") and [`ReleaseController`](http://msdn.microsoft.com/en-us/library/system.web.mvc.defaultcontrollerfactory.releasecontroller.aspx "MSDN - Default Controller Factory class, Release Controller method").

  * [`GetControllerInstance`](http://msdn.microsoft.com/en-us/library/system.web.mvc.defaultcontrollerfactory.createcontroller.aspx "MSDN - Default Controller Factory class, CreateController method") &#8211; This is the method that gets called by the MVC framework to build the Controller needed to service the HTTP request.
  * [`ReleaseController`](http://msdn.microsoft.com/en-us/library/system.web.mvc.defaultcontrollerfactory.releasecontroller.aspx "MSDN - Default Controller Factory class, Release Controller method") &#8211; This is the method that gets called by MVC when the Controller is no longer required, this allows MVC to effectively manage the life-cycle of the Controller and provides a convenient place to inform Castle that the Controller is no longer required.

#### Example Castle Controller Factory

The code required for the Castle Controller Factory can be found within the Artisan Code GitHub repository: [MvcCastleIntegration/MvcCastleIntegration/Infrastructure/CastleControllerFactory.cs](https://github.com/Tom-Kuhn/artisan-code/blob/master/MvcCastleIntegration/MvcCastleIntegration/Infrastructure/CastleControllerFactory.cs "Link to the CastleControllerFactory within the Artisan Code GitHub repository")

[csharp]

using Castle.Windsor;

using System;

using System.Web.Mvc;

using System.Web.Routing;

namespace MvcCastleIntegration.Infrastructure

{

public class CastleControllerFactory : DefaultControllerFactory

{

/// <summary>

/// Gets or sets the container.

/// </summary>

public IWindsorContainer Container { get; protected set; }

/// <summary>

/// Initializes a new instance of the class.

/// </summary>

///The container used to resolve the MVC controllers.

/// <exception cref="System.ArgumentNullException">container</exception>

public CastleControllerFactory(IWindsorContainer container)

{

if (container == null)

{

throw new ArgumentNullException("container");

}

this.Container = container;

}

/// <summary>

/// Retrieves the controller instance for the specified request context and controller type.

/// </summary>

/// <param name="requestContext">The context of the HTTP request, which includes the HTTP context and route data.</param>

///controllerType">The type of the controller.

/// <returns>

/// The controller instance.

/// </returns>

protected override IController GetControllerInstance(RequestContext requestContext, Type controllerType)

{

if (controllerType == null)

{

return null;

}

// Retrieve the requested controller from Castle

return Container.Resolve(controllerType) as IController;

}

/// <summary>

/// Releases the specified controller.

/// </summary>

/// <param name="controller">The controller to release.</param>

public override void ReleaseController(IController controller)

{

// If controller implements IDisposable, clean it up responsibly

var disposableController = controller as IDisposable;

if (disposableController != null)

{

disposableController.Dispose();

}

// Inform Castle that the controller is no longer required

Container.Release(controller);

}

}

}

[/csharp]

### Step 2 &#8211; Create a Castle installer for the MVC Controllers

In order to allow Castle to create the MVC controllers, they need to be registered with Castle along with any dependencies required. Castle is quite flexible in how it allows you to register components, but this article will use the [Fluent Registration API](http://docs.castleproject.org/Windsor.Fluent-Registration-API.ashx "Windsor.Castle Fluent Registration API documentation") to inform Castle about the components it will need to create and manage. To maintain some structure to the Castle registrations, [Castle Installers](http://docs.castleproject.org/Windsor.Installers.ashx "Windsor.Castle Installer documentation") will be used to compartmentalize the components into manageable and coherent groups. Installers are a great way to organize your Dependency Injection configuration and keep the config noise to a minimum. There are two main ways to register your Controllers with Castle (there are many more but that&#8217;s a topic for another day).

#### Individual Controller registration

You can Register each Controller separately with a new `container.Register(...)` statement e.g.

[csharp]container.Register(Component.For().LifestylePerWebRequest());[/csharp]

This has the advantage that you have absolute control over each and every Controller and how Castle manages it. However, this approach has two major drawbacks:

  1. This approach typically results in a lot of copy-and-paste code, which then leads to errors when you forget to change what you pasted!
  2. When registering Controllers individually It is often easy to forget to update your configuration when you add a new Controller to your project. This can then take you a few moments to wonder why your new Controller isn&#8217;t being created.

#### Controller registration via reflection

Another method of registering controllers is to use reflection to find all the MVC controllers within the project and then iterate through them to register with Castle. e.g.

[csharp]

var contollers = Assembly.GetExecutingAssembly().GetTypes().Where(x => x.BaseType == typeof(Controller)).ToList();

foreach (var controller in contollers)

{

container.Register(Component.For(controller).LifestylePerWebRequest());

}

[/csharp]

This method has the advantage that no matter how many Controllers are added to the solution, they will always be registered in a consistent fashion. The drawback to this approach is that each Controller is treated equally and registered in exactly the same fashion as all the other Controllers. If you have a need to delve deeper into Castle&#8217;s component registration e.g. manually configuring object dependents, then this approach might not be the best solution.

#### Example Castle MVC Controller installer

An example Castle MVC Installer can be found within the Artisan Code GitHub repository: [MvcCastleIntegration/MvcCastleIntegration/Infrastructure/ApplicationCastleInstaller.cs](https://github.com/Tom-Kuhn/artisan-code/blob/master/MvcCastleIntegration/MvcCastleIntegration/Infrastructure/ApplicationCastleInstaller.cs "Link to the GitHub example Castle Installer")

[csharp]

using Castle.MicroKernel.Registration;

using Castle.MicroKernel.SubSystems.Configuration;

using Castle.Windsor;

using MvcCastleIntegration.Dependencies;

using System.Linq;

using System.Reflection;

using System.Web.Mvc;

namespace MvcCastleIntegration.Infrastructure

{

public class ApplicationCastleInstaller : IWindsorInstaller

{

/// <summary>

/// Performs the installation in the IWindsorContainer" />.

/// </summary>

/// <param name="container">The container.</param>

/// <param name="store">The configuration store.</param>

/// <exception cref="System.NotImplementedException"></exception>

public void Install(IWindsorContainer container, IConfigurationStore store)

{

// Register working dependencies

container.Register(Component.For().ImplementedBy().LifestylePerWebRequest());

// Register the MVC controllers one by one

// container.Register(Component.For().LifestylePerWebRequest());

// Register all the MVC controllers in the current executing assembly

var contollers = Assembly.GetExecutingAssembly().GetTypes().Where(x => x.BaseType == typeof(Controller)).ToList();

foreach (var controller in contollers)

{

container.Register(Component.For(controller).LifestylePerWebRequest());

}

}

}

}

[/csharp]

If you are quite new to Castle I would highly recommend having a look at the [Castle Lifestyle documentation](http://docs.castleproject.org/Windsor.LifeStyles.ashx "Windsor.Castle lifestyle documentation") to find out a bit more about the options available.

### Step 3 &#8211; Wiring everything up

At this point you should have both the Controller Factory and the Installer ready to go. All that&#8217;s required to successfully get MVC to both these components is to add a little wiring up code to the application start-up. The following code needs to be added to the [`Application_Start`](http://msdn.microsoft.com/en-US/library/ee255103.aspx "MSDN Application_Start documentation") function within the &#8216;Global.asax.cs&#8217; file.

[csharp]

// Initialize Castle & install application components

var container = new WindsorContainer();

container.Install(new ApplicationCastleInstaller());

// Create the Controller Factory

var castleControllerFactory = new CastleControllerFactory(container);

// Add the Controller Factory into the MVC web request pipeline

ControllerBuilder.Current.SetControllerFactory(castleControllerFactory);

[/csharp]

This code effectively creates a new Windsor container, installs the Controller registrations and then inserts the Controller Factory into the MVC pipeline.

An example Global.asax.cs file can be found within the Artisan Code GitHub repository: [MvcCastleIntegration/MvcCastleIntegration/Global.asax.cs](https://github.com/Tom-Kuhn/artisan-code/blob/master/MvcCastleIntegration/MvcCastleIntegration/Global.asax.cs "Example Globabl.asax.cs file")

### Step 4 &#8211; That&#8217;s all there is!

It&#8217;s _that_ simple, in three easy steps you should have Windsor.Castle integrated into the MVC pipeline and serving correctly configured Controllers with any dependencies already injected. If you would like to have a look at an example solution that has Castle integrated with MVC, check out the [MvcCastleIntegration](https://github.com/Tom-Kuhn/artisan-code/tree/master/MvcCastleIntegration "Example project containing functioning Castle integration with MVC") solution hosted on GitHub.
