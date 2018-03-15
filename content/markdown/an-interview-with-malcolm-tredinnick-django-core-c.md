Title: An interview with Malcolm Tredinnick - Django core contributor
Date: 2008-04-23 14:07:28
Modified: 2008-04-24 00:37:28+05:30
Slug: an-interview-with-malcolm-tredinnick-django-core-c
Authors: shabda
Tags: interviews
Summary: <img class="right frame" src="http://uswaretech.com/blog/wp-content/uploads/2008/04/malcolm_headshot.png" />[Malcolm Tredinnick](http://www.pointy-stick.com/) is a core contributor to Django, and was the driving force behind the [Queryset-refactor](http://code.djangoproject.com/wiki/QuerysetRefactorBranch) branch of Django, which adds important capabilities such as model inheritance. He has a long association with OSS, and contributed significantly to GNOME and Django. He graciously agreed to be interviewed at 42topics blog. Malcolm's blog, Defying Classification, can be read [here](http://www.pointy-stick.com/blog/). **Shabda**: Would you tell a few things about yourself, how did you get involved with OSS and Gnome, and with Django? **Malcolm**: Here are some recollections I've written about: [1](http://www.pointy-stick.com/blog/2006/07/03/time-already/) and [2](http://www.pointy-stick.com/blog/2007/08/18/10-years-gnome/) Short version; started using Linux in
<img class="right frame" src="http://uswaretech.com/blog/wp-content/uploads/2008/04/malcolm_headshot.png" />[Malcolm Tredinnick](http://www.pointy-stick.com/) is a core contributor to Django, and was the driving force behind the [Queryset-refactor](http://code.djangoproject.com/wiki/QuerysetRefactorBranch) branch of Django, which adds important capabilities such as model inheritance. He has a long association with OSS, and contributed significantly to GNOME and Django. He graciously agreed to be interviewed at 42topics blog. Malcolm's blog, Defying Classification, can be read [here](http://www.pointy-stick.com/blog/).

**Shabda**: Would you tell a few things about yourself, how did you get involved with OSS and Gnome, and with Django?

**Malcolm**: Here are some recollections I've written about: [1](http://www.pointy-stick.com/blog/2006/07/03/time-already/) and [2](http://www.pointy-stick.com/blog/2007/08/18/10-years-gnome/)
Short version; started using Linux in university as a poor undergrad; kept using it since then (started back in 1993).
Started using GNOME in 1999 (after trying a very early version of Qt and what became KDE), started contributing to GNOME about 12 months later (mid-2000).
Started using Django in October 2005, I guess (a few months after it was open sourced) and haven't stopped. Started contributing patches more or less immediately and was given commit privileges about March of 2006, from memory.

**Shabda**: So what is one thing in Django which you absolutely love, and one thing which you think Django should have done differently?

**Malcolm**: I guess Django's separation of responsibilities is something that has always been attractive to me. It's very easy to keep the cross-application business logic and the persistent storage logic and the state control and the visual presentation separate.
I guess the piece that probably routinely annoys me the most is a slight inconsistency in the template tags: we often mix up how to use a literal string in template tags (sometimes it's in quotes, sometimes not), which makes it very hard to later make that tag also take a template variable.
As somebody who handles bug reports a lot, something like that ends up taking up a lot of time in my life, which could probably be better spent elsewhere. Still, it's almost impossible to change now, so that's the way life goes.

**Shabda**: [Queryset-refactor](http://code.djangoproject.com/wiki/QuerysetRefactorBranch) is almost done, what would were the major overarching goals for this branch?

**Malcolm**: A number of things...
1. Clean things up internally, so that future extensions are easier. A lot of the existing (trunk) query construction grew from something much smaller. It all mostly worked, but it was getting pretty hard to manage. Some bugs were almost impossible to fix in the trunk form too, which was the real motivation.
2. Organise things so that I could add model inheritance, which was the main thing that started me looking deeply at all that. This has been accomplished.
3. Make it easier to add backends like Oracle and MS SQL Server that don't support, say, limit and offset in SQL. That meant providing a more object-based query construction so that they can tweak things before creating the SQL string.
In the process, we've made it a lot easier for people to extend this for backends and to add functionality to existing backends.
For example, adding new lookup types is now possible (not particularly easy, but possible). And the [geo-django](http://code.djangoproject.com/wiki/GeoDjango) branch can do their stuff without having to patch the query construction code any longer.
(Justin Bronn, the geo-django maintainer has been tracking qs-rf very closely, so we know that geo-django works nicely with qs-rf).
I guess they're the main points. The first one -- a better/different internal organisation -- was the main one and was really the bulk of the work. Getting everything to mostly work was relatively easy. Getting it all to work perfectly took a lot longer. :-)

**Shabda**: So would this make, say, writing a backend for non relational DBs like Bigtable easier. [Michael Trier](http://blog.michaeltrier.com/) was working on a [Sqlachemy based ORM](http://gitorious.org/projects/django-sqlalchemy/) for Django, how would this be affected by qs-rf merge?

**Malcolm**: Well, that's really two questions. So, one at a time...
Whether it would make a non-relation backend easier,... yes. The QuerySet class passes all the work of talking to the backend, whatever it is, off to a class called Query. Writing a different sort of backend would require writing a new Query class that knew how to take the function calls QuerySet makes and turn them into the right return types.
Mostly, QuerySet does not poke at the internals of Query: it uses "public" methods only. This is deliberate. It means that writing something like a Query class for a different sort of backend (RDF store, or LDAP or whatever) might very well be possible.
As for something like SQLAlchemy, I gather it's made things easier. I know Michael and Brian have been writing their code against the qs-rf code and it seems to be working.
However, I haven't looked at their code, so I'm not sure if they're using the QuerySet/Query split very much or not. I gather they're doing it slightly differently from how I might have done based on somethings they mentioned when I was talking to them yesterday -- apparently a recent qs-rf commit "broke" their approach to something and that was a little surprising to me. But it wasn't a showstopper apparently, so I'm not too worried.

**Shabda**: As you said, a commit sort of "broke" their approach. How backwards comaptible is qs-rf. Is it mostly backwards compatible, but some corner cases will break, or would any thing which worked previously, but does not now should be reported as a bug?

**Malcolm**: It's almost 100% backwards compatible. The thing I "broke" (and I'm using quotes, because it wasn't really a breakage, just a change) was a very internal thing. Obviously if you're writing something like a new storage backend, you need to dive into the internal API of the ORM a bit and that's what the django-sqlalchemy stuff is doing...
For normal user code, there should be very few changes required. There's a list on the wiki page for [the branch](http://code.djangoproject.com/wiki/QuerysetRefactorBranch) but they are mostly things that won't affect normal code.
Only if you are doing something very "tricky" or slightly unsupported will changes be required.
Of course, some people will see slightly different results in complex querysets because we have fixed a lot of bugs (over 60 that were reported to Trac plus some that never got that far) and some of those fixes change the results that were returned.
So some people may be relying on currently incorrect behavior, but I doubt that's going to be too troublesome.
Also, I've tried to make the error reporting a lot clearer so that when somebody does try to do something undefined, it should hopefully give an error message (e.g. introducing an infinite ordering loop in models)

**Shabda**: Speaking of Bigtable, Google's [App Engine](http://code.google.com/appengine/) seemed to be sort of a letdown from the initial hype. What are your opinions about the effect of App Engine on Django?

**Malcolm**: Well, I think the letdown came from people letting their expectations get ahead of their brains a bit. Google didn't really seem to over-hype it or misrepresent it.
Obviously when Google releases something, everybody gets excited. But you have to at least take the time to look at what was released.
For a particular class of applications, I think App Engine is probably ideal. It gives cheap hosting, great reliability and access to some Google's storage stuff.
For example, if you have something that presents a read-only view onto a large bunch of data, App Engine would be very appropriate. Okay, you have to do data extraction from multiple models manually, since there's no joining, but that's not too hard to work around and people will write little helper functions for those cases.
Remember that a lot of very successful, very popular websites are basically read-only: a lot of newspaper sites, things like [EveryBlock](http://www.everyblock.com/) (and, formerly, Chicago Crime) -- all those sorts of sites. Even news.google.com.
There's a great deal of information that wants to be "presented" to people. Not everything requires form entry and comments and the like and even those are possible on App Engine if you need them.
So I'm kind of glad the initial reaction phase has settled down a bit and people are starting to think about what is possible, rather than focusing on what isn't possible. Some interesting things will come out of App Engine, I suspect. We just don't know what they are yet because people are still writing them.
As far as how it affects Django, that's probably both good and bad.
If people view App Engine as being representative of everything Django, it's not going to be too handy. However, I doubt that will be the prevailing opinion. People realise that Google have used portions of Django to implement portions of the App Engine experience. And those pieces of Django are quite handy. The URL dispatching, the general data flow, the templates -- all will be nice for people to use.
It's also getting a few people interested in the internals of Django as they try to work out whether they can move the extra bits that are 'missing' from App Engine into the fold.
In addition, we've also already received some direct positive benefits: some of the Google team, particularly Guido van Rossum, have been filing a few bugs against Django as they did the initial App Engine development. So there are a few bug fixes in things like newforms and the templating component that are a direct result of App Engine development inside Google.

**Shabda**: There is [app-engine-django](http://code.google.com/p/google-app-engine-django/), which is trying to bridge the impedance mismatch between Django ORM, and App Engine ORM. Is there any effort to bridge this from Django side by making Django ORM play nice with Appengine. If such a request comes in what would Django's response be?

**Malcolm**: *shrug*. I have no idea.
I think this is an ideal opportunity for some people to write stuff like django-appengine and work out what might be needed and come up with a concrete list of things that might be needed. Predicting the future is hard enough to be mostly futile and it's not really worth speculating.
Right now, the core Django developers are fully focused on getting Django 1.0 released. That's where our time is going. How other people spend their time is up to them and it's definitely not for me to judge whether it's worthwhile or not.
People need to experiment. It's the way new software products are developed. So anybody who's interested and working on this should be encouraged. That's how open source development has always worked and I think it works quite well.
I guess I could hazard a guess that any intrusive changes just to support some App Engine style thing probably aren't interesting prior to Django 1.0, since they're not necessary to get that out of the door. But that's as much of a prediction as I'm willing to make and anybody who actually attaches any value to that prediction is probably being foolish. I'm wrong more often than I'm right. :)

**Shabda**: Well this gets asked a lot on the mailing lists, qs-rf is almost done, Newforms admin should be done soon, so when can we expect Django 1.0, and what new goodies will this bring?

**Malcolm**: *heh*
Well, the real answer is "as soon as possible".
Notice that this is a big step up from "when it's ready", which is the normal answer here. Hopefully people appreciate that the maintainers want to get 1.0 out the door as much as anybody else. It's just that release 1.0 when it's not at a point we're happy with won't be useful in the future.
More concretely, we have a few major things to do between now and 1.0 and they're all getting pretty close.
Then there's a period of stabilizing and random bug squashing and triage to make sure we haven't missed anything big.
The idea (and driving goal) is that whatever we release as 1.0 should remain backwards-compatible on an API level until, say, 2.0. So we need to get all the big infrastructure changes in before 1.0.
That isn't the same as saying 1.0 will be bug free or any pipe dream like that, because it won't be. But fixing bugs is an ongoing job and as long as we can feel comfortable that we can fix existing bugs without having to make major breakages, we can feel happy about 1.0.

**Shabda**: This has happened to me a few times, I am trying to pitch Django to a client, and then I mention Django has not hit 1.0 and we would be developing their site against a subversion checkout, and boom, I am against a brick wall. I am guessing this happens to a lot of people. Any suggestions on overcoming client's reluctance on this point.

**Malcolm**: Well, this is the hard problem, of course. There are at least three issues here.
Firstly, it's well known that in a lot of corporate situations, they only want to go with "released" versions, whatever that means. The fact that released versions are often a disaster (e.g. Microsoft Vista) seems to be forgotten, but that's the way things go. However, it shouldn't be the ultimate driving force in this equation. It's one consideration only and having the software released faster just to meet some immovable, often-times unsupportable policy isn't the answer.
The second aspect is to realise that there are a lot of successful sites built on Django 0.96 and even Django 0.91. So it's not impossible to stick with the last release.
Now, of course ,that tends to show up the problem with the first point, since 0.96 has some bugs that have subsequently been fixed, so sticking to a release handicaps you there.
However, if we did more frequent releases, that adds a lot of burden to the release process. For example, distributions (Debian, Suse, Fedora, etc), now have a bigger problem in choosing the right version to ship since they also want some stability. Security releases either become harder (we have to patch more versions) or else we have to "end of life" earlier releases more frequently, which harms those people using those releases (again, it reduces stability for the userbase).
The third point is somewhat related to that: in order to make sure that a release can be relied upon, we sometimes need to make sure that we do all the necessary preliminary work so that we don't immediately break any code that relies on this new release.
That is exactly the situation we're in now. We release 0.96 as a sort of "stability point" for people to rely on. Almost immediately, we then started to introduce a bunch of necessary changes to things that will require code to be ported when people upgrade. When this round of churning has finished, we'll make the next release. That happens to be 1.0 in this case.
If we released, say 0.97 at some point in the past few months, people would have a similar but slightly different sort of marketing problem: now they have 0.96 code that has to be ported if they want to move to 0.97. But it's also just "more work" on some level, since they'll have to do even more porting when 1.0 comes out. So we aren't helping developers by providing them with a lot of stability.
It's fairly well understood by most experienced developers that this is a tough path to walk. With open source software it's even harder, since everybody is a volunteer and we have a largely unknown userbase to satisfy.
Sometimes that difference is forgotten by people who argue that "no company ever works like this". A corporation can set release deadlines and in exchange for everybody having to work to those deadlines, those people get paid. It's called a salary. Open Source software doesn't work like that.
I'm sure that once Django reaches 1.0 or perhaps a little later, we'll slip into time-based releases so that things go a bit more smoothly. At the moment that simply isn't practical because of all the changes we need to make.
Every project goes through that phase. Before Linux had time-based releases, they had to get an amount of features in and a certain stability level reached. Ditto for GNOME (which only started doing time-based releases at 2.0 and there was a long gap between 1.4 and 2.0 precisely because we needed to be able to guarantee that at 2.0). Ditto for KDE and Ubuntu and ...
So, yeah ,it's slightly tough times for everybody at the moment. Partly Django is a victim of its own success here: everybody wants to use it and we're trying to keep up.
Hopefully people realise that ultimately that's a good thing. It's an investment in the future in the sense that we'll still be here in a year, two years, five years... because of this great use.

**Shabda**: What are the future plans for Django. Django has most of the things which I want in a framework, ([import soul](http://xkcd.com/413/)), and we would hate to have Django suffer from [featuritis](http://headrush.typepad.com/creating_passionate_users/2005/06/featuritis_vs_t.html). After 1.0 what are the areas Django wants to tackle?

**Malcolm**: Again, this is asking for a predication and I don't do those.
Partly because I don't know and am not really in a position to say, in any case. (I'm just a contributor). Partly because having too many goals too far ahead of time is possibly going to restrict people.
Django runs on its contributors. A lot of the ideas that are implemented are initially suggested and often developed in quite some detail by people other than the core developers. It might not always seem that way, because you sometimes have to see the common thread in a bunch of requests before you notice the lurking feature requirement ,but it's true.
So what happens post 1.0 is going to depend a lot on how people use Django, on what people offer in the way of code and, particularly, what good ideas are suggested.
Who could have predicted Google AppEngine when Django 0.96 was released? What other things like that will appear as a result of Django 1.0? Who knows?
Of course, there are some things that we could probably say will be worked on (successfully or not is another question entirely). Multi-database support seems to be popular and one day somebody might do it; maybe there'll be more front-end developments -- e.g. somebody like you who keeps wanting more Ajax support will actually develop something that provides this support everybody seems to want but hasn't ever specified. :-)
We can sort of see a few things coming up again and again, so I guess they'll be worked on. But it's really going to be up to the people who write the code, who come up with the ideas, who write the websites that use Django, who try to teach Django to others. And right now, I have no idea what direction that will go in.

**Shabda**: Before we close, would you like to share a handy tip which you use a lot, but does not get used so much otherwise?

**Malcolm**: People possibly overlook both the `{% debug %}`  template tag (best to use it as `<pre>{% debug %}</pre>` in a template) and the debug context processor [django.core.context_processors.media](http://www.djangoproject.com/documentation/settings/#template-context-processors).
Both of those are very useful for trying to see what's going on when you're passing information between a view function and a template.

**Shabda**: Thanks Malcolm. It was great talking to you.

---------------------------------------------------------

I plan to interview Leaders from Django community, in coming few weeks, so if you would like me to ask any specific questions, put them in comments, and I will ask them when I interview.

