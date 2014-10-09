---
layout: post
title: "Dive into AngularJS | ngHide"
date: 2014-07-15 03:36:27 -0500
comments: true
tags: [Angular, ngHide]
---

**Good Day!**

One of the things I like being a developer is working with the DOM (Document Object Model), more specifically is manipulating and changing it. The normal way to do so is use jQuery. Although its much better than pure JS but still I need to write more code. Recently I'm enjoying work with AngularJS. The framework support so many 'helpers' such as directives, filters and other helper comes out of the box for DOM manipulation. One of these helpers is **ngHide** directive, but first lest defined directives.

### What are Directives?

According to Angular DOC, [Directives](https://docs.angularjs.org/guide/directive) are:

> Markers on a DOM element (such as an attribute, element name, comment or CSS class) that tell AngularJS's HTML compiler ($compile) to attach a specified behaviour to that DOM element or even transform the DOM element and its children.
> 
> Angular comes with a set of these directives built-in, like `ngBind`, `ngModel`, `ngHide`, and `ngView`. Much like you create controllers and services, you can create your own directives for Angular to use. When Angular bootstraps your application, the HTML compiler traverses the DOM matching directives against the DOM elements.

What else we can say about Directives:

* Directive teaches HTML to write new tricks. Such as dome manipulation, data binding, handle events, modify CSS, … etc
- Most directives are used as attributes. And some are used as new element/tag.
- Append `data` to the directive such as `data-ng-show` to match HTML5 attributes. Doing so you could avoid some browsers complains about Anglular directives.
- Avoid writing explicit logic or expression in views.

### ngHide
 
One of these directive I want to talk about it is **ngHide**. 
It's purpose is show or hide a given HTML element based on what is been provided by the `ngHide` attribute.

{% highlight html linenos %}
<span>Hide:</span> <input type="checkbox" data-ng-model="isHidden">
<br>
<div data-ng-hide="isHidden">
	<span>Name:</span> <input type="text" ng-model="name" />{{name}}
</div>
{% endhighlight %}

When the `checkbox` element is been selected, the second DIV HTML element is hidden by adding a `ng-hide` CSS class onto the element. The CSS class sets the element display to none `display: none;` using an `!important` flag.


I really enjoy working with Angular, I feel more productive and write concise code, similar to Ruby and Rails. I'll write more about Angular and it's magic coming weeks/months.

Until then, Happy coding :)