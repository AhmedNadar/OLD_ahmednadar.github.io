---
layout: post
title: "Overwrite how Capybara ignores hidden elements"
date: 2013-09-12 11:22:54 -0400
comments: true
tags: [Capybara, Rspec, Rails, Test, TDD, tips]
---

Working with [Caybara](http://rubydoc.info/github/jnicklas/capybara/master) is fun and joy for me as an early web designer and front-end developer. I always care about the user experience -[UX](https://en.wikipedia.org/wiki/User_experience)- and how user would interact with the page.

While I was working on simple test using Capybara, the test didn't pass although I have what is require for test to pass. Here is my test:
 
{% highlight ruby %} 
	feature "view home page" do
 		scenario "user see page title" do
 	  	visit root_path
 	   	expect(page).to have_css 'title', text: 'Birds'
		end 
	end
{% endhighlight %}


In my test I'm expecting the page to have the title element with value of "**Bird**". But when I run my test, it faild:

{% highlight ruby %}
view home page
  user sees relevant information (FAILED - 1)

Failures:

  1) view home page user see page title
     Failure/Error: expect(page).to have_css 'title', text: 'Todos'
     Capybara::ExpectationNotMet:
       expected to find css "title" with text "Birds" but there were no matches. Also found "", which matched the selector but not all filters.
     # ./spec/integeration/view_home_spec.rb:6:in `block (2 levels) in <top (required)>'
{% endhighlight %}

This is strange since I know the page has the **Birds** title. 

### Background

Capybara has an interesting option ([Capybara.ignore_hidden_elements](https://github.com/jnicklas/capybara/blob/master/lib/capybara.rb#L20)) that configures the default query behaviour of finding nodes within the DOM. The default option value is '[true](https://github.com/jnicklas/capybara/blob/master/lib/capybara.rb#L352)', but we can overwrite the that value by passing `visible: false|true`. But it's not that easy because Capybara support different [drivers](https://github.com/jnicklas/capybara#drivers) such as `RackTest` and `Selenium`. Each one has it's `visible?` method; which means each driver has it's implementation for how to detect when an element on the page is visible or not. 

Capybara uses the `rack_test` driver by [default](https://github.com/jnicklas/capybara/blob/master/lib/capybara.rb#L50). But it's not that good, here is why from the documantation:

> Rack_test driver is fast but limited: it does not support JavaScript, nor is it able to access HTTP resources outside of your Rack application, such as remote APIs and OAuth services. To get around these limitations, you can set up a different default driver for your features.
 
This limitation causes the `visible?` method to only look for elements with `style="display: none"` attribute. Which means we have to query selectors to find page nodes.

### have_css with visible option

The [have_css](http://rubydoc.info/github/jnicklas/capybara/master/Capybara/RSpecMatchers:have_css) matcher is an instance method of [RSpecMatchers](http://rubydoc.info/github/jnicklas/capybara/master/frames/Capybara/RSpecMatchers), which is one of [Caybara](http://rubydoc.info/github/jnicklas/capybara/master)'s modules. 

If you look closely to `have_css` method you will see, it accepts one or more [options](http://rubydoc.info/github/jnicklas/capybara/master/Capybara/Query), such as `:visible` 

{% highlight ruby %}
def have_css(css, options={})
  HaveSelector.new(:css, css, options)
end
{% endhighlight %}

### Solution

Since I know the page title is visible `visible: true` and my default capybara `rack_test` driver only sees invisible elements `visible: false`. I know i have to make make sure my element is not visible `visible: false`.

Here is my test with the fix:

{% highlight ruby %}
 feature "view home page" do
 	scenario "user see page title" do
 	   visit root_path
 	   expect(page).to have_css 'title', text: 'Birds', visible: false
 	 end 
end
{% endhighlight %}

### Conclusion

If you chose rack test as your test driver then you would face issues with elements visibility. I would recommend using [Selenium](https://github.com/jnicklas/capybara#selenium), where it has no issue with visibility and it has Firefox already installed and you are ready to test.

Happy Coding, eh! ;)
