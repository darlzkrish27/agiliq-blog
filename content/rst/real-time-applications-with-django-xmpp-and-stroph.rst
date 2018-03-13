Real time applications with Django, XMPP and StropheJS
######################################################
:date: 2010-12-22 02:00:47
:modified: 2010-12-22 13:30:47+05:30
:slug: real-time-applications-with-django-xmpp-and-stroph
:authors: Javed
:tags: xmpp
:summary: Introduction: ============== I'll be discussing how to build real time applications using Django and XMPP. Twitter's homepage, Facebook notifications and gmail are just a few examples which have real time updates. Usually, you would want to use this when you require instant updates about things happening right now. A naive approach would be to have an intensive ajax poll every 'x' seconds. This works, but has a few obvious drawbacks such as scalability and resource wastage.

TL;DR:
==============

- django-pubsub allows you to create Twitter like real-time updates for your models with Admin like ease.
- Demo at http://chat.agiliq.com/pubsub/
- Code at https://github.com/agiliq/django-pubsub


Introduction:
==============

PubSub is a XMPP extension which allows publishing and subscribing to events.
This is useful when you instantly want to notify many clients about something
interesting happening on your server.

Quoting the authors of `PubSub specs`_::

    The protocol enables XMPP entities to create nodes (topics) at a
    pubsub service and publish information at those nodes; an event
    notification (with or without payload) is then broadcasted to all
    entities that have subscribed to the node. Pubsub therefore
    adheres to the classic Observer design pattern and can serve as
    the foundation for a wide variety of applications, including news
    feeds, content syndication, rich presence, geolocation, workflow
    systems, network management systems, and any other application
    that requires event notifications.



To understand this better, think of newspapers as publishers and the
people who subscribe to the newspapers as subscribers. Getting your
daily newspaper is similar to checking your RSS feeds because you
only get information at regular intervals. (Of course you could keep
buying newspapers every 'x' hours, i.e. keep refreshing your RSS
reader so often). This is as inefficient as it sounds. For
instance, you never know if a newspaper actually has new stuff, you
need to buy and read it before you realize there's nothing new. And
there will be always be a delay (unless you are really lucky) between
the time something happens and you read it in the your 'n'th
newspaper of the day.

PubSub solves this problem by having a system where subscribers subscribe to
nodes and the publisher notifies its subscribers when something happens. Nodes
are like TV channels, so you only subscribe to the channels you are interested
in.

How it can be useful to you:
============================

In addition to the points mentioned in the quote, pubSub can be used
in your project to send instant notifications, maintain the same
state across multiple sessions, or monitor live activity.

Introducing django-pubsub:
==========================

`django-pubsub`_ allows you write PubSub enabled applications easily.

`This demo`_ using django-pubsub shows how to use the module. The stack used
in the demo is:

* fcgi
* nginx
* ejabberd with mod_http_bind and mod_pubsub

You can register a model with the pubsub app and all registered models
will automatically send out notification (with the payload) on ``save``
to the default node.

Then you can use the PubSub client provided in ``media`` to subscribe and
receive events from the registered models.

The default payload includes an XML serialized instance of the model.
You can use jquery to extract required ``fields`` from the payload.

You can also directly use ``pubsub.publish`` to publish any item to any
node. An example is provided in the demo included with the app.

How it works:
=============

Since XMPP and HTTP speak different languages, you need some kind of a
bridge to connect these two. This can be achieved using `BOSH`_.

The BOSH bridge is provided by ejabberd's ``mod_http_bind``
module. (You can also use `Punjab`_ for this purpose)

If you are using mod_http_bind, you will also need to setup
appropriate url forwarding using nginx because mod_http_bind listens
on the port 5280 and will not be able to communicate with the client
on port 8080.

Strophe will use the BOSH url to talk to ejabberd. Our PubSub
client which is written using Strophe will then be able to subscribe
and receive notifications from the server.

On the server side, the pubsub app uses xmpppy to send out the correct ``Iq``
stanzas on ``save`` or ``publish``. The client will receive these stanzas
on subscribing to the appropriate node.

Conclusion:
===========

If your app needs real-time updates, push notifications, and scalable
infrastructure, you can make use of XMPP and Strophejs.

References:
===========

ejabberd's blog post on related topic:

http://www.process-one.net/en/blogs/article/introducing_the_xmpp_application_server

a similar project using comet, orbit and twisted instead of xmpp:

http://www.clemesha.org/blog/realtime-web-apps-python-django-orbited-twisted

.. _PubSub specs: http://xmpp.org/extensions/xep-0060.html
.. _django-pubsub: https://github.com/agiliq/django-pubsub
.. _This demo: http://chat.agiliq.com/pubsub/
.. _BOSH: http://en.wikipedia.org/wiki/BOSH
.. _Punjab: https://github.com/twonds/punjab


ab


