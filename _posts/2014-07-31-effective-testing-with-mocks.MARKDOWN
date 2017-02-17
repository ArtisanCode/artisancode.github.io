---
id: 195
title: Effective testing with mocks
slug: effective-testing-with-mocks
date: 2014-07-31T20:15:03+00:00
author: Tom Kuhn
excerpt: 'An article exploring some of the benefits of using mocks within unit tests: Isolation, Safety, Speed, Flexibility, Sanity check for good design. This article also looks at how the way you write and structure your functions can have an impact on their testing as well as the need for mocks within the tests themselves.'
layout: post
guid: http://www.artisancode.co.uk/?p=195
categories:
  - mocks
  - opinion
  - system-design
  - testing
tags:
  - Design
  - mocks
  - testing
---
> It is an (almost) universally held opinion that software testing is usually a "good thing"&nbsp;<sup><a href="#fn1" id="ref1">[1]</a></sup><br/>
> [Tom Kuhn](https://twitter.com/Tom_Kuhn)

<div class="row">
  <div class="col-lg-6 col-sm-12">
    <p>
    This article will take you through some of my thoughts and musings about the role mocks have when writing unit tests. Please note that I mentioned <strong><em>_UNIT_</em></strong> tests and deliberately eschewed any mention of other forms of testing (acceptance, integration, manual etc.), whilst mocks can be helpful in these scenarios their main strength really gets highlighted during unit testing. Before we dive into the depths of this article, if you want to know more about Mocks and what they are or aren't here are a couple of invaluable articles exploring them in more depth then I ever could is one article:
    <ul>
      <li><a href="http://martinfowler.com/articles/mocksArentStubs.html">Mocks aren't stubs - Martin Fowler</a></li>
      <li><a href="http://blog.8thlight.com/uncle-bob/2014/05/14/TheLittleMocker.html">The little mocker - Uncle Bob</a></li>
    </ul>
    </p>
  </div>
  <div class="col-lg-6 col-sm-12">
    <a href="#" class="blog-image" data-featherlight="/content/2015/Mocks-testing-preference.png" ><img src="/content/2015/Mocks-testing-preference.png" alt="Image demonstrating the dangers of testing without Mocks"></a>
  </div>
</div>

## The benefits of Mocks

I'd just like to spend a little amount of time looking at some of the benefits that you can get from the use of mocks (this is by no means a definitive list if there are any I"ve missed off, please feel free to comment below!)

### Isolation

By using a mock framework, it allows you to be able to really be able to separate the unit of code that is under test from all other aspects of the system. This allows you to make your tests short and sweet by only focussing on the bare minimum needed to test the unit of code at hand. Some people argue that this is the exact reason that mocks are considered harmful as they don"t bare any relationship to normal operation. To this I would simply point out that the purpose of these unit tests are to check that the function behaves as you expect. If it fails to perform as expected, are the expectations wrong or is it some form of subtle error? These are the sorts of questions that isolated testing can really help with. To cover more realistic usage scenarios, this is where end-to-end/integration/acceptance tests really come to the fore to help exercise the code in a manner consistent with production.

### Safety

As mentioned before mocks allow you to test very specific scenarios within the code in a completely isolated fashion. As the image to the right shows someone testing a dynamite plunger, would you rather the plunger were attached to real sticks of dynamite when you were first designing and testing the plunger, or would you prefer to use a buzzer/lamp to indicate successful detonation? Certainly the second scenario is important to the product, but for the necessary repeatability required for testing I"d always choose the buzzer otherwise product testing would become very expensive very quickly! If we look at this analogy and bring it into the world of software development you could think of this as a piece of functionality that deletes a whole load of records within a database. If you"re are running tests every time you check-in then surely you don"t want to go through the pain of inserting loads of test data into the database only to delete the data a minute later. more to the point, what if the same test is running at the same time in two separate locations most likely, one test run is going to fail. Mocks allow you to be confident that no matter what the test environment, the tests should always pass.

### Speed

As we have eluded to before, the time taken to execute a test can be a major factor in it"s success and value. If your CI (Continuous Integration) build took fifty-five minutes to run every time you checked-in a file, then the soonest indication you would have that something was wrong is almost an hour after check-in and by this point you will likely have moved onto another piece of work. This will then cause you to have to drop the current work to refocus on the failing build. Mocks can allow you to skip or trim down the amount of time required to set up for tests as you aren"t reliant on external systems/data in order to get the tests to pass.

### Flexibility

Mocks give you complete control of the execution of your code and allow you to be in complete control over what happens to that unit of code during the test. Mocks also allow you to simulate any behaviour you need to in order to see what would happen if one of your dependencies did something unexpected e.g. threw an exception, how does that unit under test handle it? Coupled with the fact that the mocks allow you a degree of safety, this can be an invaluable tool when checking the expected behaviour of a unit of code.

### Sanity check for code design

This is probably one of my favourite reasons for looking to mocks to help test a function. The basic premise is this: if a test is too painful to write (e.g. it is taking too long, or it"s difficult to read and understand) then it"s an indication that the function is trying to do too much or your design could be improved. By having the unit test as a mirror held up to your code it allows you to feel the pain of having to test an overly large function, coincidently, this is the exact same pain someone else will have to go through when maintaining the same function. This can be said of unit tests in general but by utilising the mock framework it forces you to think about and analyse the dependencies being used by the function as you need to specifically provide setup code for each dependency.

Like any tool Mocking frameworks can either drastically help or hinder your tests and by extension your code quality, the trick to getting the best out of mocks comes from knowing when and where to reach for the mocking tool. There are those that take a fairly dim view of mocking frameworks (recently [To Kill a Mockingtest](http://techblog.realestate.com.au/to-kill-a-mockingtest/ "To Kill a Mockingtest")) and there are those that like to take the time to analyse where and when mocks can add the greatest benefit. There are of course people out there who try and use the same tool for all purposes, but this isn"t an option that I"d recommend!

## What are we testing?

Hopefully by now I"ve given you the sales pitch and convinced you that mocks have their place within the world of unit testing, but this doesn"t come close to actually talking about _what_ we are testing. FUNCTIONS! More and more these days I find that the code I write falls into two main categories: Orchestration functions and algorithmic functions. By identifying the type of function we are writing and testing, it can help inform what tools and techniques are appropriate for testing. What do these categories actually mean?

### Orchestration functions

These functions manage class dependencies in order to achieve the desired behaviour. What do I mean by this? If you take the ***extremely*** contrived example below that orders a product for a customer. This function performs a number of business checks to ensure that the user is registered and that there is sufficient stock for the order to process, then it redirects to the payment section with any payment details auto-filled. As you can see, this function isn"t responsible for actually performing any of the tasks, rather it"s only concern is to manage it"s dependencies and achieve the correct function flow for the required business scenario.

```csharp
public ICustomers Customers { get; set; }
public IProducts Products { get; set; }
public IPayments PaymentGateway { get; set; }
public IRedirectionHandler Redirection { get; set; }

public ActionResult OrderProduct(string customerUserName, IEnumerable<int> productIDs)
{
    if (!Customers.IsRegistered(customerUserName))
    {
        return Redirection.RedirectToRegistration(customerUserName);        
    }

    var reserveStockResult = Products.ReserveProductsForCustomer(customerUserName, productIDs);
    if(reserveStockResult.Status != StockReservationStatus.OK)
    {
        return Redirection.RedirectToUserConfirmation(reserveStockResult);
    }

    var savedPaymentDetails = PaymentGateway.RetrievedAnySavedDetails(customerUserName);
    return Redirection.RedirectToPaymentConfirmation(savedPaymentDetails);
}
```

### Algorithmic functions

These functions do not manage many (if at all) dependencies and are often concerned with only achieving the correct result. Again, below there is yet another contrived example of what I mean by an algorithmic function. All this function is trying to do is to return the total value of a shopping cart including the applicable tax.<sup><a href="#fn2" id="ref2">[2]</a></sup>

```csharp
public ShoppingCartTotal CalculateShoppingCartTotal(IEnumerable<ProductSelection> products)
{
    ShoppingCartTotal result = new ShoppingCartTotal();
 
    result.TotalProductValue = products.Sum(x => x.PricePerUnit * x.Quantity);
    result.TotalTax = products.Where(x => !x.IsTaxExempt).Sum(x => (x.PricePerUnit * x.Quantity) * 0.2M); // 20% VAT
 
    return result;
}
```

Given these two types of functions I find myself trying to separate my code into these two categories as much as possible with as little overlap as I can get away with. If I find orchestration functions that contain algorithmic sections, I attempt to refactor the code and split up the responsibilities into smaller functions. The same is obviously true in the other direction. So I hear yourself saying that "_this is all nice and dandy, but what does it have to do with effective testing with mocks?!_". Given the two categories of functions listed above, it shouldn"t be too much of a leap to see that mocks can help more with one category than the other.

By splitting the code as much as you can into its constituent parts, one of the benefits that you get is that you can confine your mock usage to a smaller subsection of the functions under test. You actually get to choose whether you want to use the "classic" test route for algorithmic type functions e.g. "given an input of _x_ I expect the function to return _y_". This is particularly suited to functions that perform calculations, transformations, or validations as the test criteria are usually black or white e.g. [MSDN - Assert.AreEqual Method](http://msdn.microsoft.com/en-us/library/microsoft.visualstudio.testtools.unittesting.assert.areequal.aspx) `Assert.AreEqual(expectedTaxValue, result.TotalTax);`. This form of unit tests allow the developer to claim that the code produces the expected results, not necessarily that the software behaves correctly or is well designed. Conversely, you also get to choose when you bring in mocks to help test the behaviour and functional flow of you software and gain all the benefits mentioned above. This choice is a powerful thing as it allows you to pick the best method for your particular scenario without always being forced blindly down the same path/testing strategy.

Mocks are a tool, just like any tool you need to learn when it is appropriate to use them and practice your mastery of this tool to improve the quality of your software.

* * *

<p id="fn1">
  1. Although I did once work with a colleague who insisted that they didn"t need to test their code &#8220;because it compiles&#8221;.<a href="#ref1" title="Jump back to footnote 1 in the text.">↩</a>
</p>

<p id="fn2">
  2. The issue I have with all these contrived examples is the fact that they are mostly nasty and don"t particularly represent best practices, but please bare with me as they"re present to make a point!.<a href="#ref2" title="Jump back to footnote 2 in the text.">↩</a>
</p>
