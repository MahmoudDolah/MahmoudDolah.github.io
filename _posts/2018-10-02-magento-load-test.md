---
layout: post
title:  "How to Perform a Load Test for Magento using Gatling 2.3.1"
date:   2018-10-02 06:39:32 -0400

categories: DevOps Magento
---

This week I'd been given a very important assignment - Write load testing scripts for one of our large enterprise clients. Previously, I had only done this once before, and it was for a much smaller Magento site that didn't have anything close to the infrastructure that this enterprise client has.    

I put on my headphones, started blasting [some house music][edm-spotify], and got to work


## Figuring out the paths
Fortunately, this part of the plan stayed the same    
The easiest way to find out the most visited pages are to check Google Analytics and New Relic

I had to create multiple different scenarios to simulate common userflow, such as browsing, searching, and, most importantly, bouncing.
Despite what most businesses wish, most potential customers will visit a site and then immediately leave after visiting one . . . maybe 2 pages if they're lucky

Using data from Google Analytics and New Relic, I was able to determine 1) the most popular pages and 2) common routes that users take in order to simulate their actions

I was able to hit most of the pages in Gatling like this
```
val Search = scenario("Search")
    .exec(http("Home")
        .get("/")
    .pause(7)
    .exec(http("Category Page")
        .get("/vitamins"))
    .pause(7)
    .exec(http("Search Headache")
        .get("/catalogsearch/result?q=headache")
    .pause(2)
```

In the above example, I'm hitting the homepage, a category page, and the search page


## Adding to Cart
A very important part of a load test is hitting the most visited pages - and the cart page certainly qualifies as such (especially since we're trying to emulate the userflow)

So let's try hitting a product page, adding to cart, and then visiting checkout

```
val Search = scenario("Search")
    .exec(http("PDP")
        .get("/selenium-natural-200-mcg-tablets-100-tablets-8012653")
        .pause(10)
    .exec(http("Add to Cart")
        .post("/checkout/cart/add/uenc/aHR0cHM6Ly93d3cucml0ZWFpZC5jb20vc2hvcC9uZXV0cm9nZW5hLWNsZWFuc2luZy10b3dlbGV0dGVzLXByZS1tb2lzdGVuZWQtbWFrZXVwLXJlbW92ZXItcmVmaWxsLXBhY2stMjUtdG93ZWxldHRlcy0yMDEzMzky/product/18453/form_key/V7xdnrKjPfdhRV8S/")
        .formParam("qty", "3"))
        .pause(15)
    .exec(http("Checkout Cart")
        .get("/checkout/cart")
    .pause(8)
```

***Note: If you want to learn more about what a uenc is, I recommend this [source][uenc]***

This won't work - astute readers might already know that the form key is a unique identifier for a user session. In other words, it's not something that you can hardcode in a script like this.

Fortunately, there's a solution!

```
val Search = scenario("Search")
    .exec(http("PDP")
        .get("/selenium-natural-200-mcg-tablets-100-tablets-8012653")
            .check(regex("""<input name="form_key" type="hidden" value="(.*)" />""")
            .saveAs("formKey")))
    .pause(10)
    .exec(http("Add to Cart")
        .post("/checkout/cart/add/uenc/aHR0cHM6Ly93d3cucml0ZWFpZC5jb20vc2hvcC9uZXV0cm9nZW5hLWNsZWFuc2luZy10b3dlbGV0dGVzLXByZS1tb2lzdGVuZWQtbWFrZXVwLXJlbW92ZXItcmVmaWxsLXBhY2stMjUtdG93ZWxldHRlcy0yMDEzMzky/product/18453/form_key/${formKey}/")
        .formParam("qty", "3"))
    .pause(15)
    .exec(http("Checkout Cart")
        .get("/checkout/cart")
    .pause(8)
```

The above Gatling code works by scraping the PDP for the form key and using it in the POST in order to correctly add to cart

***Note: If you want to validate that the above code works, I recommend checking the sales_flat_quote table in Magento and confirming that a new row is added***

In general, I'm fairly pleased with how this turned out, however, in the future, I would probably add more scenarios to simulate different types of user flows and events that might go on, such as for email campaigns and other sales


[edm-spotify]: https://open.spotify.com/user/spotify/playlist/37i9dQZF1DX6GJXiuZRisr?si=zGmj7ZTmSnmmA0v-HFERZA
[terraform]: https://www.terraform.io/
[uenc]: https://maxchadwick.xyz/blog/wtf-is-uenc
