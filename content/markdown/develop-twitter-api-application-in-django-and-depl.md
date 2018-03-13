Title: Develop Twitter API application in django and deploy on Google App Engine
Date: 2009-04-28 16:44:01
Modified: 2009-04-29 03:14:01+05:30
Slug: develop-twitter-api-application-in-django-and-depl
Authors: lakshman
Tags: django
Summary: Twitter's robust <a href="http://apiwiki.twitter.com/Twitter-API-Documentation">REST API</a> enables building of good applications that leverage its increasingly large and real-time data. <img class="alignright" title="Google App Engine" src="http://code.google.com/appengine/images/appengine_lowres.gif" alt="" width="142" height="109" /> Google's cloud system <a href="http://code.google.com/appengine/">App Engine</a> not only provides a scalable platform for deploying applications, but also proves to be economically viable because of its pay per resource usage model and a large free quota. Little wonder then, there are increasingly large number of twitter apps being deployed on App Engine. In this post, I am going to examine how to create a simple application based on twitter's REST API and deploy
Twitter's robust <a href="http://apiwiki.twitter.com/Twitter-API-Documentation">REST API</a> enables building of good applications that leverage its increasingly large and real-time data.

<img class="alignright" title="Google App Engine" src="http://code.google.com/appengine/images/appengine_lowres.gif" alt="" width="142" height="109" />

Google's cloud system <a href="http://code.google.com/appengine/">App Engine</a> not only provides a scalable platform for deploying applications, but also proves to be economically viable because of its pay per resource usage model and a large free quota.

Little wonder then, there are increasingly large number of twitter apps being deployed on App Engine.

In this post, I am going to examine how to create a simple application based on twitter's REST API and deploy it on Google App Engine. A deployed version can be found on [Twitter-Follow](http://twitter-follow.appspot.com/). The specification is simple. It finds if a twitter user is following another twitter user, given their user names.

