Title: Writing styled forms with absolutely minimal HTML (Part-2)
Date: 2014-04-28 09:43:02
Modified: 2014-04-28 20:13:02+05:30
Slug: writing-styled-forms-with-absolutely-minimal-html2
Authors: Rakesh
Tags: crispyforms
Summary: Writing styled forms with absolutely minimal HTML (Part-2) =================================================================== In the [earlier post](http://agiliq.com/blog/2014/04/writing-styled-forms-with-absolutely-minimal-html-/) we have cruised through the fundamentals of using crispy forms for writing styled forms with minimal HTML. At this time, let us dip into getting a clear understanding on how to customize the crispy forms. Before diving into the crux of this post we will become aware with the following key words: 1.FormHelper 2.Layout FormHelper ----------------------- * Django-crispy-forms implements a class called FormHelper to define the form rendering behavior. * Helpers give us a way to control the form attributes and its layout. * They control global

In the [earlier post](http://agiliq.com/blog/2014/04/writing-styled-forms-with-absolutely-minimal-html-/) we have cruised through the fundamentals of using crispy forms for writing styled forms with minimal HTML.

At this time, let us dip into getting a clear understanding on how to customize the crispy forms.

Before diving into the crux of this post we will become aware with the following key words:

1. FormHelper
2. Layout

FormHelper
-----------------------
* Django-crispy-forms  implements a class called FormHelper to define the form rendering behavior.
* Helpers give us a way to control the form attributes and its layout.
* They control global form rendering behavior.

Layout
---------------------------
* It is an another powerful class of Django-crispy-forms which allow us to change the way the form fields are rendered.
* This allows us to set the order of the fields, wrap them in divs or other structure, etc.
* Custom output
* Flexible and highly reususable


In the first place we will get to know how to render a model form and then we will be customizing the divs and fields in the template.


Handling ModelForm with a Django-crispy-form.
--------------------------------------------------------

As we know ModelForms render Model fields as HTML, let us do this using the crispy-forms now.

Now lets render an Address form containing the fields like name, street, landmark and so on...

The basic form stays the same except that we will be appending a FormHelper class.

The snippet of forms.py:

    class AddressModelForm(forms.ModelForm):
        def __init__(self, *args, **kwargs):
            super(AddressModelForm, self).__init__(*args, **kwargs)
            self.helper = FormHelper(self)

        class Meta:
            model = Address

In the above code when the AddressModelForm is called the crispy-forms builds a default layout using form.fields for us, so we don't have to manually list all the fields whatever number of fields it may contain.

Then creation of the views stays the normal way as we do for a normal model-form like this:

views.py:
    `form = AddressModelForm()`

In the template we just need to add the crispy form tag and that's it and our mission is accomplished.

address.html

    {% load crispy_form_tags %}
    {% crispy form %}

That gets us done with handling ModelForm with a Django-crispy-form.

**********************

Customizing the form
-------------------------------

Now in the second phase we shall start customizing our form using the powerful classes of crispy-forms -- Layouts and FormHelper.

In the above example, we could just render the form fields, but we are couldn't able to assign any attributes to the form.

In the normal scenario we tend to use the form_action and form_method in the template's form attributes, but in the crispy-forms we can do it by specifying them in the form itself. For this we take the help of FormHelper.

Here is the above example rewritten to get the needed task done:

    The snippet of forms.py:

    class AddressModelForm(forms.ModelForm):
        def __init__(self, *args, **kwargs):
            super(AddressModelForm, self).__init__(*args, **kwargs)
            form_method = 'post'
            form_id = 'address-form-id'
            form_show_errors = True
            render_hidden_fields = False
            self.helper = FormHelper(self)
        
        class Meta:
            model = Address

Additionally, we have got the capability to add custom helper attributes and Bootstrap Helper attibutes:

Examples for Bootstrap helper attributes:

    `help_text_inline`
    `error_text_inline`
    `label_class`
    `field_class` and so on..


Reorder the form fields
----------------------------------------------

Now it's time for learning the approach to render the form of a template in the customized way. For this we can use the Layout class.

With Layout class we can arrange the fields according to the requirement.


* What if we want to reorder the fields? And what if I have 100 such fields?

Example: forms.py

    class ExampleFieldForm(forms.Form):
        helper = FormHelper()
        helper.layout = Layout('name', 'email', 'password')


Power of Layouts:
-------------------------

The Power of Layouts becomes much more powerful with layout objects.
Every layout object has an associated template.

There are three types of layout objects.

1. Universal layout objects (Div, HTML, Field, Fieldset..)
2. Uniform layout objects (ButtonHolder, MultiField)
3. Bootstrap layout objects (FormActions, AppendedText, Alert...)

Using the layout objects will result in tailor made outputs in the relatively simplest way.

Example:

    class RegistrationForm(forms.Form):
    [...]
    def __init__(self, *args, **kwargs):
        super(RegistrationForm, self).__init__(*args, **kwargs)
        self.helper = FormHelper()
        self.helper.layout = Layout(
            Fieldset(
                'Registration form',
                'name',
                'Father's name,
                'email',
               ),
            Div(
                'Address',
                'City',
                'pincode'
                css_id = 'coloured_fields'
                ),
            Button(
                Submit('submit', 'Submit'),
            HTML(" {% if success %} <p> Successfully submitted </p> {% endif %}")
            )
        )

Explaination:

- The fieldset attribute renders the name, fathers's name, email fields wrapped in a fieldset and the first argument in the fieldset is rendered as a legend for the fieldset.

- The Div attribute wraps the fields Address, city, pincode in a div.

- Button takes name and value resulting a beautiful button.

- With HTML attribute we can render HTML content and the context can be accessed from where the form is rendered


