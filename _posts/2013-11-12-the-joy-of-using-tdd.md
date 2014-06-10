---
layout: post
title: "The joy of using TDD"
date: 2013-11-12 05:10:19 -0500
comments: true
tags: [Rails, RSpec, TDD]
---

I wanted to practice RSpec more and feel that joy of TDD. So I start with simple tests aiming to cover and learn more about RSpec. During the practice I learned more about [Matcher](https://www.relishapp.com/rspec/rspec-expectations/docs/built-in-matchers), [Exceptions](https://www.relishapp.com/rspec/rspec-expectations/v/2-5/docs/built-in-matchers/raise-error-matcher), missing method and the thinking process of solving an issue using test first.


As you see below I have bunch of tests with Movie. Also I have required the movie file from `"../lib/movie.rb"`

`require_relative "../lib/movie.rb"`

{% highlight ruby linenos %} 
describe Movie do
	it "has a name"
	it "has a list of casts"
	it "complains if there is a bad word in the name"
	it "is favoured if it has a celebrity in it"
end
{% endhighlight %} 

Although we have nothing written within our spec but we can run it and see all four tests are pending. It always good practise to run our specs to understand the whole picture of our specifications.

Now lets tackle each spec.

### 1- Movie has a name

In order to test Movie has a name, we should first create an object of that name by creating a new instance of Movie
	
{% highlight ruby %} 
movie_rspec.rb
it "has a name" do
	Movie.new
end
{% endhighlight %} 

When we call that new object/instance of Movie, it 'should' 'respond to' us. Luckly Rspec has a matcher for that propose `respond_to`.
	
{% highlight ruby %} 
movie_rspec.rb
it "has a name" do
	Movie.new.should respond_to
end
{% endhighlight %} 

Now we should pass a 'symbol' for what we are calling here which is 'name'. 
	
Because of the nature of Ruby where we able to pass a message and not method call, `respond_to` which is a rspec matcher check if the object response to a certain message.

{% highlight ruby %} 
movie_rspec.rb
it "has a name" do
	Movie.new.should respond_to :name
end
{% endhighlight %} 

Now lets run our spec which it will fail:
{% highlight bash %} 
» rspec --color
.F
Failures:
	1) Movie has a name
 		Failure/Error: Movie.new.should respond_to :name
   		expected #<Movie:0x007ffe942b9720> to respond to :name
{% endhighlight %} 

This is error shows we have an expectation error and not a run time error.
What the error says is, it expected to respond to :name but it fails. Which means the new Movie object doesn't not have a property/attribute of :name 

To fix that we should declare :name as an attribute of Movie

{% highlight ruby %}
class Movie
	attr_accessor :name
end
{% endhighlight %}

Lets make sure our code works by running our spec:

`rspec --color`

and yappy we passed out first test, next.

### 2- Movie has a list of casts

We are testing if the Movie object has a 'list' of casts.

We can use the same matcher `respond_to` but to be more accurate we are testing if it has a 'list'. As we know Array contain a collection of items which is `kind of` a list. So our spec goes like this:

{% highlight ruby %}
it "has a list of casts" do
	Movie.new.casts.should be_kind_of Array
end
{% endhighlight %}

We are saying that **Casts** which are we testing against should be kind of an array. This would fail because we introduced a new method without defending it. Run the test and see for your self.

{% highlight bash %}
» rspec --color
.F
Failures:
  1) Movie has a list of casts
     Failure/Error: Movie.new.casts.should be_kind_of Array
     NoMethodError:
       undefined method `casts' for #<Movie:0x007fcd2ba52c88>
{% endhighlight %}

To fix this we should add Casts to Movie object attributes, like so:

{% highlight ruby %}
	class Movie
		attr_accessor :name, :casts
	end
{% endhighlight %}

Here we added the new attribute `casts`, which is supposed to be an Array as we specified in the spec. Therefor this spec would fail.
{% highlight bash %}
Failures:

1) Movie has a list of casts
   Failure/Error: Movie.new.casts.should be_kind_of Array
     expected nil to be a kind of Array
{% endhighlight %}
The error message is clear. It expected a nil to be kind of Array. Which means it expected the new attribute 'casts' to be Array but it is not. We need to return an Array, we should fix this.

We would create new constructor with our variables `name` and `casts`. 

{% highlight ruby %}
def initialize(name, casts)
	@name = name
	@casts = casts
end
{% endhighlight %}
Doing that only would not solve our problem, because our 'casts' is not defined as an Array. We have 2 ways to do so:
		
**1- First option**. Declare our variable `casts` as an empty Array to the constructor if nothing passed, `casts= []`

{% highlight ruby %}
def initialize(name, casts =[] )
	@name = name
	@casts = casts
end
{% endhighlight %}

This would solve our current spec because we are not passing any parameter to 'casts', but this would cause a new error.
{% highlight bash linenos %}
Failures:

1) Movie has a name
   Failure/Error: Movie.new.should respond_to :name
   ArgumentError:
     wrong number of arguments (0 for 1..2)

2) Movie has a list of casts
   Failure/Error: Movie.new.casts.should be_kind_of Array
   ArgumentError:
     wrong number of arguments (0 for 1..2)
{% endhighlight %}

This failed because we passed two parameter to the constructor but we didn't update our spec. So we should do that:
{% highlight ruby linenos %}
it "has a name" do
	Movie.new("Some Name").should respond_to :name
end

it "has a list of casts" do
	Movie.new("Some Name").casts.should be_kind_of Array
end
{% endhighlight %}

Now our specs are passed, Hurray!
		 
2- Second option, declare our list of casts as an Array when we pass it. 
Our goal here is not be able to create the Movie if we are passing a list of funny casts names.

We would use 'context' with our spec, which is short version if you would say of `describe`. We would create a variable for funny names using `let()` method which would pass a block with expression assigned to the funny name variable.

{% highlight ruby %}
context "given a funny list/array of casts" do
let(:funny_casts) {{ }}
	it "fails to create given a funny cast list" do
		expect {Movie.new("Some name", funny_casts)}.to raise_error
	end
end
{% endhighlight %}

As you would guess, we expect to see an error when we pass a funny cast name.

{% highlight bash %}
Failures:

1) Movie given a funny list/array of casts fails to create given a funny cast list
   Failure/Error: expect {Movie.new("Some name", funny_casts)}.to raise_error
     expected Exception but nothing was raised

Lets write code to pass our spec. We would raise an Exception unless Casts is an Array.

	def initialize(name, casts = [])
		raise Exception unless casts.is_a? Array
		.
		.
		.
	end
{% endhighlight %}	

### 3- Complains when Movie name has a bad word
Our goal is to raise an error if the Movie name include a bad name in it. 

{% highlight ruby %}
movie_rspec.rb
it "complains if there is a bad word in the name" do
	expect {Movie.new("Ugly Name")}.to raise_error
end

To make sure it fails first we run the test, and it does. So lets fix that.

movie.rb
def initialize(name, casts = [])
	.
	@name = name
	@casts = casts
	raise Exception if @name && has_bad_name?
	.
	.
	.
end
{% endhighlight %}

Our spec would fails because it doesn't know about our new method `has_bad_name?'
So we have to create that method which it will include list of words that are bad.

