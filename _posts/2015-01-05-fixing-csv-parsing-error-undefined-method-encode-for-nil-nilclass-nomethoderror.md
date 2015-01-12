---
layout: post
title: "Fixing CSV parsing error undefined method 'encode' for nil NilClass"
date: 2015-01-05 14:18:29 -0500
comments: true
tags: [Ruby, CSV]

---
I was working with Ruby CSV library parsing code and got this puzzling...

#Error:

{% highlight ruby %}
undefined method 'encode' for nil:NilClass (NoMethodError)
{% endhighlight %}
I know i didn't defined `encode` method, here is my code:

{% highlight ruby %}
contents = CSV.open "students.csv", headers: true, header_converters: :symbol
{% endhighlight %}

# Curiosty time

So, **why my code caused this error?!**

I narrowed it down and removed `header_converters:` method; and it worked. Horaaay... This is good, now I know which area is causing this weird error. But... I still couldn't answer; why this method is not happy with my code or CSV file?!

Reading thourgh [CSV source code](http://www.ruby-doc.org/stdlib-1.9.3/libdoc/csv/rdoc/CSV.html#method-c-open), I see we can optionally pass a Hash to the `open` method, right?. Here I'm using `:header_converters` which it's default [value is](http://www.ruby-doc.org/stdlib-1.9.3/libdoc/csv/rdoc/CSV.html#DEFAULT_OPTIONS) `nil` and it's [defined](http://www.ruby-doc.org/stdlib-1.9.3/libdoc/csv/rdoc/CSV.html#new-method) as:

  > ... All built-in converters try to transcode headers to UTF-8 before converting. The conversion will fail if the data cannot be transcoded, leaving the header unchanged.

From that definition I see the method `header_converters:` failed because my CSV file/data couldn't be transcoded, that's why I got `nil`.

# Ahaaa moment
After reviewed my file/data I noticed the first row starts with a comma like so:
{% highlight ruby %}
(,RegDate,first_Name,last_Name,......)
{% endhighlight%}
When I initiate that row with a space before the comma, it worked. The row now looks like this:

{% highlight ruby %}
( ,RegDate,first_Name,last_Name,......)
{% endhighlight %}
Also when I removed both initial comma and space like so...

{% highlight ruby %}
  (RegDate,first_Name,last_Name,......)
{% endhighlight %}

it worked as well; and `NoMethodError` went away.  Horray.