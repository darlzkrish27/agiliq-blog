Title: Writing styled forms with absolutely minimal HTML (Part-1)
Date: 2014-04-24 09:05:34
Modified: 2014-04-24 19:35:34+05:30
Slug: writing-styled-forms-with-absolutely-minimal-html-
Authors: Rakesh
Tags: Django-crispy-forms
Summary: When controlling the form rendering, we all know that we can use the three pre-baked output format of the forms namely: as_u, as_p and as_table. For example: if we use: {{ registration_form.as_ul}} The output template would be: <li> <label for="id_uname">Userame:</label> <input type="text" name="username" id="id_uname"> <li> <li> <label for="id_email">Email:</lable> <input type="text" name="email" id="id_email"> <li> And so on... Here are some questions that come into the picture: ----------------------------------------------------- 1. What if we to want deal with divs? 2. What if we want to reorder the fields? and what if I have 100 such fields? 3. And what if I want to deal
When controlling the form rendering, we all know that we can use the three pre-baked output format
of the forms namely: as_u, as_p and as_table.

For example: if we use: {{ registration_form.as_ul}}
The output template would be:

    <li>
        <label for="id_uname">Userame:</label>
        <input type="text" name="username" id="id_uname">
    <li>

    <li>
        <label for="id_email">Email:</lable>
        <input type="text" name="email" id="id_email">
    <li>
And so on...

Here are some questions that come into the picture:
-----------------------------------------------------

1. What if we to want deal with divs?
2. What if we want to reorder the fields? and what if I have 100 such fields?
3. And what if I want to deal with model forms? How about field.errors and form.non_field_errors?

* * *

To answer all those significant questions Django-crispy-forms steps into the arena.

What are Django-crispy-forms?
---------------------------------------

* It's an app that brings up the best way to get the DRY Django forms,
providing the tag and filter that lets us quickly render forms in a div format.
This also provides an enormous amount of capability to configure and control the rendered HTML.

* As an added asset Django-crispy-forms support several frontend frameworks like Twitter Bootstrap(versions- 2 and 3),
Uniform and Foundation.

* Using Django-crispy-forms is very simple and effortless.

Let's dive in to learn to use the Django-crispy-forms with Twitter Bootstrap in straightforward 5 steps:

(We assume that you have set up the minimal Django configuration before following the below steps)

+ Step1: Installing using pip.

    `pip install django-crispy-forms`

+ Step-2: Add crispy_forms to the INSTALLED_APPS in settings.py.

    `INSTALLED_APPS = (
        ....
        'crispy_forms',
        )`

+ Step-3: Set up a default template pack in the settings.py for the project (I assume Bootstrap3 here)

    ` CRISPY_TEMPLATE_PACK = 'bootstrap3' `

+ Step-4: Create your desired custom form and view.

Eg: forms.py

    class SimpleForm(forms.Form):
        user_name = forms.CharField(label='Username', required=True)
        ..........

Eg: views.py

    class Index1View(FormView):
        template_name = 'crispy_example/index.html'
        form_class = SimpleForm

+ Step-5: Loading crispy forms tag in the template

In the template, add the below template tags:

    1. {% load crispy_forms_tags %}
    2. {% crispy form %}

Add the first tag in the initial part to the template and the second tag in the place where you want to render the form.

That's all, the above steps results you an impressive bootstrap form.

You can have a look at the complete example that I have created in github over here: [Link](https://github.com/krvc/crispy_example)


