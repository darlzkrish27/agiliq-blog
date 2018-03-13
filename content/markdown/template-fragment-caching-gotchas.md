Title: Template fragment caching gotchas
Date: 2015-08-06 00:42:19
Modified: 2015-08-06 11:12:19+05:30
Slug: template-fragment-caching-gotchas
Authors: akshar
Tags: django
Summary: #### Variables in cached template fragment Assuming this is in template. {% cache 300 nums %} {% for i in nums %} <p>i</p> {% endfor %} {% endcache %} And assuming we send {'nums': range(100)} from context, then 0 to 99 will be sent in the response. Now suppose we change context to {'nums': range(1000)}, still for next 5 minutes i.e until the cache expires, 0 to 99 will be sent in the response. 0 to 999 will not be sent in the response. To fix this, we should use the variable too with the {% cache %} tag. So
#### Variables in cached template fragment

Assuming this is in template.

	{% cache 300 nums %}
	{% for i in nums %}
		<p>i</p>
	{% endfor %}
	{% endcache %}

And assuming we send {'nums': range(100)} from context, then 0 to 99 will be sent in the response.

Now suppose we change context to {'nums': range(1000)}, still for next 5 minutes i.e until the cache expires, 0 to 99 will be sent in the response. 0 to 999 will not be sent in the response.

To fix this, we should use the variable too with the {% cache %} tag. So correct code would be

	{% cache 300 nums_cache nums %}
	{% for i in nums %}
		<p>i</p>
	{% endfor %}
	{% endcache %}

After this whenever context **nums** changes, cache would be reevaluated.

#### Boolean variable in cached template fragment

Assuming template contains

	{% cache 300 hello %}
	{% if hello %}
	  <p>Hello</p>
	{% endif %}
	{% endcache %}

and assuming {'hello': True} is sent in context. Then &lt;p&gt;Hello&lt;/p&gt; will be sent in response.

Now even when we send {'hello': False} in context, "&lt;p&gt;Hello&lt;/p&gt;" will still be sent in response because it's already cached.

To fix this.

	{% cache 300 hello_cache hello %}
	{% if hello %}
	  <p>Hello</p>
	{% endif %}
	{% endcache %}

#### request.user in cached template fragment

	{% cache 300 username_cache %}
	<p>{{request.user.username}}</p>
	{% endcache %}

When current user logs out and a new user logs in, still the username of old user is shown on the web page because template fragment is already cached and would not be reevaluated.

Fix it like

	{% cache 300 username_cache request.user.username %}
	<p>{{request.user.username}}</p>
	{% endcache %}

#### Gotcha while using base template

Assuming we are using template inheritance, and our base template i.e base.html looks like:

	{% load cache %}
	<html>
		<head>
		</head>
		<body>
		{% cache 300 base_body %}
		{% block body %}
		{% endblock %}
		{% endcache %}
		</body>
	</html>

And the child template, say test.html looks like

	{% extends "base.html" %}
	{% load cache %}
	{% block body %}
	  {% cache 300 username request.user.username %}
		<p>{{request.user.username}}</p>
	  {% endcache %}
	{% endblock %}

Assuming our view uses test.html. Suppose cache is empty now, and you are logged in as user1 and then you make the request to a url which uses test.html. So response will contain "&lt;p&gt;user1&lt;/p&gt;".

Now if you logout and login as user2 and make the request, still the response will be "&lt;p&gt;user1&lt;/p&gt;" instead of "&lt;p&gt;user2&lt;/p&gt;".

The reason for this is, as per <a href="https://docs.djangoproject.com/en/1.7/topics/templates/" target="_blank">Django docs</a>.

	The extends tag is the key here. It tells the template engine that this template “extends” another template. When the template system evaluates this template, first it locates the parent – in this case, “base.html”.

But in our case, the parent template itself has some cached content, which Django will use. Django will not even bother to look at the `{% block body %}` of child template if it finds cached content `base_body` for the parent template.

The fix for this is to differentiate the cache of different users even in the base template. So we could change the base template to look like.

	{% load cache %}
	<html>
		<head>
		</head>
		<body>
		{% cache 300 base_body request.user.username %}
		{% block body %}
		{% endblock %}
		{% endcache %}
		</body>
	</html>

After this, if user1 is logged in the "&lt;p&gt;user1&lt;/p&gt;" is sent in response. If user2 is logged in then "&lt;p&gt;user2&lt;/p&gt;" is sent in response.

#### Gotcha with {% include %} tag.

It is similar to the gotcha of template inheritance discussed in last section.

Assuming the view uses test.html, which contains.

    {% cache 60 body_cache %}
    {% include "body.html" %}
    {% endcache %}

And body.html contains

	<p>Hello {{request.user.username}}</p>

So when first user, say user1 logs in, cache is evaluated and &lt;p&gt;Hello user1&lt;/p&gt; is sent in response. When user2 logs in, still &lt;p&gt;Hello user2&lt;/p&gt; will be sent in response.

To fix this, test.html should also use all the variables on which **included** template depends.

So code in test.html should change to

    {% cache 60 body_cache request.user %}
    {% include "body.html" %}
    {% endcache %}

If **included** template, i.e body.html depended on any other variable, that other variable too should be used with {% cache %} tag.

