Behind the Scenes: Request to Response
######################################
:date: 2012-01-02 18:00:00
:modified: 2012-01-03 02:30:00+05:30
:slug: behind-the-scenes-request-to-response
:authors: thejaswi
:tags: response
:summary: In the previous installment of "Behind the Scenes", we saw how the control flows from `Form to File Storage`_. Today, we are going to see how the application reacts from request to response. In this post, we are going to assume that we are using django's inbuilt ``runserver``. The flow doesn't change much for other WSGI servers available. When you invoke the ``runserver`` management command, the command line options are validated and an instance of `WSGIServer`_ is created and passed the `WSGIRequestHandler`_, which is used to create the request object (`WSGIRequest`_). After the request object is created and the request

In the previous installment of "Behind the Scenes", we saw how the control flows from `Form to File Storage`_. Today, we are going to see how the application reacts from request to response.

In this post, we are going to assume that we are using django's inbuilt ``runserver``. The flow doesn't change much for other WSGI servers available.

When you invoke the ``runserver`` management command, the command line options are validated and an instance of `WSGIServer`_ is created and passed the `WSGIRequestHandler`_, which is used to create the request object (`WSGIRequest`_). After the request object is created and the request started signal is fired, the response is fetched through the `WSGIRequestHandler.get_response(request)`_.

In the ``get_response`` method of the request handler, first the urlconf location (by default the ``urls.py``) is setup based on the ``settings.ROOT_URLCONF``. Then a `RegexURLResolver`_ compiles the regular expressions in the urlconf file. Next, the request middlewares are called in the order specified in the ``settings.MIDDLEWARE_CLASSES`` followed by the view middlewares after matching the view (``callback``) function against the compiled regular expressions from the urlconf. Then the view (``callback``) is invoked and verified that it does not return ``None`` before calling the response middlewares.

You can see the pictorial representation of the flow below:

.. image:: http://agiliq.com/static/dumps/images/20120102/request_to_response.png
   :align: center
   :height: 792 
   :width: 543
   :alt: Request to Response Flow
   :target: http://agiliq.com/static/dumps/images/20120102/request_to_response.png

.. _`Form to File Storage`: http://agiliq.com/blog/2011/09/behind-the-scenes-from-html-form-to-storage/
.. _`WSGIServer`: https://code.djangoproject.com/browser/django/trunk/django/core/servers/basehttp.py#L113
.. _`WSGIRequestHandler`: https://code.djangoproject.com/browser/django/trunk/django/core/servers/basehttp.py#L130
.. _`WSGIRequest`: https://code.djangoproject.com/browser/django/trunk/django/core/handlers/wsgi.py#L128
.. _`WSGIRequestHandler.get_response(request)`: https://code.djangoproject.com/browser/django/trunk/django/core/handlers/base.py#L72
.. _`RegexURLResolver`: https://code.djangoproject.com/browser/django/trunk/django/core/urlresolvers.py#L219

