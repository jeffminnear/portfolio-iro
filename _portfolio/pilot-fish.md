---
layout: post
title: Pilot-Fish
thumbnail-path: "img/pf-meteor.png"
short-description: A Price Comparison Tool For PC Games
feature-img: "img/blue_flash.jpg"

---
###Summary

Pilot-Fish is a price comparison tool for digital distribution PC games. It exists in two separate forms: a [command-line utility](https://www.npmjs.com/package/pilot-fish) built in Node.js, and a [web app](http://www.pf-app.com) built on the MeteorJS platform. Both versions show the current and regular price of games on four different retail sites.

As a student, I wanted to learn the fundamentals of Node.js module development and publication, as well as build a responsive, fully functional web application that I would actually want to use.

###Development

For the command-line version of Pilot-Fish, I used Commander to parse user input and options and structure the overall application. The user calls the application with the `pf` command followed by the title of the game they are searching for as a string. They can optionally provide an integer (the `limit` parameter) to specify the number of results to return from each store.

From the `--help` documentation:

> If the game you're looking for doesn't show up in results, try increasing the limit. For example: `$ pf 'dark souls' 5`

Commander then provides the arguments to `pf-scrapers`, a custom Node package I wrote to send `GET` requests to the stores' sites and scrape the necessary data using Cheerio (a Node implementation of jQuery). This data is returned as a promise in the form of a standardized JSON object.

{% highlight javascript %}
// create search result object
var searchResult = {
    title: null,
    simplifiedTitle: null,
    price: null,
    normalPrice: null,
    imageURL: null,
    linkURL: null,
    store: 'amazon.com'
};

    ...

searchResult.imageURL = list_result.find('img').attr('src');

    ...

searchResult.linkURL = list_result.find('a.a-link-normal').attr('href');

    ...
{% endhighlight %}

Because each store uses slightly different formatting to display the titles of games (e.g. 'The Elder Scrolls© V: Skyrim' vs. 'The Elder Scrolls V - Skyrim'), I wrote a function to simplify the titles and then compared the simplified strings instead when grouping results.

{% highlight javascript %}
var simplify = {
    simplifyString : function (string) {
        return string.toLowerCase().split(/\W/).filter(function(e) { return e; }).join(' ');
    }
};
{% endhighlight %}

Finally, I used the Chalk module to output a color-coded list of games ordered by title and price.

![Example]({{ site.baseurl }}/img/pilot-fish.png)

When building the web version of Pilot-Fish, I chose the MeteorJS platform in part because of its ability to use NPM packages. This meant that I could import my previously published `pf-scrapers` module and avoid rewriting all my custom jQuery.

I used Jade and the Materialize CSS framework to simplify the rapid creation of a responsive and professional looking UI.

{%highlight jade %}
div.row(class="search-results grey lighten-4")
      +each(results)
        +result
{% endhighlight %}

![Example 2]({{ site.baseurl }}/img/pf-meteor.png)

I also utilized jQueryUI and MongoDB to create a self-populating database of titles that are suggested as autocompletion results once the user begins typing in the search field.

![Autocomplete]({{ site.baseurl }}/img/autocomplete.gif)

Whenever a result is returned by the app, its simplified title is inserted into the Mongo collection, making the suggestions smarter and more inclusive every time a user performs a search.

{% highlight javascript %}
Meteor.call('titles.insert', results[i].simplifiedTitle);
{% endhighlight %}

I made use of the `levenshtein` module, allowing me to calculate each simplified title's similarity to the search query and provide the most relevant results first, and added a 'Sale' switch that displays only items currently on sale or alerts the user if no such item exists in the results.

###Conclusion

Pilot-Fish was the culmination of everything I had learned about web development up to that point and I am extremely proud of it. I was able to satisfy all of my initial goals for the project, and even managed to add some bonus functionality such as the autocomplete suggestions mentioned above.
