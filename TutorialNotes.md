<!-- TOC depth:6 withLinks:1 updateOnSave:1 orderedList:0 -->

- [Tutorial 1](#tutorial-1)
	- [Creating models](#creating-models)
	- [Activating the Model](#activating-the-model)
- [Tutorial 2](#tutorial-2)
<!-- /TOC -->

# Tutorial 1

To create a site at very beginning

```python
django-admin startproject mysite
```

It creates several files under `my_site` directory.
* `manage.py`: command-line utility that lets you interact with this Django
  project in various ways. Here is [more details](https://docs.djangoproject.com/en/1.8/ref/django-admin/)
* `settings.py`: Settings/configuration for this Django project. Reference [here](https://docs.djangoproject.com/en/1.8/topics/settings/)

The most important parts of `settings.py` are `INSTALLED_APPS` and `DATABASES`.

We create database tables by doing and then we run the server by doing
```python
$ python manage.py migrate
$ python manage.py runserver
```

## Creating models

We create an app by doing:
```
$ python manage.py startapp polls
```

We will create 2 models in `polls/models.py`:
```python
class Question(models.Model):
    question_text = models.CharField(max_length=200)
    pub_date = models.DateTimeField('data published')

    def __str__(self):
        return self.question_text

    def was_published_recently(self):
        return self.pub_date >= timezone.now() - datetime.timedelta(days=1)


class Choice(models.Model):
    question = models.ForeignKey(Question)
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)

    def __str__(self):
        return self.choice_text
```

## Activating the Model

We need to add polls in `INSTALLED_APPS` in `mysite/settings.py`. To tell Django
we made some changes to our model, we need to do:
```
$ python manage.py makemigrations polls
$ python manage.py migrate
```

# Tutorial 2

To create an admin:
```
$ python manage.py createsuperuser
```

After creating username and password and start the server, go to the address
`127.0.0.1:8000/admin/`

Then we can add `polls` application in the admin site. Modify `polls/admin.py` by
adding the following:
```python
admin.site.register(Question, QuestionAdmin)
```

We could then customize the forms by adding the following. Note that the `classes`
can be any HTML classes.
```python
class QuestionAdmin(admin.ModelAdmin):
    fieldsets = [
        ('Melon',     {'fields': ['question_text']}),
        ('Date info', {'fields': ['pub_date'], 'classes': ['collapse']})
    ]

admin.site.register(Question, QuestionAdmin)
```
