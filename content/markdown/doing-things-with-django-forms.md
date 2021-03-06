Title: Doing things with Django forms
Date: 2010-01-14 06:26:18
Modified: 2010-01-14 17:56:18+05:30
Slug: doing-things-with-django-forms
Authors: shabda
Tags: tutorial
Summary: Forms are one of the best features of Django. (After models, admin, url routing etc :) ). Here is a quick tutorial describing how to do things with Django forms. 1. Basic form Prob. You want to show a form, validate it and display it. Ans. Create a simple form. class UserForm(forms.Form): username = forms.CharField() joined_on = forms.DateField() This wil take care that the form is displayed with two text field, and a value for them are filled in, and the second field has correct formatting for a date. 2. Prob. Create a form which has values populated depending upon
Forms are one of the best features of Django. (After models, admin, url routing etc :) ). Here is a quick tutorial describing how to do things with Django forms.


1. Basic form

Prob. You want to show a form, validate it and display it.

Ans. Create a simple form. 

    class UserForm(forms.Form):
        username = forms.CharField()
        joined_on = forms.DateField()

This wil take care that the form is displayed with two text field,
and a value for them are filled in, and the second field has correct formatting for a date.

2.

Prob. Create a form which has values populated depending upon a runtime value,
eg you might want to show only the values corresponding to the current subdomain.

Ans. Create a form with an custom `__init__`.

    class UserForm(forms.Form):
        username = forms.CharField()
        plan = forms.ModelChoiceField(queryset = Plan.objects.none())
        
        def __init__(self, subdomain, *args, **kwargs):
            self.default_username = default_username
            super(UserForm, self).__init__(*args, **kwargs)
            self.fields['plan'].queryset = Plan.objects.filter(subdomain = subdomain)
            
Here in the `__init__` we are overriding the default queryset on field `plan`. Any of the attributes can similarly be overridden.

However the `self.fields` is populated only after `super(UserForm, self).__init__(*args, **kwargs)` is called.

3.

Prob. The same form is used in multiple views, and handles the `cleaned_data` similarly.

Ans. Create a form with a custom .save()

    class UserForm(forms.Form):
        username = forms.CharField()
        
        def save(self):
            data = self.cleaned_data
            user = User.objects.create(username = data['username'])
            #create a profile
            UserProfile.objects.create(user = user, ...some more data...)
            
You could have called this method anything, but this is generally called save, to maintain similarity with `ModelForm`
            
    
4.

Prob. You need to create a form with fields which have custom validations.

Ans. Create a form with custom clean_fieldname
    
    class UserForm(forms.Form):
        username = forms.CharField()
        
        def clean_username(self):
            data = self.cleaned_data
            try:
                User.objects.get(username = data['username'])
            except User.DoesNotExist:
                return data['username']
            raise forms.ValidationError('This username is already taken.')
            
Here we can validate that the usernames are not repeated.
            

5.

Prob. You want to create a field which has cross field validations.

Ans. Create a field with a .clean

    class UserForm(forms.Form):
        username = forms.CharField()
        
        password1 = forms.PasswordField()
        password2 = forms.PasswordField()
        
        def clean(self):
            data = self.cleaned_data
            if "password1" in data and "password2" in data and data["password1"] != data["password2"]:
                raise forms.ValudationError("Passwords must be same")
            



6.

Problem.

You need a form the fields of which depends on some value in the database.
Eg. The field to be shown are customisable per user.


Create a form dynamically

    def get_user_form_for_user(user):
            class UserForm(forms.Form):
                username = forms.CharField()
                fields = user.get_profile().all_field()
                #Use field to find what to show.
                
[This post provides much more details](http://uswaretech.com/blog/2008/10/dynamic-forms-with-django/)


7.

Prob. You need to create a Html form which writes to multiple Django models.

Ans. Django forms are not a one to one mapping to Html forms. 

    #in forms.py
    class UserForm(forms.ModelForm):
        class Meta:
            model = User
            fields = ["username", "email"]
            
    class UserProfileForm(forms.ModelForm):
        class Meta:
            model = UserProfile
    
    #in views.py
    def add_user(request):
        ...
        if request.method == "POST":
            uform = UserForm(data = request.POST)
            pform = UserProfileForm(data = request.POST)
            if uform.is_valid() and pform.is_valid():
                user = uform.save()
                profile = pform.save(commit = False)
                profile.user = user
                profile.save()
                ....
        ...
        
    #in template
    <form method="post">
        {{ uform.as_p }}
        {{ pform.as_p }}
        <input type="submit" ...>
    </form>


8.

Prob. You want to use multiple forms of same type on one page.

Ans.

a. If you want a datagrid style ui, use formset.

    from django.forms.formsets import formset_factory
    forms = formset_factory(UserForm, extra = 4)
    #
[Formsets are described much more comprehensively here](http://docs.djangoproject.com/en/dev/topics/forms/formsets/)
    
    
b. If you do not need a datagrid style ui, use `prefix` argument to forms.

    Eg. you have a survey app, and you want a page with all questions from that survey displayed.
    
    #IN views.py
    def survey(request, survey_slug)
        ...
        questions = survey.questions.all()
        question_forms = []
        for question in questions:
            qform = QuestionForm(question=question, prefix = question.slug)
            question_forms.append(qform)
            ...
        if request.method == "POST":
            for question in questions:
                qform = QuestionForm(question=question, prefix = question.slug, data = request.POST)
            #Validate and do save action
            ...
        ...

----------------------------
[Do you subscribe to our feed](http://feeds.feedburner.com/uswarearticles)? We recently made a full text feed available, so if you are using the old feed, you should change it. [Subscribe now](http://feeds.feedburner.com/uswarearticles).


