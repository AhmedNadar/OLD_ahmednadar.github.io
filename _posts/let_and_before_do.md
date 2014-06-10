<!--
layout: post
title: "Let and Before_do"
date: 2013-11-17 10:32:54 -0500
comments: true
tags: [Rails, RSpec, Test]
-->

## 1- How I write my specs - Let and Before Do

[RSpec](https://www.relishapp.com/rspec/) is a fantastic tool and a way of writing a [BDD (Behaviour Driven Development)](http://en.wikipedia.org/wiki/Behavior-driven_development) process that validates the development of your application.

Writing a test is always challenging and fun with such a great tool such as [RSpec](https://www.relishapp.com/rspec/). Applying good practise is not an easy task because best practise is always debatable thing in the community which it makes exciting.

Those best practices are applied to Before and let methods.
 
#### WHY

RSpec has two methods [`before`](https://www.relishapp.com/rspec/rspec-core/v/2-0/docs/hooks/before-and-after-hooks) and [`let`](https://www.relishapp.com/rspec/rspec-core/docs/helper-methods/let-and-let) that shares great purpose which is creating common variables across your tests.

#### Before block
Using `before{}` block creates and initialize an instance variable with nil value as default. Which it leads to false positives, such as allowing certain type of test to pass where it shouldn't:

{% highlight ruby %} 
	before(:each) do
    @user = User.find(username: "Catinthehat")
    @user.logout
	end
{% endhighlight %} 

{% highlight ruby %} 
	it "should log the user out" do
    expect(@usr).to be_nil
	end
{% endhighlight %} 	





=====
let(:inital_rank) { 10 }

let(:movie) { ::Movie.new "madacascar", inital_rank }



before do

@inital_rank = 10

@movie = ::Movie.new "madacascar", @inital_rank

there is a different way for writing test here:

end