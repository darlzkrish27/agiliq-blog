Title: Building SEO optimised Django web applications
Date: 2008-10-07 11:59:44
Modified: 2008-10-07 22:29:44+05:30
Slug: building-seo-optimised-django-web-applications
Authors: shabda
Tags: seo
Summary: This is an article about building [SEO](http://www.seobook.com/glossary/#seo), optmised web applications with Django. I assume you already have an idea how Seo works. We will take the example of a hypothetical Blog app to explain the suggestions made. Without further ado, here are the tips. Basic 1. Use beautiful URLs. 2. Learn the difference between HTTP 301 and HTTP 302 redirects. 3. Layout content in a SEO friendly way. 4. Create a sitmaps. Not so obvious 1. Use the Admin to allow end users to update fields. 2. Do not get your Url structures too deep. 3. Create a robots.txt file.
This is an article about building [SEO](http://www.seobook.com/glossary/#seo), optmised web applications with Django. I assume you already have an idea how Seo works. We will take the example 
of a hypothetical Blog app to explain the suggestions made.

Without further ado, here are the tips.

Basic

1. Use beautiful URLs.
2. Learn the difference between HTTP 301 and HTTP 302 redirects.
3. Layout content in a SEO friendly way.
4. Create a sitmaps.

Not so obvious

1. Use the Admin to allow end users to update fields.
2. Do not get your Url structures too deep.
3. Create a robots.txt file. 
4. Link to your pages wisely.
5. Minimise duplicate content.
6. Ping Google when your site gets updated.

## Basic tips.

If you are building a production web app, you should really be using these techniques.

### Use beautiful URLs.

Say our app has a model like, 

	class Entry(models.Model):
	      title = models.CharField(max_length = 100)
	      body = models.TextField()

It would be very tempting to have Urls like /entry/15/, and map this url to the entry with PK 15. Resist this temptation. Search Engines need help to decide what a page is about, and the Urls plays a
major part in that. Instead we want the Url to depend on the title.

	class Entry(models.Model):
	      title = models.CharField(max_length = 100)
	      body = models.TextField()
	      slug = models.SlugField()
	      
	      def save(self):
		    self.slug = '-'.join(self.title.split())#And clean title, and make sure this is unique.
		    super(Entry, self).save()
	    
Then we can use slug, and have Urls like /entry/seo-tips-for-django/

Read more:  
[1]: [Dynamic URLs vs. Static URLs](http://www.seomoz.org/blog/dynamic-urls-vs-static-urls-the-best-practice-for-seo-is-still-clear)

### Learn the difference between HTTP 301 and HTTP 302 redirects.

A HTTP 301 redirects says that the url currently being accessed has moved permanenetly, while a HTTP 302 is temporary redirect which asks search engines to try the same url next time. In Django a Http 301
maps to HttpResponsePermanenetRedirect, and a HTTP 302 maps to HttpResponseRedirect. A 301 redirect makes the search engines count the links pointing to a old Url for the new Url.

Suppose our old Url was /entry/seo-tips-for-django/, and we edited the title to "Best SEO tips for Django", and want the new url to be /entry/best-seo-tips-for-django/. Now as ["Cool URIs Do not change"](http://www.w3.org/Provider/Style/URI), we want
to do an redirect from  /entry/seo-tips-for-django/ to /entry/best-seo-tips-for-django/. As some people may have already linked to /entry/seo-tips-for-django/, we want a HttpResponsePermananentRedirect here.

### Layout content in a SEO friendly way.

Django gives you complete control over how your Html will appear. Use this to your advantage. 

Search engines give more weight to the content which appears earlier in a page layout. So layout your page where the the content area appears before the footer, and then layout your pages using CSS.

For example the first base template is better than second,

	<body>
	{% block content %}
	{% endblock %}
	{% block sidebar %}
	{% endblock %}
	{% block footer %}
	{% endblock %}
	</body>


	<body>
	{% block footer %}
	{% endblock %}
	{% block sidebar %}
	{% endblock %}
	{% block content %}
	{% endblock %}
	</body>



### Create a sitmaps.

Django comes with a wonderful [Sitemaps framework](http://docs.djangoproject.com/en/dev/ref/contrib/sitemaps/). Use this to generate a Sitemap for all our dynamic pages.

Here is the all the code you need to generate the sitemap for our Entries, taken directly from [Django Sitemaps page]((http://docs.djangoproject.com/en/dev/ref/contrib/sitemaps/)), and it works perfectly.
Plus this is your urls.py and you have a shiny xml sitemap ready.

Read More  
[1]: [Site Map](http://en.wikipedia.org/wiki/Site_map)



### Use the Admin to allow end users to update fields.

Use the wonderful Admin interface to your advantage. For example, for /entry/seo-tips-for-django/ we can use the Admin to allow users to update the slug without updating the title.

### Do not get your Url structures too deep.

Search Engines give more weight to pages which are closer to the root of the site. Instead of /blog/entry/2008/oct/2008/seo-tips-for-django/, prefer /entry/seo-tips-for-django/. As Django makes including urlconfs inside
other urlconfs so easy, this can lead to deeper Url structure than needed.

For example your Django project has a main urls.py,

which does,

	url(r'^blog/', include('blog.urls')),

and blog/urls.py does

	url(r'^search/', include('blog.searchurls')),

and blog/urls.py does

	url(r'^search/$', 'search_view'),
	
then your serach page will be at /blog/search/search/, when it should probably be at /search/. Include with care.


which includes blog/urls.py

### Create a robots.txt file. 

Use [Django Robots](http://code.google.com/p/django-robots) to create robots,txt file for your Django site.

### Link to your pages wisely.

Eg. When you are linking to the the entry page from main page/other page on your site use, use descriptive anchor texts. SO the permalink for our entry pages shuld not be,

	<a href="{{ entry.get_absolute_url }}">Read More</a>

But rather,

	<a href="{{ entry.get_absolute_url }}">{{ entry.title }}</a>

### Minimise duplicate content.

Search engines hate duplicate content. So try not to have the same content on multiple Urls.

For example see this innocous looking url pattern 

	urlpatterns = patterns('',
	    (r'^entry/(\w+)/', 'blog.view.entry'),
	    
	    ...

This url pattern will match for all of /entry/seo-tips-for-django/1, /entry/seo-tips-for-django/2, /entry/seo-tips-for-django/3 and similar. 

To make sure only /entry/seo-tips-for-django/ matches, use 

	urlpatterns = patterns('',
	    (r'^entry/(\w+)/$', 'blog.view.entry'),
	    ...
	    
On a similar note, say you have commenst for your Blog, which you want permalinks for.

Do not do this.

	class Comment(models.Model):
	     def get_absolute_url(self):
		  #You would use permalink here.
		  return '/comment/%s/' % self.id # or even '/comment/%s/'  % self.slug
		  
	Instead what you want here is,

	class Comment(models.Model):
	     def get_absolute_url(self):
		  return '%s#%s' % (self.entry.get_absolute_url(), self.id)
	  
And create named anchors in your entries template. This makes sure that each comment is visible at only one Url.

### Ping Google when your site gets updated.

You would have created a Dynamic sitemap using the sitemaps framework. SO when your content gets updated, you sitemap would get updated too. Let Google know of this using django.contrib.sitemaps.ping_google

from django.contrib.sitemaps import ping_google

	 class Entry(models.Model):
	     # ...
	     def save(self, force_insert=False, force_update=False):
		 super(Entry, self).save(force_insert, force_update)
		 try:
		     ping_google()
		 except Exception:
		     # Bare 'except' because we could get a variety
		     # of HTTP-related exceptions.
		     pass

http://docs.djangoproject.com/en/dev/ref/contrib/sitemaps/#pinging-google

---------------

Need a web application built? [Talk to us](http://uswaretech.com/contact/).

