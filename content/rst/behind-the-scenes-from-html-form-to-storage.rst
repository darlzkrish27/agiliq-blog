Behind the Scenes: From HTML Form to Storage
############################################
:date: 2011-09-21 17:00:00
:modified: 2011-09-21 11:54:11+05:30
:slug: behind-the-scenes-from-html-form-to-storage
:authors: thejaswi
:tags: upload
:summary: In this blog post, we are going to see what happens behind the scenes when a file is uploaded in django. .. image:: http://agiliq.com/dumps/images/20110921/file_storage.gif :align: center :target: http://agiliq.com/dumps/images/20110921/file_storage.gif

In this post, we are going to see what happens behind the scenes when a file is uploaded to a django powered web application.

.. image:: http://agiliq.com/dumps/images/20110921/file_storage.gif
   :align: center
   :height: 792 
   :width: 543
   :alt: File Upload Flow
   :target: http://agiliq.com/dumps/images/20110921/file_storage.gif


An HTML form with a file input (atleast one) and encoding set to `multipart/form-data`_ is submitted. The MultiPartParser_ parses the POST request and returns a tuple of the POST and FILES data ``(request.POST, request.FILES)``. The MultiPartParser processes the uploaded data using the `File Upload Handlers`_ objects (through the `new_file`_, `receive_data_chunk`_ and `upload_complete`_ methods). The ``request.FILES`` values are a sequence of instances of `UploadedFile`_.

In the django form, we pass the ``request.FILES`` MultiValueDict. These UploadedFile instances are validated by the ``full_clean`` method on the Form_. The `full_clean`_ method in turn calls the `_clean_fields`_ method which calls the `clean`_ method on the ``forms.FileField`` and checks if the data is empty.

After the form is successfully validated, we might assign and save the ``cleaned_data`` of the form to a model instance (or by saving the ``ModelForm``). When the ``save`` method on the model instance is called, the ``save_base`` method calls a ``pre_save`` method on each field of the model. This `pre_save`_ method returns the value of the file instance bound to that FileField and calls it's ``save`` method. This `save`_ method (on the models.FileField) in turn calls the `save (Storage)`_ method on the ``Storage`` which is either picked up from the arguments passed to the FileField (``FileField(storage=...)``) or the `DEFAULT_FILE_STORAGE`_ settings attribute.

This is the flow from the HTML Form all the way upto the File Storage. Hope you liked it.

.. _`multipart/form-data`: https://docs.djangoproject.com/en/dev/ref/forms/api/#binding-uploaded-files
.. _MultiPartParser: https://code.djangoproject.com/browser/django/trunk/django/http/multipartparser.py#L31
.. _`File Upload Handlers`: https://docs.djangoproject.com/en/dev/topics/http/file-uploads/#upload-handlers
.. _`new_file`: https://code.djangoproject.com/browser/django/trunk/django/core/files/uploadhandler.py#L87
.. _`receive_data_chunk`: https://code.djangoproject.com/browser/django/trunk/django/core/files/uploadhandler.py#L100
.. _`upload_complete`: https://code.djangoproject.com/browser/django/trunk/django/core/files/uploadhandler.py#L116
.. _`UploadedFile`: https://docs.djangoproject.com/en/dev/topics/http/file-uploads/#uploadedfile-objects
.. _Form: https://docs.djangoproject.com/en/dev/ref/forms/api/
.. _`full_clean`: https://code.djangoproject.com/browser/django/trunk/django/forms/forms.py#L254
.. _`_clean_fields`: https://code.djangoproject.com/browser/django/trunk/django/forms/forms.py#L273
.. _`clean`: https://code.djangoproject.com/browser/django/trunk/django/forms/fields.py#L493
.. _`pre_save`: https://code.djangoproject.com/browser/django/trunk/django/db/models/fields/files.py#L244
.. _`save`: https://code.djangoproject.com/browser/django/trunk/django/db/models/fields/files.py#L84
.. _`save (Storage)`: https://code.djangoproject.com/browser/django/trunk/django/core/files/storage.py#L34
.. _`DEFAULT_FILE_STORAGE`: https://docs.djangoproject.com/en/dev/ref/settings/#std:setting-DEFAULT_FILE_STORAGE