<img class="alignright size-thumbnail wp-image-461" title="twitter-follow" src="http://uswaretech.com/blog/wp-content/uploads/2009/04/screenshot-150x150.png" alt="" width="150" height="150" />
The application is developed using django and deployed on Appengine using the [app engine patch](http://code.google.com/appengine/articles/app-engine-patch.html) project.

The code is open sourced with GPL v3 and can be checked out from [Google Code](http://code.google.com/p/twitter-follow/).

Lets get started building this application.

### Create an application

Create a new application from <a href="http://appengine.google.com/">App Engine Dashboard</a> by registering an unique appspot.com sub-domain. This will be your application's unique identifier. You may have to register first.

### Install App Engine SDK

[Download](http://code.google.com/appengine/downloads.html#Google_App_Engine_SDK_for_Python) and install the App Engine SDK. There is no _installation_ for linux users, just place the folder in `/usr/local/google_appengine`

### Download app-engine-patch

Download the <a href="http://app-engine-patch.googlecode.com/files/app-engine-patch-sample-1.0.zip">sample project</a>. It comes bundled with all the essential django 1.0 files that are required.
Since we do not need the sample application, you may remove the unnecessary sample application folders and correspondingly modify settings.py, but for this demo app, it doesn't matter even if you dont.

### Modify **`app.yaml`**:
	application: twitter-follow
	version: 1
	runtime: python
	api_version: 1

	default_expiration: '3650d'

	handlers:
	- url: /media
	static_dir: media

	- url: /.*
	script: common/appenginepatch/main.py

The only thing you need to modify in this file is the unique application name- for which you registered, and the static files' folder.

As we are using the patch, the handler script to run should always point to `main.py` of the appenginepatch folder. Leave it as it is.

The `/media` maps to the static directory `./media`, in which you place the static JS and CSS files.

### Modify **`urls.py`**
	urlpatterns = auth_patterns + patterns('',
		(r'^$', 'views.disp'),

Pretty much straight forward, we handle only 1 page and the request is directed to a specified view function ie `disp` in `views.py` file which we are going to create.

There are some more imports in the file than django `urls.py` for the patch to work. Leave them as it is.

### What about **`models.py`**?

For this application, we do not need any database. We just query twitter as and when user requests.

What about session persistence? Oh, we achieve that using hidden input fields in the form, as we will see in the views.

Hence we don't even need to create an application, just a view and a form in the root directory

### Write **`forms.py`**

	from django import forms

	class input_form(forms.Form):
	    follower = forms.CharField(max_length=50, required=True, error_messages={'required':'Follower Name is required'})
	    following = forms.CharField(max_length=50, required=True, error_messages={'required':'Following Name is required'})

We need a simple form that we can create in our function, so that it is displayed in the template, by the framework.

After all, the django forms framework is one big advantage of using django over the App Engine's builtin webapp framework, even for simple applications. With forms now done, we proceed to the views.

### Write **`views.py`**

#### is_follows:

	def is_follows(follower, following):

	    url = 'http://twitter.com/friendships/exists.json?user_a=%s&user_b=%s' %(
		follower, following)

	    password_mgr = urllib2.HTTPPasswordMgrWithDefaultRealm()
	    top_level_url = "http://twitter.com/"
	    password_mgr.add_password(None, top_level_url, settings.TWNAME, settings.TWPASSWORD)
	    handler = urllib2.HTTPBasicAuthHandler(password_mgr)
	    opener = urllib2.build_opener(urllib2.HTTPHandler, handler)
	    urllib2.install_opener(opener)
	    
	    try:
		return simplejson.load(urllib2.urlopen(url))
	    except IOError, e:
		#print "Something wrong. This shouldnt happen"
		print e,"Protected user's details cant be found "
		return False

This function queries twitter and returns whether a `follower` is following `following`. Lot of boilerplate code, a standard way of adding authentication headers to all requests to the specified domain. Otherwise, the function just examines json returned by twitter and returns corresponding value.

#### disp and helper functions:
	def format_is_follows(follower, following):
	    fol = 'YES' if is_follows(follower,following) else 'NO'
	    new_entry = (fol,follower,following)
	    return "%s:%s:%s" %new_entry
		
	def get_entry_tuples(str1="YES:str1:str2;NO:str3:str4"):
	    list1 = str1.split(';')
	    list2 = [tuple(i.split(":")) for i in list1]
	    return list2
		
		
	def disp(req):
	    if req.method == 'POST':
		form = forms.input_form(req.POST)

		if form.is_valid():
		    new_str = format_is_follows(form.cleaned_data['follower'],
						form.cleaned_data['following'])
		    old_str = req.POST['old']
		    
		    form_data = "%s;%s" %(new_str,old_str)
		    entries = get_entry_tuples(form_data)[:-1]
		    
		    payload = {'entries':entries,
			       'form':form,
			       'disp':True,
			       'form_data':form_data}
		else:
		    payload = {'form':form,
			       'disp':True}
	    else:
		form = forms.input_form()
		payload =  {'form':form,'disp':False}
	    return render_to_response('page3.html',payload,)

`disp` is the function called by the framework each time an HTTP request arises. Validates the form data and passes contextual variables for displaying. Some additional code, for examining the previously queried values from the hidden field in the form.

`format_is_follows` returns the follower following data in the format required by the template we are going to be using- a tuple of 3 values and `get_entry_tuples` returns the data
that we are going to store in the hidden form. They are Separated into multiple functions here, just for clarity.

### **`template.html`**
	<html><body>

		{% for fol in entries %} 
			{% ifequal fol.0 "YES" %}
				Yes. <a href="http://twitter.com/{{fol.1}}">@{{fol.1}}</a> is following <a href="http://twitter.com/{{fol.2}}">@{{fol.2}}</a>  {% else %} 
				No. <a href="http://twitter.com/{{fol.1}}">@{{fol.1}}</a> is not following <a href="http://twitter.com/{{fol.2}}">@{{fol.2}}</a>
			{% endifequal %}
		{% endfor %}

	<form action="." method="POST">
		{{ form.following.errors }} {{ form.follower.errors }}
		Is {{ form.follower }} following {{ form.following }} 
		<input type="hidden" name="old" value="{{ form_data }}">
		<input type="submit" align="center" value="Find out!" /> 
	</form>

	</body></html> 

It contains, the result display part and the form. The form also has a hidden input field. Thanks to django form framework not adding the `<form>` tags and the `submit` button. This provides us the flexibility to be able to add the hidden field with the content to be displayed in the next post request.

### Run the server

With all the necessary files ready, you are now ready to run the server. In the command prompt enter

`python manage.py runserver`

Yes, you run the django `manage.py`. The patch application has placed hooks around and starts the App Engine server. Verify the running application from [localhost](http://localhost:8000/).

If you happen to have the latest version of Python, sorry. Use the python2.5 instead.

### Upload the Application

Thats it. If you have got it working on the localhost, now throw the code at google's _scalable and no maintenance cloud_ and it starts working.

`python manage.py update`

You don't bother about deployment, server maintenance or log files. Rather, check out the awesome <a href="http://appengine.google.com/dashboard?&amp;app_id=twitter-follow">application admin dashboard</a> and if you run out of the allocated free quota, buy some more resource time. Its costs $0.10/CPU hour and one cent for 100 emails sent!

A lot is happening in the field of App Engine support for twitter API. There is even a new App Engine module for twitter <a href="http://github.com/tav/tweetapp/tree/master">oAuth authentication</a>, out of the box, as there is <a href="http://www.djangosnippets.org/snippets/1353/">one for django</a>.

App Engine is a powerful platform. django is a high productive framework. Combining them is very powerful and there is also an [ongoing project](http://code.djangoproject.com/wiki/AppEngine) for enhancing the the patch project and making it totally compatible with django.

Resources:

* [Jesaja Everling Tutorial](http://code.google.com/appengine/articles/app-engine-patch.html)
* [Twitter API Documentation](http://apiwiki.twitter.com/Twitter-API-Documentation)
* [App Engine Tutorial on Webapp Framework](http://code.google.com/appengine/docs/python/gettingstarted/)
* [App Engine Patch - Getting Started](http://code.google.com/p/app-engine-patch/wiki/GettingStarted)
* [App Engine Patch - Documentation](http://code.google.com/p/app-engine-patch/wiki/Documentation)

<hr />
Want to develop an application based on twitter API? [We can help](http://uswaretech.com/contact/).

.

