---
layout: post
title: "Submit form using Enter key in Java Script"
date: 2013-12-23 01:32:54 -0500
comments: true
tags: [jQuery, JavaScript, HTML, tips]
---

**Good Day!**

While I'm I was working on the designing an existing form, I found it's been constructed as bunch for input elements except the submit 'button'. It wasn't an input or a button element, rather it was a DIV that should have the look and feel for rounded corner button. 

## The problem

While the user working on filling each input element such as, 'first name', 'last name'... and so on. User can't click on the Enter key to submit the form. Here is an example for a very simple form:

{% highlight html %}
<form id="contact_form" class="contact_form" method="post" action="welcome.html">
	<div class="first_name">
		<input type="text" name="f_name" id="f_name" class="fn_textbox">
	</div>
	<div class="last_name">
		<input type="text" name="l_name" id="l_name" class="ln_textbox">
	</div>
	<div class="email">
		<input type="text" name="e_mail" id="e_mail" class="email_textbox">
	</div>
	<div class="submit" id="submit_form">Submit</div>
</form>
{% endhighlight %}

As you may notice that submit button is a 'DIV'. Using its ID, a jQuery function triggers it on a 'click' event (not the Enter key) and submits the form.

I haven't faced such a problem like that, because I always construct forms with an input or button element for Submit action. 

	
### Solution

In order to give user the ability to click the Enter key on the keyboard to submit a form, I have to attach the **key Enter** location or **KeyCode** with a function when its been pressed. I got to know more about key codes from this [cool site](http://www.cambiaresearch.com/articles/15/javascript-char-codes-key-codes) by [@CambiaResearch](https://twitter.com/CambiaResearch). It gives you the ability to enter any keyboard key and it displays the key name and key code as well, very neat.


Here is simple and easy solution.

{% highlight javascript %}
<!-- <script type="text/javascript">
		$("#contact_form *").keypress(function (e) {
  		if ((e.which && e.which == 13) || (e.keyCode && e.keyCode == 13)) {
	      	$(this).parents('#contact_form').find('#submit_form').eq(0).focusin().click();
  		}
  	});
</script> -->
{% endhighlight %}


Lets speak code. Here I'm saying, when the pressed key is `==13` or its' `keyCode ==13`, find `id="submit_form"`, `focusin` and `click` it.


Happy Coding, eh! ;)
