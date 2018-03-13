Title: Five things I love about Django.
Date: 2008-04-18 06:05:37
Modified: 2008-04-18 16:35:37+05:30
Slug: five-things-i-love-about-django
Authors: shabda
Tags: Uncategorized
Summary: I posted [five things I hate about Django](http://42topics.com/blog/2008/04/five-things-i-hate-about-django/), so as a penance, I will of course have to tell the "Five things I love about Django". ### The Admin interface rocks: I have demoed Django to a fair number of People, and when you write a few lines in `models.py`, and then show the auto generated Admin interface, this is a *jaw-dropping* moment. Happened with me every time I introduced Django to someone. In [Barcamp Hyderabad 05](https://barcamp.pbwiki.com/BarCampHyderabad5), people could not believe this was so easy, and asked if there was some more code <acronym title="Any sufficiently advanced technology is indistinguishable
I posted [five things I hate about Django](http://42topics.com/blog/2008/04/five-things-i-hate-about-django/), so as a penance, I will of course have to tell the "Five things I love about Django".

### The Admin interface rocks:

I have demoed Django to a fair number of People, and when you write a few lines in `models.py`, and then show the auto generated Admin interface, this is a *jaw-dropping* moment. Happened with me every time I introduced Django to someone. In [Barcamp Hyderabad 05](https://barcamp.pbwiki.com/BarCampHyderabad5), people could not believe this was so easy, and asked if there was some more code <acronym title="Any sufficiently advanced technology is indistinguishable from magic.">behind this.</acronym>

Of course Admin is much more useful than as a show off tool. When I am developing a new website I write the views to query the DB first, at that time the Admin is indispensable. And in many cases, just writing the Models, and tweaking the Admin is enough for me.

### Documentation is comprehensive, available, and always maintained:

When I was looking around to learn a python framework, after search I has to choose between Django and Turbogears. Now after having used Django a lot, and Turbogears for comparison, I believe Django to be better. But this decision I could not have taken when I was just starting out. I choose Django because it seemed better documented, with tutorials more freely available.

Having the whole framework documented, from the [most basic](http://www.djangoproject.com/documentation/tutorial01/), to the more [esoteric](http://www.djangoproject.com/documentation/legacy_databases/) is huge time saver.

### The community is extremely helpful:

Whenever I have asked, a question in [django-users](http://groups.google.com/group/django-users) or on [IRC#Django](irc://irc.freenode.com/django) I always get helpful responses, and a lot of help. Few communities have this culture where, people with so much experience are willing to help out people who are just starting out.

### There is a reusable app for almost everything.
Need to handle hierarchical data as relational data, [django-mptt](http://code.google.com/p/django-mptt/) to the rescue. Need to handle [voting](http://code.google.com/p/django-voting/), [tagging](http://code.google.com/p/django-tagging/), or [registration](http://code.google.com/p/django-registration/). Well we got you [covered](http://djangoplugables.com/).

### Easy things are easy, and hard things are possible:

You just want to do as `SELECT * FROM ... WHERE ...`, just use `Model.objects.filter`. Ok, so your queries span a number of relationships, and you want to reduce the number of queries, use `select_related`. Or might be you want to model a relationship, which Django ORM can not, might be a little sql in `.extra` will help you? Or might be you have a lot of `GROUP BY` or `UNION ALL`, drop to raw sql in `connection.cursor`. Depending upon your requirement, you moved from the trivially easy to the somewhat hard.

Similarly with templates, you just want to substitute some variables. Use `{% for ... %}` and `{{ ... }}`. Or you want to do something difficult/custom in template, (and are sure that template is the right place to do this), just write a template tag.

### Bonus

*The development web server*: When you are developing, not worrying about Apache, and any configuration, is a huge help. And the fact that pdb plays nice with web server is awesome, (compared to GAE servers which give a `BdbQuit` exception).

