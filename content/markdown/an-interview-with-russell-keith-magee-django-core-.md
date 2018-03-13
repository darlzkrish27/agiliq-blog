Title: An interview with Russell Keith-Magee - Django core contributor
Date: 2008-05-04 01:30:26
Modified: 2008-05-04 12:00:26+05:30
Slug: an-interview-with-russell-keith-magee-django-core-
Authors: shabda
Tags: interviews
Summary: <img src="http://uswaretech.com/blog/wp-content/uploads/2008/05/russellm.jpg" alt="" title="russellm" width="100" height="69" class="right frame" /> [Russell Keith-Magee](http://djangopeople.net/freakboy3742/) is a longtime core contributor to Django. He has worked extensively with the Django testing and serialization components. He is currently working on [django-evolution](http://code.google.com/p/django-evolution/), the ability to do schema evolution from Django, an often requested feature, and is mentoring a GSOC proposal to add [aggregate queries](http://groups.google.com/group/django-developers/browse_thread/thread/749ebaf0fa80f807) support to Django ORM. He is currently a Senior R&D Software Engineer at [plugger.com.au](http://plugger.com.au). He graciously agreed to an email interview with the [42topics blog](http://42topics.com/blog/). ------------------------------------------------- **Shabda:** Would you tell a little about yourself? How did you get started with Django? What areas
<img src="http://uswaretech.com/blog/wp-content/uploads/2008/05/russellm.jpg" alt="" title="russellm" width="100" height="69" class="right frame" />
[Russell Keith-Magee](http://djangopeople.net/freakboy3742/) is a longtime core contributor to Django. He has worked extensively with the Django testing and serialization components.  He is
currently working on [django-evolution](http://code.google.com/p/django-evolution/), the ability to do schema evolution from Django, an often requested feature, and is mentoring a GSOC proposal to add
[aggregate queries](http://groups.google.com/group/django-developers/browse_thread/thread/749ebaf0fa80f807) support to Django ORM. He is currently a Senior R&D Software Engineer at [plugger.com.au](http://plugger.com.au). He graciously agreed to an email interview with the [42topics blog](http://42topics.com/blog/).

-------------------------------------------------

**Shabda:** Would you tell a little about yourself? How did you get started with
Django? What areas of Django have you contributed to?

**Russell:** I'm a thirty-one year old, father of one, living in the most isolated
capital city in the world - Perth, Western Australia.
Strangely, for all my involvement in Django, my background has almost
nothing to do with the web. I studied Physics as an undergraduate, and
studied neural networks for my PhD. My first job was with a startup in
the defence industry developing simulation frameworks. Over time,
mostly as a result of my association with Django, I've become more
involved in building web services.

**Shabda:** How did you get started with
Django? What areas of Django have you contributed to?

**Russell:** I've always had a bunch of little pet projects that I've fiddled with
in my spare time. Over time, these projects have migrated over a range
of languages until I found (and fell in love with) Python. Around the
time of Web 1.0, I started looking at various web frameworks (PHP,
[Zope](http://www.zope.org/WhatIsZope), etc), but I never really managed to get mental traction - they
were either so complex that it was hard to work out where to start, or
they provided a bunch of tools but didn't really make it obvious how
the pieces all fitted together. Plus, the documentation for these
frameworks was up to the common open source standard - an even mix of
the awful, the incomprehensible and the non-existent.
Around mid 2005, the buzz about Ruby on Rails started to grow, so I
took a look. I could see that it had promise - it was an interesting
new way of looking at solving web problems - but again it suffered
from the documentation problem. One day, I found a blog entry
referring to Django, and finally all the pieces fell into place. The
documentation was excellent, the simple stuff was painfully simple,
and there was a clear separation of concerns - there is the URL module
which routes requests, an ORM to access the database, etc.
At the time, the pet project I was using to test new frameworks was a
time/task tracking system. After working through the tutorials, I
tried implementing a task tracker, and quickly realized that Django
didn't have support for aggregate functions. I made a few suggestions,
which were universally rejected (in retrospect, for very good
reasons), so I went digging in the code to try and come up with some
better ideas.
In the process, I ended up spending a lot of time in the query system,
which put me in a position to submit patches for a few small bugs in
queries. Once I had found my feet, I tackled some larger enhancements.
One of my first big contributions was modifying the filter syntax so
that queries could span reverse relations; that is, if Book has a
foreign key on Author, I modified the syntax to allow Author queries
to ask about their related books. Of course, between the [Magic Removal](http://code.djangoproject.com/wiki/RemovingTheMagic)
changes and [Malcolm's](http://pointy-stick.com/blog/) [Queryset refactor](http://code.djangoproject.com/wiki/QuerysetRefactorBranch), most of this code has now
been thrown to the wind, but at the time it was a big improvement.
[Adrian](http://www.holovaty.com/) noticed the contributions I was making, and offered to give me
commit access and make me a core developer.
Not long after I joined as a core developer, the Magic Removal branch
started, which consumed pretty much all my efforts for about six
months. This involved more changes to the query system, but also with
the way models were defined and instantiated. Following that, I
developed the [test framework](http://www.djangoproject.com/documentation/testing/), which also involved working with the
[serialization framework](http://www.djangoproject.com/documentation/serialization/). I did some work on [newforms-admin](http://code.djangoproject.com/wiki/NewformsAdminBranch) in the
early days, including developing the new Media definition framework
and a refactor of form input fields for files.
I have also spent some time on side projects that aren't directly
targeting the Django trunk, but might be integrated one day. The most
notable of these is [Django Evolution](http://code.google.com/p/django-evolution/) - an implementation of schema
evolution for Django.
I've been a little bit quiet over the last few months; I've just
changed jobs, which is consuming a lot of my time, and I've had a few
bouts of illness which have slowed my progress. I have also been
helping out [James Bennett](http://b-list.com/) by acting as Technical Reviewer for his
upcoming book "[Practical Django Projects](http://www.apress.com/book/view/9781590599969)"; there won't be much to show
for this until the book is published, but when it comes out, you're
all in for a treat - it's going to be a great resource.
I'm hoping to get back into the swing of things in the next few
months. One of the first new tasks will be a very old issue - I'm
mentoring a student (Nicolas Lara) in a [GSOC project](http://code.google.com/soc/2008/django/appinfo.html?csaid=6207B3094B97F424) to implement
aggregates in the query system. So - after almost 3 years, my original
itch may actually get scratched.

**Shabda:** Have you worked with
other competing frameworks, say [Ruby On Rails](http://www.rubyonrails.org/), [Turbogears](http://turbogears.org/) etc?

**Russell:**
I've looked briefly at a lot of other web frameworks. In the early
days I looked at Zope and PHP while gaining a foothold in the web
world. Since joining the Django project, I occasionally look at what
the other frameworks - in particular, Ruby on Rails and TurboGears -
are doing. It's occasionally helpful to see how other people are (or
aren't) solving various problems, borrowing the good ideas where
possible.

**Shabda:** What other
software/applications have you worked with. (Both OSS and otherwise)?

**Russell:** A little bit of everything. I used Linux as a personal computing
platform back in the glory days of [Slackware](http://www.slackware.com/) and [Yggdrasil](http://en.wikipedia.org/wiki/Yggdrasil_Linux). I was
compiling KDE and GNOME before they were even v1.0, and I've toyed
around with writing for both frameworks. I've used Windows in a
commercial environment, and I've gotten to know the eccentricities of
every version of Microsoft Visual Studio from v6 to 2008 in far too
much detail. I've written Java, both server side and client side.
I've spent a lot of time with a simulation protocol called <acronym title="High
Level Architecture">HLA</acronym>. One of the more notable products of that work was
securing some funding and commercial support for an [open source
implementation of the HLA protocol](http://porticoproject.org).
I'm a huge fan of Linux, especially on the server side. However, I
attribute most of my recent productivity to being a Mac Switcher. I
found that once I made the switch, I spent a lot less time messing
around with compiling the latest, slightly less buggy version of some
esoteric utility, and a lot more time building useful code. It's
possibly a little '[post hoc ergo propter hoc](http://en.wikipedia.org/wiki/Post_hoc_ergo_propter_hoc)', but my Mac switch
correlates almost exactly with being accepted as a Django core
developer. Regardless or the causation, give me a MacBook Pro, a copy
of TextMate and a good net connection and I'll be a very happy man for
a long time.

**Shabda:** You have worked with a number of web frameworks. What made you choose
Django over them? What are the strong points of Django compared to say ROR?
Compared to Turbogears? What can Django learn from these other frameworks?

**Russell:** I haven't worked extensively with any other web framework. I've had
the occasional poke around - mostly to see how the other guys do
things. To my eyes, at a technical level the various modern web
frameworks have a lot more in common than they have differences. All
of this generation of web frameworks are about providing a clear layer
between the database and the user, proving a simple mechanism for
mapping web requests to functions that serve those requests, and
providing a clean way to render content for the responses.
Sure, there differences in language of implementation. There are some
philosophical differences like whether templates should allow
executable code. There are also some feature differences. However,
these are all minor in the grand scheme of things. The philosophical
differences haven't stopped anyone from building web applications in
any of the frameworks. Given enough time and resources, any feature
could be implemented in any framework. Language choice is also largely
irrelevant. I've used a bit of Ruby, and while it has some interesting
features as a language, the difference between Python and Ruby is a
lot less than the difference between, say, Python and C++.
What this leaves is the stuff outside the technical realm - resources,
documentation, and the community. This is the reason I choice Django
in the first place, and I still think that they are areas of strength
- I certainly don't regret my original decision.
I remember when I first found the Django website - I very quickly got
the impression that it was built by a group of people that really
cared about what they were building, and how it was perceived. The
website and other resources weren't just rapid constructions or
reflections of this week's design fad - they were designed with care.
The same went for the documentation - it wasn't just a bunch of
documents autogenerated from source code comments or a sprawling wiki
- they were custom written by someone who knew how to write, and more
importantly, knew how to edit. As a result, Django's tutorials and
documentation made getting started an almost painless process - both
as a user, and as a developer. The attitude of the community is also
important; Adrian and Jacob have established a very civil community
and the community as a whole tries to keep it that way.
I think the biggest lessons Django can learn from other frameworks is
in how to market itself. Because we're still pre-v1.0, we haven't
really been aggressive about marketing ourselves. Some of the other
frameworks have been selling themselves for a long time, so we are in
a position to learn from their experience.

**Shabda:** One of the most requested features with Django is schema-evolution, which
you are working on. What are the design goals for [django-evolution](http://code.google.com/p/django-evolution/)? How
production ready is this?

**Russell:** At the moment, Django Evolution handles the simple cases
(add/delete/rename simple field) fairly well. There are some bugs in
the more complex cases, particularly those involving foreign keys and
many-to-many fields. It tends to work quite well for SQLite and
Postgres; MySQL tends to be more problematic. At this time, I'd
classify it as "production ready for capable developer". As long as
your schema evolution needs aren't too advanced and you're willing to
do a bit of hand holding and carefully check the SQL that is produced,
you can use Django Evolution right now. If you're not as confident
with raw SQL, you might want to wait a while.
Obviously, the long term goal is to make it "production ready for
everyone", and support every possible change that you can make to a
Django model. Completely aside from fixing bugs and supporting other
databases, one of the big open issues is populating data into new
columns. At present, we have some simple mechanisms to put in default
data, but these are somewhat limited. Longer term, we're aiming to be
able to populate a new column with data from existing columns. This
will require providing a subquery as initial data, and my hope is that
the new Queryset Refactor features will make this sort of thing much
easier.

**Shabda:** I asked this to Malcolm as well, but he was unwilling to make a
prediction. So let me ask this slightly differently. What problems would you
like Django to tackle after 1.0? What big features would you be most
interested in having in Django, after it hits 1.0?

**Russell:** My efforts in prognostication have a nasty habit of going badly
wrong... but here goes:
One of the recent shifts in in the Django project as a whole has been
the move to allow user extension as much as possible. The ability to
have user-defined field types, database backends, and management
commands are all examples of the push to move Django development into
the community as much as possible. I would be very happy to see the
majority of Django v2.0 features be a merge of established
community-based projects into the trunk, rather than projects tightly
integrated with Django internals.
That said, some changes can't be done as extensions. One such
reoccurring feature request is for multi-database support. It isn't
something I have a huge need for myself, but it is a reoccurring
request on the Django users mailing list, so it would be good to be
able to support it.
Support for aggregation functions (sum, average, etc) in the ORM is a
feature I have a use for, and I'd like to see - especially since the
lack of aggregation functions is the reason I got involved with Django
in the first place. I'm currently mentoring a GSOC project to
implement this capability, but I expect that this won't be complete
until post v1.0.
I would also expect that the resources around the community will get
some attention. There is plenty of room for new tutorials. There are
also a few contrib services that need much better documentation (in
some cases, _any_ documentation). Given that v1.0 is a point of API
stability, it also makes a good time to add lots of new tutorials and
other documentation. Formalization and documentation of some of
Django's more notable internals (like the _meta model class and the
various extension mechanisms) would also be nice to have.

**Shabda:**  You have worked in many different areas of Django. What suggestion would
you give to somebody who is just looking to start contributing to Django?
Are there some areas, where the learning curve is flatter, and people can
make meaningful contribution more easily?

**Russell:** My suggestion would be "don't be timid - jump in". Even the most
complex parts of Django - like the query system - are relatively
straightforward once you've spent a bit of time walking through them.
If someone is looking to contribute to Django, my suggestion would be
to do the same thing I did when I was starting out - pick a Django
component with a bunch of bugs, pick one, write a test case for the
bug (if there isn't one already), then walk through the code until you
can fix it. Rinse and repeat, but stay in the same component.
Eventually, you'll know that component as well as anyone in the world,
and you'll be able to work on more complex bugs or contribute a
feature. Occasionally, tracking down one bug will lead you into
another component; use that as an opportunity to learn how another
part of Django works.
The other thing to keep in mind is that working on Django means more
than just code - if you want your contributions to be taken seriously,
you need to get in the habit of writing rigorous tests and
comprehensive documentation. Don't just fix the bug and then complain
that we don't commit your tickets - you need to do the whole job.

**Shabda:** A lot of Django people come from diverse backgrounds, You have studied
Physics, Adrian studied journalism, [Malcolm](http://pointy-stick.com/blog/) mathematics and [James](http://b-list.com/)
philosophy. Is there something about with Django which attracts people from
diverse backgrounds (keep simple things simple ...)? Has a lack of people
with CS degree been a problem sometime, as in "We wish there was a CS guy to
discuss this problem with"?

**Russell:** My undergraduate degree is in Physics, but my honours and PhD program
were both in pure CS. Plus, for all their lack of CS degrees, Adrian,
Malcolm and James have certainly proven they know how to cut a line of
code, and there are plenty of people in the Django community with pure
CS backgrounds that contribute ideas and code. I don't think Django is
suffering from a lack of CS experience.
However, I do think that having a multidisciplinary background does
predispose you to a certain way of looking at the world. It has been
my experience that some people with a narrow IT/CS focus lose sight of
the fact that at the end of the day, a computer is a tool, most people
don't like them very much, and only put up with them to the extent
that they help to make their lives better. I can appreciate the
elegance of a neat algorithm as much as the next CS major, but at the
back of my mind I always have the question "How can I use this to
solve a real world problem?"
One of the reasons that I suspect Django has been a success is that it
has never lost sight of the fact that it exists to solve problems.
Internally, it does a lot of smart things, but outwardly it tries to
be as simple as possible. You download the code, and it works - you
don't need to resolve dozens of dependencies. The development server
just works. There is a simple and easily understood pipeline from
request to rendering. Django identifies that building web applications
is a problem that people need to solve, and tries to provide the
simplest possible path to let that happen.

**Shabda:** You have worked with a defense startup, and defense industry is presumed
to be very conservative. What can Django do to market itself to market
itself to people outside web 2.0 niches, such as in large corporations or
defense related software?

**Russell:** There is a running joke in the Australian military that the Army's
idea of the perfect technology advancement would be a self-sharpening
grease pencil. It's not quite that bad, but the military is very
conservative. This is understandable, because lives are potentially on
the line if a technology fails. However, it does breed some
interesting limitations on new technology: in the circles I was
working, IE6 on Windows 2000 was considered to be a very modern web
platform.
The way to market to people outside of Web 2.0 niches is to stop
trying to sell Web 2.0 as if it was an answer to anything. Web 2.0 is
a technology. Web 2.0 may give you a competitive advantage. However,
your customer doesn't give a pair of fetid dingo's kidneys about your
competitive advantage. The way to sell your Web 2.0 idea is to forget
all about how cool the technology is, or how great it is that you can
do this as a RESTful web service. Instead, focus on how your solution
solves a specific problem of your customer in a way that your customer
is comfortable with.
A person in industry has a problem. They need a solution. They don't
particularly care if the solution is implemented using an alien slave
hiding in a cardboard box connected to the server using electric
spaghetti - as long as the solution is cheap, reliable, solves their
problem, and doesn't require them to fundamentally change the way they
do business. If you can convince your customer that your product meets
these criteria, you'll have a long and profitable career in the
military-industrial complex - regardless of whether you use Web 2.0 or
not.

**Shabda:** What is one thing about
Django which you absolutely love, and one thing which you think Django
should do differently?

**Russell:** On a technical level: I think the decision to keep logic out of
templates is 100% correct. Keeping logic out of templates forces the
separation between content and presentation, and simplifies the
interface for the design team, who often will not be gung-ho
programmers. I think this is one of the best decisions that Django has
made - I know there are many in the world that claim that Django's
templates are it's biggest weakness, but I simply don't agree. I take
some solace from the fact that Google has tacitly agreed with this
position by providing Django's templating engine inside their
[App Engine](http://code.google.com/appengine/).
One area that I think Django could do better is in error handling -
especially in the way that exceptions get caught and rethrown. There
are a number of ways that import errors or dangling named URL
references can be misreported in stack traces or 500 pages; given that
both of these are relatively common errors, it would be nice to
provide better handling.
On a community level: I love the general disposition of the Django
community. It's polite and professional, and appreciates the value of
good design.
If I had to pick a flaw, it's probably the way that the core
developers (myself included) sometimes let the community down in the
way we communicate overall project direction. When plans are made
(however informally), they aren't always communicated effectively to
the community. It also isn't always clear (beyond the simple fact that
we're busy) why specific tickets with bug fixes don't get committed to
trunk in a timely fashion.
Even I get bitten by this sometimes - being based in Perth, Western
Australia, the cost of travel prevents me from attending PyCon or the
Sprints in person. As a result, I don't get to participate in the
in-person discussions - I sometimes have to read between the lines of
blog posts and mailing list messages to work out what the plans are,
sometimes confirming details using our secret core developer BatPhone.
This is certainly an area where we could do better. Constructive
suggestions are always welcome.

**Shabda:** Frameworks like Turbogears and Zope take a "best of breed" approach, and
tie together a lot of existing components to build the full framework. In
comparison, Django takes an integrated approach, and most of Django's
components are Django specific. For this reason, many people have accused
Django of having a <acronym title="not invented here">NIH</acronym>.
[syndrome](http://www.b-list.org/weblog/2006/oct/21/django-and-nih/).
What are the pros and cons of both these approaches? Why did Django rebuild
most of its components?

**Russell:** Saying that Django rebuilt most of its components is one way of
looking at it. The other way to look at it is to say that Django
identified a core set of functions that need to be tightly integrated,
and need to meet some specific design goals, and built tools that met
those requirements. In some cases, it's not even fair to say that
components were rebuilt. Remember, Django went public in mid 2005, but
existed as an in-house project at the [Journal-World](http://www2.ljworld.com/) for some time
before that. For some of these components, Django's implementation
predates the 'existing component'.
Those that throw the NIH argument at Django are also ignoring two
important points. Firstly, Django already uses a number of prebuilt
components from elsewhere - the signal dispatcher, the JSON
serializer, and any number of other components are taken from existing
projects. We also have dependencies on external software - most
notably in database backends, but also in serialization and markup
engines.
Secondly, there is the presumption that plumbing together external
projects requires no effort. This is patently false - you only have to
look at the places that Django does rely on external projects to see
why. The MySQLdb 1.2.2 prerelease bindings had all sorts of problems
that require special handling; similar workarounds are required for
unicode handling in recent versions of Markdown. Each version of these
external components can potentially require different handling,
sometimes in subtly incompatible ways. Once you introduce multiple
interacting components, you introduce the problem of incompatibility
between versions of those tools. Integration like this isn't easy.
The Django approach guarantees that everything works out of the box,
and there is only one core configuration to document and test. It
reduces the barrier to entry: making the first act of a new user a
decision about which slightly different template engine will best meet
their needs isn't the best way to make a good impression. It also
focuses the efforts of the community in testing, documentation,
tutorials and advice. If improvements are required, the entire
community is in a position to provide an opinion, not just the subset
of the community that works with that component option. In a
Balkanized world, there will always be a tendency for the question
"How do I do X using tool Y" to be answered "don't use tool Y, use Q
instead".
Building the components from scratch also guarantees that all the
components fit - and I don't just mean that they satisfy their mutual
interfaces. There is also the issue that the core components should
all 'taste' the same - that they follow similar design philosophies,
apply similar assumptions, and so on. This sort of consistency is an
invaluable resource, as it forms a kind of implicit documentation:
once you have learned one part of the system, you automatically know
how other parts of the system work. You don't need to relearn the
rules for each component.
One notable (and frequently made) argument against this approach is
that you lose flexibility. While this is theoretically true, I don't
think it matters much in practice. For all the supposed inadequacies
of the Django template engine, I challenge anyone to present a problem
that can only be solved by using a different templating engine. On top
of that - there's absolutely nothing preventing you from using your
own template engine, or your own database backend, or any other
component if you don't like Django's offerings. If you don't believe
me, go look at Google App Engine.

**Shabda:** What are the focus areas Django testing framework? What utilities does
this add too unittest or doctests? How does this fit in with browser based
tools like [Selenium](http://selenium.openqa.org/)?

**Russell:** Like most things in Django, the test framework aims to be as simple as
possible, but no simpler. It provides an environment in which tests
can be run, it provides the facilities for finding and invoking test
cases, and provides a few helpful web-specific assertions that can be
used during testing. Base Python unittests and doctests get a dummy
database to work with, plus a dummy mail server; the Django TestCase
extends unittest.TestCase to provide fixture loading and a dummy web
client that can be used to programatically poke views.
So far, the Django testing tools have concentrated on testing Django
views as if they were ordinary Python methods - after all, that's what
they are. Selenium takes things another step, allowing for tests of
how a web browser will behave when given the output of a view. I tend
to think that testing the view itself is a more rigorous test, as it
allows for direct inspection of view output. However, Selenium does
have a place, especially when dealing with Javascript or other dynamic
on-page interactions. Adding support for Selenium to Django is on the
to-do list - it just requires someone to do the work.

**Shabda:** Thanks for the interview Russell.

------------------------------------------

This was the interview of Russell Keith-Magee, a longtime Django core contributor based in Perth, Australia. Next we interview Michael Trier, a longtime Django user and evangelist.
Michael's *This Week in Django* has been a great resource for the Django community, and we discuss Django, and how best to market Django. So stay tuned.

Oh and yes, [42topics.com](http://42topics.com/) is live now! ([How are we different](http://42topics.com/blog/what-is-42topics/)?) [And we have a Django section](http://www.42topics.com/django/). So [regsiter](http://42topics.com/register/) let's get rolling!

