---
title: Django CMS에 Application 추가 방법
date: 2019-09-04 11:37:28 -0400
categories: jekyll update
---

# Django CMS에 Application 추가 방법

## App 생성

polls 이름의 Application 생성

``` python
$> python manage.py startapp polls
```

## app.py 수정

`verbose_name`을 추가하면, Django CMS에서 앱의 이름을 설정 할 수 있다.
Django CMS Admin 페이지에서는 보일때 대문자로 보인다.

``` python
from django.apps import AppConfig

class PollsConfig(AppConfig):
    name = 'polls'
    verbose_name = 'Polls' # Django CMS에 보이는 앱 이름 설정
```

## __init__.py

``` python
from __future__ import absolute_import # 표준 모듈과 동일한 이름의 로컬 모듈을 사용 가능하게 해줌

__version__ = '0.0.4' # app version

default_app_config = 'polls.apps.PollsConfig' # app의 기본 설정을 polls.apps.PollsConfig애서 가지고와서 설정
```

## models.py

Model를 상속하여 Class를 생성하고, Class 안에는 하나의 리턴값을 가지는 함수를 선언하다.

``` python
from django.db import models

class Choice(models.Model):

	def __str__(self):
    	return self.choice_text
```

## admin.py
models.py에서 생성한 클래스를 import하여 admin.site.register에 등록한다.

``` python
from django.contrib import admin
from .models import Choice

admin.site.register(Choice)
```