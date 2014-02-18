---
layout: post
title: "How to Create Codeblock for Octopress"
date: 2013-09-23 23:35:46 -0500
comments: true
categories: [meta, octopress]
---

While I was searching for different syntax highlighting for my [Octopress](http://octopress.org/) blog, I found this great [post](http://blog.codebykat.com/2013/05/23/gorgeous-octopress-codeblocks-with-coderay/) by [Kat](http://codebykat.com/), where she applied Github codeblock (highlighter colors, softer edges,…) alike using [Coderay](http://coderay.rubychan.de/).
[Kat](http://codebykat.com/) mentioned every stpes on how she done it, it’s worth reading.

<!--more-->

What I want to write about is what you need to do in order to show different syntax highlightings for each language codeblock.

Following is a Ruby codeblock, all what you have to do for Coderay to display the prober syntax highlighting is add <code>Ruby</code> as argument for <code>lang:</code> liks so <code>lang:ruby</code>:


{% coderay Open my... lang:ruby https://github.com/ahmednadar Githiub! %}
	... code goes here
{% endcoderay %}

Once you do that, wrap your code within <code>{ % codeblock % }</code> tag in order to Coderay does it’s magic:

####lang:ruby

{% coderay app/controllers/sessions_controller.rb lang:ruby https://github.com/AhmedNadar/local_merchant/blob/ef2315944646a3a7f8963fe37b5125b707388d8a/app/controllers/sessions_controller.rb Got to...! %}
  def create
    user = login(params[:email], params[:password], params[:remember_me])
    if user
      redirect_back_or_to root_url, :notice => "Logged in!"
    else
      flash.now.alert = "Email or password was invalid."
    end
  end
{% endcoderay %}

####lang:diff

{% coderay app/views/devise/passwords/new.html.erb lang:diff https://github.com/AhmedNadar/pinpic/commit/fa3ff6f42d8d6361365a101bc4f82559f9cd3672#diff-4 %}
@@ -1,10 +1,10 @@

 <h2>Forgot your password?</h2>

-<%= form_for(resource, :as => resource_name, :url => password_path(resource_name), :html => { :method => :post }) do |f| %>
+<%= form_for(resource, as: resource_name, url: password_path(resource_name), html: { method: :post }) do |f| %>
  <%= devise_error_messages! %>

- <div><%= f.label :email %><br />
- <%= f.email_field :email, :autofocus => true %></div>
+ <div><%= f.label :login %><br />
+ <%= f.text_field :login, :autofocus => true %></div>

  <div><%= f.submit "Send me reset password instructions" %></div>
 <% end %>
{% endcoderay %}



####lang:javascript

{% coderay JavaScript snippet lang:javascript %}
var recipe = {
    title: "Spaghetti Vongole",
    serving: 4,
    ingredients: ["spaghetti","clam","parsley", "cherry tomatoes"]
    };
    
console.log(recipe.title);
console.log("Serving: " + recipe.serving);
console.log("Ingredients :");
    for(i=0; i < recipe.ingredients.length; i++){
        console.log(recipe.ingredients[i]);
    }
{% endcoderay %}

If you noticed at the beginning of the post I have codeblock with line numbers looks like it’s not highlighted. Actually it’s highlighted as text 
<code>lang:text</code>. You may also notice the codeblock missing it’s caption and link.

The way it’s been done is; after the code snippet I added <code>{:lang="text"}</code>.

{% coderay Time to be Awesome! (awesome.rb) %}
... code goes here
{% endcoderay %}
{:lang="text"}


###Adding code snippets

There are several ways to add code snippets to your blog using Kramdown which offers 2 ways to add code snippets Fenced and Indented.

1- **Fenced**:

{% coderay %}
~~~~~
~~~
def write(dest)
  old_write(dest)
  post_write if respond_to?(:post_write)
end
~~~
{:lang="ruby"}
~~~~~
{% endcoderay %}

2- **Indented**:

{% coderay %}
~~~~~
~~~
puts "hello world"
~~~
{:lang="ruby"}
~~~~~
{% endcoderay %}

You can also use signle backtick for inline code snippet: <code>`print 'hello'`{:lang="ruby"}</code> or <code>`def hello`{:lang="ruby"}</code>.

Here you go, now you can apply [Coderay](http://coderay.rubychan.de/) syntax highlighting for your codeblock and add them in several ways.