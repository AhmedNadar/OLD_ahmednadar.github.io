---
layout: post
title: "Ternary Operator as one bite chocolate"
date: 2014-01-07 01:32:54 -0500
comments: true
tags: [ruby, rails, code]
---

### Ruby Ternary Operator

Ruby has lots of magic or shortcut to get the job done with beauty and efficiency. One of these magic is **Ternary Operator**. Since I like to understand why things are named the way they are, lets understand what is the 'Ternary' word.

### Defination
**Ternary** is a latin word, means "composed of three parts". Wikipedia lists different meanings and examples for it [here](http://en.wikipedia.org/wiki/Ternary).

**Ternary** *is a conditional operator where it evaluates giving expression and returns a value when it is true or return another value when it's false.* Like so:

{% highlight %}
expression/condition ? result if condition is true : result if condition is false
{% endhighlight %}


You see, Ternary Operator is simple and more readable, as if you are reading English. *Don't you agree?*  Check out this simple examples:

{% highlight ruby %}
Human = true ? 'Human' : 'Robot
human = (true && 'Human') || 'Robot
{% endhighlight %}

As we know it is a short versions of `if / then` statements.

#### If else example

```
max_speed = 100
if max_speed = 120
	puts("Slow down, it's dangerous!")
else
	puts("You drive safely.")
end	
```


#### The Ternary way

```
max_speed = 100 
puts max_speed = 120 ? "Slow down, it's dangerous!" : "You drive safely."
```
**OR**

```
max_speed = 120 ? (puts "Slow down, it's dangerous!") : (puts "You drive safely.")
```
### How it works

* Giving expression `max_speed = 100`,
* Evaluate this condition `max_speed = 120`
	* If its true, output `("Slow down, it's dangerous!")`
* Otherwise/else
	* If its false, output `("You drive safely.")`   
	
	
### Similar operator

Ternary operator is smiler to the following:
`language ||= "Ruby"`. Which you can expand it to `language = language || "Ruby"` If we have `language` is set to certain value then it will return that value otherwise its assigned to `Ruby`.


### Real scenario
Here is a real scenario where you can use Ternary Operator. Lets say within your Rails view you want to show the user full name when user is Signed in; otherwise you show 'Welcome guest".

Using If else you can write it as follow:

```
<% if @user_signed_in? %>
	<%= "Welcome", @current_user.full_name %>
<% else %>
	<%= "Guest" @new_user %>
```

And with Ternary Operator you can say like so

```
<%= @user_signed_in?  ? "Welcome", @current_user.full_name : "Guest" @new_user %>
```

As you see Ternary operator is short, simple and sweet as a bite chocolate makes developer happy.

Happy coding :)

