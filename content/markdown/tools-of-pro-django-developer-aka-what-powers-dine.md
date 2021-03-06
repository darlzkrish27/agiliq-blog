Title: Tools of Pro Django developer - aka What powers dinette and almost every app we write.
Date: 2010-01-13 07:32:11
Modified: 2010-01-13 19:02:11+05:30
Slug: tools-of-pro-django-developer-aka-what-powers-dine
Authors: shabda
Tags: python
Summary: There are some tools and apps which we use with almost all apps we write, and in particular which, we used for [dinette](http://uswaretech.com/forum/). Here they are broken into useful during development, and (also) useful post development. #### During Development 0. [Ipython](http://ipython.scipy.org/) and [ipdb](http://pypi.python.org/pypi/ipdb) 3. [South](http://south.aeracode.org/) 4. [Django test utils](http://ericholscher.com/projects/django-test-utils/) 1. [Django extensions](http://github.com/django-extensions/django-extensions) 2. [Django debug toolbar](http://github.com/robhudson/django-debug-toolbar) ##### [Ipython](http://ipython.scipy.org/) and [ipdb](http://pypi.python.org/pypi/ipdb) Ipython is a enhanced shell for python. Ipdb similarly add extra capacity to the builtin pdb debugger. It is extremely convenient to drop into a ipython shell right where a complex piece of code is being hit. from IPython.Shell import
There are some tools and apps which we use with almost all apps we write, and in particular which,
we used for [dinette](http://uswaretech.com/forum/). Here they are broken into useful during development,
and (also) useful post development.


#### During Development

0. [Ipython](http://ipython.scipy.org/) and [ipdb](http://pypi.python.org/pypi/ipdb)
3. [South](http://south.aeracode.org/)
4. [Django test utils](http://ericholscher.com/projects/django-test-utils/)
1. [Django extensions](http://github.com/django-extensions/django-extensions)
2. [Django debug toolbar](http://github.com/robhudson/django-debug-toolbar)


##### [Ipython](http://ipython.scipy.org/) and [ipdb](http://pypi.python.org/pypi/ipdb)

Ipython is a enhanced shell for python. Ipdb similarly add extra capacity to the builtin pdb debugger.
It is extremely convenient to drop into a ipython shell right where a complex piece of code is being hit.

    from IPython.Shell import IPShellEmbed
    ipython = IPShellEmbed()
    ipython()
    
Of course this does not allow you to step through, so if you want to step through your code, you need to do

    import ipdb
    ipdb.set_trace()
    
Of course, you can just do `pdb.set_trace()`, but with tab completion and syntax highlighting, using ipdb is a no brainer.

##### [South](http://south.aeracode.org/)

South adds schema migration to Django. It has lot of advanced options, but just three command will get you along way.

Convert a normal app to use the south way.

    ./manage.py convert_to_south

Once you have made any changes to your apps model, write a new migration which can update the database to latest models.py

    ./manage.py startmigration appname nameofmigration --auto
    
Once you have written a migration, write a command to update the db.
    
    ./manage.py migrate appanme 

##### [Django test utils](http://ericholscher.com/projects/django-test-utils/)

When you are running tests, a huge time sink is the time spent dropping and recreating tests. You might add just a single test,
and the default Django testrunner will spend a huge time recreating the database. Enter `manage.py quicktest` which reuses databases
between test runs. One of the side effects of this is that if you modify `models.py` between test runs you should manually
recreate the database.

The tests (or whatever little of it exists) for Dinette were written using testutils'
[testmaker](http://github.com/ericholscher/django-test-utils/tree/master/test_utils/testmaker/).


##### [Django extensions](http://github.com/django-extensions/django-extensions)

Django extensions, (earlier Django command extensions) is a app which adds extra commands to `manage.py`. It has lot of goodies, but just
`./manage.py shell_plus` makes it worthwhile. (It imports all the models from all the apps automatically).

Get the full list at [Github](http://wiki.github.com/django-extensions/django-extensions/current-command-extensions)


##### [Django debug toolbar](http://github.com/robhudson/django-debug-toolbar)

I do not personally use it but some people on our [team](http://uswaretech.com/team/) swear by it.


### After development

1. [Django-pagination](http://github.com/ericflo/django-pagination)
2. [sorl-thumbnails](http://thumbnail.sorl.net/docs/)
3. [django-compressor](http://github.com/mintchaos/django_compressor/)
4. [Haystack](http://haystacksearch.org)

##### [Django-pagination](http://github.com/ericflo/django-pagination)

I have decided on a standard way to build apps which need pagination. a. Build apps without pagination. b. The night
before release spend an hour and add pagination to all pages. Really this is all you need

    {% load pagination tags %}
    {% autopaginate queryset %}
    ....
    {% paginate %}
    
There is no change required to the views. 
  
##### [sorl-thumbnails](http://thumbnail.sorl.net/docs/)

To thumbnail an image.

    {% thumbnail image size %}

"Things should be as simple as possible, but not simpler."

##### [django-compressor](http://github.com/mintchaos/django_compressor/)

During development we work with multiple unminified js and css files. Django-compressor takes care of minifying and compressing
it when we deploy. All we need to do is,

    {% compress css %}
    ...
    all css files
    ....
    {% endcompress %}
    
    
    {% compress js %}
    ...
    all js files
    ....
    {% endcompress %}

    

##### [Haystack](http://haystacksearch.org)

Haystack is "modular search for Django". It make writing search views as easy as writing the admin. You declaratively tell what
fields you want to search on, and Haystack can take care of creating and updating the index, and providing a generic view
to handle the search. Check a search view written using Haystack [http://uswaretech.com/forum/search/?q=django](http://uswaretech.com/forum/search/?q=django)

------
To everyone who contributed to these apps, thanks. Django development won't be the same without these great tools.
This post was inspired by [Kevin Fricovsky's post](http://montylounge.com/) post, [The apps that power Django-Mingus](http://blog.montylounge.com/2009/sep/24/apps-that-power-django-mingus/)
and is stolen about 50% from there. Thanks. :)

----------

We build Amazing web apps, and we can build it for you. [Contact us today](http://uswaretech.com/contact/).




