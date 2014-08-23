---
layout: post
title: "Modals made easy with Colorbox"
date: 2014-02-13 09:16:29 -0500
comments: true
tags: [JavaScript, HTML, CSS, jQuery]
---

**Good Day!**

I was working on the front-end part of a page which requires creating a modal/dialog for a specific section on the page. By default I was thinking about [jQuery dialog](http://jqueryui.com/dialog/). I have used it many times, reliable, easy to use and its part of the awesomeness jQuery UI. 

Before I start put the modal (coding and design) together, a friend of mine told me about the plugin [Colorbox](http://www.jacklmoore.com/colorbox/) for jQuery. When I read it's documentation it was interesting to know it has different approach	from the jQuery UI Modal. It can do more with less code. Its so customizable with an easy settings form of an object of key and value (similar to [hash](http://www.ruby-doc.org/core-2.1.2/Hash.html) in Ruby).

I'm only going to cover simp example on how to user Colorbox. Its [demos](http://www.jacklmoore.com/colorbox/) has more information and example for different scenarios.

Here is hot to write/format your code:

{% highlight javascript %}
	$(selector).colorbox({ key:value, key:value })
{% endhighlight %}

The selector could be an HTML Class or ID. Next is the Colorbox object where all the magic happens using keys and values. Colorbox offer lots of awesome properties (keys) with default values you change them to fit your projects' need. 

Lets see very simple example.


{% highlight html %}
<div class="click" href="#boxy">Open Colorbox Modal!</div>
<div style="display:none">
  <div class="box" id="boxy">
    <h1>Colorbox - a jQuery lightbox</h1>
    <p class="description">A lightweight customizable lightbox plugin for jQuery</p>
  </div>
</div>
{% endhighlight %}

My simple goal is, when user clicks on the first div `<div class="click">....</div>` the other div shows up as part of colorbox. 
You have to have the target div within another div that has display style is set to none. When user clicks on the first div, the target div will pop ups.

Here is the minimum JS code you need to get it done:

{% highlight javascript %}
$(document).ready(function () {
  $('.click').click(function () {
    $.colorbox({inline:true, href:"#boxy"});
  });
});
{% endhighlight %}

In Line 3 I'm calling the object `$.colorbox` and passing two properties. First one is `inline`, which	 is set to `true` in order to display the desired content (my target div) `<div class="box" id="boxy" >...</div>`. Second, is passing my target div using it's ID not class property as a `href`.

Check out my [Codepen](http://codepen.io/egyamado/pen/Jnxvi) page of full example.

Happy Coding, eh! ;)