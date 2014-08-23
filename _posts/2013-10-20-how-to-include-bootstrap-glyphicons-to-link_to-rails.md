---
layout: post
title: "How to include Bootstrap glyphicons to 'link_to' Rails"
date: 2013-10-20 10:12:34 -0500
comments: true
tags: [Rails, Bootstrap, Glyphicons, Tips]
---

**Good Day!**

## How to include Bootstrap glyphicons to link_to 

## Idea
I"m scrapping headlines form Reddit. I was able to pull each headers' link and content and display them in a list. Being a designer, I want my list to look nice. So I added Twitter Bootstrap gem. 

Here is initial link without Glyphicons:

##### HAML:
	{% highlight ruby %} 
		= link_to url[:content], url[:href], target: '_', class: "list-group-item"
	{% endhighlight %} 	
	
##### ERB:
	{% highlight ruby %} 
		<%= link_to url[:content], url[:href], target: '_', class: "list-group-item" %>
	{% endhighlight %} 

## Problem

I wanted to differentiate each links' target wither it's internal to Reddit or external. I thought of adding Bootstrap Glyphicons to "link_to" element.

**First**, thought is to add Glyphicons class to `link_to`, like so:

	{% highlight ruby %}
		= link_to url[:content], url[:href], target: '_', class: "list-group-item glyphicon glyphicon-link"
	{% endhighlight %}
	
	{% highlight ruby %}
		<%= link_to url[:content], url[:href], target: '_', class: "list-group-item glyphicon glyphicon-link" %>
	{% endhighlight %}	

But Bootstrap documentation recommended, [Don't mix Glyphicons with other components](http://getbootstrap.com/components/#glyphicons-how-to-use).

**Second**, I thought to add `span` tag after `link_to` element, like so:

	{% highlight ruby %}
		= link_to url[:content], url[:href], target: '_', class: "list-group-item"
	  	  %span.glyphicon.glyphicon-link`
	{% endhighlight %}
	
	{% highlight ruby %}
		<%= link_to url[:content], url[:href], target: '_', class: "list-group-item" %>
		<span class="glyphicon glyphicon-link"></span>
	{% endhighlight %}

But it didn't work as well. Glyphicons shows below each link, not beside it as I was hopping.
	
### Solution

I know I can [use a block with `link_to`](http://apidock.com/rails/ActionView/Helpers/UrlHelper/link_to). So I added Glyphicons and link content within the block and it worked. Here:

{% highlight ruby %}
	%h1 Giri with Rails
	%p
	  Read top Current headlines about #{link_to "Rails on Reddit", 'http://www.reddit.com/r/	rails/'}
	%ul.list-group
	  - @link.each do |url|
	    = link_to url[:href], target: '_', class: "list-group-item" do
	      %span.glyphicon.glyphicon-link
	      #{url[:content]}
{% endhighlight %}

Happy Coding, eh! ;)