With our new method `had_bad_name?` we want to show an error if list of bad words such as 'ugly' includes downcase or uppercase `@name`.
{% highlight ruby %}
def has_bad_name?
	list_of_words = %w{ugly bad lousy}
	list_of_words.include? @name.downcase
end
{% endhighlight %}

When we run our test it would fails because `list_of_words` it contains all specified words as one `{"crappy bad lousy"}` instead of each one by it self `{"crappy", "bad", "lousy"}`. to fix that, we would use split method and make sure the final list of words is been subtracted from the Movie name that include bad name.

Run our test and it would pass.
Yeaaay we are one test away from finishing our specs.

### 4- Favour cast name when it is a celebrity
We want to favour a cast name when it is a celebrity. 
{% highlight ruby %}
it "is favoured of it has a celebrity in it" do
	Movie.new("Some name", ["Matt Damon", "Someone"]).should be_favoured
end
{% endhighlight %}

Out test would fail because it doesn't know 'be_favoured' method. 
We are saying here that Movie should be favoured if it has a celebrity 'Matt Damon'. But rspec doesn't have `be_favoured` method. When this happens 'missing method' comes to work. It checks if the subject of the spec has a predicate matcher or a method with last part of it match our favoured method. So let's check if the favoured method return true for 'Matt Demon' like so:
{% highlight ruby linenos %}
def favoured?
	@casts.include? "Matt Damon"
end

Run our last test and it would pass.
» rspec --color
	.....

	Finished in 0.00796 seconds
	5 examples, 0 failures
{% endhighlight %}

Yeaaay we have done it. We have covered a lot of aspects with Rspec. 
Lets recap:

####1- respond_to matcher
We use it when we want to check if our subject response to certain message
`subject.should respond_to :message`

####2- be_kind_of matcher
We use it when we want to check if our subject belongs to an Object, like inheritance.
`subject.should be_kind_of Object`

####3- be_predicate matcher
We use it when we want to check if our subject has a method that returns true or false. Using `missing method` feature in rspec this allows us to identify boolean method.
`subject.should be_predicate` and 
`def predicate?; end`

####4- Exception
We use it when we expect an exception to be raised  
`expect {block}.to raise_error`

Here are the final rspec file and our Movie class:

###A- movie_rspec.rb
Movie RSpec:

{% highlight ruby linenos %}
describe Movie do
	it "has a name" do
		Movie.new("Some Name").should respond_to :name
	end
	it "has a list of casts" do
	Movie.new("Some Name").casts.should be_kind_of Array
	end
	context "given a bad list/array of casts" do
		let(:funny_casts) {{ }}
		it "fails to create given a bad player list" do
			expect {Movie.new("Some name", funny_casts)}.to raise_error
		end
	end
	it "complains if there is a bad word in the name" do
		expect {Movie.new("ugly Name")}.to raise_error
	end
	it "is favored of it has a celebrity in it" do
		Movie.new("Some name", ["Matt Damon", "Someone"]).should be_favored
	end
end
{% endhighlight %}


###B- movie.rb
{% highlight ruby linenos %}
class Movie
	attr_accessor :name, :casts
	def initialize(name, casts = [])
		raise Exception unless casts.is_a? Array
		@name = name
		@casts = casts
		raise Exception if @name && has_bad_name?
	end
	def has_bad_name?
		list_of_words = %w{ugly bad lousy}
		list_of_words - @name.downcase.split(" ") != list_of_words
	end
	def favored?
		@casts.include? "Matt Damon"
	end
end
{% endhighlight %}

Rspec is fun to use therefor; Happy Coding, eh! ;)