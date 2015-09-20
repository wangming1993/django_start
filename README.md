title: Python and Django
speaker: Mike Wang
url: https://github.com/ksky521/nodePPT
transition: cards
files: /js/demo.js,/css/demo.css

[slide]

# Python and Django
## 演讲者：Mike Wang

[slide]

# Install

## setuptools

```shelll
wget https://bootstrap.pypa.io/ez_setup.py -O - | sudo python
```

## Django
1. Download the latest release from our [download page](https://www.djangoproject.com/download/).

2. Untar the downloaded file (e.g. tar xzvf Django-X.Y.tar.gz, where X.Y is the version number of the latest release). 

3. Change into the directory created in step 2 (e.g. cd Django-X.Y).

4. Enter the command sudo python setup.py install at the shell prompt. 

[slide]
## Verifying

- To verify that Django can be seen by Python, type python from your shell. Then at the Python prompt, try to import Django:
```shell
 import django
 print(django.get_version())
```
## Resources

[setuptools](https://pypi.python.org/pypi/setuptools)
[Django](https://www.djangoproject.com/download/)
[Quick start](https://www.djangoproject.com/start/)


[slide]

# Install mysql

1. ```sudo apt-get install mysql-server```
 
2. ```apt-get isntall mysql-client```
 
3. ```sudo apt-get install libmysqlclient-dev```

## Verifying
```
sudo netstat -tap | grep mysql
```

[slide]

# Install MySQLdb

1. ```sudo apt-get install python-dev```
2. Download MySQL for Python, Choose 1.2.4
 `https://pypi.python.org/packages/source/M/MySQL-python/MySQL-python-1.2.4.zip#md5=ddf2386daf10a97af115ffad2ed4a9a0`
3.
```
unzip MySQL-python-1.2.4.zip
cd MySQL-python-1.2.4 
python setup.py build 
sudo python setup.py install
```

## Verifying
```
python
import MySQLdb
```

[slide]
# Start a project
## Create a project: ***mysite***
```
django-admin startproject mysite
```
## The file structure:
```
mysite/
    manage.py
    mysite/
        __init__.py
        settings.py
        urls.py
        wsgi.py
```
[slide]
# Use MySQL
## Change the settings.py

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'django',
        'USER': 'root',
        'PASSWORD': 'abc123_',
        'HOST': '127.0.0.1',
        'PORT': '3306',
    }
}
```

[slide]

> Make sure *django* is a database in your mysql. If not create one.

## Run server
```shell
python manage.py migrate
python manage.py runserver
```

Now you can access `http://127.0.0.1:8000` to view page.

[slide]
# Django model

## Create a app
```shell
python manage.py startapp polls
```
----
## The file structure
```python
polls/
    __init__.py
    admin.py
    migrations/
        __init__.py
    models.py
    tests.py
    views.py
```

[slide]
## Edit the polls/models.py
```python
from django.db import models
import datetime
from django.utils import timezone

# Create your models here.
class Question(models.Model):
    question_text = models.CharField(max_length=200)
    pub_data = models.DateTimeField('data published')

    def __str__(self):
        return self.question_text
    def was_published_recently(self):
        return self.pub_data >= timezone.now() - datetime.timedelta(days=1)


class Choice(models.Model):
    question = models.ForeignKey(Question)
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)

    def __str__(self):
        return self.choice_text
```

[slide]
- Edit the mysite/settings.py file again, and change the INSTALLED_APPS setting to include the string 'polls'.
```python
INSTALLED_APPS = (
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'polls',
)
```
- Now Django knows to include the polls app. 
```shell
 python manage.py makemigrations polls
```
## Show the execute sql.
```shell
python manage.py sqlmigrate polls 0001
```
##  run migrate again to create those model tables in your database:
```shell
python manage.py migrate
```
[slide]
# Playing with the API

```shell 
python manage.py shell
```

```python
import django
import os
from polls.models import Question, Choice
os.environ['DJANGO_SETTINGS_MODULE'] = 'mysite.settings'
django.setup()

Question.objects.all()
Question.objects.filter(id=1)
Question.objects.filter(question_text__startswith='What')
Question.objects.get(pk=1)

q = Question.objects.get(pk=1)
q.choice_set.all()
q.choice_set.create(choice_text='Not much', votes=0)
q.choice_set.create(choice_text='The sky', votes=0)
c = q.choice_set.create(choice_text='Just hacking again', votes=0)
c.question
q.choice_set.all()
q.choice_set.count()

c = q.choice_set.filter(choice_text__startswith='Just hacking')
c.delete()
```


[slide]

nodeppt是基于nodejs写的支持 **Markdown!** 语法的网页PPT，当前版本：1.2.5

Github：https://github.com/ksky521/nodePPT

